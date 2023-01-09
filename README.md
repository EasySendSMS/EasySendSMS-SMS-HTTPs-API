# EasySendSMS HTTP SMS API

This page provides a reference for all features available to you via the HTTP interface for sending SMS.

The HTTP-API allows you to integrate your system (client) to Easy Send SMS using the HTTP protocol to send SMS. HTTPS is also supported for secure transactions using SSL encryption.

The Client issues either a HTTP GET or POST request to the Easy Send SMSHTTP interface supplying a list of required parameters. Our system issues back a HTTP response which indicates the status of the sent message.

The HTTP-API is used for one-way messaging only. Therefore, you need to provide a valid the Sender ID of the message to enable the recipient to respond.

| Parameter | Description | Presence |
| --------- | ----------- | -------- |
| Username | Your Easy Send SMS username | Mandatory |
| Password | Your Easy Send SMS password | Mandatory |
| From | Sender Name that the message will appear from. <br /> **Max Length of 18 if numeric.** <br /> **Max Length of 11 if alphanumeric.** <br /> To prefix the plus sign (+) to the sender’s address when the message is displayed on their cell phone, please prefix the plus sign to your sender’s address while submitting the message (note the plus sign should be URL encoded). Additional restrictions on this field may be enforced by the SMSC. | Mandatory |
| To | MSIDSN of the recipient that the message will be sent to. Eg: 61409317436 (Do not use + before the country code) | Mandatory |
| Text | The message to be sent. It can be used for 'long' messages, that is, messages longer than 160 characters for plain text, 280 for Unicode. For concatenated (long) messages every 153 characters are counted as one message for plain text and 268 characters for Unicode, as the rest of the characters will be used by the system for packing extra information for re-assembling the message on the cell phone | Mandatory |
| Type | It indicates the type of message. Values for type include: <br /> 0: Plain text (GSM 3.38 Character encoding) <br /> 1: Unicode (160 characters for plain text, 280 for Unicode. For concatenated (long) messages every 153 characters are counted as one message for plain text and 268 characters for Unicode, as the rest of the characters will be used by the system for packing extra information for re-assembling the message on the cell phone. ) | Mandatory |

## HTTP Response

The HTTP response from our system contains the following:

Status Code
Sent OK Message ID (Internal use only)
SMS ID
Error message (if present)

## Status Codes

If the message has been sent successfully the status code will return **OK: 0**

**Example:**

```
OK:1234567891011
```

If the message was unable to be delivered it will return **ERROR: {Error code}**

**Example:**

```
ERROR:1001
```

**Examples**

Below are example requests when using the HTTP interface.

#### Example Send Single SMS (English)

**Username:** testuser
**Password:** secret
**From:** Test
**To:** 103333333333
**Text:** Hello World
**Type:** 0

**Request:**
```
https://www.easysendsms.com/sms/bulksms-api/bulksms-api?username=testuser&password=secret&from=Test&to=12345678910&text=Hello%20world&type=0
```

**Output:**
```
OK:1234567890123
```

### Example Send To Multi Numbers ( Bulk SMS )

```
https://www.easysendsms.com/sms/bulksms-api/bulksms-api?username=testuser&password=secret&from=Test&to=12345678910,12345678910,12345678910,12345678910,12345678910&text=Hello%20world&type=0
```

A request containing multiple destinations will be aborted immediately if any error other than “Invalid Destination” is found. In case an invalid destination is found we just skip that destination and proceed to the next destination, maximum you can send 30 numbers per submission

**Output:**

```
OK:1234567890123,OK:1234567890123,OK:1234567890123,OK:1234567890123
```

### Example Send Single SMS ( Unicode )

```
https://www.easysendsms.com/sms/bulksms-api/bulksms-api?username=testuser&password=secret&from=Test&to=12345678910&text=006500610073007900730065006E00640073006D0073002E0063006F006D&type=1
```

calling the above link by replacing the username and password by your account credentials, you should get the SMS ‘easysendsms.com’ on the mobile number in the destination field. The message has to be encoded on the UTF-16BE format and the type parameter has to be set to (type=1).

**Output:**

```
OK:1234567890123
```

### Status Codes

| Parameter | Description |
| --------- | ----------- |
| Pending | The message has been sent to the route and not yet received by the handset. |
| Delivrd | The message has been received by the handset. |
| Expired | The carrier has timed out. |
| Undeliv | The messages failed to reach the handset. |

### Error Codes

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

### Call The API By Code|

## .NET
```
var client = new RestClient("https://www.easysendsms.com/sms/bulksms-api/bulksms-api?username=testuser&password=secret&from=Test&to=12345678910,12345678910,12345678910,12345678910,12345678910&text=Hello%20world&type=0");
client.Timeout = -1;
var request = new RestRequest(Method.GET);
request.AddHeader("Cookie", "ASPSESSIONIDAWRTTQDQ=AELBOALACIGEEPOBCMFMMJBG");
IRestResponse response = client.Execute(request);
Console.WriteLine(response.Content);
```

## PHP
```
<?php

$curl = curl_init();

curl_setopt_array($curl, array(
  CURLOPT_URL => 'https://www.easysendsms.com/sms/bulksms-api/bulksms-api?username=username&password=password&from=Test&to=12345678910&text=Hello%2520world&type=0',
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

## PHP
```
//The OkHttpClient is an external package, you need to install it before making the request, 
//And don't forget to add user permission for the internet inside "\MyApplication\app\src\main\AndroidManifest.xml"
//Also, you need to add required dependencies inside "MyApplication\app\build.gradle"

OkHttpClient client = new OkHttpClient().newBuilder()
  .build();
MediaType mediaType = MediaType.parse("text/plain");
RequestBody body = RequestBody.create(mediaType, "");
Request request = new Request.Builder()
  .url("https://www.easysendsms.com/sms/bulksms-api/bulksms-api?username=username&password=password&from=Test&to=12345678910&text=Hello%20world&type=0")
  .method("POST", body)
  .addHeader("Cookie", "ASPSESSIONIDCWQRASQQ=BLLIBPGCFLJIPPNALJBOCADC")
  .build();
Response response = client.newCall(request).execute();
```
