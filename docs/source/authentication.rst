Authentication
==============

Every request sent to the 10X Cloud requires a token. A token is a unique key that authorizes requests sent to the 10X IoT Cloud.

Tokens valid for 6 hours and can be generated using your account’s API Key.

Tokens are short lived. Tokens are auto renewed once deployment is complete..

How to send a token in a request:

Sending the token in the Header (recommended):  
``X-Auth-Token=token``

>>> Note: Do not send tokens as a query parameter as the token is visible (to logs, browser history and packet sniffers)

//Example Request to create a device with Token in the Header

.. code-block:: console

    curl -X POST 'https://tenx.live/api/v1.0/devices/' \

    -H 'Content-Type: application/json' \

    -H 'X-Auth-Token: oaXBo6ODhIjPsusNRPUGIK4d72bc73' \

    -d '{}'

