---
title: 'Utilizzare mySQL con Entity Framework'
date: Fri, 10 Apr 2015 16:06:00 +0000
draft: false
tags: ['.NET', '.NET', 'Entity Framework', 'mySQL', 'mySQL', 'WPF', 'XAML', 'XAML']
---


#### 
[Marco Tonchella]( "marco.tonchella@gmail.it") - <time datetime="2020-05-12 19:41:51">May 2, 2020</time>

Ciao, ottimo articolo ti ringrazio. Io ho provato oggi in data 12/05/2020 poiché gradirei sviluppare un applicativo con MySQL basato su interfaccia grafica WPF. Ho provato ma a me non funziona! Ho installato sulla macchina MySQL 8.0, i .NET connector ed il relativo MySQL for Visual Studio. Ho usato NuGet per l'installazione di MySQL,Data e di MySQL.Data.Entity ed ho seguito tutti i passaggi sino alla costruzione della connessione. Io a differenza tua volevo lavorare in modalità DBFirst, ossia aggiornando il model attraverso MySQLWorkBech per poi ricaricare il modello, La connessione di DB pare farla senza problema alcuno, il problema è che poi non mi apre la struttura delle tabelle. Si chiude e non costruisce nessun modello. Se provo a farlo DBFirst mi pare accada la medesima cosa. Non capisco dove ho sbagliato, se puoi aiutarmi o darmi dei riferimenti te ne sarei davvero grato.
<hr />
#### 
[Giulio Pierucci]( "giulio.pierucci77@gmail.com") - <time datetime="2020-06-03 07:37:34">Jun 3, 2020</time>

Sei un grande! Grazie
<hr />
#### 
[Marco Tonchella]( "marco.tonchella@gmail.it") - <time datetime="2020-05-12 23:50:29">May 2, 2020</time>

Ok, la soluzione sta qui: https://dev.mysql.com/doc/visual-studio/en/visual-studio-application-config.html Facevo tutto in modo corretto, l'unica cosa che mi mancava era di abilitare l'App all'utilizzo di EF6 con l'apposito strumento presente nel Solution Explorer. Dopo aver abilitato l'App bisogna ricompilare la soluzione e poi si può tranquillamente procedere all'inserimento di EF6 Code First oppure DB First. Un saluto, mt
<hr />
