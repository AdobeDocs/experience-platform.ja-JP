---
description: このページでは、Destination SDK を使用してストリーミングの宛先を設定する手順について説明します。
title: Destination SDK を使用したストリーミングの宛先の設定
exl-id: d8aa7353-ba55-4a0d-81c4-ea2762387638
source-git-commit: 804370a778a4334603f3235df94edaa91b650223
workflow-type: tm+mt
source-wordcount: '865'
ht-degree: 58%

---

# Destination SDK を使用したストリーミングの宛先の設定

## 概要 {#overview}

このページでは、[Destination SDK の設定オプション](../functionality/configuration-options.md)や、他の Destination SDK 機能と API リファレンスのドキュメントに記載されている情報の利用方して[ストリーミング宛先](../../destination-types.md#streaming-destinations)を設定する方法について説明します。以下にその手順を、順を追って説明します。

## 前提条件 {#prerequisites}

以下に示す手順に進む前に、[Destination SDKの概要 ](../getting-started.md) ページを参照して、Destination SDK API を使用するために必要なAdobe I/O認証資格情報およびその他の前提条件について確認してください。 ここでは、パートナーシップと権限の前提条件を満たし、宛先開発を開始する準備が整っていることを前提としています。

## Destination SDK の構成オプションを使用して宛先を設定する手順 {#steps}

![Destination SDK エンドポイントの使用手順の図解](../assets/guides/destination-sdk-steps.png)

## 手順 1：サーバーとテンプレートの構成を作成する {#create-server-template-configuration}

まず、`/destinations-server` エンドポイントを使用して [ サーバーとテンプレートの設定を作成 ](../authoring-api/destination-server/create-destination-server.md) します。

次に構成の例を示します。 `requestBody.value` パラメーターでのメッセージ変換テンプレートは、手順 3 の[変換テンプレートの作成](#create-transformation-template)で対応します。

```shell
POST platform.adobe.io/data/core/activation/authoring/destination-servers
```

```json {line-numbers="true" highlight="14"}
{
   "name":"Moviestar destination server",
   "destinationServerType":"URL_BASED",
   "urlBasedDestination":{
      "url":{
         "templatingStrategy":"PEBBLE_V1",
         "value":"https://api.moviestar.com/data/{{customerData.region}}/items"
      }
   },
   "httpTemplate":{
      "httpMethod":"POST",
      "requestBody":{
         "templatingStrategy":"PEBBLE_V1",
         "value":"insert after you create a template in step 3"
      },
      "contentType":"application/json"
   }
}
```

## 手順 2：宛先の構成の作成 {#create-destination-configuration}

以下に、`/destinations` API エンドポイントを使用して作成された、宛先テンプレートの構成の例を示します。 詳しくは、[ 宛先設定の作成 ](../authoring-api/destination-configuration/create-destination-configuration.md) を参照してください。

手順 1 のサーバーとテンプレート設定をこの宛先構成に接続するには、サーバーのインスタンス ID とテンプレート設定を、`destinationServerId` として追加します。

>[!IMPORTANT]
>
>正しく設定されたリアルタイム（ストリーミング）宛先を作成するには、次に示すように *少なくとも 1 つのターゲット ID を `identityNamespaces` に追加する* 必要があります）。 ターゲット ID が設定されていない場合、ユーザーはアクティベーションワークフローの[マッピング手順](../../ui/activate-segment-streaming-destinations.md#mapping)以降に進むことができません。

```shell
POST platform.adobe.io/data/core/activation/authoring/destinations
```

```json {line-numbers="true" highlight="74"}
{
   "name":"Moviestar",
   "description":"Moviestar is a fictional destination, used for this example.",
   "status":"TEST",
   "customerAuthenticationConfigurations":[
      {
         "authType":"BEARER"
      }
   ],
   "customerDataFields":[
      {
         "name":"endpointsInstance",
         "type":"string",
         "title":"Select Endpoint",
         "description":"Moviestar manages several instances across the globe for REST endpoints that our customers are provisioned for. Select your endpoint in the dropdown list.",
         "isRequired":true,
         "enum":[
            "US",
            "EU",
            "APAC",
            "NZ"
         ]
      },
      {
         "name":"customerID",
         "type":"string",
         "title":"Moviestar Customer ID",
         "description":"Your customer ID in the Moviestar destination (e.g. abcdef).",
         "isRequired":true,
         "pattern":""
      }
   ],
   "uiAttributes":{
      "documentationLink":"http://www.adobe.com/go/destinations-moviestar-en",
      "category":"mobile",
      "connectionType":"Server-to-server",
      "frequency":"Streaming"
   },
   "identityNamespaces":{
      "external_id":{
         "acceptsAttributes":true,
         "acceptsCustomNamespaces":true
      },
      "another_id":{
         "acceptsAttributes":true,
         "acceptsCustomNamespaces":true
      }
   },
   "segmentMappingConfig":{
      "mapExperiencePlatformSegmentName":false,
      "mapExperiencePlatformSegmentId":false,
      "mapUserInput":false
   },
   "audienceMetadataConfig":{
      "audienceTemplateId":"cbf90a70-96b4-437b-86be-522fbdaabe9c"
   },  
   "aggregation":{
      "aggregationType":"CONFIGURABLE_AGGREGATION",
      "configurableAggregation":{
         "aggregationPolicyId":null,
         "aggregationKey":{
            "includeSegmentId":true,
            "includeSegmentStatus":true,
            "includeIdentity":true,
            "oneIdentityPerGroup":true,
            "groups":null
         },
         "splitUserById":true,
         "maxBatchAgeInSecs":2400,
         "maxNumEventsInBatch":5000
      }
   },
   "destinationDelivery":[
      {
         "authenticationRule":"CUSTOMER_AUTHENTICATION",
         "destinationServerId":"9c77000a-4559-40ae-9119-a04324a3ecd4"
      }
   ]
}
```

## 手順 3：メッセージ変換テンプレートの作成。テンプレート言語を使用して、メッセージの出力形式を指定します {#create-transformation-template}

宛先がサポートするペイロードに基づいて、AdobeXDM 形式から書き出されたデータの形式を、宛先でサポートされる形式に変換するテンプレートを作成する必要があります。 [ID、属性、オーディエンスメンバーシップの変換にテンプレート言語を使用する ](../functionality/destination-server/message-format.md#using-templating) の節のテンプレートの例を参照し、Adobeの [ テンプレートオーサリングツール ](../testing-api/streaming-destinations/create-template.md) を使用します。

自分の用途に合ったメッセージ変換テンプレートを作成したら、そのテンプレートを手順 1 で作成したサーバーおよびテンプレートの構成に追加します。

```json {line-numbers="true" highlight="13-14"}
{
   "name":"Moviestar destination server",
   "destinationServerType":"URL_BASED",
   "urlBasedDestination":{
      "url":{
         "templatingStrategy":"PEBBLE_V1",
         "value":"https://api.moviestar.com/data/{{customerData.region}}/items"
      }
   },
   "httpTemplate":{
      "requestBody":{
         "templatingStrategy":"PEBBLE_V1",
         "value":"{\n    \"users\": [\n        {% for profile in input.profiles %}\n            {{profile|raw}}{% if not loop.last %},{% endif %}\n        {% endfor %}\n    ]\n}"
      },
      "contentType":"application/json"
   }
}
```

## 手順 4：オーディエンスメタデータ構成の作成 {#create-audience-metadata-configuration}

一部の宛先では、Destination SDK は、宛先のオーディエンスをプログラムで作成、更新、削除するように、オーディエンスメタデータを構成する必要があります。 この設定をセットアップする必要がある場合やその方法について詳しくは、[オーディエンスメタデータ管理](../functionality/audience-metadata-management.md)を参照してください。

オーディエンスメタデータの構成を使用する場合は、手順 2 で作成した宛先構成に接続する必要があります。 オーディエンスメタデータ設定のインスタンス ID を、`audienceTemplateId` として宛先構成に追加します。

```json {line-numbers="true" highlight="53"}
{
   "name":"Moviestar",
   "description":"Moviestar is a fictional destination, used for this example.",
   "status":"TEST",
   "customerAuthenticationConfigurations":[
      {
         "authType":"BEARER"
      }
   ],
   "customerDataFields":[
      {
         "name":"endpointsInstance",
         "type":"string",
         "title":"Select Endpoint",
         "description":"Moviestar manages several instances across the globe for REST endpoints that our customers are provisioned for. Select your endpoint in the dropdown list.",
         "isRequired":true,
         "enum":[
            "US",
            "EU",
            "APAC",
            "NZ"
         ]
      },
      {
         "name":"customerID",
         "type":"string",
         "title":"Moviestar Customer ID",
         "description":"Your customer ID in the Moviestar destination (e.g. abcdef).",
         "isRequired":true,
         "pattern":""
      }
   ],
   "uiAttributes":{
      "documentationLink":"http://www.adobe.com/go/destinations-moviestar-en",
      "category":"mobile",
      "connectionType":"Server-to-server",
      "frequency":"Streaming"
   },
   "identityNamespaces":{
      "external_id":{
         "acceptsAttributes":true,
         "acceptsCustomNamespaces":true
      },
      "another_id":{
         "acceptsAttributes":true,
         "acceptsCustomNamespaces":true
      }
   },
   "segmentMappingConfig":{
      "mapExperiencePlatformSegmentName":false,
      "mapExperiencePlatformSegmentId":false,
      "mapUserInput":false
   },
   "audienceMetadataConfig":{
      "audienceTemplateId":"cbf90a70-96b4-437b-86be-522fbdaabe9c"
   },   
   "aggregation":{
      "aggregationType":"CONFIGURABLE_AGGREGATION",
      "configurableAggregation":{
         "aggregationPolicyId":null,
         "aggregationKey":{
            "includeSegmentId":true,
            "includeSegmentStatus":true,
            "includeIdentity":true,
            "oneIdentityPerGroup":true,
            "groups":null
         },
         "splitUserById":true,
         "maxBatchAgeInSecs":2400,
         "maxNumEventsInBatch":5000
      }
   },
   "destinationDelivery":[
      {
         "authenticationRule":"CUSTOMER_AUTHENTICATION",
         "destinationServerId":"9c77000a-4559-40ae-9119-a04324a3ecd4"
      }
   ]
}
```


## 手順 5：認証の設定 {#set-up-authentication}

上記の宛先設定で `"authenticationRule": "CUSTOMER_AUTHENTICATION"` と `"authenticationRule": "PLATFORM_AUTHENTICATION"` のどちらを指定したかによって、`/destination` または `/credentials` のエンドポイントを使用して宛先の認証を設定できます。

>[!NOTE]
>
>`CUSTOMER_AUTHENTICATION` は、2 つの認証ルールの中でより一般的で、接続の設定やデータの書き出しを行う前に、ユーザーが宛先に何らかの形の認証を提供する必要がある場合に使用するルールです。

宛先設定で `"authenticationRule": "CUSTOMER_AUTHENTICATION"` を選択し、宛先が OAuth 2 認証方式をサポートしている場合は、[OAuth 2 認証 ](../functionality/destination-configuration/oauth2-authorization.md) をお読みください。

`"authenticationRule": "PLATFORM_AUTHENTICATION"` を選択した場合、[ 資格情報設定 ](../credentials-api/create-credential-configuration.md) を作成する必要があります。

## 手順 6：宛先のテスト {#test-destination}

前の手順の構成エンドポイントを使用して宛先を設定した後、 [宛先テストツール](../testing-api/streaming-destinations/streaming-destination-testing-overview.md)を使用して、Adobe Experience Platform と宛先の統合をテストすることができます。

宛先をテストするプロセスの一環として、Experience Platform UI を使用してセグメントを作成し、宛先に対してアクティブ化する必要があります。 Experience Platformでオーディエンスを作成する方法については、以下の 2 つのリソースを参照してください。

* [オーディエンスドキュメントページの作成](/help/segmentation/ui/audience-portal.md#create-audience)
* [ オーディエンスの作成のビデオチュートリアル ](https://experienceleague.adobe.com/docs/platform-learn/tutorials/segments/create-segments.html?lang=ja)

## 手順 7：宛先を公開する {#publish-destination}

>[!NOTE]
>
>自分で使用するためにプライベートな宛先を作成していて、他の顧客が使用できるように宛先カタログに公開する予定がない場合は、この手順は必要ありません。

宛先を設定してテストした後、[Destination Publishing API](../publishing-api/create-publishing-request.md) を使用して、構成をレビュー用にアドビに送信します。

## 手順 8：宛先のドキュメント化 {#document-destination}

>[!NOTE]
>
>自分で使用するためにプライベートな宛先を作成していて、他の顧客が使用できるように宛先カタログに公開する予定がない場合は、この手順は必要ありません。

独立系ソフトウェアベンダー（ISV）またはシステムインテグレータ（SI）で[製品化統合](../overview.md#productized-custom-integrations)を作成する場合、[セルフサービスドキュメント化プロセス](../docs-framework/documentation-instructions.md)を使用して、宛先の製品ドキュメントページを [Experience Platform 宛先カタログ](/help/destinations/catalog/overview.md)に作成します。

## 手順 9:Adobeによるレビュー用に宛先を送信する {#submit-for-review}

>[!NOTE]
>
>自分で使用するためにプライベートな宛先を作成していて、他の顧客が使用できるように宛先カタログに公開する予定がない場合は、この手順は必要ありません。

最後に、宛先をExperience Platform カタログに公開して、すべてのExperience Platform ユーザーに表示する前に、Adobeのレビュー用に宛先を正式に送信する必要があります。 [Destination SDKで作成した製品化された宛先をレビュー用に送信 ](../guides/submit-destination.md) する方法に関する詳細を確認します。
