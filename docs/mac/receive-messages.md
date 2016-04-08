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

First of all, we need to update the UI. We'd like to add a text view to the top, in order to display messages received. As we did before, let's edit the "Main.storyboard" file with XCode Interface Builder:

![Open Interface Builder](/screenshots/open-interface-builder.png)

Update the UI in XCode Interface Builder to make it like below:

![Interface Builder Chat UI](/screenshots/ib-chat-ui.png)

The area with dark background is where we display messages received. The area is an `UITextView`, and its outlet name is `consoleTextView`. We will reference to this name later, please keep it in your mind.

Knowledge about XCode Interface Builder and outlets is out of the scope of this tutorial. If you have difficulties with them, you'll find these videos from Youtube are quite helpful: [XCode Interface Builder](https://www.youtube.com/results?search_query=xcode+interface+builder), [Outlets & Actions](https://www.youtube.com/results?search_query=xcode+outlets+actions).

Let's move on to receive messages.

Whenever some one send you a message via RingCentral platform, the message is sent to RingCentral's server instead of your app client. Think about it: how could the app client receive the message as soon as it's available on the server? One way is to check the server again and again, we call this way "pull". For example, every 5 seconds, the app sends a message to the server: "are there any messages for me?". Most of the time, server will respond "Nope" and the app does nothing. If the server responds with "Yes", the app would send another request to the server "give me the latest message" and fetch the message back.

The solution described above is not ideal. There are too much traffic between the app and server. If there are thousands of app clients, the server will be brought down by huge amount of traffic. An better approach is to let the server to notify the app client whenever new messages are available, we call this approach "push". And here comes a new topic from RingCentral API documentation: [Notifications and Subscriptions](https://developer.ringcentral.com/api-docs/latest/index.html#!#Notifications.html).

> There are two strategies of client-service interaction providing data renewal: poll and push. Polling implies that the client periodically queries the server in order to get the updated data. Pushing implies that the server immediately sends notifications to the client on any data update. RingCentral API supports both types of data renewal. However in case of rarely changing data push notifications are evidently more effective, as they reduce client-server traffic, server load and improve user experience by notifying client applications on-the-fly with a minimal delay about important events.

To start receiving push notifications the client application should subscribe for the required events; in this case, for the new messages.

We've learned enough theory up to now. I highly recommend you to read the [API documentation](https://developer.ringcentral.com/api-docs/latest/index.html#!#Notifications.html). I cannot elaborate further in this tutorial because I don't want to duplicate the whole API documentation here. Let me show you some code instead.

```csharp
// subscribe
var sub = new RingCentral.Subscription.SubscriptionServiceImplementation() { _platform = platform };
sub.AddEvent("/restapi/v1.0/account/~/extension/~/message-store");
sub.Subscribe(ActionOnNotification, null, null);

void ActionOnNotification(object message)
{
    Console.WriteLine (message);
}
```

Code above is a typical example for how to do subscription. We've subscribed to `message-store` and whenever there are new messages, the method `ActionOnNotification` will be invoked.

Let's try the code in Xamarin Studio. Add the code to `ViewController.cs`:

![Subscribe Code](/screenshots/subscribe-code.png)

`CMD - Enter` to run the application. When it is up and running, send a message to `username` number with another account. The message will be sent to the server, and server will notify our app since we have subscribed. Wait for several seconds and watch the "Application Output" window for the notification message. It should be something like this:

```
2016-04-08 10:56:30.700 MyTestApp[4385:48725] [
  {
    "uuid": "b2f862e0-632b-4fb2-80ea-d8cbcc0418da",
    "event": "/restapi/v1.0/account/~/extension/850957020/message-store",
    "timestamp": "2016-04-08T02:56:30.564Z",
    "body": {
      "extensionId": 850957020,
      "lastUpdated": "2016-04-08T10:56:22.478+08:00",
      "changes": [
        {
          "type": "SMS",
          "newCount": 1,
          "updatedCount": 0
        }
      ]
    }
  },
  "14600841906208302",
  "4303989563124995_5e263ba5"
]
```

The SMS message itself is not included in the notification message! From the notification message, we are unable to get the content of the SMS message received. In order to get the message content, we also need one more API endpoint: `message-sync`.

`message-sync`, as its name indicates, it's used to synchronize messages between server and clients. Let me elaborate by sample code:

```csharp
var request = new RingCentral.Http.Request("/restapi/v1.0/account/~/extension/~/message-sync",
  new List<KeyValuePair<string, string>> {
    new KeyValuePair<string, string>("dateFrom", DateTime.UtcNow.AddHours(-1).ToString("o")),
    new KeyValuePair<string, string>("syncType", "FSync"),
  });
var response = platform.Get(request);
var syncToken = response.GetJson().SelectToken("syncInfo.syncToken").ToString();
```

In the code above, we sent a request to the `message-sync` endpoint together with two query string items: `dateFrom` and `syncType`.

- `dateFrom` is self-explanatory, it means the start date of the messages.
- `syncType` has at least two possible values: `FSync` and `ISync`. The former is short for "Full Synchronization", the latter is short for "Incremental Synchronization".

The last line of code: `var syncToken = response.GetJson().SelectToken("syncInfo.syncToken").ToString();` is to extract the `syncToken` from the response. `syncToken` is important because we need it to do incremental synchronization:

```csharp
var request = new RingCentral.Http.Request("/restapi/v1.0/account/~/extension/~/message-sync",
  new List<KeyValuePair<string, string>> {
    new KeyValuePair<string, string>("syncToken", syncToken),
    new KeyValuePair<string, string>("syncType", "ISync"),
  });
var response = platform.Get(request);
syncToken = response.GetJson().SelectToken("syncInfo.syncToken").ToString();

foreach(var m in ExtractMessages(response.GetJson())) {
  InvokeOnMainThread(() => {
    consoleTextView.Text += "\n" + m;
  });
}
```

In the code snippet above, we sent another request to the `message-sync` endpoint. Unlike last time, it's an incremental synchronization(Please note that `syncType` is `ISync`). We also sent the `syncToken` we saved as a query parameter. The `syncToken` is mandatory because the server needs it to know when we did the sync last time.

The new response also contains a `syncToken`, we saved it. Because next time we do incremental synchronization, we'd have to include it as a parameter to tell the server when we did the sync last time.

At the end of the code snippet, there is a `foreach` statement. We extracted all the messages from the response, and append them to the `consoleTextView` UI element.

OK, up to now you should understand the fundamentals to receiving messages. Let's go back to Xamarin Studio and finish the demo. Edit "ViewController.cs" to make it look like the following:

```csharp
using System;
using System.Collections.Generic;
using RingCentral.SDK;
using UIKit;

namespace MyTestApp
{
	public partial class ViewController : UIViewController
	{
		private string syncToken;

		public ViewController (IntPtr handle) : base (handle)
		{
		}

		private Platform platform;
		private const string appKey = "appKey";
		private const string appSecret = "appSecret";
		private const string username = "username";
		private const string extension = "extension";
		private const string password = "password";
		private const string sandboxServer = "https://platform.devtest.ringcentral.com";
		// private const string productionServer = "https://platform.ringcentral.com";

		private void Authenticate ()
		{
			if (platform == null) {
				var sdk = new SDK (appKey, appSecret, sandboxServer, "MyTestApp", "1.0.0");
				platform = sdk.GetPlatform ();
			}
			if (!platform.IsAuthorized ()) {
				platform.Authorize (username, extension, password, true);
			}
		}

		private void PrintAuthenticationStatus ()
		{
			if (platform != null && platform.IsAuthorized ()) {
				Console.WriteLine ("App is authenticated");
			} else {
				Console.WriteLine ("App is not authenticated");
			}
		}

		public override void ViewDidLoad ()
		{
			base.ViewDidLoad ();
			// Perform any additional setup after loading the view, typically from a nib.

			// do the authentication
			Authenticate ();

			// make sure authentication is successful
			PrintAuthenticationStatus ();


			// get initial syncToken
			var request = new RingCentral.SDK.Http.Request("/restapi/v1.0/account/~/extension/~/message-sync",
				new List<KeyValuePair<string, string>> {
					new KeyValuePair<string, string>("dateFrom", DateTime.UtcNow.AddHours(-1).ToString("o")),
					new KeyValuePair<string, string>("syncType", "FSync"),
				});
			var response = platform.Get(request);
			syncToken = response.GetJson().SelectToken("syncInfo.syncToken").ToString();


			// subscribe
			var sub = new RingCentral.Subscription.SubscriptionServiceImplementation () { _platform = platform };
			sub.AddEvent ("/restapi/v1.0/account/~/extension/~/message-store");
			sub.Subscribe (ActionOnNotification, null, null);
		}

		void ActionOnNotification (object message)
		{
			var request = new RingCentral.SDK.Http.Request("/restapi/v1.0/account/~/extension/~/message-sync",
				new List<KeyValuePair<string, string>> {
					new KeyValuePair<string, string>("syncToken", syncToken),
					new KeyValuePair<string, string>("syncType", "ISync"),
				});
			var response = platform.Get(request);
			syncToken = response.GetJson().SelectToken("syncInfo.syncToken").ToString();

			foreach(var m in ExtractMessages(response.GetJson())) {
				InvokeOnMainThread(() => {
					consoleTextView.Text += "\n" + m;
				});
			}
		}

		private HashSet<string> ids = new HashSet<string> ();
		private List<string> ExtractMessages(Newtonsoft.Json.Linq.JObject jo) {
			var result = new List<string> ();

			foreach (var jt in jo.SelectToken ("records")) {
				var subject = jt.SelectToken ("subject").ToString ();
				var direction = jt.SelectToken ("direction").ToString ();
				var id = jt.SelectToken ("id").ToString ();
				if (!ids.Contains (id)) {
					result.Add (string.Format ("{0} : {1}", direction, subject));
					ids.Add (id);
				}
			}
			result.Reverse ();

			return result;
		}

		public override void DidReceiveMemoryWarning ()
		{
			base.DidReceiveMemoryWarning ();
			// Release any cached data, images, etc that aren't in use.
		}

		partial void SendMessage (Foundation.NSObject sender)
		{
			var requestBody = new Dictionary<string, dynamic> {
				{ "text", messageTextField.Text },
				{ "from", new Dictionary<string, string>{ { "phoneNumber", username } } }, {
					"to",
					new List<Dictionary<string, string>> { new Dictionary<string, string> { {
								"phoneNumber",
								receiverNumberTextField.Text
							}
						}
					}
				}
			};
			var jsonBody = Newtonsoft.Json.JsonConvert.SerializeObject (requestBody);
			var request = new RingCentral.SDK.Http.Request ("/restapi/v1.0/account/~/extension/~/sms", jsonBody);
			var response = platform.Post (request);
			Console.WriteLine ("Sms sent, status code is: " + response.GetStatus ());
		}
	}
}
```

Let's run and test it. Please note that, you need two accounts to test both send and receive messages. If everything is all right, you should see something similar to this:

![Chat Testing](/screenshots/chat-testing.png)


OK, it's done. This tutorial contains quite a lot of content. If you cannot follow it, I suggest you to download the [source code](https://github.com/tylerlong/ringcentral-csharp-tutorials/tree/master/mac/receive-messages) and play with it inside Xamarin Studio.
