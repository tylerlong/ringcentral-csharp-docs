# Send SMS

This tutorial demonstrates how to send SMS with RingCentral C# SDK.

This tutorial is based on the tutorial [Authentication](/mac/authentication/). If you didn't follow that tutorial, you can download its [source code](https://github.com/tylerlong/ringcentral-csharp-tutorials/tree/master/mac/authentication) and start our experiment right here.


## Requirements

In order to complete this tutorial. You need two RingCentral accounts. One for sending SMS, the other for receiving SMS. You should also install the [RingCentral app](https://developer.ringcentral.com/app-gallery.html#/apps), and logon with the account which is for receiving message.

If you only have one account, or you don't want to install the RingCentral app, you can still continue to experiment this tutorial. But you won't be able to tell whether the receiver has received the message.


## Experiment

Launch Xamarin Studio

![Xamarin Studio](/screenshots/xamarin-studio.png)

Press `CMD - O` to open the MyTestApp solution created in [Authentication](/mac/authentication/) tutorial.

![Open Test App](/screenshots/open-test-app.png)

First of all, make sure to replace the following with real credentials

![Fake Credentials](/screenshots/fake-credentials.png)

In the last tutorial, when the app is up and running, what we get is just a blank window, which is not cool.
In this tutorial, we are going to create some UI elements.
Select "Main.storyboard", right click and choose "Open With -> XCode Interface Builder"

![Open Interface Builder](/screenshots/open-interface-builder.png)

Create the following UI in XCode Interface Builder.

![Send Message UI](/screenshots/send-message-ui.png)

How to build UI with XCode Interface Builder is out of the scope of this tutorial. There are some great videos about XCode Interface Builder from [Youtube](https://www.youtube.com/results?search_query=xcode+interface+builder).

Still in XCode Interface Builder, Create outlets for the two text fields, create an action for the button. Please name the two outlets as `receiverNumberTextField` and `messageTextField`. Please name the action as `SendMessage`. The resulting Objective-C code should be like following:

![Outlets & Action](/screenshots/send-message-oc.png)

How to create outlets and actions in XCode Interface Builder is out of the scope of this tutorial. There are some wonderful videos from [Youtube](https://www.youtube.com/results?search_query=xcode+outlets+actions).

Now go back to Xamarin Studio. And review the source code of `ViewController.designer.cs`. There should be some code automatically generated.

![Auto Generated Code](/screenshots/auto-generated-code.png)

Let's edit `ViewController.cs`, add the following code:

```csharp
partial void SendMessage (Foundation.NSObject sender)
{
    Console.WriteLine("Button clicked");
}
```

Now `CMD - Return` to run the app.
