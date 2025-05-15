# Leggere un Finder serie 6M via Modbus RTU con Finder OPTA in CODESYS

## Panoramica

Finder OPTA è dotato di una porta RS-485 che consente la comunicazione con dispositivi compatibili con il protocollo Modbus RTU, come i Finder serie 6M. In questo tutorial mostreremo, passo dopo passo, come configurare Finder OPTA in CODESYS per leggere correttamente i dati da un analizzatore di rete Finder serie 6M.

## Obbiettivi

- Configurare Finder OPTA tramite Ethernet per la lettura dei registri di un Finder serie 6M in CODESYS
- Leggere i registri di un Finder serie 6M tramite Modbus RTU in CODESYS

## Requisiti

Prima di iniziare, assicurati di avere:

- [PLC Finder OPTA CODESYS](https://opta.findernet.com/it/codesys) (x1)
- [Alimentatore switching 12W o 25W per OPTA](https://opta.findernet.com/it/codesys#moduli-espansione)(1x)
- Analizzatore di rete Finder serie 6M (x1)
  - [6M.TA.9.024.1200](https://www.findernet.com/it/italia/series/serie-6m-analizzatori-di-rete/type/tipo-6mta-analizzatore-di-rete-monofase/)
  - [6M.TB.9.024.1200](https://www.findernet.com/it/italia/series/serie-6m-analizzatori-di-rete/type/tipo-6mtb-analizzatore-di-rete-monofase/)
  - [6M.TF.9.024.1200](https://www.findernet.com/it/italia/series/serie-6m-analizzatori-di-rete/type/tipo-6mtf-analizzatore-di-rete-monofase/)
- Cavo Ethernet (x1)
- Cavo per la connettività RS-485 con una delle seguenti specifiche (x2):
  - STP/UTP 24-18AWG (non terminato) con impedenza di 100-130Ω.
  - STP/UTP 22-16AWG (terminato) con impedenza di 100-130Ω.
- Ambiente di sviluppo CODESYS installato con plug-in OPTA Configurator. Trovi una guida all'installazione [a questo
  link](https://opta.findernet.com/it/tutorial/codesys-plugin-tutorial).
- Rete configurata correttamente: il PC deve comunicare correttamente con Finder OPTA tramite Ethernet. Trovi una guida alla
  configurazione [a questo link](https://opta.findernet.com/it/tutorial/codesys-via-ethernet).

Per seguire questo tutorial, sarà necessario collegare l'analizzatore di rete Finder serie 6M alla rete elettrica e fornire un
carico adeguato. Sarà inoltre necessario alimentare il Finder Opta con un alimentatore da 12-24VDC/500mA e configurare correttamente
la connessione seriale RS-485. Il diagramma sottostante mostra la configurazione corretta dei collegamenti tra il Finder Opta e il
Finder serie 6M.

![Connecting Opta and Finder 6M](assets/connection.svg)

La configurazione Modbus del Finder serie 6M è determinata dalla posizione degli switch DIP, come indicato alla pagina 6 del
[manuale utente](https://cdn.findernet.com/app/uploads/6M.Tx-User-Guide.pdf). In questo tutorial i parametri di comunicazione
previsti sono:

- Indirizzo Modbus: `1`.
- Baudrate: `38400`.

Possiamo impostare questi valori **posizionando entrambi gli switch DIP del Finder serie 6M alla posizione `UP`**, come mostrato
nella figura qui sotto.

![Finder 6M series diagram](assets/6m-diagram.svg)

## Istruzioni

### Creazione progetto CODESYS

Apri CODESYS.

![Open CODESYS](assets/it/01-welcome.png)

Crea un nuovo progetto e scegli `Progetto standard`.

![New project](assets/it/02-new-project.png)

Assicurati che il dispositivo sia `Finder Opta`, poi seleziona il linguaggio del programma (in seguito usiamo ST).

![Standard project](assets/it/03-standard-project.png)

### Identificazione Finder OPTA via Ethernet

A questo punto fai doppio click sulla voce `Device (Finder Opta)` del menu `Dispositivi`, si aprirà una scheda come mostrato qui
sotto.

![Device](assets/it/04-device.png)

Premi il bottone `Sfoglia la rete` e assicurati di vedere il dispositivo Finder OPTA comparire sotto il Gateway, poi premi `OK`.

![Scan Network](assets/it/05-scan-network.png)

### Configurazione Modbus

In questa fase configuriamo la porta RS-485 e i parametri del protocollo Modbus per garantire che il Finder OPTA sia in grado di
comunicare con il Finder serie 6M.

Premi il tasto destro sulla voce `Device (Finder Opta)` e seleziona `Aggiungi dispositivo...`.

![Device Menu](assets/it/06-device-menu.png)

Dalla lista seleziona `Modbus COM Port` e fai click su `Aggiungi dispositivo`.

![Add Modbus COM port](assets/it/07-add-modbus-com-port.png)

Adesso imposta i valori della porta seriale:

- Porta COM: `2`, ovvero la porta RS-485 del Finder OPTA.
- Velocità in baud: `38400`.
- Parità: `NONE`.
- Bit di dati: `8`.
- Bit di stop: `1`.

![Set Modbus COM port](assets/it/08-set-modbus-com-port.png)

Dopo aver impostato i valori della porta seriale, premi il tasto destro su `Modbus_COM_Port(Modbus COM Port)` e poi su `Aggiungi
dispositivo`.

![Modbus COM port menu](assets/it/09-modbus-com-port-menu.png)

Dalla lista seleziona `Modbus Client, COM Port` e fai `Aggiungi dispositivo`.

![Add Modbus client](assets/it/10-add-modbus-client.png)

Poi premi il tasto destro sull'elemento appena aggiunto nel menu laterale `Modbus_Client_COM_Port(Modbus Client, COM Port)` e fai
`Aggiungi dispositivo`.

![Modbus client menu](assets/it/11-modbus-client-port-menu.png)

Seleziona `Server Modbus, porta COM`, poi `Aggiungi dispositivo`.

![Add Server Modbus](assets/it/12-add-server-modbus.png)

A questo punto clicca sull'elemento appena aggiunto nel menu laterale e assicurati che `Indirizzo del server` sia `1`.

![Set Server Modbus](assets/it/13-set-server-modbus.png)

Nella stessa schermata clicca su `Canale del server Modbus` poi su `Aggiungi canale` in basso a destra. In questo tutorial leggiamo
il valore della frequenza del Finder serie 6M. Come definito nel [manuale tecnico del
dispositivo](https://cdn.findernet.com/app/uploads/Modbus_RS485_6MTx.pdf), il valore di frequenza è
contenuto negli Input Register `40085` e `40086` in formato `float`. Pertanto, imposta i valori del canale come segue:

- Nome: `Frequenza`.
- Tipo di accesso: `Read Input Registers (Codice funzione 4)`.
- Trigger: `Ciclico`.
- Tempo di ciclo: `1000`, ovvero una lettura al secondo.
- Offset: `84`.
- Lunghezza: `2`.
- Trattamento errore: `Mantieni ultimo valore`.

Il manuale tecnico del Finder serie 6M conta gli indirizzi a partire da `1` mentre CODESYS li conta a partire da `0`. Per questo
motivo per accedere all'Input Register `40085` indichiamo l'indirizzo `84`.

![Add Channel](assets/it/14-add-channel.png)

Dopo aver premuto `OK` vedrai il riepilogo del canale appena impostato.

![Set Channel](assets/it/15-set-channel.png)

### Preparazione del programma ST

Adesso scriviamo il programma ST che legge il valore di frequenza.

Questo programma accede direttamente all’indirizzo di memoria della variabile e ne interpreta il contenuto come un valore `float`.
Questo passaggio è fondamentale per ottenere la misura reale memorizzata nel dispositivo, trasformandola in un formato leggibile e
utilizzabile dall’utente. In CODESYS il formato `float` è indicato come `REAL`.

Nel menu laterale, clicca su `PLC_PRG (PRG)`.

![PLC PRG](assets/it/16-plc-prg.png)

Nella parte superiore dell'editor inserisci il seguente codice:

```st
PROGRAM PLC_PRG
VAR
    words: ARRAY[0..1] OF WORD;
    ptr: POINTER TO REAL;
    frequency: REAL;
END_VAR
```

![Program Variables](assets/it/17-program-variables.png)

Nella parte inferiore dell'editor inserisci il seguente codice:

```st
// Interpret as float
ptr := ADR(words);
frequency := ptr^;
```

![Program Code](assets/it/18-program-code.png)

Adesso è necessario associare le variabili del programma al canale Modbus, in modo che le variabili contengano i valori letti dal canale.

Nel menu laterale clicca due volte su `Server_Modbus_porta_COM`. Adesso clicca sulla sezione `ModbusGenericSerialServer mapping I/O`
e nella tabella clicca due volte sulla cella `Variabile` per far comparire il pulsante opzioni.

![Add mapping](assets/it/19-add-mapping.png)

Clicca sul pulsante opzioni per far comparire la lista di variabili, espandi la voce `Application` e la voce `PLC_PRG`. A questo
punto clicca sulla variabile `words` e premi `OK` per assegnarla al canale `Frequenza`.

![Variable association](assets/it/20-variable-association.png)

Il riepilogo mostra la variabile assegnata al canale `Frequenza`.

![Variable Summary](assets/it/21-variable-summary.png)

### Caricamento del programma su Finder OPTA

In questa fase, CODESYS scarica il programma e la configurazione sul dispositivo. Questo passaggio è fondamentale per aggiornare
correttamente la configurazione del dispositivo, soprattutto se il Finder OPTA contiene ancora una configurazione obsoleta.

Adesso è possibile caricare il programma e la configurazione sul dispositivo premendo il pulsante verde in alto etichettato come
`Login`.

![Login](assets/it/22-login.png)

Il programma viene caricato su Finder OPTA, per farlo partire premi il pulsante `Start`.

![Start](assets/it/23-start.png)

La tab `PLC_PRG` mostra in tempo reale il valore della frequenza contenuta nella variabile `frequency`, in questo caso `49.94 Hz`.

![Working](assets/it/24-working.png)

## Conclusioni

Seguendo questi passaggi, hai effettuato una lettura Modbus RTU dai registri di un Finder serie 6M utilizzando Finder OPTA in
CODESYS.

Se riscontri problemi, verifica di aver cablato correttamente i dispositivi e di aver configurato i parametri Modbus come
specificato nel tutorial.

<!-- Inserire informazioni di contatto per supporto -->