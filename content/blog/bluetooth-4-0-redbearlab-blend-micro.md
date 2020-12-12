---
title: 'Bluetooth 4.0 - RedBearLab Blend Micro'
date: Thu, 22 May 2014 17:03:00 +0000
draft: false
tags: ['Arduino', 'FabLab Romagna', 'Maker Space Rimini']
author: "Maurizio Conti"
type: "post"
image: "images/post/14200080047_f045f57d67_o.jpg"
---

C’è un nuovo modulo Bluetooth 4.0 nato in casa RedBear e, neanche a dirlo, al suo interno batte un cuore Arduino.

Si tratta di un modulo per le [**IoT**](https://it.wikipedia.org/wiki/Internet_delle_cose).  
Un Arduino con integrato un modulo Bluetooth 4.0 Low Energy.
 
![BlendMicro](/images/post/14200080047_f045f57d67_o.jpg)
<!-- Immagine di Sho Hashimoto (https://www.flickr.com/photos/shokai/) distribuita sotto la licenza CC BY 2.0 (https://creativecommons.org/licenses/by/2.0/deed.it). BlendMicro -->

E’ arrivato oggi al FabLab Romagna, direttamente dalla Cina e lo stiamo collaudando.

Risponde bene.

Windows 8.1 lo vede, esegue il “pairing” e associa i vari [GATT](https://developer.bluetooth.org/gatt/Pages/GATT-Specification-Documents.aspx) che il dispositivo espone.

Anche IPad lo vede e lo “sfoglia”… l’unico a rimanere indietro è il Lumia 520 che avendo a bordo per ora solo la preview di Windows 8.1, fa finta di non vederlo, ma in realtà sono già diventati amici.

![BlendMicro](/images/post/BlendMicro_thumb.jpg)

Anche il nostro Packet Sniffer Bluetooth 4.0 sembra essere contento del nuovo vicino di casa…

![Finestra di "Texas Instruments SmartRF Packet Sniffer Bluetooth Low Energy](/images/post/image_thumb.png)

Sono due settimane che Lorenzo sta arduinizzando [questo modulo della Olimex](https://www.olimex.com/Products/Modules/RF/MOD-nRF8001/)

![nrf8001Lorenzo](/images/post/nrf8001Lorenzo1.png)

ma a conti fatti di sicuro cercherò di convincerlo a fare il porting su questo nuovo nato che, tra ingombri fisici molto ridotti e costi totali ragionevoli (25Euro + spese) alla fine, rispetto alle soluzioni attuali, è molto vantaggioso visto che a bordo ha un modulo BLE Nordic rf8001 e un   Arduino (Atmel ATMega 32u4).

La “cosa Internet” che Lorenzo ha in mente sarà una App che mantiene le mamme tranquille… ma questo è Top Secret e non si deve sapere perchè sarà presentata all’esame di Stato presso l’ITIG Belluzzi Da Vinci di Rimini.

Stay tuned
