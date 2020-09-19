---
title: '.NET Core, Continuous Integration e provider di identità esterni'
date: Fri, 20 Mar 2020 22:16:49 +0000
draft: false
tags: ['.net core', '.NET Core', 'identity', 'integration testing', 'jwt']
---

Sviluppando una Api il primo problema da affrontare è sicuramente la gestione degli utenti, una possibile soluzione può essere sicuramente demandare le operazioni di registrazione e accesso a **Identity Provider** **esterni** quali, ad esempio, Apple (principalmente per il mercato macOS e iOS) e Google (per Android, gli utilizzatori di Chrome e in generale per chiunque non abbia un Id Apple).

Lavorando con delle Api utilizzeremo come mezzo di identificazione un token Jwt (Json Web Token), uno standard abbastanza recente con cui il server dell'Identity Provider (che potremmo essere anche noi stessi) restituisce al client un **token firmato, generalmente non crittografato**, con delle informazioni base dell'utente (id, nome, cognome, email, ecc) con le quali il client potrà completare l'interfaccia senza dover fare un'ulteriore richiesta ma soprattutto, contendo delle informazioni utili anche al server, quando quest'ultimo dovrà identificare l'utente non dovrà più collegarsi al database o verificare una sessione (se l'applicazione lavora dietro un load balancer quest'operazione richiederà nuovamente un collegamento ad una base di dati), in quanto il token **contiene già le informazioni necessarie ed è firmato da un'autorità di nostra fiducia**. Di contro un token firmato aggiunge un leggero (se non lo appesantiamo con troppe informazioni, bisogna sempre trovare l'equilibrio tra tutti gli attori in gioco) **overhead** su ogni richiesta autenticata ed è più sensibile ad attacchi MitM (Man-in-the-Midlle, nel 2020 non ci dovrebbe essere bisogno di dirlo, **HTTPS è tassativo** con questa soluzione!).

Molto importante è il **testing del codice**, non siamo infallibili, ma utilizzando i giusti approcci (es. [Tdd](https://it.wikipedia.org/wiki/Test_driven_development)) possiamo limitare il danno, evitando che certe oscenità arrivino in produzione. Testare però un provider esterno **non è il nostro compito** (lui stesso durante la stesura del proprio server, o eventualmente colui che l'ha sviluppato per suo conto, provvederà ad eseguire i propri integration test) quindi dobbiamo **isolare il più possibile l'autenticazione**. Il compito non è semplice, se sviluppi (a titolo esemplificativo) un gestionale o la nuova applicazione per le Tesla di Elon Musk, hai delle sezioni protette da autenticazione (e conseguente identificazione) e non puoi semplicemente applicare l'accesso anonimo.

!["Unit tests? We don't do that here", meme from Avangers 3](https://fablabromagna.org/wp-content/uploads/2020/03/383h35.jpg)

La tipica persona che non leggerà quest'articolo (fonte: [imgflip.com](https://imgflip.com/i/383h35))

### TL;DR

La nostra soluzione sarà aggiungere un provider di autenticazione fittizio (non effettueremo nemmeno i controlli sulla validità del token) che ci permetterà di effettuare il test delle nostre Api.

Gli strumenti che utilizzeremo
------------------------------

Oltre ai sopracitati token Jwt ci servirà **una libreria** che ci permetterà di effettuare la **verifica** (e successivamente anche **l'emissione**) degli stessi, un **client Http** (in questo caso utilizzeremo uno strumento presente in Visual Studio Code e nativamente anche in Rider, non ho verificato - anche se immagino ci sia - in Visual Studio; alternativamente consiglio **Postman**) e **xUnit** per i test. Do per scontato che sia già presente un progetto configurato e già strutturato, l'obiettivo non è spiegare l'architettura di un progetto, che varia molto anche da cosa si deve sviluppare.

**Microsoft ha già realizzato una libreria** per .NET Core estremamente semplice ma allo stesso tempo completa per la gestione dell'autenticazione bearer Jwt, in _nuget_ la potete trovare con il namespace [Microsoft.AspNetCore.Authentication.JwtBearer](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.JwtBearer/5.0.0-preview.1.20124.5), aggiungetela sia al progetto Api che al progetto di Integration Test.

Creazione del Dto
-----------------

Un Dto (Data Transfer Object) per definizione è un oggetto di trasporto, normalmente lo utilizziamo nelle Api per dati in ingresso o in uscita, in questo caso però lo utilizzeremo per rappresentare i singoli Identity Provider, con le loro proprietà.

Successivamente spiegherò meglio le proprietà, per ora limitiamoci alla sua creazione.

Il file di configurazione appsettings.json
------------------------------------------

Nel progetto Api è presente un file _appsettings.json_, è necessario inserire una proprietà `IdentityProviders` di tipo vettore, con **almeno un oggetto valido** (l'esempio sotto non può essere usato in produzione, i codici ovviamente non sono validi, ma sono comunque funzionanti per l'obiettivo della guida). Gli oggetti devono contenere le stesse proprietà del Dto che abbiamo creato precedentemente.

Spieghiamo meglio le proprietà della sezione:

*   **Name:** è il nome del client dell'Identity Provider (es. Google-Browser, Google-Android, Google-iOS), **deve essere univoco**;
*   **Issuer:** è l'autorità emittente del token; qualsiasi token provieniente da Google (inclusi i G Suite) sono emessi da `https://accounts.google.com`, se non avete nessun token vi consiglio di lasciare questo, senza uno valido il server genererà degli errori 500 ad ogni richiesta in quanto non riesce a verificare la firma sul token Jwt;
*   **Audience:** rappresenta il bacino di utenza; è l'unico campo che ci costringe ad avere più configurazioni per lo stesso identity provider in quanto è comune se si supportano più piattaforme (es. Browser, Android, iOS) avere più _audience_ facenti capo allo stesso progetto/sviluppatore, ai fini dell'autenticazione in questo progetto sono tutti provider diversi.

Nel caso non fosse chiaro, `IdentityProviders`, essendo un campo array, possiamo inserire più oggetti con varie configurazioni di vari provider.

Realizziamo il nostro Startup
-----------------------------

Arriviamo nella prima fase critica del progetto, nel metodo `ConfigureServices` dobbiamo dire al server di autenticare tramite Jwt, quali sono gli IdP e soprattutto che ne abbiamo più di uno.

**Nelle righe 6-10** stiamo dicendo a .Net di prepararsi a configurare l'autenticazione e che il metodo di default è Jwt, questo perché **sono supportati vari metodi di autenticazione e autorizzazione** contemporaneamente, quindi è necessario specificare quello principale.

**Nelle righe 12-13** stiamo creando una lista di Idp (ecco che torna il nostro Dto!) a partire dal file di configurazione.

**Nelle righe 15-28** stiamo ripetendo per ogni Identity Provider l'aggiunta di uno schema specifico, ogni schema per essere identificato **deve avere un nome univoco**; successivamente specifichiamo ulteriori controlli (scadenza, ecc sono già abilitati di default, ho sovrascritto solo quelli che mi interessava cambiare): la validazione della chiave di firma e l'emittente. Da notare che **non ho indicato un certificato o una chiave**, non mi interessa conoscerli, nelle righe successive oltre a specificare il bacino d'utenza di riferimento dello schema (_audience_), specifico anche chi è l'autorità emittente del token, .Net **si preoccuperà di visitare poi una pagina specifica** (es. [quella di Google](https://accounts.google.com/.well-known/openid-configuration)) **contente tutte le informazioni di firma** (è fondamentale che la pagina supporti Https, è possibile disabilitare l'errore ma lo sconsiglio vivamente per gli ambienti di produzione), ecc.

**Nelle righe 30-38** indico a .Net chi sono gli schema da utilizzare tra quelli precedentemente aggiunti (tutti, creo un vettore a runtime contente tutti i nomi) e **quali ambiti di dati supporto** (gli _scope_).

Proviamo ad autenticarci...
---------------------------

Sì, stiamo facendo **una deroga al Tdd**, in cui dovremmo prima fare i test e poi sviluppare il resto ma altrimenti l'articolo risulterebbe veramente noioso.

Creiamo ora **un controller di prova**, ci servirà per avere qualcosa da vedere (ora) e testare (dopo), molto semplicemente quando visiteremo la pagina `/Index` se l'autenticazione avrà successo restituirà `Hello, World.` altrimenti riceveremo un bel **codice di stato 401**.

Con la seguente richiesta Http il dovremmo venir **respinti**:

Mentre con quest'altra **dovremmo ricevere il nostro saluto** (sostituite `<token>` con un token dal vostro Idp, es. Google tramite [Google Playground](https://developers.google.com/oauthplayground/)).

Integration Testing (pt. 1)
---------------------------

Arriviamo alla parte che avremmo dovuto fare fin dall'inizio, a noi non interessa (in quest'articolo, sia chiaro) creare dei test ma come creare l'ambiente in cui eseguirli.

![](https://fablabromagna.org/wp-content/uploads/2020/03/2uxn79-300x199.jpg)

Non siamo Chuck Norris, testate il codice, grazie. (fonte: [imgflip.com](https://imgflip.com/i/2uxn79))

_<spoiler>_  
Utilizzeremo una **fixture** (per chi non le conoscesse ecco [la spiegazione su Wikipedia \[EN\]](https://en.wikipedia.org/wiki/Test_fixture), in breve si assicurano di ricreare **sempre lo stesso contesto per rendere ripetibili i test**) per creare l'ambiente dove poi verranno eseguiti i test specifici per il progetto che stiamo realizzando (ne vedremo uno elementare, ovviamente).  
_</spoiler>_

_Piccola nota prima di partire con la spiegazione del codice sopra: **sarebbe opportuno applicare la Dependency Injection** anche su questa classe._

**Nelle righe 8-9** stiamo memorizzando per uso successivo la chiave di firma del token. **Anche se non verrà controllata è comunque richiesta.**

**Nella riga 13** creiamo un server fittizio per l'esecuzione delle chiamate alle Api, come si può notare utilizziamo lo stesso `Startup` di produzione (ovviamente, dovendo avere le stesse condizioni).

**Nella riga 14** stiamo dicendo a .Net che la configurazione di `Startup` non è sufficiente (mi raccomando: non è sufficiente, non la stiamo sovrascrivendo ma integrando).

**Nelle righe 16-34** stiamo aggiungendo **un nuovo schema di autenticazione** come nello `Startup`, questo ci permetterà di avere una credenziale fittizia (ma per il calcolatore comunque valida) per testare le nostre Api. Attenzione, non essendo un token valido (l'issuer, ecc non esistono) e **non essendo esposto ad Internet** sono stati disabilitati tutti i controlli sul token (infatti a differenza dello `Startup` non abbiamo impostato una _authority_ di verifica).

Tutto questo non basterebbe però: abbiamo aggiunto lo schema ma dobbiamo dire a .Net di utilizzare quello schema. Non possiamo procedere come prima, se eseguissimo semplicemente `AddAuthenticationSchemes("Testing")` **sovrascriveremmo tutti gli schema di produzione** (e dovremmo ripetere del codice che in produzione potrebbe cambiare, malissimo, ricordate sempre il principio [Dry](https://it.wikipedia.org/wiki/Don%27t_repeat_yourself)), male, devono rimanere. Per questo motivo nelle **righe 36-42** combiniamo la configurazione precedente con quella attuale. La riga successiva serve esclusivamente per evitare che .Net nasconda dei dati nei log che in produzione sarebbero riservati ma che durante il testing **velocizzano l'eventuale debugging di una build fallita**.

**Nella riga 45** stiamo salvando per utilizzi successivi durante i test il **client Http fittizio associato al server**.

**Nelle righe 48-78** abbiamo creato un metodo per **generare un token Jwt** per autenticare le richieste durante i test.

Integration Testing (pt. 2)
---------------------------

Siamo arrivati alla parte finale: il test vero e proprio.

Nel codice di seguito utilizzando la fixture appena creata creiamo una richiesta che autentichiamo con il token Jwt. Dopo aver ottenuto la risposta del server verifichiamo che il codice di stato sia positivo e il contenuto uguale a quello previsto.

Tutto il codice fa parte del progetto, attualmente in sviluppo, _Makers Portal_ di FabLab Romagna, appena rilasciato su GitHub.