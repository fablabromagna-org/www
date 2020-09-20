---
title: 'Trinket: appunti sulla prima installazione'
date: Mon, 28 Apr 2014 10:11:00 +0000
draft: false
tags: ['Arduino', 'Trinket']
author: "Maurizio Conti"
type: "post"
---

Questo documento è una sintesi veloce dei passi necessari all’installazione di Trinket su un PC Windows 8.1 64Bit. Il documento completo è [questo](https://learn.adafruit.com/introducing-trinket/introduction)

### Fase 1: Installare i drivers

Scaricare e dezippare il driver USB di Trinket per Windows 8.1 64bit [da qui](http://learn.adafruit.com/system/assets/assets/000/010/319/original/usbtinyisp_w32_driver_v1.12.zip) 

![Screenshot di esplora risorse che elenca i seguenti file: libusb0.dll, libusb0.sys, libusb0_x64.dll, libusb0_x64,sys, usbtinyisp.cat, usbtinyisp.inf, usbtinyisp_x64.cat](/images/post/image_thumb8.png) 

Visto che i drivers di Trinket non sono firmati, Windows 8.1 vuole essere riavviato in una modalità particolare come [riportato qui](http://answers.microsoft.com/en-us/windows/forum/windows_8-hardware/how-to-install-a-driver-that-does-not-contain/7c3f299b-3483-4c96-8c44-87c7451af222) Per fare questo, riavviare il sistema con il tasto SHIFT premuto poi scegliere: - Risoluzione dei problemi - Opzioni avanzate - Impostazioni di Avvio 

![Menù a tendina del bottone "Arresta", in Impostazioni PC, che elenca: Sospendi, Arresta il sistema e Riavvia il sistema. Con quest'ultima opzione selezionata](/images/post/image_thumb9.png) 

Appena Windows si riavvia, appare una schermata azzurra con una serie di scelte: premere il tasto 7 (Disabilita Impostazioni Firma Drivers) Attento: non F7… proprio il tasto 7 (assicurati che il Num-Lock sia a posto sulla tastiera!!!) Appena Windows riparte, attaccare il Trinket ad una porta USB e andare in gestione dispositivi per installare il driver a mano. 

![Menù a tendina dell'elemento Trinket (figlio di "Altri dispositivi") con l'opzione "Aggiornamento software driver..." evidenziata](/images/post/image_thumb10.png) 

Puntare alla cartella dei drivers scaricati al passo precedente e accettare l’avviso rosso di Windows che ci allerta sul fatto che i drivers non sono firmati. ![Finestra di dialogo di "Sicurezza Windows" che presenta le opzioni: "Non installare il driver" e "Installa il software del driver"](/images/post/image11.png)

Ma non basta. La versione di Windows 8.1 64Bit rompe ancora le scatole!!! In realtà il messaggio è inutile perchè è sufficiente premere il pulsante reset del Trinket per riavviarlo e tutto sembra tornare a posto. ![Finestra di dialogo di "Risoluzione di problemi di compatibilità programma". È necessario un driver con firma digitale. LibUSB-Win32 - Kernel Driver. L'installazione di un driver senza firma digitale è stata bloccata. Disinstallare il programma o il dispositivo che utilizza il driver e visitare il sito Web dell'entità di pubblicazione per ottenere una versione del driver con firma digitale.](/images/post/image_thumb12.png) 

_Nota: Il sito segnato in questo messaggio, contiene i sorgenti del drivers USB. Varrebbe la pena metterci il naso dentro. Si tratta infatti di un driver USB generico che consente ad un programmatore di campagna di bypassare tutta la parte di programmazione kernel mode di Windows per accedere in semplicità a propri progetti hardware basati su USB._ 

Eccoci qua. Siamo a posto.

![Lista di dispositivi. Tra cui "USBtinyISP AVR Programmer", figlio di "LibUSB-Win32 Devices"](/images/post/image_thumb13.png)

### Fase 2: Configurare l’IDE di Arduino

Ora è necessario modificare leggermente l’ambiente IDE di Arduino per supportare un nuovo tipo di Arduino (il Trinket). Dopo aver installato l’IDE (usare la versione 1.0.5!!) scaricare [questo file](http://learn.adafruit.com/system/assets/assets/000/010/777/original/trinkethardwaresupport.zip?1378321062) zip. Al suo interno c’è una cartella hardware che va messa cartella Documenti/Arduino. _(Attento. Questa cartella NON è nell’albero delle cartelle dell’IDE… si trova in Documenti)_ ![Finestra di esplora risorse che mostra le cartelle "libraries" e "hardware"](/images/post/image_thumb14.png) 

OK. Ora lanciamo l’IDE e verifichiamo dal menu Strumenti che veda un nuovo tipo di Arduino che è appunto il Trinket. ![Screenshot dell' IDE di Arduino con l'opzione "Strumenti" > "Tipo di Arduino" > "Arduino Uno" selezionata](/images/post/image_thumb15.png)

Ora serve un file .conf che va scaricato [da qui](http://learn.adafruit.com/system/assets/assets/000/010/769/original/avrdude.conf?1378221965)   e va messo nella cartella hardware\\tools\\avr\\etc dell’IDE al posto di quello di Arduino originale (meglio rinominare l’attuale quindi). 

![Finestra di esplora risorse che mostra il file "avrdude.conf"](/images/post/image_thumb16.png)

### Fase 3 (Opzionale): sostituire il loader/linker
Sul sito di Lady Ada, si raccomanda di sostituire il loader/linker di Arduino perchè ha un bug. Non consente di linkare moduli superiori di 4K quindi, se si desidera patchare il proprio IDE per questa cosa, [scaricare questo EXE](http://learn.adafruit.com/system/assets/assets/000/010/983/original/WINDOWS_ld.zip?1379343097) zippato e inserirlo nella directory dell’IDE hardware\\tools\\avr\\bin.

### Fase 4: test A questo punto è possibile provare il mitico blink.ino, ma attenzione a due cose:

* il led da far lampeggiare è sul pin 1 e non sul 13
* il tipo di Arduino da usare è il Trinket.

![Screenshot dell' IDE di Arduino con l'opzione "Strumenti" > "Tipo di Arduino" > "Adafruit Trinket 8MHz" selezionata](/images/post/image_thumb17.png) 

Per programmarlo inoltre fare attenzione alle seguenti cose

* selezionare il programmatore USBTinyISP
* premere il pulsante di reset sul Trinket in modo da portarlo in boot mode (led rosso lampeggia a 2Hz)

![Screenshot dell' IDE di Arduino con l'opzione "Strumenti" > "Programmatore" > "USBtinyISP" selezionata](/images/post/image_thumb18.png)

Stay Tuned!