---
description: "Learn more about: <wsHttpContextBinding>"
title: "<wsHttpContextBinding>"
ms.date: "03/30/2017"
ms.assetid: 1e40b5c9-0df2-4d66-afc5-a99d0e4ae7a4
---
# \<wsHttpContextBinding>

Provides a context for the <xref:System.ServiceModel.WSHttpBinding> that requires that the protection level be signed.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<wsHttpContextBinding>**  
  
## Syntax  
  
```xml  
<wsHttpContextBinding>
  <binding allowCookies="Boolean"
           bypassProxyOnLocal="Boolean"
           closeTimeout="TimeSpan"
           contextProtectionLevel="EncryptAndSign/None/Sign"
           hostNameComparisonMode="StrongWildCard/Exact/WeakWildcard"
           maxBufferPoolSize="integer"
           maxReceivedMessageSize="Integer"
           messageEncoding="Text/Mtom"
           name="string"
           openTimeout="TimeSpan"
           proxyAddress="URI"
           receiveTimeout="TimeSpan"
           sendTimeout="TimeSpan"
           textEncoding="UnicodeFffeTextEncoding/Utf16TextEncoding/Utf8TextEncoding"
           transactionFlow="Boolean"
           useDefaultWebProxy="Boolean">
    <reliableSession ordered="Boolean"
                     inactivityTimeout="TimeSpan"
                     enabled="Boolean" />
    <security mode="Message/None/Transport/TransportWithCredential">
      <transport clientCredentialType="Basic/Certificate/Digest/None/Ntlm/Windows"
                 proxyCredentialType="Basic/Digest/None/Ntlm/Windows"
                 realm="string"
                 defaultClientCredentialType="Basic/Certificate/Digest/None/Ntlm/Windows"
                 defaultProxyCredentialType="Basic/Digest/None/Ntlm/Windows"
                 defaultRealm="string" />
      <message clientCredentialType="Certificate/IssuedToken/None/UserName/Windows"
               algorithmSuite="Basic128/Basic192/Basic256/Basic128Rsa15/Basic256Rsa15/TripleDes/TripleDesRsa15/Basic128Sha256/Basic192Sha256/TripleDesSha256/Basic128Sha256Rsa15/Basic192Sha256Rsa15/Basic256Sha256Rsa15/TripleDesSha256Rsa15"
               establishSecurityContext="Boolean"
               negotiateServiceCredential="Boolean" />
    </security>
    <readerQuotas maxArrayLength="Integer"
                  maxBytesPerRead="Integer"
                  maxDepth="Integer"
                  maxNameTableCharCount="Integer"
                  maxStringContentLength="Integer" />
  </binding>
</wsHttpContextBinding>
```  
  
## Attributes and Elements  

 The following sections describe attributes, child elements, and parent elements  
  
### Attributes  
  
|Attribute|Description|  
|---------------|-----------------|  
|allowCookies|A Boolean value that indicates whether the client accepts cookies and propagates them on future requests. The default is `false`.<br /><br /> When `allowCookies` is set to `true`, contextChannel will use httpCookies as the mode of exchanging context. When this attribute is set to `false`, context is exchanged as soap headers.<br /><br /> The default value is `false`.<br /><br /> You can use this property when you interact with ASMX Web services that use cookies. In this way, you can be sure that the cookies returned from the server are automatically copied to all future client requests for that service.|  
|bypassProxyOnLocal|A Boolean value that indicates whether to bypass the proxy server for local addresses. The default is `false`.|  
|closeTimeout|A <xref:System.TimeSpan> value that specifies the interval of time provided for a close operation to complete. This value should be greater than or equal to <xref:System.TimeSpan.Zero>. The default is 00:01:00.|  
|contextProtectionLevel|A valid <xref:System.Net.Security.ProtectionLevel> value that specifies the desired protection level of the SOAP header used to propagate the context information.  The default value is `Sign`.|  
|hostnameComparisonMode|Specifies the HTTP hostname comparison mode used to parse URIs. This attribute is of type <xref:System.ServiceModel.HostNameComparisonMode>, which indicates whether the hostname is used to reach the service when matching on the URI. The default value is <xref:System.ServiceModel.HostNameComparisonMode.StrongWildcard>, which ignores the hostname in the match.|  
|maxBufferPoolSize|An integer that specifies the maximum buffer pool size for this binding. The default is 524,288 bytes (512 * 1024). Many parts of Windows Communication Foundation (WCF) use buffers. Creating and destroying buffers each time they are used is expensive, and garbage collection for buffers is also expensive. With buffer pools, you can take a buffer from the pool, use it, and return it to the pool once you are done. Thus the overhead in creating and destroying buffers is avoided.|  
|maxReceivedMessageSize|A positive integer that specifies the maximum message size, in bytes, including headers, that can be received on a channel configured with this binding. The sender of a message exceeding this limit will receive a SOAP fault. The receiver drops the message and creates an entry of the event in the trace log. The default is 65536.|  
|messageEncoding|Defines the encoder used to encode the message. Valid values include the following:<br /><br /> -   Text: Use a text message encoder.<br />-   Mtom: Use a Message Transmission Organization Mechanism 1.0 (MTOM) encoder.<br />-   The default is Text.<br /><br /> This attribute is of type <xref:System.ServiceModel.WSMessageEncoding>.|  
|name|A string that contains the configuration name of the binding. This value should be unique because it is used as an identification for the binding. Starting with .NET Framework 4, bindings and behaviors are not required to have a name. For more information about default configuration and nameless bindings and behaviors, see [Simplified Configuration](../../../wcf/simplified-configuration.md) and [Simplified Configuration for WCF Services](/previous-versions/dotnet/framework/wcf/samples/simplified-configuration-for-wcf-services).|  
|openTimeout|A <xref:System.TimeSpan> value that specifies the interval of time provided for an open operation to complete. This value should be greater than or equal to <xref:System.TimeSpan.Zero>. The default is 00:01:00.|  
|proxyAddress|A URI that specifies the address of the HTTP proxy. If `useSystemWebProxy` is `true`, this setting must be `null`. The default is `null`.|  
|receiveTimeout|A <xref:System.TimeSpan> value that specifies the interval of time provided for a receive operation to complete. This value should be greater than or equal to <xref:System.TimeSpan.Zero>. The default is 00:01:00.|  
|sendTimeout|A <xref:System.TimeSpan> value that specifies the interval of time provided for a send operation to complete. This value should be greater than or equal to <xref:System.TimeSpan.Zero>. The default is 00:01:00.|  
|textEncoding|Specifies the character set encoding to be used for emitting messages on the binding. Valid values include the following:<br /><br /> -   UnicodeFffeTextEncoding: Unicode BigEndian encoding.<br />-   Utf16TextEncoding: 16-bit encoding.<br />-   Utf8TextEncoding: 8-bit encoding.<br /><br /> The default is Utf8TextEncoding.<br /><br /> This attribute is of type <xref:System.Text.Encoding>.|  
|transactionFlow|A Boolean value that specifies whether the binding supports flowing WS-Transactions. The default is `false`.|  
|useDefaultWebProxy|A Boolean value that specifies whether the system’s auto-configured HTTP proxy is used. The default is `true`.|  
  
### Child Elements  
  
|Element|Description|  
|-------------|-----------------|  
|[\<security>](security-of-wshttpbinding.md)|Defines the security settings for the binding. This element is of type <xref:System.ServiceModel.Configuration.WSHttpSecurityElement>.|  
|[\<readerQuotas>](/previous-versions/dotnet/netframework-4.0/ms731325(v=vs.100))|Defines the constraints on the complexity of SOAP messages that can be processed by endpoints configured with this binding. This element is of type <xref:System.ServiceModel.Configuration.XmlDictionaryReaderQuotasElement>.|  
|[\<reliableSession>](/previous-versions/ms731375(v=vs.90))|Specifies if reliable sessions are established between channel endpoints.|  
  
### Parent Elements  
  
|Element|Description|  
|-------------|-----------------|  
|[\<bindings>](bindings.md)|This element holds a collection of standard and custom bindings.|  
  
## See also

- <xref:System.ServiceModel.WSHttpBinding>
- <xref:System.ServiceModel.WSHttpContextBinding>
- <xref:System.ServiceModel.Configuration.WSHttpContextBindingElement>
- <xref:System.ServiceModel.Channels.ContextBindingElement>
- [Bindings](../../../wcf/bindings.md)
- [Configuring System-Provided Bindings](../../../wcf/feature-details/configuring-system-provided-bindings.md)
- [Using Bindings to Configure Services and Clients](../../../wcf/using-bindings-to-configure-services-and-clients.md)
- [\<binding>](bindings.md)
- [\<wsHttpBinding>](wshttpbinding.md)
