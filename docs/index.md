# RingCentral C# SDK


## RingCentral

[RingCentral, Inc.](http://www.ringcentral.com/) is a leading provider of flexible and cost-effective software as a service (SaaS) solutions for business
communications, including a cloud-based contact center solution and Glip, an advanced team collaboration workspace.

RingCentral provides a secure, reliable, and flexible phone system that is intuitive to use and manage,
effortlessly adapting no matter how fast your business grows and changes.


## RingCentral API

The [RingCentral API](https://developer.ringcentral.com/api-docs/latest/index.html) is based on the Representational State Transfer (REST) architecture.

One of the key advantages of REST architecture is simplicity. You can build your application on the fly without any additional tools, development kits or specifications. If you know how to interact with a simple web server, then you are ready to use the RingCentral API.

You can try RingCentral API online with [API Explorer](https://developer.ringcentral.com/api-explorer/latest/index.html).


## RingCentral SDKs

SDKs provide easy-to-use libraries to access RingCentral APIs in your language of choice. RingCentral currently provides official SDKs for Javascript, PHP, CSharp and Python.

The SDKs are built on our APIs which handle all communication with external programs, allowing you much more flexibility.


## RingCentral C# SDK

The SDK is available under an MIT-style license. The source code is hosted on GitHub: [https://github.com/ringcentral/ringcentral-csharp](https://github.com/ringcentral/ringcentral-csharp).


### We are making incompatible API changes

Please compare the [develop branch](https://github.com/ringcentral/ringcentral-csharp/tree/develop) with the [master branch](https://github.com/ringcentral/ringcentral-csharp) and see how different they are!

In master branch we maintain code base for each target platform in addition to the [PCL](https://msdn.microsoft.com/en-us/library/gg597391(v=vs.100).aspx) project. Example: `RingCentral` is PCL, `RingCentral.Android` is for Android only and `RingCentral.iOS` is for iOS only...etc.

In develop branch, we only maintain the PCL project, which means all the target platforms share the same code base. Eventually, develop branch will be merged into master branch.


### Here we talk about the develop branch

Because the develop branch installation process differs a lot from the master branch, and develop branch is the future of the C# SDK, here we only talk about the [develop branch](https://github.com/ringcentral/ringcentral-csharp/tree/develop).


## NuGet

The binary files of RingCentral C# SDK are distributed via [NuGet](http://www.nuget.org/packages/RingCentralSDK).

To install the latest stable version:

```
Install-Package RingCentralSDK
```

This will download the Ring Central Portable Class Library into your project as well as the PubNub dependencies.


To install the prerelease version:
```
Install-Package RingCentralSDK -Pre
```

This will download the prerelease version which is based on develop branch.
