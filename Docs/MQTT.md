# MQTT

[https://www.hivemq.com/mqtt-essentials/](https://www.hivemq.com/mqtt-essentials/)

On Linux with package `mosquitto`:
- `mosquitto_sub` ([https://mosquitto.org/man/mosquitto_sub-1.html](https://mosquitto.org/man/mosquitto_sub-1.html))
  - `-v` or `--verbose` prints topic and payload instead of only payload 
- `mosquitto_pub` ([https://mosquitto.org/man/mosquitto_pub-1.html](https://mosquitto.org/man/mosquitto_pub-1.html))

Port 1883: Unsecure TCP or TLS/SSL (paho.mqtt ssl://)
Port 443: Websockets WS (paho.mqtt wss://)