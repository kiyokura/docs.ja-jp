---
title: "構成ファイルにおける探索の構成 | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: b9884c11-8011-4763-bc2c-c526b80175d0
caps.latest.revision: 7
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 7
---
# 構成ファイルにおける探索の構成
探索で使用される構成設定は、4 つの主なグループに分類されます。  このトピックでは、各グループについて簡単に説明し、各グループの構成方法の例を紹介します。  以下の各セクションは、各領域についてのより詳細なドキュメントにリンクされます。  
  
## 動作の構成  
 探索では、サービスの動作とエンドポイントの動作が使用されます。  <xref:System.ServiceModel.Discovery.ServiceDiscoveryBehavior> 動作により、サービスのすべてのエンドポイントの探索が有効になるだけでなく、アナウンス エンドポイントの指定が可能になります。  次の例は、<xref:System.ServiceModel.Discovery.ServiceDiscoveryBehavior> を追加し、アナウンス エンドポイントを指定する方法を示しています。  
  
```  
<behaviors>  
      <serviceBehaviors>  
        <behavior name="helloWorldServiceBehavior">  
          <serviceDiscovery>  
            <announcementEndpoints>  
              <endpoint kind="udpAnnouncementEndpoint"/>  
            </announcementEndpoints>  
          </serviceDiscovery>  
        </behavior>  
      </serviceBehaviors>  
```  
  
 動作を指定したら、これを \<`service`\> 要素から参照します。次にその例を示します。  
  
```  
<system.serviceModel>  
   <services>  
      <service name="HelloWorldService" behaviorConfiguration="helloWorldServiceBehavior">  
         <!-- Application Endpoint -->  
         <endpoint address="endpoint0"  
                   binding="basicHttpBinding"  
                   contract="IHelloWorldService" />  
         <!-- Discovery Endpoints -->  
         <endpoint kind="udpDiscoveryEndpoint" />  
        </service>  
    </service>  
```  
  
 サービスを探索可能にするには、探索エンドポイントを追加する必要もあります。上の例では、<xref:System.ServiceModel.Discovery.UdpDiscoveryEndpoint> 標準エンドポイントを追加しています。  
  
 アナウンス エンドポイントを追加する場合は、アナウンス リスナー サービスを \<`services`\> 要素に追加する必要もあります。次にその例を示します。  
  
```  
<services>  
   <service name="HelloWorldService" behaviorConfiguration="helloWorldServiceBehavior">  
      <!-- Application Endpoint -->  
      <endpoint address="endpoint0"  
                binding="basicHttpBinding"  
                contract="IHelloWorldService" />  
      <!-- Discovery Endpoints -->  
      <endpoint kind="udpDiscoveryEndpoint" />  
   </service>  
   <!-- Announcement Listener Configuration -->  
   <service name="AnnouncementListener">  
      <endpoint kind="udpAnnouncementEndpoint" />  
   </service>  
```  
  
 <xref:System.ServiceModel.Discovery.EndpointDiscoveryBehavior> 動作は、特定のエンドポイントの探索を有効または無効にするために使用されます。  次の例では、サービスに 2 つのアプリケーション エンドポイントを構成します。1 つのエンドポイントでは探索を有効し、もう 1 つでは探索を無効にします。  それぞれのエンドポイントには <xref:System.ServiceModel.Discovery.EndpointDiscoveryBehavior> 動作が追加されます。  
  
```  
<system.serviceModel>  
   <services>  
      <service name="HelloWorldService"  
               behaviorConfiguration="helloWorldServiceBehavior">  
  
        <!-- Application Endpoints -->  
        <endpoint address="endpoint0"  
                 binding="basicHttpBinding"  
                 contract="IHelloWorldService"   
                 behaviorConfiguration="ep0Behavior" />  
  
        <endpoint address="endpoint1"  
                  binding="basicHttpBinding"  
                  contract="IHelloWorldService"  
                  behaviorConfiguration="ep1Behavior" />  
  
        <!-- Discovery Endpoints -->  
        <endpoint kind="udpDiscoveryEndpoint" />  
      </service>  
   </services>  
    <behaviors>  
      <serviceBehaviors>  
        <behavior name="helloWorldServiceBehavior">  
          <serviceDiscovery />  
        </behavior>  
      </serviceBehaviors>  
      <endpointBehaviors>  
        <behavior name="ep0Behavior">  
          <endpointDiscovery enabled="true"/>  
        </behavior>  
        <behavior name="ep1Behavior">  
          <endpointDiscovery enabled="false"/>  
        </behavior>  
     </endpointBehaviors>  
   </behaviors>  
  
```  
  
 <xref:System.ServiceModel.Discovery.EndpointBehavior> 動作を使用すると、サービスから返されるエンドポイント メタデータにカスタム メタデータを追加することもできます。  その方法を次の例に示します。  
  
```  
<behavior name="ep4Behavior">  
   <endpointDiscovery enabled="true">  
      <extensions>  
         <CustomMetadata>This is custom metadata.</CustomMetadata>  
         <d:Types xmlns:d="http://schemas.xmlsoap.org/ws/2005/04/discovery" xmlns:i="http://printer.example.org/2003/imaging">i:PrintBasic</d:Types>  
         <CustomMetadata netsted="true">  
            <NestedMetadata>This is a nested custom metadata.</NestedMetadata>  
         </CustomMetadata>  
      </extensions>  
   </endpointDiscovery>  
</behavior>  
```  
  
 <xref:System.ServiceModel.Discovery.EndpointDiscoveryBehavior> 動作を使用すると、クライアントがサービスの検索に使用するスコープと型を追加することもできます。  クライアント側の構成ファイルでこの構成を行う方法を次の例に示します。  
  
```  
<behavior name="ep2Behavior">  
   <endpointDiscovery enabled="true">  
      <scopes>  
         <add scope="http://www.microsoft.com/building42/floor1"/>  
         <add scope="ldap:///ou=engineeringo=examplecomc=us"/>  
      </scopes>  
      <types>  
         <add name="test" namespace="http://example.microsoft.com/" />  
         <add name="additionalContract" namespace="http://example.microsoft.com/" />  
      </types>  
   </endpointDiscovery>  
</behavior>  
```  
  
 [!INCLUDE[crabout](../../../../includes/crabout-md.md)] <xref:System.ServiceModel.Discovery.ServiceDiscoveryBehavior> および <xref:System.ServiceModel.Discovery.EndpointDiscoveryBehavior> については、「[WCF Discovery の概要](../../../../docs/framework/wcf/feature-details/wcf-discovery-overview.md)」を参照してください。  
  
## バインディング要素の構成  
 バインディング要素の構成は、クライアント側で最も興味深い構成です。  構成を使用して、WCF クライアント アプリケーションからのサービスの探索に使用する検索条件を指定できます。  次の例では、<xref:System.ServiceModel.Discovery.DiscoveryClient> チャネルとのカスタム バインディングを作成し、型とスコープを含む検索条件を指定しています。  また、<xref:System.ServiceModel.Discovery.FindCritera.Duration%2A> プロパティと <xref:System.ServiceModel.Discovery.FindCriteria.MaxResults%2A> プロパティの値も指定しています。  
  
```  
<bindings>  
   <customBinding>  
      <!-- Binding Configuration for the Application Endpoint -->  
      <binding name="discoBindingConfiguration">  
         <discoveryClient>  
            <endpoint kind="discoveryEndpoint"  
                      address="http://localhost:8000/ConfigTest/Discovery"  
                      binding="customBinding"  
                      bindingConfiguration="httpSoap12WSAddressing10"/>  
            <findCriteria duration="00:00:10" maxResults="2">  
              <types>  
                <add name="IHelloWorldService"/>  
              </types>  
              <scopes>  
                <add scope="http://www.microsoft.com/building42/floor1"/>  
              </scopes>              
            </findCriteria>  
          </discoveryClient>  
          <textMessageEncoding messageVersion="Soap11"/>  
          <httpTransport />  
        </binding>  
```  
  
 このカスタム バインディング構成は、クライアント エンドポイントから参照される必要があります。  
  
```  
<client>  
      <endpoint address="http://schemas.microsoft.com/discovery/dynamic"  
                binding="customBinding"  
                bindingConfiguration="discoBindingConfiguration"  
                contract="IHelloWorldService" />  
    </client>  
```  
  
 検索条件[!INCLUDE[crabout](../../../../includes/crabout-md.md)]、「[探索検索と FindCriteria](../../../../docs/framework/wcf/feature-details/discovery-find-and-findcriteria.md)」を参照してください。  [!INCLUDE[crabout](../../../../includes/crabout-md.md)] 探索およびバインディング要素については、「[WCF Discovery の概要](../../../../docs/framework/wcf/feature-details/wcf-discovery-overview.md)」を参照してください。  
  
## 標準エンドポイントの構成  
 標準エンドポイントは定義済みのエンドポイントで、これには、1 つ以上のプロパティ \(アドレス、バインディング、またはコントラクト\) の既定値、または、変更できない 1 つ以上のプロパティ値が設定されています。  .NET 4 には、<xref:System.ServiceModel.Discovery.UdpDiscoveryEndpoint>、<xref:System.ServiceModel.Discovery.UpdAnnouncementEndpoint>、および <xref:System.ServiceModel.Discovery.DynamicEndpoint> という 3 種類の探索関連の標準エンドポイントが用意されています。  <xref:System.ServiceModel.Discovery.UdpDiscoveryEndpoint> は、UDP マルチキャスト バインディングを使用した探索操作用に事前に構成されている標準エンドポイントです。  <xref:System.ServiceModel.Discovery.UdpAnnouncementEndpoint> は、UDP バインディングを使用したアナウンスの送信用に事前に構成されている標準エンドポイントです。  <xref:System.ServiceModel.Discovery.DynamicEnpoint> は、実行時に探索対象のサービスのエンドポイント アドレスを動的に検索するために探索が使用する標準エンドポイントです。  標準のバインディングは、追加する標準エンドポイントの種類を指定した種類属性を含む \<`endpoint`\> 要素を使用して、指定されます。  <xref:System.ServiceModel.Discovery.UdpDiscoveryEndpoint> および <xref:System.ServiceModel.Discovery.UpdAnnouncementEndpoint> を追加する方法を次の例に示します。  
  
```  
<services>  
   <service name="HelloWorldService">  
      <!-- ...  -->          
      <endpoint kind="udpDiscoveryEndpoint" />  
   </service>  
   <service name="AnnouncementListener">  
      <endpoint kind="udpAnnouncementEndpoint" />  
   </service>  
</services>  
```  
  
 標準エンドポイントは、\<`standardEndpoints`\> 要素で構成します。  <xref:System.ServiceModel.Discovery.UdpDiscoveryEndpoint> および <xref:System.ServiceModel.Discovery.UdpAnnouncementEndpoint> を構成する方法を次の例に示します。  
  
```  
<standardEndpoints>  
      <udpAnnouncementEndpoint>  
        <standardEndpoint   
            name="udpAnnouncementEndpointSettings"   
            multicastAddress="soap.udp://239.255.255.250:3703"    
            maxAnnouncementDelay="00:00:00.800">  
          <transportSettings  
            duplicateMessageHistoryLength="1028"  
            maxPendingMessageCount="10"  
            maxMulticastRetransmitCount="3"  
            maxUnicastRetransmitCount="2"  
            socketReceiveBufferSize="131072"  
            timeToLive="2" />  
        </standardEndpoint>  
      </udpAnnouncementEndpoint>  
      <udpDiscoveryEndpoint>  
        <standardEndpoint  
            name="udpDiscoveryEndpointSettings"  
            multicastAddress="soap.udp://239.255.255.252:3704"  
            maxResponseDelay="00:00:00.800">  
          <transportSettings  
            duplicateMessageHistoryLength="2048"  
            maxPendingMessageCount="5"  
            maxReceivedMessageSize="8192"  
            maxBufferPoolSize="262144"/>  
        </standardEndpoint>  
      </udpDiscoveryEndpoint>  
```  
  
 標準エンドポイント構成を追加したら、各エンドポイントの \<`endpoint`\> 要素でこの構成を参照します。次にその例を示します。  
  
```  
<services>  
   <service name="HelloWorldService">  
      <!-- ...  -->          
      <endpoint kind="udpDiscoveryEndpoint" endpointConfiguration="udpDiscoveryEndpointSettings"/>  
   </service>  
   <service name="AnnouncementListener">  
      <endpoint kind="udpAnnouncementEndpoint" endpointConfiguration="udpAnnouncementEndpointSettings" >  
   </service>  
</services>  
```  
  
 探索で使用されるその他の標準エンドポイントとは異なり、<xref:System.ServiceModel.Discovery.DynamicEndpoint> にはバインディングとコントラクトを指定します。  <xref:System.ServiceModel.Discovery.DynamicEndpoint> を追加し、構成する方法を次の例に示します。  
  
```  
<system.serviceModel>  
    <client>  
      <endpoint kind="dynamicEndpoint" binding="basicHttpBinding" contract="IHelloWorldService" endpointConfiguration="dynamicEndpointConfiguration" />  
    </client>   
   <standardEndpoints>  
      <dynamicEndpoint>  
         <standardEndpoint name="dynamicEndpointConfiguration">  
             <discoveryClientSettings>  
              <findCriteria scopeMatchBy="http://schemas.microsoft.com/ws/2008/06/discovery/rfc" duration="00:00:10" maxResults="2">  
                 <types>  
                   <add name="IHelloWorldService"/>  
                 </types>  
                 <scopes>  
                   <add scope="http://www.microsoft.com/building42/floor1"/>  
                 </scopes>  
                 <extensions>  
                   <CustomMetadata>This is custom metadata.</CustomMetadata>          
                 </extensions>  
               </findCriteria>  
             </discoveryClientSettings>  
           </standardEndpoint>  
         </dynamicEndpoint>  
   </standardEndpoints>  
</system.ServiceModel>  
```  
  
 [!INCLUDE[crabout](../../../../includes/crabout-md.md)] 標準エンドポイント、「」を参照してください。 [標準エンドポイント](../../../../docs/framework/wcf/feature-details/standard-endpoints.md)