# Installare il plug-in di Finder OPTA in CODESYS

## Panoramica

CODESYS è uno dei principali ambienti di sviluppo per PLC e consente di programmare Finder OPTA con linguaggi standard, come LD e ST.

Per programmare Finder OPTA utilizzando CODESYS, è necessario installare il plug-in ufficiale. Questo tutorial guida
all'installazione del plug-in in pochi semplici passi.

## Obiettivi

- Scaricare ed installare il plug-in OPTA Configurator.
- Verificare il corretto funzionamento di OPTA CODESYS all'interno dell'ambiente di sviluppo CODESYS.

## Requisiti

Prima di iniziare, assicurati di avere:

- [PLC Finder OPTA CODESYS](https://opta.findernet.com/it/codesys) (x1)
- Cavo USB-C (x1)
- Ambiente di sviluppo CODESYS installato. Lo puoi scaricare [a questo
  link](https://opta.findernet.com/it/codesys#download-software).
- Plug-in OPTA Configurator. Lo puoi scaricare [a questo link](https://opta.findernet.com/it/codesys#download-software).

## Istruzioni

### Installazione del plug-in

Dopo aver scaricato il plug-in OPTA Configurator, estrai il contenuto dell’archivio compresso. Il file estratto si chiamerà
`OPTA_Configurator`.

![OPTA Configurator](assets/file.png)

Apri CODESYS e dal menu _Tools_, seleziona _CODESYS Installer..._:

![Apri CODESYS Installer](assets/launch_installer.png)

Lascia CODESYS Installer aperto e chiudi CODESYS:

![Chiudi CODESYS](assets/exit_codesys.png)

Ora, nel CODESYS Installer, fai clic sul pulsante _Install File(s)_:

![Installa file](assets/install_file.png)

Seleziona il file `OPTA_Configurator` che hai estratto in precedenza:

![Seleziona file](assets/package.png)

Dopo aver selezionato il file, apparirà una schermata di conferma. Fai clic su _OK_, autorizza CODESYS ad apportare modifiche al
sistema:

![Conferma installazione](assets/package_confirm.png)

Attendi il completamento dell'installazione:

![Progresso installazione](assets/installation_progress.png)

Al termine dell'installazione, verrà visualizzato un messaggio di successo:

![Successo installazione](assets/installation_success.png)

Per verificare che il plug-in sia stato installato correttamente, controlla l’elenco dei componenti installati: tra questi dovresti
trovare OPTA Configurator.

![Lista dei plugin](assets/plugin.png)

### Configurazione del Gateway CODESYS

Ora chiudi CODESYS Installer:

![Chiudi installer](assets/exit_installer.png)

Riapri CODESYS. A questo punto, collega il PLC Finder OPTA al computer utilizzando il cavo USB-C e crea un nuovo progetto standard:

![Progetto standard](assets/project.png)

Quando viene richiesto di selezionare un dispositivo, scegli _Finder Opta (FINDER SPA)_. Per il linguaggio di programmazione, puoi
scegliere tra le opzioni disponibili, come ad esempio ST:

![Seleziona dispositivo](assets/device.png)

Fai doppio clic su _Device (Finder Opta)_:

![Fai clic sul dispositivo](assets/device_click.png)

Espandi le icone nascoste della taskbar di Windows e fai clic con il tasto destro sull'icona del servizio _CODESYS Gateway_:

![Icona gateway](assets/gateway/gateway_icon.png)

Fai clic sul pulsante _Start Gateway_:

![Avvia gateway](assets/gateway/gateway_start.png)

Avviato il servizio del Gateway, non resta che configurarlo. Come prima cosa aggiungi un nuovo Gateway:

![Nuovo gateway](assets/gateway/gateway_new.png)

Assegna un nome al Gateway e selezione _TCP/IP_ come driver, poi conferma facendo clic su _OK_:

![Conferma nuovo gateway](assets/gateway/gateway_new_confirm.png)

Ora nella schermata dovresti vedere il Gateway con il nome ad esso assegnato:

![Aggiunta gateway](assets/gateway/gateway_added.png)

Fai clic sul pulsante _Gateway_, poi su _Configure the Local Gateway..._:

![Configura gateway](assets/gateway/gateway_configure.png)

Apparirà una schermata con una lista di interfacce configurate. Se vedi altre interfacce configurate oltre a quella _UDP interface_,
rimuovile. La tua schermata di configurazione sarà identica a questa:

![Interfaccia UDP del gateway](assets/gateway/gateway_interfaces.png)

Connetti OPTA CODESYS al computer con il cavo USB, poi lancia il _Device Manager_ di Windows:

![Device manager](assets/gateway/device_manager.png)

Verifica la porta COM a cui è connesso OPTA CODESYS, in particolare prendi nota del numero di porta. Nell'esempio, la porta COM è la
numero 7:

![Device port](assets/gateway/device_port.png)

Torna al configuratore del Gateway, premi il pulsante _Add_ e in seguito _Add Interface..._:

![Aggiungi interfaccia](assets/gateway/add_interface.png)

Vedrai apparire una porta COM. Fai clic sul suo riquadro _Settings_:

![Porta COM](assets/gateway/port_com.png)

Imposta il parametro _Port_ con il numer di porta letto nel Device Manager di Windows. Nell'esempio impostiamo il valore a 7:

![Numero porta COM](assets/gateway/port_number.png)

Gli altri parametri della porta non vanno cambiati. Fai clic sul pulsante _OK_ e procedi:

![Gateway configurato](assets/gateway/gateway_configured.png)

Infine, è necessario far ripartire il servizio del Gateway di CODESYS. Sempre dalla taskbar di Windows, ferma il servizio:

![Ferma servizio](assets/gateway/stop_service.png)

In seguito, fallo ripartire:

![Riavvia servizio](assets/gateway/start_service.png)

### Verifica della configurazione

Dalla schermata _Device_, fai clic sul pulsante _Scan Network_ per avviare la scansione dei dispositivi connessi:

![Scan network](assets/scan.png)

Se viene mostrato un errore di comunicazione con il Gateway, chiudi CODESYS e riaprilo, per permettere all'IDE di riconoscere il
servizio del Gateway precedentemente riavviato.

Se invece la configurazione è avvenuta correttamente, Finder Opta verrà rilevato e saranno visibili le informazioni relative al
dispositivo, come il nome, il numero di serie e il vendor. Per proseguire premi il pulsante _OK_.

![Dispositivo riconosciuto](assets/detection.png)

Apparirà una schermata che mostra la configurazione della connessione: il computer sarà collegato al Gateway, che a sua volta
comunicherà con Finder Opta. A questo punto, Finder Opta è pronto per essere programmato utilizzando CODESYS.

![Connessione](assets/connection.png)

## Conclusioni

Seguendo questi passaggi, hai installato con successo il plug-in Finder OPTA Configurator in CODESYS e verificato che il dispositivo
venga riconosciuto correttamente. Questo permette di sfruttare appieno OPTA CODESYS all'interno dell'ambiente di sviluppo CODESYS,
utilizzando i linguaggi standard per PLC, come LD e ST.

Se riscontri problemi durante l'installazione o la configurazione, verifica di aver seguito correttamente tutti i passaggi.

<!-- Inserire informazioni di contatto per supporto -->
