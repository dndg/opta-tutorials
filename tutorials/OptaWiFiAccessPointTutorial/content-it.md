# Utilizzare Finder OPTA come Access Point WiFi per servire una pagina Web locale

Un tutorial che mostra come utilizzare Finder OPTA per creare una rete WiFi ad-hoc e servire una pagina web ai dispositivi connessi.

## Panoramica

In questo tutorial impareremo a configurare Finder OPTA per creare una rete
WiFi ad-hoc, trasformandolo in un Access Point in grado di servire una pagina
web locale. I dispositivi che si connetteranno a Finder OPTA potranno accedere
direttamente ai contenuti offerti, senza la necessità di una connessione a
Internet.

La procedura inizia con la configurazione della rete WiFi, definendo SSID e
password. Assegneremo quindi un indirizzo IP statico al Finder OPTA e
imposteremo un server web locale, in grado di gestire le richieste HTTP
provenienti dai client connessi, restituendo una pagina HTML.

Questa soluzione è particolarmente utile in scenari in cui non è disponibile
l’accesso a Internet, ma è comunque necessario leggere informazioni da Finder
OPTA o controllarlo tramite una semplice interfaccia web accessibile da uno
smartphone o un computer connessi alla rete wireless.

Per seguire il tutorial è necessario disporre di un Finder OPTA Advanced,
l’unico modello della gamma dotato di modulo WiFi integrato.

## Di cosa avrai bisogno

- [PLC Finder OPTA Advanced]((https://opta.findernet.com/it/arduino)) (x1)
- Cavo USB-C (x1)
- [Codice di esempio](assets/OptaWiFiAP.zip).
- [Arduino IDE 2.0+](https://opta.findernet.com/it/arduino#download-software).

### Configurazione di Arduino IDE

Per seguire correttamente questo tutorial è necessario installare [l'ultima
versione di Arduino
IDE](https://opta.findernet.com/it/arduino#download-software). Se stai
configurando Finder OPTA per la prima volta, ti consigliamo di consultare il
tutorial [Getting Started with
OPTA](https://opta.findernet.com/it/tutorial/getting-started), dove spieghiamo
nel dettaglio come installare il *Board Manager* per la piattaforma Mbed OS
OPTA.

Il *Board Manager* fornisce tutti gli strumenti essenziali per compilare e
caricare sketch su Finder OPTA tramite Arduino IDE, rendendo possibile lo
sviluppo e la distribuzione di applicazioni su questa piattaforma.

## Istruzioni

Finder OPTA Advanced è dotato di un modulo WiFi integrato che permette sia di
connettersi a reti WiFi esistenti, sia di creare una rete ad-hoc. In questo
tutorial utilizzeremo la modalità Access Point per configurare una rete WiFi
locale, alla quale sarà possibile collegarsi con dispositivi esterni per
accedere a contenuti web ospitati direttamente su Finder OPTA.

Per realizzare questa configurazione utilizzeremo la libreria `WiFi`, che
fornisce le funzioni necessarie ad impostare Finder OPTA come Access Point e ad
avviare un server web locale.

### Panoramica del codice

Il codice completo dell'esempio è disponibile [qui](assets/OptaWiFiAP.zip).
Estrai il contenuto del file `.zip` e copialo nella cartella
`~/Documents/Arduino`, oppure - se preferisci - crea un nuovo sketch chiamato
`OptaWiFiAP` utilizzando Arduino IDE e incolla il codice presente nel tutorial.

Come prima cosa, creiamo un file di configurazione che conterrà alcune costanti
da utilizzare nello sketch principale. Crea un file chiamato `secrets.h` nella
stessa cartella dello sketch e inserisci al suo interno le seguenti righe per
definire SSID e password della rete WiFi ad-hoc:

```cpp
#define NETWORK_SSID "Opta"
#define NETWORK_PASSWORD "psw12345"
```

Passiamo ora a scrivere lo sketch `OptaWiFiAP`, che come tutti gli sketch per
Arduino sarà composto da una funzione di `setup()` e una funzione `loop()`:

```cpp
void setup()
{
  // Codice di setup, eseguito all'avvio
}

void loop()
{
  // Codice di loop, eseguito all'infinito
}
```

All'inizio del nostro sketch importiamo le librerie e i file necessari al
funzionamento del programma:

```cpp
#include <Arduino.h>
#include <WiFi.h>
#include "secrets.h"

void setup()
{
  // Codice di setup, eseguito all'avvio
}

void loop()
{
  // Codice di loop, eseguito all'infinito
}
```

In particolare abbiamo importato le librerie:

- `Arduino`: contiene numerose funzionalità di base per le schede Arduino, ed è
  quindi buona norma importarla all'inizio di tutti gli sketch.
- `WiFi.h`: necessaria a creare l'Access Point WiFi e il server web.

Inoltre abbiamo importato il file `config.h`, che contiene la configurazione
della rete WiFi ad-hoc. Procediamo dichiarando il server sulla porta `80`:

```cpp
#include <Arduino.h>
#include <WiFi.h>
#include "secrets.h"

WiFiServer server(80);

void setup()
{
  // Codice di setup, eseguito all'avvio
}

void loop()
{
  // Codice di loop, eseguito all'infinito
}
```

A questo punto siamo pronti a scrivere la funzione `setup()`, eseguita una sola
volta all'avvio di Finder OPTA. Nel nostro caso all'avvio del programma è
necessario effettuare le seguenti operazioni:

- Configurare un indirizzo IP statico per Finder OPTA.
- Creare un Access Point WiFi utilizzando SSID e password definiti in
  `secrets.h`.
- Avviare il server web.

```cpp
#include <Arduino.h>
#include <WiFi.h>
#include "secrets.h"

WiFiServer server(80);

void setup()
{
    // Configurazione IP statico.
    IPAddress serverIp = IPAddress(192, 168, 2, 1);
    WiFi.config(serverIp);

    // Creazione dell'Access Point.
    uint8_t isListening = WiFi.beginAP(NETWORK_SSID, NETWORK_PASSWORD) == WL_AP_LISTENING;
    if (!isListening)
    {
        digitalWrite(LEDR, HIGH);
        while (1)
            ;
    }
    digitalWrite(LEDB, HIGH);

    // Avvio del server web.
    server.begin();
}

void loop()
{
  // Codice di loop, eseguito all'infinito
}
```

Se non è possibile creare l'Access Point, Finder OPTA accende il LED rosso e
ferma l'esecuzione del programma. Questo comportamento serve a segnalare
all'utente se non si sta utilizzando un Finder OPTA Advanced. Se invece
l'Access Point viene creato correttamente, si accende il LED blu.

Nella funzione `loop()` dovremo verificare se ci sono dispositivi connessi alla
rete WiFi ad-hoc. Per fare ciò, utilizzeremo la funzione `status()`:

```cpp
#include <Arduino.h>
#include <WiFi.h>
#include "secrets.h"

WiFiServer server(80);

void setup()
{
    // Configurazione IP statico.
    IPAddress serverIp = IPAddress(192, 168, 2, 1);
    WiFi.config(serverIp);

    // Creazione dell'Access Point.
    uint8_t isListening = WiFi.beginAP(NETWORK_SSID, NETWORK_PASSWORD) == WL_AP_LISTENING;
    if (!isListening)
    {
        digitalWrite(LEDR, HIGH);
        while (1)
            ;
    }
    digitalWrite(LEDB, HIGH);

    // Avvio del server web.
    server.begin();
}

void loop()
{
    bool isConnected = WiFi.status() == WL_AP_CONNECTED;
    if (isConnected)
    {
        // Codice che verifica se ci sono richieste per il server e serve pagina web.
    }
}
```

Infine, scriviamo il codice che verifica la presenza di richieste HTTP e serve
una pagina web ai dispositivi connessi:

```cpp
#include <Arduino.h>
#include <WiFi.h>
#include "secrets.h"

WiFiServer server(80);

void setup()
{
    // Configurazione IP statico.
    IPAddress serverIp = IPAddress(192, 168, 2, 1);
    WiFi.config(serverIp);

    // Creazione dell'Access Point.
    uint8_t isListening = WiFi.beginAP(NETWORK_SSID, NETWORK_PASSWORD) == WL_AP_LISTENING;
    if (!isListening)
    {
        digitalWrite(LEDR, HIGH);
        while (1)
            ;
    }
    digitalWrite(LEDB, HIGH);

    // Avvio del server web.
    server.begin();
}

void loop()
{
    bool isConnected = WiFi.status() == WL_AP_CONNECTED;
    if (isConnected)
    {
        WiFiClient client = server.available();
        if (client)
        {
            if (client.connected())
            {
                client.println("HTTP/1.1 200 OK");
                client.println("Content-type:text/html");
                client.println();
                client.print("<html><head></head><body><h1>");
                client.print("You are connected to the Opta");
                client.print("</h1><p>");
                client.print("Your IP address is ");
                client.print(client.remoteIP());
                client.print("</p></body></html>");
                client.println();
            }
            client.stop();
        }
    }
}
```

La risposta inviata al client avrà codice di stato HTTP `200` e sarà di tipo
`text/html`. Il corpo della risposta conterrà una pagina HTML che mostra un
messaggio di benvenuto, seguito dall'indirizzo IP del dispositivo connesso.

A questo punto, carichiamo lo sketch su Finder OPTA e colleghiamoci alla rete
WiFi ad-hoc creata, inserendo la password specifica in `secrets.h`. Apriamo un
browser e inseriamo l'indirizzo IP di Finder OPTA nella barra degli indirizzi
(in questo caso `192.168.2.1`). La pagine mostrata sarà la seguente:

<img src="assets/web-page.png" width=800 alt="web page">

## Conclusioni

In questo tutorial abbiamo visto come trasformare Finder OPTA in un Access
Point WiFi, capace di servire contenuti web localmente senza una connessione a
Internet.

In particolare abbiamo:

- Configurato una rete WiFi ad-hoc con SSID e password.
- Assegnato un indirizzo IP statico al dispositivo.
- Avviato un server HTTP in grado di gestire richieste e restituire una pagina
  HTML.

Questa configurazione potrebbe essere utilizzata - ad esempio - per
visualizzare una pagina di diagnostica che mostra lo stato dei dispositivi
connessi a Finder OPTA. Anche senza accesso a Internet, un operatore potrebbe
connettersi alla rete ad-hoc di Finder OPTA e accedere alla pagina di
diagnostica dal proprio smartphone o computer.
