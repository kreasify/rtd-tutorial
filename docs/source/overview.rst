Overview
========

HTTP is the main internet protocol for information exchange in the Internet. It also supports data encryption, which is usually called secure HTTP or HTTPs. When you use a web browser (Firefox, Chrome, Edge, etc.) you are sending and receiving packages using HTTP or HTTPs. Because of this, it was not surprising to see that HTTPs has become a popular option for data exchange for IoT.

When a developer implements HTTPs communication, he should look for a RESTful Application Programming Interface, or REST API, which exposes all the endpoints required to interact with a third-party application (like the 10X Cloud). In this section, you have a reference of our REST API that allows you to understand how to send and retrieve data from our servers.

.. note::

    Note: Our goal with this section is to help you understand what’s happening behind the scenes when communicating with Ubidots, enabling you to replicate it within your own firmware. That's why we avoid using our custom libraries in all the examples.

HTTP requests

The following methods are specified within the HTTP standard:

.. list-table::
   :widths: 50 120
   :header-rows: 1

   * - HTTP METHOD
     - DESCRIPTION
   * - GET
     - Used to retrieve information
   * - POST
     - Used to send information

HTTP is a request/response protocol, this means that every request that you make is answered by the server. This response includes a number (response code) and a body. For example, when you make a request to retrieve a file on a website "(e.g. "Get me the file 'webside.html'" )", you are effectively sending a GET request. If the request is correct, the server will typically return a 200 response code, along with requested body, in this case the file.

An HTTP request also needs the following parameters:

* ``Host``: Specifies the server you will be making HTTP requests to, e.g. https://rapid-iot.dev
* ``Path``: This is typically the remaining portion of the URL that specifies the resource you want to consume, e.g. if an API endpoint is: rapid-iot.dev/api/v1.6/devices/my-device the path is /api/v1.6/devices/my-device
* ``Headers``: Define the operating parameters of the HTTP request such as X-Auth-Token, Content-Type, Content-Length, etc.
* ``Body/payload``: In the case of POST and PATCH requests, the body is the data sent to the server, usually as a stringified JSON. By convention GET requests should not have a body because they are meant to request data, not sending them.

TenX accepts data as JavaScript Object Notation or JSON. JSON is a typical HTTP data type, it is a collection of name/value pairs. In various programming languages, this is treated as an object, record, struct, dictionary, hash table, keyed list, or associative array. It is also human readable and language independent. An example of a JSON that TenX accepts can be seen below:

JSON

.. code-block:: json

    {"Frequency": {"value":10, "timestamp": 1534881387000, "context": {"machine": "1st floor"}}}

A typical HTTP request to TenX should be set as below:

Text

.. code-block:: text

    POST {PATH} HTTP/1.1<CR><LN>
    Host: {HOST}<CR><LN>
    User-Agent: {USER_AGENT}<CR><LN>
    X-Auth-Token: {TOKEN}<CR><LN>
    Content-Type: application/json<CR><LN>
    Content-Length: {PAYLOAD_LENGTH}<CR><LN><CR><LN>
    {PAYLOAD}
    <CR><LN>

Where:

* ``{PATH}``: Path to the resource to consume  
    Example: ``/api/v1.6/variables/`` to get user's variables information.
* ``{HOST}``: Host URL.  
    Example: rapid-iot.dev
* ``{USER_AGENT}``: An optional string used to identify the type of client, be it by application type, operating system, software vendor or software version of the requesting user agent.  
    Examples: ``OEM\_CLOUD, CUST\_DEVICE``
* ``{TOKEN}``: Unique key that authorizes your device to ingest data inside your TenX account.
* ``{PAYLOAD_LENGTH}``: The number of characters of your payload.  
    Example: The payload ``{"Frequency": 20}`` will have a content-length of 19.
* ``{PAYLOAD}``: Data to send.  
    Example: ``{"Frequency": 20}``
