---
description: "Learn more about: Internet Information Services Hosting Best Practices"
title: "Internet Information Services Hosting Best Practices"
ms.date: "03/30/2017"
ms.assetid: 0834768e-9665-46bf-86eb-d4b09ab91af5
---
# Internet Information Services Hosting Best Practices

This topic outlines some best practices for hosting Windows Communication Foundation (WCF) services.  
  
## Implementing WCF Services as DLLs  

 Implementing a WCF service as a DLL that is deployed to the \bin directory of a Web application allows you reuse the service outside of the Web application model, for example, in a test environment that may not have Internet Information Services (IIS) deployed.  
  
## Service Hosts in IIS-Hosted Applications  

 Do not use the imperative self-host APIs to create new service hosts that listen on network transports not natively supported by the IIS hosting environment (For example, IIS 6.0 to host TCP services, because TCP communication is not natively supported on IIS 6.0). This approach is not recommended. Service hosts created imperatively are not known within the IIS hosting environment. The critical point is that processing done by imperatively created services is not accounted for by IIS when it determines whether the hosting application pool is idle. The result is that applications that have such imperatively created service hosts have an IIS hosting environment that aggressively disposes of IIS host processes.  
  
## URIs and IIS-Hosted Endpoints  

 Endpoints for an IIS-hosted service should be configured using relative Uniform Resource Identifiers (URIs), not absolute addresses. This guarantees that the endpoint address falls within the set of URI addresses that belong to the hosting application and ensures that message-based activation happens as expected.  
  
## State Management and Process Recycling  

 The IIS hosting environment is optimized for services that do not maintain local state in memory. IIS recycles the host process in response to a variety of external and internal events, causing any volatile state stored exclusively in memory to be lost. Services hosted in IIS should store their state external to the process (for example, in a database) or in an in-memory cache that can easily be re-created if an application recycle event occurs.  
  
> [!NOTE]
> The protocols WCF uses for message-layer reliability and security make use of the volatile in-memory state. WCF reliable sessions and security sessions may terminate unexpectedly due to application recycles. IIS-hosted applications that make use of these protocols should either depend on something other than the WCF-provided session key for correlating application-layer state (for example, an application-layer construct or custom correlation header) or disable IIS process recycling for the hosted application.  
  
## Optimizing Performance in Middle-Tier Scenarios  

 For optimal performance in a *middle-tier scenario*—a service that calls out to other services in response to incoming messages—instantiate the WCF service client to the remote service once and reuse it across multiple incoming requests. Instantiating WCF service clients is an expensive operation relative to making a service call on a pre-existing client instance, and middle-tier scenarios produce distinct performance gains by caching remote clients across requests. WCF service clients are thread-safe, so it is not necessary to synchronize access to a client across multiple threads.  
  
 Middle-tier scenarios also produce performance gains by using the asynchronous APIs generated by the `svcutil /a` option. The `/a` option causes the [ServiceModel Metadata Utility Tool (Svcutil.exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md) to generate `BeginXXX/EndXXX` methods for each service operation, which allows potentially long-running calls to remote services to be made on background threads.  
  
## WCF in Multi-Homed or Multi-named scenarios  

 You can deploy WCF services inside of an IIS Web farm, where a set of computers share a common external name (such as `http://www.contoso.com`) but are individually addressed by different hostnames (for example, `http://www.contoso.com` might direct traffic to two different machines named `http://machine1.internal.contoso.com` and `http://machine2.internal.contoso.com`). This deployment scenario is fully supported by WCF, but requires special configuration of the IIS Web site hosting WCF services to display the correct (external) hostname in the service's metadata (Web Services Description Language).  
  
 To ensure that the correct hostname appears in the service metadata WCF generates, configure the default identity for the IIS Web site that hosts WCF services to use an explicit hostname. For example, computers that reside inside of the `www.contoso.com` farm should use an IIS site binding of *:80:www.contoso.com for HTTP and \*:443:www.contoso.com for HTTPS.  
  
 You can configure IIS Web site bindings by using the IIS Microsoft Management Console (MMC) snap-in.  
  
## Application Pools Running in Different User Contexts Overwrite Assemblies from Other Accounts in the Temporary Folder  

 To ensure that application pools running in different user contexts cannot overwrite assemblies from other accounts in the temporary ASP.NET files folder, use different identities and temporary folders for different applications. For example, if you have two virtual applications /Application1 and / Application2, you can create two Application pools, A and B, with two different identities. Application pool A can run under one user identity (user1) while application pool B can run under another user identity (user2), and configure /Application1 to use A and /Application2 to use B.  
  
 In Web.config, you can configure the temporary folder using \<system.web/compilation/@tempFolder>. For /Application1, it can be "c:\tempForUser1" and for application2 it can be "c:\tempForUser2". Grant corresponding write permission to these folders for the two identities.  
  
 Then user2 cannot change the code-generation folder for /application2 (under c:\tempForUser1).  
  
## Enabling asynchronous processing  

 By default messages sent to a WCF service hosted under IIS 6.0 and earlier are processed in a synchronous manner. ASP.NET calls into WCF on its own thread (the ASP.NET worker thread) and WCF uses another thread to process the request. WCF holds onto the ASP.NET worker thread until it completes its processing. This leads to synchronous processing of requests. Processing requests asynchronously enables greater scalability because it reduces the number of threads required to process a request –WCF does not hold on to the ASP.NET thread while processing the request. Use of asynchronous behavior is not recommended for machines running IIS 6.0 because there is no way to throttle incoming requests that open up the server to *Denial Of Service* (DOS) attacks. Starting with IIS 7.0, a concurrent request throttle has been introduced: `[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\ASP.NET\2.0.50727.0]"MaxConcurrentRequestsPerCpu`. With this new throttle it is safe to use the asynchronous processing.  By default in IIS 7.0, the asynchronous handler and module are registered. If this has been turned off, you can manually enable asynchronous processing of requests in your application's Web.config file. The settings you use depend on your `aspNetCompatibilityEnabled` setting. If you have `aspNetCompatibilityEnabled` set to `false`, configure the `System.ServiceModel.Activation.ServiceHttpModule` as shown in the following configuration snippet.  
  
```xml  
<system.serviceModel>  
    <serviceHostingEnvironment aspNetCompatibilityEnabled="false" />
  </system.serviceModel>  
  <system.webServer>  
    <modules>  
      <remove name="ServiceModel"/>  
      <add name="ServiceModel"
           preCondition="integratedMode,runtimeVersionv2.0"
           type="System.ServiceModel.Activation.ServiceHttpModule, System.ServiceModel,Version=3.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089"/>  
    </modules>  
    </system.webServer>  
```  
  
 If you have `aspNetCompatibilityEnabled` set to `true`, configure the `System.ServiceModel.Activation.ServiceHttpHandlerFactory` as shown in the following config snippet.  
  
```xml  
<system.serviceModel>  
    <serviceHostingEnvironment aspNetCompatibilityEnabled="true" />
  </system.serviceModel>  
  <system.webServer>  
    <handlers>  
          <clear/>  
          <add name="TestAsyncHttpHandler"
               path="*.svc"
               verb="*"
               type="System.ServiceModel.Activation.ServiceHttpHandlerFactory, System.ServiceModel, Version=3.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089"
               />  
    </handlers>
  </system.webServer>  
```  
  
## See also

- [Service Hosting Samples](/previous-versions/dotnet/framework/wcf/samples/hosting)
- [Windows Server App Fabric Hosting Features](/previous-versions/appfabric/ee677189(v=azure.10))
