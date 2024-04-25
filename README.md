# The Repository of Thingsboard convertors for Integrations

## Add new converters

The Repository contains decoders for different device integrations.

Users can contribute by adding their own version of the decoder for a specific device.

To add a decoder, submit a fork pull request to this repository.

### Files and Directories

Each device in the Converters Repository consists of several JSON files:

There are next file in the Convertors Repository (format only json) for one Device 
- config is mandatory 
- uplink is mandatory
- uplink(downlink)/metadata is optional
- downlink is optional

> All files and directories must have lowercase titles.
> The "the name of device with version" as directory is a distinct value for every unique device listed in the devices directory.

1. **config.json** Contains information about the device (`devices/"the name of device with version"/config.json`)
2. **uplink/decoder.json** Decodes uplink messages from the device (`devices/"the name of device with version"/uplink/decoder.json`)
3. **uplink/payload.json** Contains sample payload data for uplink messages (`devices/"the name of device with version"/uplink/payload.json`)
4. **uplink/result.json** Contains the expected result after decoding uplink messages (`devices/"the name of device with version"/uplink/result.json`)
5. **uplink/metadata.json** Contains sample metadata for uplink messages (`devices/"the name of device with version"/uplink/metadata.json`)
6. **downlink/decoder.json** for downlink of a single device (`devices/"the name of device with version"/downlink/decoder.json`)
7. **downlink/payload.json** for downplink of a single device (`devices/"the name of device with version"/downlink/payload.json`)
8. **downlink/result.json** for downlink of a single device (`devices/"the name of device with version"/downlink/result.json`)
9. **downlink/metadata.json** Contains sample metadata for downlink messages (`devices/"the name of device with version"/downlinkk/metadata.json`)


 ```arduino
Convertors Repository for devices
├── devices
│   ├── <the name of device with version>
│   │   ├── config.json                # config device
│   │   ├── uplink                     # directory uplink for device
│   │   │   ├── decoder.json           # decoder uplink for device
│   │   │   ├── payload.json           # payload uplink (input data to decoder) for device
│   │   │   ├── metadata.json (otional)# metadata uplink (input data to decoder) for device
│   │   │   ├── result.json            # result after running decoder uplink for device
│   │   ├── downlink (otional)         # directory downlink for device
│   │   │   ├── decoder.json           # decoder downlink for device
│   │   │   ├── payload.json           # payload downlink (input data to decoder) for device
│   │   │   ├── metadata.json (otional)# metadata downlink (input data to decoder) for device
│   │   │   ├── result.json            # result after running decoder downlink for device
```

#### config.json

Contains fields such as scriptLang, name, version, and manufacturer.

> **NOTE** "scriptLang" can be set: *"javaScript"* or *"tbel"*
> *Example*:

```json5
{
  "scriptLang": "tbel",
  "name": "device-A",
  "version": "12.01",
  "manufacturer": "My_manufacturer"
}
```

#### payload.json
Contains sample payload data for testing the decoder.

After **Test** the converter in "ThingsBoard Pe" and successfully **Save**, copy-paste value from **Paylod** and saving its to file **payload.json**.

> *Example*:

```json5
{
  "devName": "devA",
  "param1": 1,
  "param2": "test"
}
```

#### metadata.json
Contains sample metadata for testing the decoder.

> **NOTE** "metadata.json" is optional. Only if **metadata** is used in the **decoder**.

After **Test** the converter in "ThingsBoard Pe" and successfully **Save**, copy-paste key and value from **Metadata** and saving its to file **metadata.json**.

> *Example*:

```json5
{
  "topic": "1/22/12.01/12"
}
```

#### decoder.json
Decodes messages from the device and contains the decoding logic.

After saved converter in "ThingsBoard Pe" execute operation **"Export converter"** and result (file) change the filename to "decoder" with the extension "json".

> *Example*:

```json5
{
  "name": "uplinkConverterTest",
  "type": "UPLINK",
  "debugMode": true,
  "configuration": {
    "scriptLang": "TBEL",
    "decoder": "// Decode an uplink message from a buffer\n// payload - array of bytes\n// metadata - key/value object\n\n/** Decoder **/\n\n// decode payload to string\nvar payloadStr = decodeToString(payload);\n\n// decode payload to JSON\n// var data = decodeToJson(payload);\n\nvar deviceName = 'Device A';\nvar deviceType = 'thermostat';\nvar customerName = 'Customer C';\nvar groupName = 'thermostat devices';\nvar manufacturer = 'Example corporation';\n// use assetName and assetType instead of deviceName and deviceType\n// to automatically create assets instead of devices.\n// var assetName = 'Asset A';\n// var assetType = 'building';\n\n// Result object with device/asset attributes/telemetry data\nvar result = {\n// Use deviceName and deviceType or assetName and assetType, but not both.\n   deviceName: deviceName,\n   deviceType: deviceType,\n// assetName: assetName,\n// assetType: assetType,\n// customerName: customerName,\n   groupName: groupName,\n   attributes: {\n       model: 'Model A',\n       serialNumber: 'SN111',\n       integrationName: metadata['integrationName'],\n       manufacturer: manufacturer\n   },\n   telemetry: {\n       temperature: 42,\n       humidity: 80,\n       rawData: payloadStr\n   }\n};\n\n/** Helper functions **/\n\nfunction decodeToString(payload) {\n   return String.fromCharCode.apply(String, payload);\n}\n\nfunction decodeToJson(payload) {\n   // covert payload to string.\n   var str = decodeToString(payload);\n\n   // parse string to JSON\n   var data = JSON.parse(str);\n   return data;\n}\n\nreturn result;",
    "tbelDecoder": "// Decode an uplink message from a buffer\n// payload - array of bytes\n// metadata - key/value object\n\n/** Decoder **/\n\n// decode payload to string\nvar payloadStr = decodeToString(payload);\n\n// decode payload to JSON\n// var data = decodeToJson(payload);\n\nvar deviceName = 'Device A';\nvar deviceType = 'thermostat';\nvar customerName = 'Customer C';\nvar groupName = 'thermostat devices';\nvar manufacturer = 'Example corporation';\nvar ver = metadata.topic.split(\"/\")[2];\n// use assetName and assetType instead of deviceName and deviceType\n// to automatically create assets instead of devices.\n// var assetName = 'Asset A';\n// var assetType = 'building';\n\n// Result object with device/asset attributes/telemetry data\nvar result = {\n// Use deviceName and deviceType or assetName and assetType, but not both.\n   deviceName: deviceName,\n   deviceType: deviceType,\n// assetName: assetName,\n// assetType: assetType,\n// customerName: customerName,\n   groupName: groupName,\n   attributes: {\n       model: 'Model A',\n       serialNumber: 'SN111',\n       integrationName: metadata['integrationName'],\n       manufacturer: manufacturer,\n       ver: ver\n   },\n   telemetry: {\n       temperature: 42,\n       humidity: 80,\n       rawData: payloadStr\n   }\n};\n\n/** Helper functions 'decodeToString' and 'decodeToJson' are already built-in **/\n\nreturn result;",
    "encoder": null,
    "tbelEncoder": null,
    "updateOnlyKeys": [
      "manufacturer"
    ]
  },
  "additionalInfo": {
    "description": "device-A ver: 12.01"
  },
  "edgeTemplate": false
}
```

#### result.json\
Contains the expected result after decoding the messages.

After **Test** the converter in "ThingsBoard Pe" and successfully **Save**, copy-paste value from **Output** and saving its to file **result.json**.

> *Example*:

```json5
{
  "deviceName": "Device A",
  "deviceType": "thermostat",
  "groupName": "thermostat devices",
  "attributes": {
    "model": "Model A",
    "serialNumber": "SN111",
    "integrationName": "Test integration",
    "manufacturer": "Example corporation",
    "ver": "12.01"
  },
  "telemetry": {
    "temperature": 42,
    "humidity": 80,
    "rawData": "{\n    \"devName\": \"devA\",\n    \"param1\": 1,\n    \"param2\": \"test\"\n}"
  }
}
```