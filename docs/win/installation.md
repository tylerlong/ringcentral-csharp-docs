# Install RingCentral C# SDK

This tutorial demonstrates how to create a new project and install the latest RingCentral C# SDK.

<video width="640" height="400" controls>
  <source src="../../videos/win-ringcentral-nuget.mp4" type="video/mp4">
Your browser does not support the video tag.
</video>

As the video demonstrated, the recommended way is to install RingCentral SDK is via [NuGet](http://www.nuget.org/packages/RingCentralSDK):

```
Install-Package RingCentralSDK -Pre
```

We installed the prerelease version (`-Pre`). This is because the prerelease version contains incompatible major changes to the stable release. We want to stay on the cutting edge.

As soon as we release RingCentral C# SDK 1.0.0, you will be recommended to install the stable release instead. Please keep this in your mind.

We added a line of code to the C# project:

```csharp
private RingCentral.SDK.Platform platform = null;
```

The code above compiled successfully, which proved that we have installed the RingCentral C# SDK via NuGet successfully.
