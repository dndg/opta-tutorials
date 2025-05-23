# Using Finder Opta as a WiFi Access Point to serve a local Web page

A tutorial demonstrating how to use Finder OPTA to create an ad-hoc WiFi network and serve a web page to connected devices.

## Overview

In this tutorial, we will learn how to configure Finder OPTA to create an
ad-hoc WiFi network, turning it into an Access Point capable of serving a local
web page. Devices that connect to Finder OPTA will be able to access the
provided content directly, without the need for an Internet connection.

The process begins by setting up the WiFi network, defining the SSID and
password. We will then assign a static IP address to Finder OPTA and set up a
local web server capable of handling HTTP requests from connected clients and
responding with an HTML page.

This solution is particularly useful in scenarios where Internet access is not
available, but there is still a need to read data from the Finder OPTA or
control it via a simple web interface accessible from a smartphone or computer
connected to the wireless network.

To follow this tutorial, you will need a Finder OPTA Advanced, the only model
in the lineup equipped with an integrated WiFi module.

## What you will need

- [PLC Finder OPTA Advanced]((https://opta.findernet.com/en/arduino)) (x1)
- USB-C cable (x1)
- [Example code](assets/OptaWiFiAP.zip).
- [Arduino IDE 2.0+](https://opta.findernet.com/en/arduino#download-software).

### Arduino IDE setup

To follow this tutorial properly, you need to install [the latest version of
the Arduino IDE](https://opta.findernet.com/en/arduino#download-software). If
you're setting up the Finder OPTA for the first time, we recommend checking out
the [Getting Started with
OPTA](https://opta.findernet.com/en/tutorial/getting-started) tutorial, where
we explain in detail how to install the *Board Manager* for the Mbed OS OPTA
platform.

The *Board Manager* provides all the essential tools to compile and upload
sketches to the Finder OPTA using the Arduino IDE, making it possible to
develop and deploy applications on this platform.

## Instructions

Finder OPTA Advanced is equipped with an integrated WiFi module that allows it
to either connect to existing WiFi networks or create its own ad-hoc network.
In this tutorial, we will use Access Point mode to configure a local WiFi
network, which external devices can connect to in order to access web content
hosted directly on the Finder OPTA.

To set up this configuration, we will use the `WiFi` library, which provides
the necessary functions to configure the Finder OPTA as an Access Point and to
launch a local web server.

### Code Overview

The complete example code is available [here](assets/OptaWiFiAP.zip).  
Extract the contents of the `.zip` file and copy them into the
`~/Documents/Arduino` folder, or - if you prefer - create a new sketch named
`OptaWiFiAP` using the Arduino IDE and paste the code provided in this
tutorial.

The first step is to create a configuration file that will contain some
constants to be used in the main sketch. Create a file named `secrets.h` in the
same folder as the sketch and insert the following lines to define the SSID and
password for the ad-hoc WiFi network:

```cpp
#define NETWORK_SSID "Opta"
#define NETWORK_PASSWORD "psw12345"
```

Now let's write the `OptaWiFiAP` sketch, which - like all Arduino sketches -
will consist of a `setup()` function and a `loop()` function:

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
  // Setup code, executed at startup
}

void loop()
{
  // Loop code, executed continuously
}
```

Specifically, we have imported the following libraries:

- `Arduino`: includes many basic functionalities for Arduino boards and is
  generally recommended to be included at the beginning of every sketch.
- `WiFi.h`: required to create the WiFi Access Point and the web server.

We have also imported the `config.h` file, which contains the configuration for
the ad-hoc WiFi network. Next, we declare the server on port `80`:

```cpp
#include <Arduino.h>
#include <WiFi.h>
#include "secrets.h"

WiFiServer server(80);

void setup()
{
  // Setup code, executed at startup
}

void loop()
{
  // Loop code, executed continuously
}
```

At this point, we are ready to write the `setup()` function, which is executed
once when Finder OPTA starts up. In our case, the program needs to perform the
following operations at startup:

- Configure a static IP address for the Finder OPTA.
- Create a WiFi Access Point using the SSID and password defined in
  `secrets.h`.
- Start the web server.

```cpp
#include <Arduino.h>
#include <WiFi.h>
#include "secrets.h"

WiFiServer server(80);

void setup()
{
    // Configure static IP Address.
    IPAddress serverIp = IPAddress(192, 168, 2, 1);
    WiFi.config(serverIp);

    // Create Access Point.
    uint8_t isListening = WiFi.beginAP(NETWORK_SSID, NETWORK_PASSWORD) == WL_AP_LISTENING;
    if (!isListening)
    {
        digitalWrite(LEDR, HIGH);
        while (1)
            ;
    }
    digitalWrite(LEDB, HIGH);

    // Start Web server.
    server.begin();
}

void loop()
{
  // Loop code, executed continuously
}
```

If the Access Point cannot be created, Finder OPTA turns on the red LED and
halts program execution. This behavior is intended to alert the user that a
Finder OPTA Advanced may not be in use. If the Access Point is successfully
created, the blue LED is turned on.

In the `loop()` function, we need to check if there are devices connected to
the ad-hoc WiFi network. To do this, we will use the `status()` function:

```cpp
#include <Arduino.h>
#include <WiFi.h>
#include "secrets.h"

WiFiServer server(80);

void setup()
{
    // Configure static IP Address.
    IPAddress serverIp = IPAddress(192, 168, 2, 1);
    WiFi.config(serverIp);

    // Create Access Point.
    uint8_t isListening = WiFi.beginAP(NETWORK_SSID, NETWORK_PASSWORD) == WL_AP_LISTENING;
    if (!isListening)
    {
        digitalWrite(LEDR, HIGH);
        while (1)
            ;
    }
    digitalWrite(LEDB, HIGH);

    // Start Web server.
    server.begin();
}

void loop()
{
    bool isConnected = WiFi.status() == WL_AP_CONNECTED;
    if (isConnected)
    {
        // Code that checks for server requests and serves the web page.
    }
}
```

Finally, let's write the code that checks for incoming HTTP requests and serves
a web page to the connected devices:

```cpp
#include <Arduino.h>
#include <WiFi.h>
#include "secrets.h"

WiFiServer server(80);

void setup()
{
    // Configure static IP Address.
    IPAddress serverIp = IPAddress(192, 168, 2, 1);
    WiFi.config(serverIp);

    // Create Access Point.
    uint8_t isListening = WiFi.beginAP(NETWORK_SSID, NETWORK_PASSWORD) == WL_AP_LISTENING;
    if (!isListening)
    {
        digitalWrite(LEDR, HIGH);
        while (1)
            ;
    }
    digitalWrite(LEDB, HIGH);

    // Start Web server.
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

The response sent to the client will have an HTTP status code `200` and a
content type of `text/html`. The body of the response will contain an HTML page
displaying a welcome message followed by the IP address of the connected
device.

At this point, upload the sketch to the Finder OPTA and connect to the created
ad-hoc WiFi network using the password specified in `secrets.h`. Open a browser
and enter the IP address of the Finder OPTA in the address bar (in this case,
`192.168.2.1`). The page displayed will look like this:

<img src="assets/web-page.png" width=800 alt="web page">

## Conclusion

In this tutorial, we saw how to transform Finder OPTA into a WiFi Access Point,
capable of serving web content locally without an Internet connection.

Specifically, we:

- Configured an ad-hoc WiFi network with a custom SSID and password.
- Assigned a static IP address to the device.
- Started an HTTP server capable of handling requests and returning an HTML
  page.

This configuration can be used - for example - to display a diagnostics page
showing the status of devices connected to the Finder OPTA. Even without
Internet access, an operator can connect to the Finder OPTA ad-hoc network and
access the diagnostics page from a smartphone or computer.
