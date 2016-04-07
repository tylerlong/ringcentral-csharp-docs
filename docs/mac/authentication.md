# Authentication

This tutorial demonstrates how to do authentication with RingCentral C# SDK.

This tutorial is based on the tutorial [Install RingCentral C# SDK](/mac/installation/). If you didn't follow that tutorial, you can download its [source code](https://github.com/tylerlong/ringcentral-csharp-tutorials/tree/master/mac/installation) and start our experiment right here.

Launch Xamarin Studio

![Xamarin Studio](/screenshots/xamarin-studio.png)

Press `CMD - O` to open the MyTestApp solution created in [Install RingCentral C# SDK](/mac/installation/) tutorial.

![Open Test App](/screenshots/open-test-app.png)

Open "ViewController.cs", Edit it and make the code like this:

```csharp
using System;
using RingCentral.SDK;
using UIKit;

namespace MyTestApp
{
	public partial class ViewController : UIViewController
	{
		public ViewController (IntPtr handle) : base (handle)
		{
		}

		private Platform platform;
		private const string appKey = "appKey";
		private const string appSecret = "appSecret";
		private const string username = "username";
		private const string extension = "extension";
		private const string password = "password";
		private const string sandboxServer = "https://platform.devtest.ringcentral.com";
    // private const string productionServer = "https://platform.ringcentral.com";

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
		}

		public override void DidReceiveMemoryWarning ()
		{
			base.DidReceiveMemoryWarning ();
			// Release any cached data, images, etc that aren't in use.
		}
	}
}
```

Please do replace `appKey`, `appSecret`, `username`, `extension` and `password` with real values.

Code above looks a little long. Most of them are just boilerplate code. Which we shoud pay attention to is:

```csharp
if (platform == null) {
	var sdk = new SDK (appKey, appSecret, sandboxServer, "MyTestApp", "1.0.0");
	platform = sdk.GetPlatform ();
}
if (!platform.IsAuthorized ()) {
	platform.Authorize (username, extension, password, true);
}
```

The first `if` statement create the SDK object and get the platform singleton.
The second `if` statement does the authentication.

Press `CMD - Enter` to run the app. A simulator will be started. Wait until the app is up and running, check "Application Output" window for text "App is authenticated".

![App Authenticated](/screenshots/app-authenticated.png)

The app itself in simulator is just a blank window because we didn't create any UI elements.

If the Application Output window is not already open, please click the following in Xamarin Studio's menu: "View -> Pads -> Application Output".

Please confirm that "App is authenticated" is printed to the Application Output window, which means that we have done the authentication correctly and successfully.

The [source code](https://github.com/tylerlong/ringcentral-csharp-tutorials/tree/master/mac/authentication) of this tutorial is available for downloading.
