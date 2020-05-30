# WARNING: Beta code! (But we're getting there :-)

## Table of Contents
- [Introduction](#introduction)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Nodes in this package](#nodes-in-this-package)
  - [General Information](#general-information)
  - [Light On/Off](#--light-onoff-a-light-that-can-be-swithed-on-and-off-only)
  - [Dimmable Light](#--dimmable-light)
  - [Color (HSV) Light](#--color-hsv-light)
  - [Color (RGB) Light](#--color-rgb-light)
  - [Outlet](#--outlet)
  - [Thermostat](#--thermostat)
  - [Window](#--window)
  - [Scene](#--scene)
  - [Vacuum](#--vacuum)
  - [Fan](#--fan)
  - [Management](#--management)
- [The config node](#the-config-node)
- [Google SmartHome Setup Instructions](#google-smarthome-setup-instructions)
- [Troubleshooting](#troubleshooting)
- [Credits](#credits)
- [Copyright and license](#copyright-and-license)

---
## Introduction

A collection of Node-RED nodes to control your smart home devices via Google Assistant.

---
## Prerequisites

1. You are going to need a 'real' SSL certificate e.g. from [Let’s Encrypt](https://letsencrypt.org/). The public key and the private key must copied to your Node-RED server, in a location where the Node-RED service can read them.
2. You also need to be able to forward TCP traffic coming in from the Internet to your Node-RED server on a port you
specify. This is not your full Node-RED server but a service started by `node-red-contrib-google-smarhome`, providing
only the functions needed by Google.
3. This package requires NodeJS version 7.6.0 at a minimum. If, during start of Node-RED, you get this warning, your version on NodeJS is too old:
`[warn] [node-red-contrib-google-smarthome/google-smarthome] SyntaxError: Unexpected token ( (line:30)`

---
## Installation
To install - change to your Node-RED user directory.

        cd ~/.node-red
        npm install node-red-contrib-google-smarthome

*Note:* This version can output a lot of debug messages on the console. These messages are optional.

*Note:* If you run Node-RED in Docker, set the folders `/data` and `/usr/src/node-red/.node-persist` as volumes.
Both folders contain data that needs to be persisted.

---
## Nodes in this package
### General information
1. If `online` is set to `false` for a node, Google SmartHome is not going to be able to control the node. It will also show as `offline` in the Google Home app.
2. The nodes will do their best to convert incoming payload data to the required type. You can send a string of e.g. `ON` and it will be converted to `true`.
3. Topics must be either as stated below or prepended with one or more `/`. E.g. `my/topic/on`. The nodes only looks for the part after the last `/`, if any.

#### - Light On/Off (a light that can be swithed on and off only)
`topic` can be `on`, `online` or something else.

If `topic` is `on` then `payload` must be boolean and tells the state of the light.

        msg.topic = 'on'
        msg.payload = true

If `topic` is `online` then `payload` must be boolean and tells the online state of the light.

        msg.topic = 'online'
        msg.payload = true

If `topic` is something else then `payload` must be an object and tells all the states of the light.

        msg.topic = 'set'
        msg.payload = {
          on: false,
          online: true
        }

#### - Dimmable Light
`topic` can be `on`, `online`, `brightness` or something else.

If `topic` is `on` then `payload` must be boolean and tells the state of the light.

        msg.topic = 'on'
        msg.payload = true

If `topic` is `online` then `payload` must be boolean and tells the online state of the light.

        msg.topic = 'online'
        msg.payload = true

If `topic` is `brightness` then `payload` must be a number and tells the brightness of the light. Range is 0 through 100.

        msg.topic = 'brightness'
        msg.payload = 75

If `topic` is something else then `payload` must be an object and tells all the states of the light.

        msg.topic = 'set'
        msg.payload = {
          on: false,
          online: true,
          brightness: 100
        }

#### - Color (HSV) Light
`topic` can be `on`, `online`, `brightness`, `hue`, `saturation`, `value` or something else.

If `topic` is `on` then `payload` must be boolean and tells the state of the light.

        msg.topic = 'on'
        msg.payload = true

If `topic` is `online` then `payload` must be boolean and tells the online state of the light.

        msg.topic = 'online'
        msg.payload = true

If `topic` is `brightness` then `payload` must be a number and tells the brightness of the light. Range is 0 through 100.

        msg.topic = 'brightness'
        msg.payload = 75

If `topic` is `hue` then `payload` must be a number and tells the hue of the light. Range is 0.0 through 360.0.

        msg.topic = 'hue'
        msg.payload = 120.0

If `topic` is `saturation` then `payload` must be a number and tells the saturation of the light. Range is 0.0 through 100.0.

        msg.topic = 'saturation'
        msg.payload = 100.0

If `topic` is something else then `payload` must be an object and tells all the states of the light.

        msg.topic = 'set'
        msg.payload = {
          on: false,
          online: true,
          brightness: 100,
          hue: 120.0,
          saturation: 100.0
        }

#### - Color (RGB) Light
`topic` can be `on`, `online`, `brightness`, `rgb` or something else.

If `topic` is `on` then `payload` must be boolean and tells the state of the light.

        msg.topic = 'on'
        msg.payload = true

If `topic` is `online` then `payload` must be boolean and tells the online state of the light.

        msg.topic = 'online'
        msg.payload = true

If `topic` is `brightness` then `payload` must be a number and tells the brightness of the light. Range is 0 through 100.

        msg.topic = 'brightness'
        msg.payload = 75

If `topic` is `rgb` then `payload` must be a number and tells the RGB values of the light. Range is 0 through 16777215.

        msg.topic = 'rgb'
        msg.payload = 255

*Hint:* red = 16711680, green = 65280, blue = 255.

If `topic` is something else then `payload` must be an object and tells all the states of the light.

        msg.topic = 'set'
        msg.payload = {
          on: false,
          online: true,
          brightness: 100,
          rgb: 255,
        }

#### - Outlet
`topic` can be `on`, `online` or something else.

If `topic` is `on` then `payload` must be boolean and tells the state of the outlet.

        msg.topic = 'on'
        msg.payload = true

If `topic` is `online` then `payload` must be boolean and tells the online state of the outlet.

        msg.topic = 'online'
        msg.payload = true

If `topic` is something else then `payload` must be an object and tells both the on state as well as the online state of the outlet.

        msg.topic = 'set'
        msg.payload = {
          on: false,
          online: true
        }

Example flow:

        [{"id":"4e9d5fe5.93114","type":"mqtt in","z":"33e99952.700716","name":"","topic":"home/outlet/power","qos":"2","datatype":"auto","broker":"9e796fe5.2afd1","x":190,"y":440,"wires":[["ef7309b7.696158"]]},{"id":"237087e5.8af608","type":"mqtt out","z":"33e99952.700716","name":"","topic":"home/outlet/set-power","qos":"","retain":"","broker":"9e796fe5.2afd1","x":1000,"y":440,"wires":[]},{"id":"acbea582.f9d848","type":"google-outlet","z":"33e99952.700716","client":"9046c434.79ce38","name":"Example Outlet","topic":"example","passthru":false,"x":600,"y":440,"wires":[["8ae8e10.ab2b12"]]},{"id":"ef7309b7.696158","type":"change","z":"33e99952.700716","name":"topic = on","rules":[{"t":"set","p":"topic","pt":"msg","to":"on","tot":"str"}],"action":"","property":"","from":"","to":"","reg":false,"x":400,"y":440,"wires":[["acbea582.f9d848"]]},{"id":"8ae8e10.ab2b12","type":"function","z":"33e99952.700716","name":"Split","func":"return [\n    { payload: msg.payload.on },\n];","outputs":1,"noerr":0,"x":790,"y":440,"wires":[["237087e5.8af608"]],"outputLabels":["on"]},{"id":"9e796fe5.2afd1","type":"mqtt-broker","z":"","name":"Example","broker":"example","port":"1883","clientid":"","usetls":false,"compatmode":false,"keepalive":"60","cleansession":true,"birthTopic":"","birthQos":"0","birthPayload":"","closeTopic":"","closeQos":"0","closePayload":"","willTopic":"","willQos":"0","willPayload":""},{"id":"9046c434.79ce38","type":"googlesmarthome-client","z":"","name":"Example","enabledebug":true,"username":"example","password":"example","port":"3001","ssloffload":false,"publickey":"","privatekey":"","jwtkey":"example","reportinterval":"60","clientid":"example","clientsecret":"example"}]

#### - Thermostat
`topic` can be `thermostatTemperatureAmbient`, `thermostatTemperatureSetpoint` or something else.

If `topic` is `thermostatTemperatureAmbient` then `payload` must be a float and indicates the current ambient (room) temperature.

        msg.topic = 'thermostatTemperatureAmbient'
        msg.payload = 21.5

If `topic` is `thermostatTemperatureSetpoint` then `payload` must be a float and indicates the target temperature of the thermostat.

        msg.topic = 'thermostatTemperatureSetpoint'
        msg.payload = 20.0

If `topic` is `online` then `payload` must be boolean and tells the online state of the thermostat.

        msg.topic = 'online'
        msg.payload = true

If `topic` is something else then `payload` must be an object and tells the online state, ambient and target temperature of the thermostate.

        msg.topic = 'set'
        msg.payload = {
          thermostatTemperatureAmbient: 21.5,
          thermostatTemperatureSetpoint: 20.0,
          online: true
        }

Example flow:

        [{"id":"1c91a7a5.8976d8","type":"google-thermostat","z":"33e99952.700716","client":"9046c434.79ce38","name":"Example Thermostat","topic":"example","passthru":false,"x":820,"y":260,"wires":[["cf803ec6.8dcf4"]]},{"id":"77463fb2.d6709","type":"change","z":"33e99952.700716","name":"topic = thermostatTemperatureAmbient","rules":[{"t":"set","p":"topic","pt":"msg","to":"thermostatTemperatureAmbient","tot":"str"}],"action":"","property":"","from":"","to":"","reg":false,"x":530,"y":240,"wires":[["1c91a7a5.8976d8"]]},{"id":"519811a8.ad4e1","type":"change","z":"33e99952.700716","name":"topic = thermostatTemperatureSetpoint","rules":[{"t":"set","p":"topic","pt":"msg","to":"thermostatTemperatureSetpoint","tot":"str"}],"action":"","property":"","from":"","to":"","reg":false,"x":530,"y":280,"wires":[["1c91a7a5.8976d8"]]},{"id":"cf803ec6.8dcf4","type":"function","z":"33e99952.700716","name":"Split","func":"return [\n    { payload: msg.payload.thermostatTemperatureSetpoint },\n];\n\n// Google returns thermostat mode too, but we currently don't handle different thermostat modes","outputs":1,"noerr":0,"x":1010,"y":260,"wires":[["b9f2d121.b12e6"]],"outputLabels":["thermostatTemperatureSetpoint"]},{"id":"86cd6365.f3f3c","type":"mqtt in","z":"33e99952.700716","name":"","topic":"home/kitchen/current-temp","qos":"2","datatype":"auto","broker":"9e796fe5.2afd1","x":230,"y":240,"wires":[["77463fb2.d6709"]]},{"id":"fdae90e8.744cf","type":"mqtt in","z":"33e99952.700716","name":"","topic":"home/kitchen/target-temp","qos":"2","datatype":"auto","broker":"9e796fe5.2afd1","x":230,"y":280,"wires":[["519811a8.ad4e1"]]},{"id":"b9f2d121.b12e6","type":"mqtt out","z":"33e99952.700716","name":"","topic":"home/kitchen/set-target-temp","qos":"","retain":"","broker":"9e796fe5.2afd1","x":1220,"y":260,"wires":[]},{"id":"9046c434.79ce38","type":"googlesmarthome-client","z":"","name":"Example","enabledebug":true,"username":"example","password":"example","port":"3001","ssloffload":false,"publickey":"","privatekey":"","jwtkey":"example","reportinterval":"60","clientid":"example","clientsecret":"example"},{"id":"9e796fe5.2afd1","type":"mqtt-broker","z":"","name":"Example","broker":"example","port":"1883","clientid":"","usetls":false,"compatmode":false,"keepalive":"60","cleansession":true,"birthTopic":"","birthQos":"0","birthPayload":"","closeTopic":"","closeQos":"0","closePayload":"","willTopic":"","willQos":"0","willPayload":""}]

#### - Window
`topic` can be `openPercent`, `online` or something else.

If `topic` is `openPercent` then `payload` must be integer and indicates the percentage that the window is opened where 0 is closed and 100 is fully open.

        msg.topic = 'openPercent'
        msg.payload = 50


If `topic` is `online` then `payload` must be boolean and tells the online state of the window.

        msg.topic = 'online'
        msg.payload = true

If `topic` is something else then `payload` must be an object and tells both the open state as well as the online state of the window.

        msg.topic = 'set'
        msg.payload = {
          openPercent: false,
          online: true
        }

Example flow:

        [{"id":"bc061a10.51d288","type":"google-window","z":"33e99952.700716","client":"9046c434.79ce38","name":"Example Window","topic":"example","passthru":false,"x":690,"y":140,"wires":[["f7498f9b.ac039"]]},{"id":"8402d206.bcf17","type":"change","z":"33e99952.700716","name":"topic = openPercent","rules":[{"t":"set","p":"topic","pt":"msg","to":"openPercent","tot":"str"}],"action":"","property":"","from":"","to":"","reg":false,"x":460,"y":140,"wires":[["bc061a10.51d288"]]},{"id":"f7498f9b.ac039","type":"function","z":"33e99952.700716","name":"Split","func":"return [\n    { payload: msg.payload.openPercent },\n];","outputs":1,"noerr":0,"x":870,"y":140,"wires":[["c310701b.494b1"]],"outputLabels":["openPercent"]},{"id":"a6869d62.39a14","type":"mqtt in","z":"33e99952.700716","name":"","topic":"home/window/state","qos":"2","datatype":"auto","broker":"9e796fe5.2afd1","x":230,"y":140,"wires":[["8402d206.bcf17"]]},{"id":"c310701b.494b1","type":"mqtt out","z":"33e99952.700716","name":"","topic":"home/window/set-state","qos":"","retain":"","broker":"9e796fe5.2afd1","x":1060,"y":140,"wires":[]},{"id":"9046c434.79ce38","type":"googlesmarthome-client","z":"","name":"Example","enabledebug":true,"username":"example","password":"example","port":"3001","ssloffload":false,"publickey":"","privatekey":"","jwtkey":"example","reportinterval":"60","clientid":"example","clientsecret":"example"},{"id":"9e796fe5.2afd1","type":"mqtt-broker","z":"","name":"Example","broker":"example","port":"1883","clientid":"","usetls":false,"compatmode":false,"keepalive":"60","cleansession":true,"birthTopic":"","birthQos":"0","birthPayload":"","closeTopic":"","closeQos":"0","closePayload":"","willTopic":"","willQos":"0","willPayload":""}]


#### - Scene
Messages sent to this node is simply passed through. One cannot tell Google SmartHome to activate a scene, they tell us.


#### - Vacuum
`topic` can be `on`, `online` or something else.

If `topic` is `on` then `payload` must be boolean and and tells the state of the vacuum.

        msg.topic = 'on'
        msg.payload = true

If `topic` is `online` then `payload` must be boolean and tells the online state of the vacuum.

        msg.topic = 'online'
        msg.payload = true

If `topic` is something else then `payload` must be an object and tells both the on state as well as the online state of the vacuum.

        msg.topic = 'set'
        msg.payload = {
          on: true,
          online: true
        }


#### - Fan
`topic` can be `on`, `online` or something else.

If `topic` is `on` then `payload` must be boolean and and tells the state of the fan.

        msg.topic = 'on'
        msg.payload = true

If `topic` is `online` then `payload` must be boolean and tells the online state of the fan.

        msg.topic = 'online'
        msg.payload = true

If `topic` is something else then `payload` must be an object and tells both the on state as well as the online state of the fan.

        msg.topic = 'set'
        msg.payload = {
          on: true,
          online: true
        }

#### - Management
`topic` can be `restart_server`, `report_state` or `request_sync`.

`payload` is not used for anything.

`restart_server` is used to stop then start the built-in webserver. Can be used when your SSL certificate has been renewed and needs to be re-read by the webserver.

`report_state` will force an update of all states to Google. Mostly usefull for debugging.

`request_sync` will request Google to sync to learn about new or changed devices. This usually happens automatically.

---
## The config node

**Local Authentication**

  `Username` and `Password`: A username and password used when you link Google SmartHome to this node.

**Actions on Google Project Settings**

  `Client ID`: The client id you entered in the *Google on Actions* project.

  `Client Secret`: The client secret you entered in the *Google on Actions* project.

**Google HomeGraph Settings**

  `Jwt Key`: Full path to JWT key file (the one downloaded in the *Add Report State* section).

  `Report Interval (m)`: Time, in minutes, between report updates are sent to Google.

**Built-in Web Server Settings**

  `Port`: TCP port of your choosing for incoming connections from Google. Must match what you entered in the *Google on Actions* project.

  `Use external SSL offload`: If enabled, SSL encryption is not used by the node and must be done elsewhere.

  `Public Key`: Full path to public key file, e.g. `fullchain.pem` from Let's Encrypt.

  `Private Key`: Full path to private key file, e.g. `privkey.pem` from Let's Encrypt.

---
## Google SmartHome Setup Instructions

See the developer guide and release notes at [https://developers.google.com/actions/](https://developers.google.com/actions/) for more details.

#### Create and setup project in Actions Console

1. Use the [Actions on Google Console](https://console.actions.google.com) to add a new project with a name of your choosing and click *Create Project*.
2. Click *Home Control*, then click *Smart Home*.
3. On the left navigation menu under *SETUP*, click on *Invocation*.
4. Add your App's name. Click *Save*.
5. On the left navigation menu under *DEPLOY*, click on *Directory Information*.
6. Add your App info, including images, a contact email and privacy policy. This information can all be edited before submitting for review.
7. Click *Save*.

#### Add Request Sync
~~*Note: I'm almost certain this part is not needed.*~~

The Request Sync feature allows the nodes in this package to send a request to the Home Graph to send a new SYNC request.

1. Navigate to the [Google Cloud Console API Manager](https://console.developers.google.com/apis) for your project id.
2. Enable the [HomeGraph API](https://console.cloud.google.com/apis/api/homegraph.googleapis.com/overview). This will be used to request a new sync and to report the state back to the HomeGraph.
3. ~~Click Credentials~~
4. ~~Click 'Create credentials'~~
5. ~~Click 'API key'~~
6. ~~Copy the API key shown and insert it in `smart-home-provider/cloud/config-provider.js`~~
7. ~~Enable Request-Sync API using [these instructions](https://developers.google.com/actions/smarthome/create-app#request-sync).~~

#### Add Report State
The Report State feature allows the nodes in this package to proactively provide the current state of devices to the Home Graph without a `QUERY` request. This is done securely through [JWT (JSON web tokens)](https://jwt.io/).

1. Navigate to the [Google Cloud Console API & Services page](https://console.cloud.google.com/apis/credentials)
2. Select **Create Credentials** and create a **Service account key**
3. Create the account and download a JSON file.
   Save this as `jwt-key.json`. You must copy this file to your Node-RED server, in a location where the Node-RED service can read it.

#### Final touches

1. Navigate back to the [Actions on Google Console](https://console.actions.google.com).
2. On the left navigation menu under *BUILD*, click on *Actions*. Click on *Add Your First Action* and choose your app's language(s).
3. Enter the URL for fulfillment, e.g. https://example.com:3001/smarthome, click *Done*.
4. On the left navigation menu under *ADVANCED OPTIONS*, click on *Account Linking*.
5. Select *No, I only want to allow account creation on my website*. Click *Next*.
6. For Linking Type, select *OAuth*.
7. For Grant Type, select 'Authorization Code' for Grant Type.
8. Under Client Information, enter the client ID and secret from earlier.
9. The Authorization URL is the hosted URL of your app with '/oauth' as the path, e.g. https://example.com:3001/oauth
10. The Token URL is the hosted URL of your app with '/token' as the path, e.g. https://example.com:3001/token
11. Enter any remaining necessary information you might need for authentication your app. Click *Save*.
12. On the left navigation menu under *Test*, click on *Simulator*, to begin using this app.

*Note:* The `example.com` name in the above text must be your actual domain name (and the same name as used in your SSL certficate).

#### Setup Account linking

1. On a device with the Google Assistant logged into the same account used to create the project in the Actions Console, enter your Assistant settings.
2. Click Home Control.
3. Click the '+' sign to add a device.
4. Find your app in the list of providers. It will be `[test]` and then your app name.
5. Log in to your service. Username and password is the ones you specified in the configuration node.
6. Start using the Google Assistant.

---
## Troubleshooting

- It seems that the Google Smart Home app is taken partially out of test after some time (months?). Existing devices works fine but new devices cannot be added and existing devices cannot be deleted. The Management node will output messages like this:

        "_type": "actions-requestsync",
        "msg": {
                "error": {
                        "code": 400,
                        "message": "Request contains an invalid argument.",
                        "status": "INVALID_ARGUMENT"
                }
        }
  Go to [Actions on Google Console](https://console.actions.google.com), select the *Simulator* and start the test again.
- Google might say that it cannot reach your device if that device did not update its state at least once after creation.
- Check if your service is reachable from the outside. Use [reqbin.com](https://reqbin.com/) or a similar tool to
  send a GET request to https://example.com:3001/login (with your hostname and port). It must answer with status
  200 (OK) and some HTML code in the body. Use https://www.ssllabs.com/ssltest/ to check your SSL certificate.
- Unlink and relink your account in the Google Home app.
- Check Node-RED's logfiles.
- Toggle "Enable Node debug" in the configuration node, connect a debug node to the output of the management node and
  look for debug messages.

---
## Credits
Parts of this README and large parts of the code comes from Google. [Actions on Google: Smart Home sample using Node.js](https://github.com/actions-on-google/smart-home-nodejs) in particular has been of great value.

## Copyright and license
Copyright 2018 - 2020 Michael Jacobsen under [the GNU General Public License version 3](LICENSE).
