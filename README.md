# HTTP to MQTT bridge

This bridge is a modified version from the petkov one, used to leverage Shelly's Action URL configuration to post a MQTT message to the MQTT Broker.
Using Action URL in this way permits you to enable more complex scenes for you home automation environment. I use it to send an MQTT message when the button of a Shelly is pressed once or for long time.

## Usage

The app could be hosted on any Node JS hosting like Heroku. See original project from petkov for more info: https://github.com/petkov/http_to_mqtt 

### Use Your Own Server
You can run this on basically anything that runs NodeJS.

1.  Clone the repository
2.  Install MQTT node:
    npm install mqtt --save
3.  Install Express node:
    npm install express --save
4.  Install body-parser node:
    npm install body-parser --save
5.  Install multer node:
    npm install multer --save
6.  Run index.js

### Configuration

Modify index.js with the following Config Variables:
   * MQTT_HOST - the host of your MQTT broker (eg.: mqtts://k99.cloudmqtt.com:21234).
   * MQTT_USER
   * MQTT_PASS
   * AUTH_KEY - can be any string eg.: 912ec803b2ce49e4a541068d495ab570.
   
If you configure the AUTH_KEY, you need to provide it in each POST or GET call.
If you want to use with Shelly Devices, don't use it.

##### Home Assistant Setup
You can configure HA to ping HTTP to MQTT bridge every 10 minutes during daytime. Below is an example of how to do that:

```yaml
rest_command:
  http_to_mqtt_keep_alive:
    url: https://<your_app_address>/keep_alive/
    method: get

automation:
  alias: HTTP to MQTT keep alive
  trigger:
    platform: time
    minutes: '/10'
    seconds: 00
  condition:
    condition: time
    after: '7:30:00'
    before: '23:59:59'
  action:
    service: rest_command.http_to_mqtt_keep_alive
```

#### Examples

##### Publish to a topic from Shelly Devices
Use Action URL like the following:
http://<your_app_address>:5000/post?topic=TOPIC/SUBTOPIC&path=MESSAGE

##### Other Use Cases
For other use cases, see original project from petkov: https://github.com/petkov/http_to_mqtt
