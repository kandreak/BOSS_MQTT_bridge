# BOSS_MQTT_bridge
mqtt bridge for BOSS dbms.

HOW TO TEST:
Test this using mosquitto_clients (https://mosquitto.org/man/mosquitto_pub-1.html) and the mosquitto test server (test.mosquitto.org).

EXAMPLE COMMANDS:
mosquitto_pub -t 'mosquittoTest/testTopic1' -m 'hello world' -h 'test.mosquitto.org'
mosquitto_pub -t 'mosquittoTest/testTopic2' -m 'hello world' -h 'test.mosquitto.org'

