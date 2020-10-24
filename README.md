
# CoAP server example

(See the README.md file in the upper level 'examples' directory for more information about examples.)  
This CoAP server example is very simplified adaptation of one of the
[libcoap](https://github.com/obgm/libcoap) examples.

CoAP server example will startup a daemon task, receive requests / data from CoAP client and transmit
data to CoAP client.

If the incoming request requests the use of DTLS (connecting to port 5684), then the CoAP server will
try to establish a DTLS session using the previously defined Pre-Shared Key (PSK) - which
must be the same as the one that the CoAP client is using, or Public Key Infrastructure (PKI) where
the PKI information must match as requested.

NOTE: Client sessions trying to use coaps+tcp:// are not currently supported, even though both
libcoap and MbedTLS support it.

The Constrained Application Protocol (CoAP) is a specialized web transfer protocol for use with
constrained nodes and constrained networks in the Internet of Things.   
The protocol is designed for machine-to-machine (M2M) applications such as smart energy and
building automation.

Please refer to [RFC7252](https://www.rfc-editor.org/rfc/pdfrfc/rfc7252.txt.pdf) for more details.

## How to use example

### Configure the project

```
idf.py menuconfig
```

Example Connection Configuration  --->
 * Set WiFi SSID under Example Configuration
 * Set WiFi Password under Example Configuration
Example CoAP Client Configuration  --->
 * If PSK, Set CoAP Preshared Key to use in connection to the server
Component config  --->
  CoAP Configuration  --->
    * Set encryption method definition, PSK (default) or PKI
    * Enable CoAP debugging if required

### Build and Flash

Build the project and flash it to the board, then run monitor tool to view serial output:

```
idf.py build
idf.py -p PORT flash monitor
```

(To exit the serial monitor, type ``Ctrl-]``.)

See the Getting Started Guide for full steps to configure and use ESP-IDF to build projects.

## Example Output
current CoAP server would startup a daemon task,   
and the log is such as the following:  

```
...
I (332) wifi: mode : sta (30:ae:a4:04:1b:7c)
I (1672) wifi: n:11 0, o:1 0, ap:255 255, sta:11 0, prof:1
I (1672) wifi: state: init -> auth (b0)
I (1682) wifi: state: auth -> assoc (0)
I (1692) wifi: state: assoc -> run (10)
I (1692) wifi: connected with huawei_cw, channel 11
I (1692) wifi: pm start, type: 1

I (2622) event: sta ip: 192.168.3.84, mask: 255.255.255.0, gw: 192.168.3.1
I (2622) CoAP_server: Connected to AP
...
```

If a CoAP client queries the `/Espressif` resource, CoAP server will return `"Hello World!"`  
until a CoAP client does a PUT with different data.

## libcoap Documentation
This can be found at https://libcoap.net/doc/reference/4.2.0/

## Troubleshooting
* Please make sure CoAP client fetchs or puts data under path: `/Espressif` or
fetches `/.well-known/core`

* CoAP logging can be enabled by running 'idf.py menuconfig -> Component config -> CoAP Configuration' and setting appropriate log level

## Common Error
If IDF monitor fails shortly after the upload, or you see **random garbage**, your board is likely using a 26 MHz crystal. Most development board designs use 40 MHz, so ESP-IDF uses this frequency as a default value.

If you have such a problem, do the following:

1. Exit the monitor.
2. Go back to **menuconfig**.
3. Go to Component config –> ESP32-specific –> Main XTAL frequency, then change **CONFIG_ESP32_XTAL_FREQ_SEL** to 26 MHz.
4. After that, **build and flash** the application again.

#### Reference
[ESP-IDF - Flash onto the Device](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/get-started/#step-9-flash-onto-the-device)