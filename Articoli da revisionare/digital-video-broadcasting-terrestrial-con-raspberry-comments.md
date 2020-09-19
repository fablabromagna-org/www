---
title: 'Trasmettitore DVB T con Raspberry Pi'
date: Fri, 20 Jun 2014 14:26:00 +0000
draft: false
tags: ['DVBT', 'Raspberry', 'Raspberry']
---


#### 
[Raspberry FAN]( "fabcal@katamail.com") - <time datetime="2015-04-11 11:06:15">Apr 6, 2015</time>

Comunque si la notizia è giusta, il raspberry può essere utilizzato senza hardware aggiuntivo come un trasmettitore FM molto stabile. Il segnale RF si preleva dal pin n° 7 del gpio (ricordo che la fila del pin 1 ha solo i contatti dispari quindi il 4° pin) con un filo che faccia da antenna, oppure collegando anche la massa con un connettore che consenta la connessione di un'antenna specifica per TX FM. La potenza è di pochi milliwatt, tuttavia quanto basta per mettere un modulo amplificatore di almeno 5 watt. Non solo ma il segnale RF generato è pure stereo e usando il sorgente PiRDSFM viene generato anche il messaggio RDS sia per il nome dell'emittente che per il radiotext. Da qui a prelevare l'ingresso di una scheda audio usb o della WolfSon ci vuole veramente poco e il risultato è ottimo. Unico piccolo difetto. Poiché il segnale è prelevato dividendo il clock del processore, la trasmissione potrebbe avere delle armoniche per cui è preferibile in uscita aggiungere un filtro RF che tagli queste armoniche. In realtà la frequenza di trasmissione può essere impostata da 1 a 250 MHz a livello teorico. Il programma permette l'impostazione da 87.5 a 108 a passi di 100 KHz. La cosa interessante è che senza nessun hardware aggiuntivo è possibile far trasmettere il trasmettitore. Note legali: se la portata resta tra le mura di casa non ci sono problemi, se la portata è più elevata occorre la concessione ministeriale praticamente impossibile da avere! Tuttavia il sistema può essere utilizzato anche per realizzare un radiomicrofono, usando la prima versione PiFM senza stereofonia e RDS con il raspberry A+ che ha un consumo di corrente irrisorio, con una batteria ricaricabile per cellulare e impostando la frequenza in VHF, escluso il microfono vero e proprio a filo da collegare a una scheda audio usb, con una trentina di euro si può fare molto!
<hr />
#### 
[Luigi]( "fzluigi@gmail.com") - <time datetime="2015-05-24 09:31:01">May 0, 2015</time>

Salve, sono rimasto stupefatto dalla notizia che Raspberry possa essere usato nudo e crudo come un trasmettitore FM ad anche stereo cn addirittura RDS, può fornire ulteriori dettagli qualsiasi cosa sarebbe ben accetto Grazie in anticipo G
<hr />
#### 
[Baruzzi Giacomo]( "giacomobaruzzi98@gmail.com") - <time datetime="2015-05-24 13:09:20">May 0, 2015</time>

Salve Luigi, Sostanzialmente per "trasformare" la nostra raspberry pi in un trasmettitore Fm basta installare un software, le allego il sito che spiega come fare http://www.icrobotics.co.uk/wiki/index.php/Turning\_the\_Raspberry\_Pi\_Into\_an\_FM\_Transmitter
<hr />
