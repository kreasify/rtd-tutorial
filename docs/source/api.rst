API URLs
========

API access can be over HTTP or HTTPs

Security Note
-------------

We strongly advise to use HTTPs to make sure your data travels encrypted, avoiding the exposure of your API token and/or sensor data.

HTTP/HTTPS
----------

.. list-table::
   :widths: 80 80 200 80
   :header-rows: 1

   * - Protocol
     - TenX Account
     - Endpoint
     - Port
   * - HTTP
     - OEM
     - http://oem-iot.com
     - 80
   * - HTTPS
     - OEM
     - https://oem-iot.com
     - 443

TenX supports SSL v1.1, TLS v1.2 and v1.3.

Root certificates are shared separately and are available in multiple formats.

* ``PEM``: A certificate chain with two root certificates from our certificate authorities (CAs).
* ``DER``:: Same as the PEM file, with an alternative encoding.
* ``CRT``:  Same as the PEM file, with a different extension. Often referred to as .crt, .cert or .cer.

API OVERVIEW
************

.. list-table::
   :header-rows: 1

   * - API name
     - description
   * - Get device status API
     - Gets the status information of the device
   * - Set device configuration API
     - Sets configuration data of the device
   * - Event notifier API
     - Allows webapp/ mobile app to get notified of event via webhooks
   * - Event alert API
     - Allows customers to get notified of alerts via email/ sms
   * - FOTA API
     - Allows OEM to update firmware of devices

Get Device Status
*****************

**This API fetches the last seen status of the 10X device or IoT Module.**

Usage:

.. code-block:: console

   https:/oem-iot.com/api/v1.6/devices/<device_label>/device_status

Where ``<device_label>`` is  the label of the Device

Headers
*******

The ``"X-Auth-Token"`` header is required for your request:

.. list-table::
   :header-rows: 1

   * - Header
     - Value
     - Required?
     - Description
   * - X-Auth-Token
     - Token
     - Yes
     - Auth token of account

Examples
--------

Get last seen status of a device:

.. code-block:: console

   $ curl -X GET 'https://industrial.api.ubidots.com/api/v1.6/devices/device_status’ \
   -H 'X-Auth-Token: oaXBo6ODhIjPsusNRPUGIK4d72bc73'

Expected response:

.. code-block:: json

   {
     "Name": Sentroller_mark1,
     "device_id": "A1234",
     "battery": 80,
     "timestamp" : 1635264014782
     "Config": [
       {
         "frequency": 34,
         "param1": 0,
         "param2": 22
       }
     ]
   }

Configure a single device
*************************

**This API sends configuration data to a single device**

**USAGE:**

.. list-table::
   :header-rows: 1

   * - HTTP METHOD
     - URL
   * - POST
     - https://oem-iot.com/api/v1.6/devices/<device_label>/

Where ``<device_label>`` is a string with the label of the Device to which data will be sent to.

Headers
*******

The ``"X-Auth-Token"`` header is required for your request:

.. list-table::
   :header-rows: 1

   * - Header
     - Value
     - Required?
     - Description
   * - X-Auth-Token
     - Token
     - Yes
     - Auth token of account
   * - Content-Type
     - application/json
     - Yes
     - Type of data in body

Examples
--------

Set configuration for a given device

.. code-block:: console

   $ curl -X POST 'https://oem-iot.com/api/v1.6/devices/&lt;device_label&gt;/' \
    -H 'Content-Type: application/json' \
    -H 'X-Auth-Token: oaXBo6ODhIjPsusNRPUGIK4d72bc73' \
    -d '{"frequency": 10, “param1”:340}'

Expected response:

.. code-block:: json

   {
   "Frequency":[{"status_code":201}]
   "param1":[{"status_code":201}]
   }

Event Notifier API
******************

**This API allows the 10X Cloud to inform the customer app of events of interest.**

**USAGE:**

.. list-table::
   :header-rows: 1

   * - HTTP METHOD
     - URL
   * - POST
     - https://oem-iot.com/api/v1.6/devices/<device_label>/

Where ``<device_label>`` is a string with the label of the Device to which data will be sent to.

Headers
*******

The ``"X-Auth-Token"`` header is required for your request:

.. list-table::
   :header-rows: 1

   * - Header
     - Value
     - Required?
     - Description
   * - X-Auth-Token
     - Token
     - Yes
     - Auth token of account
   * - Content-Type
     - application/json
     - Yes
     - Type of data in body

**Events of interest**

.. list-table::
   :header-rows: 1

   * - Config settings have taken effect
     - Configuration changes at console have taken effect
   * - Low battery
     - Battery low on device
   * - FOTA results
     - FOTA success/ fail
   * - Sensor event
     - Sensor reading crosses threshold
   * - Device online
     - Device has come online

.. note::

   OEM needs to provide a callback API which will get invoked when above events of interest occur on a device.

Example1
--------

Get notified when a configuration has taken effect

.. code-block:: console

   $ curl -X POST 'https://oem-iot.com/api/v1.6/devices/<device_label>/' \
   -H 'Content-Type: application/json' \
   -H 'X-Auth-Token: oaXBo6ODhIjPsusNRPUGIK4d72bc73' \
   -d '{"config_event_id":  ["https://<your-url>/<your-notification-api>"], “config_event_param”:0}'

Expected response:

.. code-block:: console

   200 OK

Example2
--------

Get notified when sensor level crosses a threshold. In this case 450 is the threshold.

.. code-block:: console

   $ curl -X POST 'https://oem-iot.com/api/v1.6/devices/<device_label>/' \
   -H 'Content-Type: application/json' \
   -H 'X-Auth-Token: oaXBo6ODhIjPsusNRPUGIK4d72bc73' \
   -d '{"sensor_event_id":  ["https://<your-url>/<your-notification-api>"], “sensor_event_param”:450}'

Expected response:

.. code-block:: console

   200 OK

Event ALERT API
***************

**This API allows the 10X Cloud to inform the customer of events of interest. While the Notifier invokes callbacks, the alert API will send emails, SMS etc**

**USAGE:**

.. list-table::
   :header-rows: 1

   * - HTTP METHOD
     - URL
   * - POST
     - https://oem-iot.com/api/v1.6/devices/<device_label>/

Where ``<device_label>`` is a string with the label of the Device to which data will be sent to.

Headers
*******

The ``"X-Auth-Token"`` header is required for your request:

.. list-table::
   :header-rows: 1

   * - Header
     - Value
     - Required?
     - Description
   * - X-Auth-Token
     - Token
     - Yes
     - Auth token of account
   * - Content-Type
     - application/json
     - Yes
     - Type of data in body

Events of interest
******************

.. list-table::
   :header-rows: 1

   * - Low battery
     - Battery low on device
   * - Sensor event
     - Sensor reading crosses threshold

**Methods of alert :**

**Email, SMS, Push Notification (in development)**

.. note::

   Note:

   OEM needs to provide an email address or phone number which will get contacted when above events of interest occur on a device.

Example1
--------

Get an SMS / text message when sensor reading crosses given threshold

.. code-block:: console

   $ curl -X POST 'https://oem-iot.com/api/v1.6/devices/<device_label>/' \
   -H 'Content-Type: application/json' \
   -H 'X-Auth-Token: oaXBo6ODhIjPsusNRPUGIK4d72bc73' \
   -d '{"sensor_alert_id":  ["+1-800-654-xxyy"], “sensor_alert_method”:”sms”}'

Expected response:

.. code-block:: console

   200 OK

Example2
--------

Get an email when battery reading crosses given threshold

.. code-block:: console

   $ curl -X POST 'https://oem-iot.com/api/v1.6/devices/<device_label>/' \
   -H 'Content-Type: application/json' \
   -H 'X-Auth-Token: oaXBo6ODhIjPsusNRPUGIK4d72bc73' \
   -d '{"battery_event_id":  ["customer_name@gmail.com"], “battery_alert_method”:”email”}'

Expected response:

.. code-block:: console

   200 OK

FOTA API
********

**This API allows the 10X Cloud to perform an over the air firmware update (FOTA)**

**USAGE:**

.. list-table::
   :header-rows: 1

   * - HTTP METHOD
     - URL
   * - POST
     - https://oem-iot.com/api/v1.6/devices/<device_label>/

Where ``<device_label>`` is a string with the label of the Device to which data will be sent to.

Headers
*******

The ``"X-Auth-Token"`` header is required for your request:

.. list-table::
   :header-rows: 1

   * - Header
     - Value
     - Required?
     - Description
   * - X-Auth-Token
     - Token
     - Yes
     - Auth token of account
   * - Content-Type
     - application/json
     - Yes
     - Type of data in body
   
Note: firmware must be placed in a web accessible location (cloud bucket, web server, file server etc)
------------------------------------------------------------------------------------------------------

Example1
--------

Get an SMS / text message when sensor reading crosses given threshold

.. code-block:: console

   $ curl -X POST 'https://oem-iot.com/api/v1.6/devices/<device_label>/' \
   -H 'Content-Type: application/json' \
   -H 'X-Auth-Token: oaXBo6ODhIjPsusNRPUGIK4d72bc73' \
   -d '{"fota_request":  [“http://s3.amazonaws.com/[bucket_name]/filename"]}'

Expected response:

.. code-block:: console

   200 OK