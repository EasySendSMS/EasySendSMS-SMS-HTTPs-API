# HTTP(s) API (Get, Post Method)

This page provides a reference for all features available to you via the HTTP interface for sending SMS.

The HTTP-API allows you to integrate your system (client) to Easy Send SMS using the HTTP protocol to send SMS. HTTPS is also supported for secure transactions using SSL encryption.

The Client issues either a GET or POST request to the EasySendSMS HTTP API supplying a list of required parameters. Our system issues back a HTTP response which indicates the status of the sent message.

The HTTP-API is used for one-way messaging only.


| Parameter | Description | Presence |
| --------- | ----------- | -------- |
| Username | Your EasySendSMS username | Mandatory |
| Password | Your EasySendSMS account password or your API password (if you set at your account settings) | Mandatory |
| From | Sender Name that the message will appear from. <br /> **Max Length of 18 if numeric.** <br /> **Length of 11 if alphanumeric.** <br /> To prefix the plus sign (+) to the sender's address when the message is displayed on their cell phone, please prefix the plus sign to your sender's address while submitting the message (note the plus sign should be URL encoded). Additional restrictions on this field may be enforced by the SMSC. | Mandatory |
| To | Mobile number of the recipient that the message will be sent to. Eg: 61409317436 (Do not use + or 00 before the country code) | Mandatory |
| Text | The message to be sent. It can be English as plain text or any other language as Unicode, max message length 5 parts. For concatenated (long) messages every 153 characters are counted as one message for plain text and 67 characters for Unicode, as the rest of the characters will be used by the system for packing extra information for re-assembling the message on the cell phone | Mandatory |
| Type | It indicates the type of message. Values for type include: <br /> 0: Plain text (GSM 3.38 Character encoding) <br /> 1: Unicode (For any other language) | Mandatory |


## HTTP Response

The HTTP response from our system contains the following:

Status Code
Sent OK Message ID (Internal use only)
SMS ID
Error message (if present)


## Status Codes

If the message has been sent successfully the status code will return as below:

**Example:**

```
OK: 760d54eb-3a82-405c-a7a7-0a0096833615
```

If the message was unable to be delivered it will return **ERROR: {Error code}**

**Example:**

```
ERROR:1001
```

**Examples**

Below are example for (Get Method) request when using the HTTP interface.


#### Example Send Single SMS (English)

**Username:** testuser
**Password:** secret
**From:** Test
**To:** 103333333333
**Text:** Hello World
**Type:** 0

**Request:**
```
https://api.easysendsms.app/bulksms?username=testuser&password=secret&from=Test&to=12345678910&text=Hello%20world&type=0
```
The message has to be URL encoded and the type parameter has to be set to (type=0).

**Output:**
```
OK: 760d54eb-3a82-405c-a7a7-0a0096833615
```


### Example Send To Multi Numbers ( Bulk SMS )

```
https://api.easysendsms.app/bulksms?username=testuser&password=secret&from=Test&to=12345678910,12345678910,12345678910,12345678910,12345678910&text=Hello%20world&type=0
```

A request containing multiple destinations numbers will be aborted immediately if any error other than â€œInvalid Destinationâ€ is found. In case an invalid destination is found we just skip that destination number and proceed to the next destination number, maximum you can send 30 numbers per submission.

Note: Duplicate numbers will be ignored.

**Output:**

```
OK: 760d54eb-3a82-405c-a7a7-0a0096833615,OK: 34763d84-683e-4b53-bd9f-fc9eb8c532b7
```


### Example Send Single SMS (Unicode)

```
https://api.easysendsms.app/bulksms?username=testuser&password=secret&from=Test&to=12345678910&text=006500610073007900730065006E00640073006D0073002E0063006F006D&type=1
```

The message has to be encoded on the UTF-16BE format and the type parameter has to be set to (type=1).

**Output:**

```
OK:1234567890123
```


## API rate limit:

To maintain a high quality of service to all customers, EasySendSMS API applies rate limits for its SMS API. The default request rate limit is 30 request per second per account and can reach up to 150 requests per second per IP address, ( Contact our support if you wish to have that ).

The API will reject all requests exceeding this rate limit with 429 Too Many Requests HTTP Status.

The SMPP connection rate limit can reach up to 300 to 500 SMS per sec and can be adjusted upon request, ( Contact our support if you wish to have that ).
You can retry the request after 1 second.



### Status Codes

| Parameter | Description |
| --------- | ----------- |
| Pending | The message has been sent to the route and not yet received by the handset. |
| Delivrd | The message has been received by the handset. |
| Expired | The carrier has timed out. |
| Undeliv | The messages failed to reach the handset. |



### API Error Codes:

| Parameter | Description |
| --------- | ----------- |
| 1001 | Invalid URL. This means that one of the parameters was not provided or left blank. |
| 1002 | Invalid username or password parameter. |
| 1003 | Invalid type parameter. |
| 1004 | Invalid message. |
| 1005 | Invalid mobile number. |
| 1006 | Invalid sender name. |
| 1007 | Insufficient credit. |
| 1008 | Internal error (do NOT re-submit the same message again). |
| 1009 | Service not available (do NOT re-submit the same message again). |



### Call The API By Code

## .NET
```
    var options = new RestClientOptions("")
    {
    MaxTimeout = -1,
    };
    var client = new RestClient(options);
    var request = new RestRequest("https://api.easysendsms.app/bulksms", Method.Post);
    request.AddHeader("Content-Type", "application/x-www-form-urlencoded");
    request.AddHeader("Cookie", "ASPSESSIONIDASCQBARR=NKOHDCHDOFEOOALJIGDGGPAM");
    request.AddParameter("username", "username");
    request.AddParameter("password", "password");
    request.AddParameter("to", "12345678900");
    request.AddParameter("from", "test");
    request.AddParameter("text", "Hello world");
    request.AddParameter("type", "0");
    RestResponse response = await client.ExecuteAsync(request);
    Console.WriteLine(response.Content);
```


## PHP
```
<?php

$curl = curl_init();

curl_setopt_array($curl, array(
  CURLOPT_URL => 'https://api.easysendsms.app/bulksms?username=username&password=password&
  from=Test&to=12345678910&text=Hello%2520world&type=0',
  CURLOPT_RETURNTRANSFER => true,
  CURLOPT_ENCODING => '',
  CURLOPT_MAXREDIRS => 10,
  CURLOPT_TIMEOUT => 0,
  CURLOPT_FOLLOWLOCATION => true,
  CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
  CURLOPT_CUSTOMREQUEST => 'POST',
  CURLOPT_HTTPHEADER => array(
    'Cookie: ASPSESSIONIDCWQRASQQ=BLLIBPGCFLJIPPNALJBOCADC'
  ),
));

$response = curl_exec($curl);

curl_close($curl);
echo $response;

?>
```


## Java
```
//The OkHttpClient is an external package, you need to install it before making the request, 
//And don't forget to add user permission for the internet inside "\MyApplication\app\src\main\AndroidManifest.xml"
//Also, you need to add required dependencies inside "MyApplication\app\build.gradle"

OkHttpClient client = new OkHttpClient().newBuilder()
   .build();
MediaType mediaType = MediaType.parse("text/plain");
RequestBody body = RequestBody.create(mediaType, "");
Request request = new Request.Builder()
   .url("https://api.easysendsms.app/bulksms?username=username&
   password=password&from=Test&to=12345678910&text=Hello%20world&type=0")
   .method("POST", body)
   .addHeader("Cookie", "ASPSESSIONIDCWQRASQQ=BLLIBPGCFLJIPPNALJBOCADC")
   .build();
Response response = client.newCall(request).execute();
```
