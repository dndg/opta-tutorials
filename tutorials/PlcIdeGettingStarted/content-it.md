# Guida al primo avvio di Finder OPTA con PLC IDE

Guida per l'installazione di Arduino PLC IDE e l'attivazione della licenza di Finder OPTA.

## Panoramica

**Arduino PLC IDE** consente di programmare **Finder OPTA** utilizzando i
cinque linguaggi standard **IEC 61131-3**: Ladder Diagram (LD), Functional
Block Diagram (FBD), Structured Text (ST), Sequential Function Chart (SFC) e
Instruction List (IL).

Questa guida mostra come collegare Finder OPTA ad Arduino PLC IDE, attivare la
licenza del dispositivo e configurarlo per il corretto utilizzo con l'ambiente
di sviluppo.

## Di cosa avrai bisogno

Prima di iniziare, assicurati di avere:

- [PLC Finder OPTA](https://opta.findernet.com/it/arduino) (x1)
- Cavo USB-C (x1)
- L'installer di **Arduino PLC IDE**. Puoi scaricarlo [a questo
  link](https://opta.findernet.com/software#arduino).

## Istruzioni

Per ottenere il software Arduino PLC IDE, visita il sito ufficiale e scarica il
file di installazione denominato **Arduino PLC IDE Installer**. Il software è
compatibile con Windows 10 o versioni successive per architetture x64.

![Website](assets/it/00-website.png)

Il programma di installazione include l'IDE e tutti i driver, le librerie e i
core necessari per utilizzare Finder OPTA.

**_Nota: se hai già installato Arduino PLC IDE in passato, assicurati di aver rimosso
le versioni precedenti. Inoltre, rimuovi la cartella `C:\Users\<nome
utente>\AppData\Local\T`._**

### Installare Arduino PLC IDE

Prima di iniziare viene richiesto di accettare la licenza d'uso. Spunta la
casella di conferma e fai clic su _Install_ per procedere.

![License Agreement](assets/01-license-agreement.png)

Per prima cosa vengono installati i **PLC IDE Tools**, un pacchetto di
strumenti essenziali per il corretto funzionamento dell'IDE. Fai clic su
_Next_ per continuare.

![PLC IDE Tools](assets/02-plc-ide-tools.png)

Successivamente, viene richiesto di selezionare una directory di installazione
per i **PLC IDE Tools**. Scegli una cartella di destinazione o utilizza il
percorso predefinito, quindi fai clic su _Next_.

![Destination Folder](assets/03-dest-folder.png)

Fai clic su _Install_ per avviare l'installazione dei **PLC IDE Tools** alla
destinazione scelta.

![Installation](assets/04-installation.png)

Una volta completata l'installazione, premi il pulsante _Finish_ e prosegui.

![Completion](assets/05-completed.png)

A questo punto verrà avviato il setup di Arduino PLC IDE. Anche in questo
caso, seleziona una directory di installazione e premi _Next_.

![PLC IDE Install](assets/06-plc-ide-folder.png)

Procedi con l'installazione premendo il pulsante _Install_.

![Installation](assets/07-installation.png)

Al termine dell'installazione, premi _Finish_ per chiudere la finestra.

![Installation Success](assets/08-installation-success.png)

Comparirà anche la seguente finestra conclusiva, chiudi la finestra per uscire
dall'installer.

![Setup Success](assets/09-setup-success.png)

**_Nota: se dopo l'installazione il software non funziona correttamente,
riavviare il computer può essere utile per completare l'integrazione di driver,
librerie e dipendenze. Se i problemi persistono, prova a ripetere
l'installazione disabilitando l'antivirus e avviando il setup con privilegi
amministrativi._**

### Configurazione della memoria di Finder OPTA

Arduino PLC IDE versione 1.1 e successive richiede che la memoria del tuo
Finder OPTA sia configurata in un modo specifico. Il modo più semplice per
farlo è utilizzare l'app **OPTA Arduino Factory Reset**, che gestisce tutto
automaticamente.

[Scarica OPTA Arduino Factory Reset](https://opta.findernet.com/software#arduino), segui i passaggi nell'app, poi torna qui per continuare.

Assicurati di utilizzare OPTA Arduino Factory Reset v2.0.0 o superiore
insieme ad Arduino PLC IDE 1.1 o superiore. In caso di dubbi, scarica sempre le
versioni più recenti dal sito di Finder.

![Factory reset](assets/10-factory-reset.png)

### Creazione del progetto e installazione del runtime

In questa sezione vedrai come creare un nuovo progetto con Finder OPTA e come
installare il runtime sul dispositivo utilizzando Arduino PLC IDE. Questo
passaggio è essenziale, poiché il runtime agisce come ponte di comunicazione
tra Finder OPTA e l’ambiente di sviluppo.

Prima di tutto **collega Finder OPTA al computer con il cavo USB-C** e apri
Arduino PLC IDE. Verrà visualizzata la schermata di benvenuto:

![New Project](assets/it/01-new-project.png)

Per iniziare, crea un nuovo progetto facendo clic sul pulsante _New project_
o su _File > New project_. Prima di procedere, assicurati di selezionare la versione di sistema più recente disponibile per il progetto: scegli _Opta 1.x_, dove _x_ è il numero più alto tra quelli elencati.

![Project Name](assets/it/02-project-name.png)

La schermata di nuovo progetto apparirà come nell'immagine seguente.

![Project Name](assets/it/03-empty-project.png)

Adesso clicca sulla tab _Risorse_ che trovi in basso a sinistra, in
alternativa clicca su _Vista > Finestre strumenti > Risorse_. Si aprirà una
struttura ad albero con una voce _Opta_, cliccala per aprire la schermata di
configurazione di Finder OPTA.

![OPTA Menu](assets/it/04-opta-menu.png)

Adesso scarichiamo il runtime sul dispositivo, per farlo scorri nella pagina di
configurazione verso il basso fino a che non compare il selettore delle porte
COM di Finder OPTA.

![COM Port](assets/it/05-com-port.png)

Adesso fai clic su _Download_ per scaricare il software su Finder OPTA.

![Runtime Download](assets/it/06-runtime-download.png)

**_Nota: scarica il runtime ogni volta che aggiorni Arduino PLC IDE a una nuova
versione. Questo passaggio garantisce la corretta comunicazione tra Finder OPTA
e Arduino PLC IDE._**

#### Risoluzione degli errori

Durante la procedura di download potresti vedere un messaggio di errore di
questo tipo:

![Runtime Download Error](assets/it/07-runtime-download-error.png)

Se compare, procedi come segue:

1. Premi due volte il pulsante di reset utilizzando la punta di una penna o un
   oggetto appuntito.
2. Verifica che il LED sopra il pulsante di reset lampeggi.
3. Clicca nuovamente sul pulsante _Download_ per ripetere l'installazione del
   runtime.

Se i problemi persistono, avvia Arduino PLC IDE come amministratore. Questo
passaggio può risolvere eventuali problemi di autorizzazione che impediscono la
comunicazione con Finder OPTA.

### Collegarsi al dispositivo

Dopo aver installato il runtime, è necessario configurare la comunicazione con
Finder OPTA. Apri il menu _On-line > Imposta comunicazione_.

![Connection Setup](assets/it/08-connection-setup.png)

Dalla finestra che appare, accedi alle proprietà del protocollo Modbus.

![Connection Setup](assets/it/09-modbus.png)

Verifica che `Port` sia la porta COM di Finder OPTA.

![Connection Setup](assets/it/10-modbus-settings.png)

Clicca su _OK_ per applicare le modifiche ed esci dalle impostazioni di
comunicazione. Adesso è il momento di collegarsi al dispositivo, per farlo
clicca su _On-line > Connect_ per stabilire la connessione tra il dispositivo
e Arduino PLC IDE.

![Connect](assets/it/11-connect.png)

Una volta stabilita la connessione, la schermata di configurazione mostra i
dettagli del dispositivo collegato, indicandone le caratteristiche e lo stato
della licenza. È possibile vedere lo stato della connessione nel riquadro in
basso.

![Connection Success](assets/it/13-connection-success.png)

### Attivare la licenza di Finder OPTA

Finder OPTA viene fornito con una licenza preconcessa che deve essere attivata.
Pertanto nella schermata di configurazione del dispositivo è presente il tasto
_Activate PLC runtime_, assicurati che Finder OPTA si collegato al PC e
clicca il pulsante per attivare la licenza.

Il messaggio di conferma avverte che Arduino PLC IDE verrà disconnesso in
seguito all'operazione, premi _OK_ e prosegui.

![License Activation](assets/it/14-license-activation.png)

Un altro avviso chiede di far ripartire il dispositivo dopo l'attivazione della
licenza, premi _OK_ e continua.

![Restart Target](assets/it/15-restart-target.png)

A questo punto fai ripartire Finder OPTA premendo una volta il tasto reset sul
dispositivo, poi ripeti la procedura di connessione vista in precedenza.

![License Activated](assets/it/16-license-activated.png)

L'attivazione del dispositivo è completa, lo **Status** di Finder OPTA é **OK**
ed è pronto per essere utilizzato con Arduino PLC IDE.

### Scaricare un programma

Per eseguire il primo programma su Finder OPTA, utilizzeremo il codice di
default incluso nel progetto. Si tratta di un semplice contatore che incrementa
il proprio valore di 1 ad ogni ciclo di esecuzione. L'obiettivo è verificare
che il dispositivo sia in grado di eseguire correttamente il codice e
aggiornare i valori in tempo reale, consentendo di monitorare l'incremento
direttamente all'interno di Arduino PLC IDE.

Apri la sezione _Progetto_ affianco a _Risorse_ oppure clicca su _Vista >
Finestre strumenti > Progetto_. In seguito seleziona la voce _main_ dalla
struttura ad albero, in questo modo verrà mostrato il codice del contatore.

![Code](assets/it/17-code.png)

Per trasferire il programma al dispositivo, compila il codice usando il
pulsante in alto a sinistra.

![Code Compilation](assets/it/18-code-compilation.png)

Adesso avvia il download sul PLC cliccando su _On-line > Trasferimento
codice_.

![Code Download](assets/it/19-code-download.png)

Comparirà un messaggio di avviso, clicca su _Sì_ e prosegui.

![Confirm Reset](assets/it/20-confirm-reset.png)

A questo punto il codice è stato scaricato correttamente sul dispositivo, che
lo sta eseguendo.

### Verificare l'esecuzione del programma

Per controllare il corretto funzionamento del programma in esecuzione su Finder
OPTA, possiamo monitorare il valore del contatore direttamente all'interno di
Arduino PLC IDE. Per farlo dobbiamo aggiungere un **Watch**, ovvero uno
strumento che accede ad una variabile e ne monitora il valore.

È necessario essere connessi a Finder OPTA, se dopo il download il tuo PC si è
disconnesso dal dispositivo, ripeti l'operazione di connessione come fatto in
precedenza. Adesso vai su _Vista > Finestra strumenti > Watch_.

![Add Watch](assets/it/21-add-watch.png)

Si aprirà una finestra sul lato destro della schermata con al suo interno un
pulsante _Inserisci nuovo elemento_, cliccalo.

![Add New Item](assets/it/22-add-new-item.png)

In questo modo stiamo indicando le variabili da monitorare, seleziona il primo
pulsante _Sfoglia_.

![Browse Variables](assets/it/23-browse-variables.png)

Dalla lista delle variabili del programma seleziona `cnt`, che rappresenta il
valore del contatore.

![Variables List](assets/it/24-variables-list.png)

Premi _OK_ per applicare le modifiche e chiudere le finestre. Se la variabile
sta venendo monitorata correttamente, vedrai una schermata come quella
seguente.

![Real Time Reading](assets/it/25-real-time-reading.png)

Se il contatore `cnt` — all'interno della cella evidenziata di rosso — viene
incrementato costantemente, significa che Finder OPTA sta eseguendo il codice e
che i dati vengono aggiornati in tempo reale all'interno di Arduino PLC
IDE.

## Conclusione

Hai completato con successo la configurazione iniziale di **Finder OPTA** con
**Arduino PLC IDE**. Ora che **Finder OPTA** è correttamente configurato e
operativo, puoi iniziare a esplorare le potenzialità offerte dai linguaggi
**IEC 61131-3**.

<!-- Per saperne di più ti consigliamo di seguire il prossimo [tutorial](...). -->
