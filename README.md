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
