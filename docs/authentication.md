# Authentication

This tutorial demonstrates how to do authentication with RingCentral C# SDK.

This tutorial is based on the tutorial [Install RingCentral C# SDK](/installation/). If you didn't follow that tutorial, you can download its [source code](https://github.com/tylerlong/ringcentral-csharp-tutorials/tree/master/mac/installation) and start our experiment right here.

Launch Xamarin Studio

![Xamarin Studio](screenshots/xamarin-studio.png)

Press `CMD - O` to open the MyTestApp solution

![Open Test App](screenshots/open-test-app.png)

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
		private const string extension = "";
		private const string password = "password";
		private const string sandboxServer = "https://platform.devtest.ringcentral.com";

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
