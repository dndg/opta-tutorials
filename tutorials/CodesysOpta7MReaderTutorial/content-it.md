# Leggere un Finder serie 7M via Modbus RTU con Finder OPTA in CODESYS

## Panoramica

CODESYS è uno dei principali ambienti di sviluppo per PLC e consente di
programmare Finder OPTA con linguaggi standard, come LD e ST.

Questo tutorial guida alla lettura di un Finder serie 7M con Finder OPTA 
in pochi semplici passi.

## Obbiettivi

- Configurare Finder OPTA tramite Ethernet per la lettura dei registri di un Finder serie 7M in CODESYS
- Leggere i registri di un Finder serie 7M tramite Modbus RTU in CODESYS

## Requisiti

Prima di iniziare, assicurati di avere:

- [PLC Finder OPTA CODESYS](https://opta.findernet.com/it/codesys) (x1)
- [Alimentatore switching 12W o 25W per
 OPTA](https://opta.findernet.com/it/codesys#moduli-espansione)(1x)
- Finder serie 7M con Modbus RTU (x1)
    - [7M.24.8230.0210](https://www.findernet.com/it/italia/series/serie-7m-contatori-di-energia/type/tipo-7m-24-contatore-di-energia-monofase-bidirezionale-con-display-lcd/)
    - [7M.38.8400.0212](https://www.findernet.com/it/italia/series/serie-7m-contatori-di-energia/type/tipo-7m-38-contatori-di-energia-multifunzione-bidirezionale-80-a/)
- Cavo Ethernet (x1)
- Ambiente di sviluppo CODESYS installato con plug-in OPTA Configurator.
  Trovi una guida all'installazione [a questo
   link](https://opta.findernet.com/it/tutorial/codesys-plugin-tutorial).
- Rete configurata correttamente: il PC deve comunicare correttamente con 
  Finder OPTA tramite Ethernet. Trovi una guida alla configurazione [a 
  questo link](https://opta.findernet.com/it/tutorial/codesys-via-ethernet).

Per seguire questo tutorial, sarà necessario collegare il contatore di energia Finder 
serie 7M alla rete elettrica e fornire un carico adeguato. Sarà inoltre necessario 
alimentare il Finder Opta con l'alimentatore e configurare correttamente 
la connessione seriale RS-485. Il diagramma sottostante mostra la configurazione 
corretta dei collegamenti tra il Finder Opta e il Finder serie 7M.

![Connection](assets/connection.svg)

## Istruzioni

In questo tutorial i parametri di configurazione utilizzati per la
comunicazione Modbus con il Finder serie 7M sono:

* Indirizzo Modbus: `1`.
* Baudrate: `38400`.
* Stop bit: `1`.
* Parity: `NO`.

Possiamo impostare questi valori tramite NFC utilizzando [l'applicazione Finder
Toolbox NFC](https://www.findernet.com/it/italia/supporto/software-e-app/).

### Creazione progetto CODESYS

Apri CODESYS.

![Open CODESYS](assets/it/01-welcome.png)

Crea un nuovo progetto e scegli `Progetto standard`.

![New project](assets/it/02-new-project.png)

Assicurati che il dispositivo sia `Finder Opta`, poi seleziona il 
linguaggio del programma (in seguito usiamo ST).

![Standard project](assets/it/03-standard-project.png)

### Identificazione Finder OPTA via Ethernet

A questo punto fai doppio click sulla voce `Device (Finder Opta)` 
del menu `Devices`, si aprirà una scheda come mostrato qui sotto.

![Device](assets/it/04-device.png)

Premi il bottone `Sfoglia la rete` e assicurati di vedere il
dispositivo Finder OPTA comparire sotto il Gateway.

![Scan Network](assets/it/05-scan-network.png)

### Configurazione Modbus

Adesso che hai verificato la connessione tra PC e Finder OPTA, premi 
il tasto destro sulla voce `Device (Finder Opta)` e seleziona `Aggiungi 
dispositivo`.

![Device Menu](assets/it/06-device-menu.png)

Dalla lista seleziona `Modbus COM Port` e fai click su `Aggiungi dispositivo`.

![Add Modbus COM port](assets/it/07-add-modbus-com-port.png)

Adesso imposta i valori della porta seriale:

* Porta COM: `2`, ovvero la porta RS-485 del Finder OPTA.
* Velocità in baud: `38400`.
* Parità: `NONE`.
* Bit di dati: `8`.
* Bit di stop: `1`.

![Set Modbus COM port](assets/it/08-set-modbus-com-port.png)

Dopo aver impostato i valori della porta seriale, premi il tasto destro su `Modbus_COM_Port(Modbus COM Port)` e poi su `Aggiungi dispositivo`.

![Modbus COM port menu](assets/it/09-modbus-com-port-menu.png)

Dalla lista seleziona `Modbus Client, COM Port` e fai `Aggiungi dispositivo`.

![Add Modbus client](assets/it/10-add-modbus-client.png)

Poi premi il tasto destro sull'elemento appena aggiunto nel menu laterale `Modbus_Client_COM_Port(Modbus Client, COM Port)` e fai `Aggiungi dispositivo`.

![Modbus client menu](assets/it/11-modbus-client-port-menu.png)

Seleziona `Server Modbus, porta COM`, poi `Aggiungi dispositivo`.

![Add Server Modbus](assets/it/12-add-server-modbus.png)

A questo punto clicca sull'elemento appena aggiunto nel menu laterale e assicurati che `Indirizzo del server` sia `1`.

![Set Server Modbus](assets/it/13-set-server-modbus.png)

Nella stessa schermata clicca su `Canale del server Modbus` poi su `Aggiungi canale` in basso a destra.
In questo tutorial leggiamo il valore della frequenza del Finder serie 7M. Come definito nel [manuale 
tecnico del dispositivo](https://cdn.findernet.com/app/uploads/2021/09/20090052/Modbus-7M24-7M38_v2_30062021.pdf), 
il valore di frequenza è contenuto negli Input Register `32498` e `32499` in formato `float`. Pertanto, imposta i valori del canale come segue:

* Nome: `Frequenza`.
* Tipo di accesso: `Read Input Registers (Codice funzione 4)`.
* Trigger: `Ciclico`.
* Tempo di ciclo: `1000`, ovvero una lettura al secondo.
* Offset: `2498`, .
* Lunghezza: `2`.
* Trattamento errore: `Mantieni ultimo valore`.

![Add Channel](assets/it/14-add-channel.png)

Dopo aver premuto `OK` vedrai il riepilogo del canale appena impostato.

![Set Channel](assets/it/15-set-channel.png)

### Preparazione del programma ST

Adesso scriviamo il programma ST che legge il valore di frequenza. 
Il programma ha il compito di invertire la endianness dei byte letti dai 
registri del Finder serie 7M, per poi interpretarli come `float`.

Nel menu laterale, clicca su `PLC_PRG (PRG)`.

![PLC PRG](assets/it/16-plc-prg.png)

Nella parte superiore dell'editor inserisci il seguente codice:

```st
PROGRAM PLC_PRG
VAR
    words: ARRAY[0..1] OF WORD;
    frequency_w: ARRAY[0..1] OF WORD;
    ptr: POINTER TO REAL;
    frequency: REAL;
END_VAR
```

![Program Variables](assets/it/17-program-variables.png)

In the lower part of the editor, insert the following code:

```st
// Flip endianness
frequency_w[0] := words[1];
frequency_w[1] := words[0];

// Interpret as float
ptr := ADR(frequency_w);
frequency := ptr^;
```

![Program Code](assets/it/18-program-code.png)

Nel menu laterale clicca due volte su `Server_Modbus_porta_COM`. Adesso clicca sulla 
sezione `ModbusGenericSerialServer mapping I/O` e nella tabella clicca due volte sulla cella `Variabile` 
per far comparire il pulsante opzioni.

![Add mapping](assets/it/19-add-mapping.png) 

Clicca sul pulsante opzioni per far comparire la lista di variabili, espandi la voce `Application` 
e la voce `PLC_PRG`. A questo punto clicca sulla variabile `words` e premi `OK` per assegnarla al 
canale `Frequenza`.

![Variable association](assets/it/20-variable-association.png)

Il riepilogo mostra la variabile assegnata al canale `Frequenza`.

![Variable Summary](assets/it/21-variable-summary.png)

### Caricamento del programma su Finder OPTA

Adesso puoi caricare il programma sul dispositivo premendo 
il pulsante verde in alto etichettato come `Login`.

![Login](assets/it/22-login.png) 

Il programma viene caricato su Finder OPTA, per farlo partire premi 
il pulsante `Start`.

![Start](assets/it/23-start.png)

La tab `PLC_PRG` mostra in tempo reale il valore della frequenza contenuta 
nella variabile `frequency`, in questo caso `49.98 Hz`.

![Working](assets/it/24-working.png)

## Conclusioni

Seguendo questi passaggi, hai effettuato una lettura Modbus RTU dai registri 
di un Finder serie 7M utilizzando Finder OPTA in CODESYS.

Se riscontri problemi, verifica di aver cablato correttamente 
i dispositivi e di aver configurato i parametri Modbus come specificato 
nel tutorial.

<!-- Inserire informazioni di contatto per supporto -->