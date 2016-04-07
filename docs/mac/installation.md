# Install RingCentral C# SDK

This tutorial demonstrates how to create a new project and install the latest RingCentral C# SDK.

Launch Xamarin Studio

![Xamarin Studio](screenshots/xamarin-studio.png)

Press keyboard shortcut: `Shift - CMD - N` to create a new solution

![New Solution](screenshots/new-solution.png)

Choose "iOS -> App -> Single View App", click "Next"

![iOS Single View App](screenshots/ios-single-view-app.png)

Here we take an iOS app for example.
The method demonstrated here should also work for Android apps and Mac apps

Name the app as `MyTestApp` , keep other settings as default, click "Next"

![Create Test App](screenshots/create-test-app-1.png)

Keep all the settings as default, click "Create"

![Create Test App](screenshots/create-test-app-2.png)

Test app created

![Test App Created](screenshots/test-app-created.png)

From Xamarin Studio menu, select "Project -> Add NuGet Packages..."

![NuGet Gallery](screenshots/nuget-gallery.png)

Search for "RingCentral", be sure to check "Show pre-release packages", check "RingCentral SDK", click "Add Package"

![NuGet RingCentral SDK](screenshots/nuget-ringcentral-sdk.png)

Here we checked "Show pre-release packages" because we want to install the latest pre-release version. In the future, we'll merge all the latest features in to the stable release, by then there is no need to install pre-release version.

Accept the license of PubnubPCL

![PubnubPCL License](screenshots/pubnubpcl-license.png)

Add `using RingCentral.SDK;` to `ViewController.cs`

![Testing Code](screenshots/one-line-testing-code.png)

Press `CMD - B` to build the project.

![Build Successful](screenshots/build-successful.png)

"Build successful." means RingCentral SDK has been installed successfully.

Download the [source code](https://github.com/tylerlong/ringcentral-csharp-tutorials/tree/master/mac/installation) for this tutorial.
