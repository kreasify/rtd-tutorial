OEM IOT API
===========

![](images/image1.png)
======================

Welcome to our Data API
=======================

Welcome to the 10X Data API reference. Here we explain how to send or retrieve data from devices in field using HTTP

The purpose of this document is to empower OEMs to build innovative IoT applications. This documentation helps you to understand what happens behind the scenes when communicating with the 10X devices or modules.

Authentication
==============

Every request sent to the 10X Cloud requires a token. A token is a unique key that authorizes requests sent to the 10X IoT Cloud.

Tokens valid for 6 hours and can be generated using your account’s API Key.

Tokens are short lived. Tokens are auto renewed once deployment is complete..

How to send a token in a request:

Sending the token in the Header (recommended):  
X-Auth-Token=token

Note: Do not send tokens as a query parameter as the token is visible (to logs, browser history and packet sniffers)

//Example Request to create a device with Token in the Header

curl -X POST 'https://tenx.live/api/v1.0/devices/' \

-H 'Content-Type: application/json' \

-H 'X-Auth-Token: oaXBo6ODhIjPsusNRPUGIK4d72bc73' \

-d '{}'

Overview
========

HTTP is the main internet protocol for information exchange in the Internet. It also supports data encryption, which is usually called secure HTTP or HTTPs. When you use a web browser (Firefox, Chrome, Edge, etc.) you are sending and receiving packages using HTTP or HTTPs. Because of this, it was not surprising to see that HTTPs has become a popular option for data exchange for IoT.

When a developer implements HTTPs communication, he should look for a RESTful Application Programming Interface, or REST API, which exposes all the endpoints required to interact with a third-party application (like the 10X Cloud). In this section, you have a reference of our REST API that allows you to understand how to send and retrieve data from our servers.

Note: Our goal with this section is to help you understand what’s happening behind the scenes when communicating with Ubidots, enabling you to replicate it within your own firmware. That's why we avoid using our custom libraries in all the examples.

HTTP requests

The following methods are specified within the HTTP standard:

|     |     |
| --- | --- |
| HTTP METHOD | DESCRIPTION |
| GET | Used to retrieve information |
| POST | Used to send information |

HTTP is a request/response protocol, this means that every request that you make is answered by the server. This response includes a number (response code) and a body. For example, when you make a request to retrieve a file on a website "(e.g. "Get me the file 'webside.html'" )", you are effectively sending a GET request. If the request is correct, the server will typically return a 200 response code, along with requested body, in this case the file.

An HTTP request also needs the following parameters:

* Host: Specifies the server you will be making HTTP requests to, e.g. https://rapid-iot.dev
* Path: This is typically the remaining portion of the URL that specifies the resource you want to consume, e.g. if an API endpoint is: rapid-iot.dev/api/v1.6/devices/my-device the path is /api/v1.6/devices/my-device
* Headers: Define the operating parameters of the HTTP request such as X-Auth-Token, Content-Type, Content-Length, etc.
* Body/payload: In the case of POST and PATCH requests, the body is the data sent to the server, usually as a stringified JSON. By convention GET requests should not have a body because they are meant to request data, not sending them.

TenX accepts data as JavaScript Object Notation or JSON. JSON is a typical HTTP data type, it is a collection of name/value pairs. In various programming languages, this is treated as an object, record, struct, dictionary, hash table, keyed list, or associative array. It is also human readable and language independent. An example of a JSON that TenX accepts can be seen below:

JSON

{"Frequency": {"value":10, "timestamp": 1534881387000, "context": {"machine": "1st floor"}}}

A typical HTTP request to TenX should be set as below:

Text

POST {PATH} HTTP/1.1&lt;CR&gt;&lt;LN&gt;

Host: {HOST}&lt;CR&gt;&lt;LN&gt;

User-Agent: {USER_AGENT}&lt;CR&gt;&lt;LN&gt;

X-Auth-Token: {TOKEN}&lt;CR&gt;&lt;LN&gt;

Content-Type: application/json&lt;CR&gt;&lt;LN&gt;

Content-Length: {PAYLOAD_LENGTH}&lt;CR&gt;&lt;LN&gt;&lt;CR&gt;&lt;LN&gt;

{PAYLOAD}

&lt;CR&gt;&lt;LN&gt;

Where:

* {PATH}: Path to the resource to consume  
    Example: /api/v1.6/variables/ to get user's variables information.
* {HOST}: Host URL.  
    Example: rapid-iot.dev
* {USER_AGENT}: An optional string used to identify the type of client, be it by application type, operating system, software vendor or software version of the requesting user agent.  
    Examples: OEM\_CLOUD, CUST\_DEVICE
* {TOKEN}: Unique key that authorizes your device to ingest data inside your TenX account.
* {PAYLOAD_LENGTH}: The number of characters of your payload.  
    Example: The payload {"Frequency": 20} will have a content-length of 19.
* {PAYLOAD}: Data to send.  
    Example: {"Frequency": 20}

API URLs
========

API access can be over HTTP or HTTPs

Security Note

We strongly advise to use HTTPs to make sure your data travels encrypted, avoiding the exposure of your API token and/or sensor data.

HTTP/HTTPS
----------

|     |     |     |     |
| --- | --- | --- | --- |
| Protocol | TenX Account | Endpoint | Port |
| HTTP | OEM | http://oem-iot.com | 80  |
| HTTPS | OEM | https://oem-iot.com | 443 |

TenX supports SSL v1.1, TLS v1.2 and v1.3.

Root certificates are shared separately and are available in multiple formats.

* PEM: A certificate chain with two root certificates from our certificate authorities (CAs).
* DER:: Same as the PEM file, with an alternative encoding.
* CRT:  Same as the PEM file, with a different extension. Often referred to as .crt, .cert or .cer.

API OVERVIEW

|     |     |
| --- | --- |
| API name | description |
| Get device status API | Gets the status information of the device |
| Set device configuration API | Sets configuration data of the device |
| Event notifier API | Allows webapp/ mobile app to get notified of event via webhooks |
| Event alert API | Allows customers to get notified of alerts via email/ sms |
| FOTA API | Allows OEM to update firmware of devices |

Get Device Status

This API fetches the last seen status of the 10X device or IoT Module.

Usage:

https:/oem-iot.com/api/v1.6/devices/&lt;device\_label&gt;/device\_status

Where &lt;device_label&gt; is  the label of the Device

Headers

The "X-Auth-Token" header is required for your request:

|     |     |     |     |
| --- | --- | --- | --- |
| Header | Value | Required? | Description |
| X-Auth-Token | Token | Yes | Auth token of account |

Examples
--------

Get last seen status of a device:

$ curl -X GET 'https://industrial.api.ubidots.com/api/v1.6/devices/device_status’ \

-H 'X-Auth-Token: oaXBo6ODhIjPsusNRPUGIK4d72bc73'

Expected response:

{

  "Name": Sentroller_mark1,

  "device_id": "A1234",

  "battery": 80,

  "timestamp" : 1635264014782

  "Config": \[

    {

      "frequency": 34,

      "param1": 0,

      "param2": 22

    }

  \]

}

Configure a single device

This API sends configuration data to a single device

USAGE:

|     |     |
| --- | --- |
| HTTP METHOD | URL |
| POST | https://oem-iot.com/api/v1.6/devices/&lt;device_label&gt;/ |

Where &lt;device_label&gt; is a string with the label of the Device to which data will be sent to.

Headers

The "X-Auth-Token" header is required for your request:

|     |     |     |     |
| --- | --- | --- | --- |
| Header | Value | Required? | Description |
| X-Auth-Token | Token | Yes | Auth token of account |
| Content-Type | application/json | Yes | Type of data in body |

Examples
--------

Set configuration for a given device

$ curl -X POST 'https://oem-iot.com/api/v1.6/devices/&lt;device_label&gt;/' \

 -H 'Content-Type: application/json' \

 -H 'X-Auth-Token: oaXBo6ODhIjPsusNRPUGIK4d72bc73' \

 -d '{"frequency": 10, “param1”:340}'

Expected response:

 {

  "Frequency":\[{"status_code":201}\]

  "param1":\[{"status_code":201}\]

}

Event Notifier API

This API allows the 10X Cloud to inform the customer app of events of interest.

USAGE:

|     |     |
| --- | --- |
| HTTP METHOD | URL |
| POST | https://oem-iot.com/api/v1.6/devices/&lt;device_label&gt;/ |

Where &lt;device_label&gt; is a string with the label of the Device to which data will be sent to.

Headers

The "X-Auth-Token" header is required for your request:

|     |     |     |     |
| --- | --- | --- | --- |
| Header | Value | Required? | Description |
| X-Auth-Token | Token | Yes | Auth token of account |
| Content-Type | application/json | Yes | Type of data in body |

Events of interest

|     |     |
| --- | --- |
| Config settings have taken effect | Configuration changes at console have taken effect |
| Low battery | Battery low on device |
| FOTA results | FOTA success/ fail |
| Sensor event | Sensor reading crosses threshold |
| Device online | Device has come online |

Note:

OEM needs to provide a callback API which will get invoked when above events of interest occur on a device.

Example1
--------

Get notified when a configuration has taken effect

$ curl -X POST 'https://oem-iot.com/api/v1.6/devices/&lt;device_label&gt;/' \

 -H 'Content-Type: application/json' \

 -H 'X-Auth-Token: oaXBo6ODhIjPsusNRPUGIK4d72bc73' \

-d '{"config\_event\_id":  \["https://&lt;your-url&gt;/&lt;your-notification-api&gt;"\], “config\_event\_param”:0}'

Expected response:

200 OK

Example2
--------

Get notified when sensor level crosses a threshold. In this case 450 is the threshold.

$ curl -X POST 'https://oem-iot.com/api/v1.6/devices/&lt;device_label&gt;/' \

 -H 'Content-Type: application/json' \

 -H 'X-Auth-Token: oaXBo6ODhIjPsusNRPUGIK4d72bc73' \

-d '{"sensor\_event\_id":  \["https://&lt;your-url&gt;/&lt;your-notification-api&gt;"\], “sensor\_event\_param”:450}'

Expected response:

200 OK

Event ALERT API

This API allows the 10X Cloud to inform the customer of events of interest. While the Notifier invokes callbacks, the alert API will send emails, SMS etc

USAGE:

|     |     |
| --- | --- |
| HTTP METHOD | URL |
| POST | https://oem-iot.com/api/v1.6/devices/&lt;device_label&gt;/ |

Where &lt;device_label&gt; is a string with the label of the Device to which data will be sent to.

Headers

The "X-Auth-Token" header is required for your request:

|     |     |     |     |
| --- | --- | --- | --- |
| Header | Value | Required? | Description |
| X-Auth-Token | Token | Yes | Auth token of account |
| Content-Type | application/json | Yes | Type of data in body |

Events of interest

|     |     |
| --- | --- |
| Low battery | Battery low on device |
| Sensor event | Sensor reading crosses threshold |

Methods of alert :

Email, SMS, Push Notification (in development)

Note:

OEM needs to provide an email address or phone number which will get contacted when above events of interest occur on a device.

Example1
--------

Get an SMS / text message when sensor reading crosses given threshold

$ curl -X POST 'https://oem-iot.com/api/v1.6/devices/&lt;device_label&gt;/' \

 -H 'Content-Type: application/json' \

 -H 'X-Auth-Token: oaXBo6ODhIjPsusNRPUGIK4d72bc73' \

-d '{"sensor\_alert\_id":  \["+1-800-654-xxyy"\], “sensor\_alert\_method”:”sms”}'

Expected response:

200 OK

Example2
--------

Get an email when battery reading crosses given threshold

$ curl -X POST 'https://oem-iot.com/api/v1.6/devices/&lt;device_label&gt;/' \

 -H 'Content-Type: application/json' \

 -H 'X-Auth-Token: oaXBo6ODhIjPsusNRPUGIK4d72bc73' \

-d '{"battery\_event\_id":  \["customer_name@gmail.com"\], “battery\_alert\_method”:”email”}'

Expected response:

200 OK

FOTA API

This API allows the 10X Cloud to perform an over the air firmware update (FOTA)

USAGE:

|     |     |
| --- | --- |
| HTTP METHOD | URL |
| POST | https://oem-iot.com/api/v1.6/devices/&lt;device_label&gt;/ |

Where &lt;device_label&gt; is a string with the label of the Device to which data will be sent to.

Headers

The "X-Auth-Token" header is required for your request:

|     |     |     |     |
| --- | --- | --- | --- |
| Header | Value | Required? | Description |
| X-Auth-Token | Token | Yes | Auth token of account |
| Content-Type | application/json | Yes | Type of data in body |

Note: firmware must be placed in a web accessible location (cloud bucket, web server, file server etc)
------------------------------------------------------------------------------------------------------

Example1
--------

Get an SMS / text message when sensor reading crosses given threshold

$ curl -X POST 'https://oem-iot.com/api/v1.6/devices/&lt;device_label&gt;/' \

 -H 'Content-Type: application/json' \

 -H 'X-Auth-Token: oaXBo6ODhIjPsusNRPUGIK4d72bc73' \

-d '{"fota_request":  \[“http://s3.amazonaws.com/\[bucket_name\]/filename"\]}'

Expected response:

200 OK