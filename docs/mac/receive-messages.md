# Receive Messages

This tutorial demonstrates how to receive messages with RingCentral C# SDK.

This tutorial is based on the tutorial [Send SMS](/mac/send-sms/). If you didn't follow that tutorial, you can download its [source code](https://github.com/tylerlong/ringcentral-csharp-tutorials/tree/master/mac/send-sms) and start our experiment right here.


## Requirements

In order to complete this tutorial. You need two RingCentral accounts. One for sending messages, the other for receiving messages. You should also install the [RingCentral app](https://developer.ringcentral.com/app-gallery.html#/apps), and logon with the account which is for sending message. Or you can run the code we created in [Send SMS](/mac/send-sms/) tutorial to send messages.


## Experiment

Launch Xamarin Studio

![Xamarin Studio](/screenshots/xamarin-studio.png)

Press `CMD - O` to open the MyTestApp solution created in [Send SMS](/mac/send-sms/) tutorial.

![Open Test App](/screenshots/open-test-app.png)

First of all, make sure to replace the following with real credentials

![Fake Credentials](/screenshots/fake-credentials.png)

In previous tutorial, we created simple UI and wrote some code to send SMS. In this tutorial we are going to experimenting how to receive messages.

Whenever some one send you a message via RingCentral platform, the message is sent to RingCentral's server instead of your app client. Think about it: how could the app client receive the message as soon as it's available on the server? One way is to check the server again and again. For example, every 5 seconds, the app sends a message to the server: "are there any messages for me?". Most of the time, server will respond "Nope" and the app does nothing. If the server responds with "Yes", the app would send another request the server "give me the latest message" and fetch the message back.

The solution described above is not ideal. There are too much traffic between the app and server. If there are thousands of app clients, the server will be brought down by huge amount of traffic. An better approach is to let the server to notify the app client whenever new messages are available. And here comes a new topic from RingCentral API documentation: [Notifications and Subscriptions](https://developer.ringcentral.com/api-docs/latest/index.html#!#Notifications.html).

> There are two strategies of client-service interaction providing data renewal: poll and push. Polling implies that the client periodically queries the server in order to get the updated data. Pushing implies that the server immediately sends notifications to the client on any data update. RingCentral API supports both types of data renewal. However in case of rarely changing data push notifications are evidently more effective, as they reduce client-server traffic, server load and improve user experience by notifying client applications on-the-fly with a minimal delay about important events.





[Source code](https://github.com/tylerlong/ringcentral-csharp-tutorials/tree/master/mac/receive-messages) for this tutorial is available.
