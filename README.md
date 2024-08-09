# SMS HTTP(s) API (Get, Post Method)

This page provides a reference for all features available to you via the HTTP interface for sending SMS.

## Overview

The [HTTP-API](https://easysendsms.com) allows you to integrate your system (client) to [EasySendSMS](https://easysendsms.com) using the HTTP protocol to send SMS. HTTPS is also supported for secure transactions using SSL encryption.

The Client issues either a GET or POST request to the [EasySendSMS HTTP API](https://easysendsms.com) supplying a list of required parameters. Our system issues back an HTTP response which indicates the status of the sent message.

The HTTP-API is used for one-way messaging only.

## Important Notes

- If you have set a dedicated API Password different from the account password, make sure to use it.
- If you have set a Whitelisted IP for your API, make sure the request will come from that IP; otherwise, the request will be rejected.

## API Base URL

```
https://api.easysendsms.app/bulksms
```

## API Parameters

| Parameter | Description                                                                 | Presence   |
|-----------|-----------------------------------------------------------------------------|------------|
| Username  | Your [EasySendSMS](https://easysendsms.com) username                        | Mandatory  |
| Password  | Your [EasySendSMS](https://easysendsms.com) account password or your API password (if you set it in your account settings) | Mandatory  |
| From      | Sender Name that the message will appear from.                              | Mandatory  |
| To        | Mobile number of the recipient that the message will be sent to. Eg: 61409317436 (Do not use + or 00 before the country code) | Mandatory  |
| Text      | The message to be sent. It can be English as plain text or any other language as Unicode, max message length 5 parts. | Mandatory  |
| Type      | It indicates the type of message. Values for type include: 0: Plain text (GSM 3.38 Character encoding), 1: Unicode (For any other language) | Mandatory  |

## HTTP Response

The HTTP response from our system contains the following:

### Status Codes

If the message has been sent successfully the status code will return as below:

**Example:**

```
OK: 760d54eb-3a82-405c-a7a7-0a0096833615
```

If the message was unable to be delivered it will return `ERROR: {Error code}`

**Example:**

```
ERROR:1001
```

### Example Queries

**Send Single SMS (English) Example:**

- **Username**: testuser
- **Password**: secret
- **From**: Test
- **To**: 103333333333
- **Text**: Hello World
- **Type**: 0

**Request URL:**

```
https://api.easysendsms.app/bulksms?username=testuser&password=secret&from=Test&to=12345678910&text=Hello%20world&type=0
```

**Output:**

```
OK: 760d54eb-3a82-405c-a7a7-0a0096833615
```

**Send To Multiple Numbers (Bulk SMS) Example:**

```
https://api.easysendsms.app/bulksms?username=testuser&password=secret&from=Test&to=12345678910,12345678910,12345678910,12345678910,12345678910&text=Hello%20world&type=0
```

A request containing multiple destination numbers will be aborted immediately if any error other than "Invalid Destination" is found. In case an invalid destination is found, we just skip that destination number and proceed to the next destination number. The maximum you can send is 30 numbers per submission. Note: Duplicate numbers will be ignored.

**Output:**

```
OK: 760d54eb-3a82-405c-a7a7-0a0096833615, OK: 34763d84-683e-4b53-bd9f-fc9eb8c532b7
```

**Send Single SMS (Unicode) Example:**

```
https://api.easysendsms.app/bulksms?username=testuser&password=secret&from=Test&to=12345678910&text=006500610073007900730065006E00640073006D0073002E0063006F006D&type=1
```

The message has to be encoded in the UTF-16BE format and the type parameter has to be set to (type=1).

**Output:**

```
OK: 34763d84-683e-4b53-bd9f-fc9eb8c532b7
```

## API Rate Limit

To maintain a high quality of service to all customers, the [EasySendSMS API](https://easysendsms.com) applies rate limits for its SMS API. The default request rate limit is 30 requests per second per account and can reach up to 150 requests per second per IP address. (Contact our support if you wish to have that).

The API will reject all requests exceeding this rate limit with a `429 Too Many Requests` HTTP Status. You can retry the request after 1 second.

The SMPP connection rate limit can reach up to 300 to 500 SMS per sec and can be adjusted upon request. (Contact our support if you wish to have that).

## SMS Status

| Status   | Description                                                                     |
|----------|---------------------------------------------------------------------------------|
| Pending  | The message has been sent to the route and not yet received by the handset.     |
| Delivrd  | The message has been received by the handset.                                   |
| Expired  | The carrier has timed out.                                                      |
| Undeliv  | The message failed to reach the handset.                                        |

## API Error Codes

| Code | Description                                                                                             |
|------|---------------------------------------------------------------------------------------------------------|
| 1001 | Invalid URL. This means that one of the parameters was not provided or left blank.                      |
| 1002 | Invalid username or password parameter.                                                                  |
| 1003 | Invalid type parameter.                                                                                  |
| 1004 | Invalid message.                                                                                         |
| 1005 | Invalid mobile number.                                                                                   |
| 1006 | Invalid sender name.                                                                                     |
| 1007 | Insufficient credit.                                                                                     |
| 1008 | Internal error (do NOT re-submit the same message again).                                                |
| 1009 | Service not available (do NOT re-submit the same message again).                                         |





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
 $curl = curl_init();
                                                                                
 curl_setopt_array($curl, array(
 CURLOPT_URL => 'https://api.easysendsms.app/bulksms',
 CURLOPT_RETURNTRANSFER => true,
 CURLOPT_ENCODING => '',
 CURLOPT_MAXREDIRS => 10,
 CURLOPT_TIMEOUT => 0,
 CURLOPT_FOLLOWLOCATION => true,
 CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
 CURLOPT_CUSTOMREQUEST => 'POST',
 CURLOPT_POSTFIELDS => 'username=username&password=password&to=12345678900&from=test&text=Hello%20world&type=0',
 CURLOPT_HTTPHEADER => array(
 'Content-Type: application/x-www-form-urlencoded',
 'Cookie: ASPSESSIONIDASCQBARR=NKOHDCHDOFEOOALJIGDGGPAM'
 ),
 ));
                                                                                
 $response = curl_exec($curl);
                                                                                
 curl_close($curl);
 echo $response;
```


## Java
```
    Unirest.setTimeouts(0, 0);
    HttpResponse response = Unirest.post("https://api.easysendsms.app/bulksms")
    .header("Content-Type", "application/x-www-form-urlencoded")
    .header("Cookie", "ASPSESSIONIDASCQBARR=NKOHDCHDOFEOOALJIGDGGPAM")
    .field("username", "username")
    .field("password", "password")
    .field("to", "12345678900")
    .field("from", "test")
    .field("text", "Hello world")
    .field("type", "0")
    .asString();
```
