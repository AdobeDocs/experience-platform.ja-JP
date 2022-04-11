---
description: このページでは、Destination SDK を使用してストリーミングの宛先を設定する手順について説明します。
title: Destination SDK を使用したストリーミングの宛先の設定
exl-id: d8aa7353-ba55-4a0d-81c4-ea2762387638
source-git-commit: 51417bee5dba7a96d3a7a7eb507fc95711fad4a5
workflow-type: tm+mt
source-wordcount: '688'
ht-degree: 100%

---

# Destination SDK を使用したストリーミングの宛先の設定

## 概要 {#overview}

このページでは、[Destination SDK の設定オプション](./configuration-options.md)や、他の Destination SDK 機能と API リファレンスのドキュメントに記載されている情報の利用方して[ストリーミング宛先](/help/destinations/destination-types.md#streaming-destinations)を設定する方法について説明します。以下にその手順を、順を追って説明します。

## 前提条件 {#prerequisites}

以下に示す手順に進む前に、[Destination SDK の概要](./getting-started.md)ページを参照して、Adobe I/O 認証に必要な認証情報や、Destination SDK API を使用するためのその他の前提条件について確認してください。

## Destination SDK の構成オプションを使用して宛先を設定する手順 {#steps}

![Destination SDK エンドポイントの使用手順の図解](./assets/destination-sdk-steps.png)

## 手順 1：サーバーとテンプレートの構成を作成する {#create-server-template-configuration}

まず、`/destinations-server` エンドポイントを使用してサーバーとテンプレートの設定を作成します（[API リファレンス](destination-server-api.md)を参照）。サーバーとテンプレートの設定について詳しくは、[サーバーとテンプレートの仕様](server-and-template-configuration.md)の節を参照してください。

次に構成の例を示します。 `requestBody.value` パラメーターでのメッセージ変換テンプレートは、手順 3 の[変換テンプレートの作成](./configure-destination-instructions.md#create-transformation-template)で対応します。

```json
POST platform.adobe.io/data/core/activation/authoring/destination-servers

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

以下に、`/destinations` API エンドポイントを使用して作成された、宛先テンプレートの構成の例を示します。 この設定について詳しくは、[宛先の設定](./destination-configuration.md)を参照してください。

手順 1 のサーバーとテンプレート設定をこの宛先構成に接続するには、サーバーのインスタンス ID とテンプレート設定を、`destinationServerId` として追加します。

>[!IMPORTANT]
>
>正しく設定された宛先を作成するには、次の手順を実行します。 以下に示すように、少なくとも 1 つのターゲット ID を `identityNamespaces` に追加する&#x200B;*必要があります*。 ターゲット ID が設定されていない場合、ユーザーはアクティベーションワークフローの[マッピング手順](/help/destinations/ui/activate-segment-streaming-destinations.md#mapping)以降に進むことができません。

```json
POST platform.adobe.io/data/core/activation/authoring/destinations
 
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
      "mapUserInput":false,
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

宛先がサポートするペイロードに基づいて、AdobeXDM 形式から書き出されたデータの形式を、宛先でサポートされる形式に変換するテンプレートを作成する必要があります。 [ID、属性、セグメントメンバーシップの変換にテンプレート言語を使用する](./message-format.md#using-templating)の節のテンプレートの例を参照し、アドビの[テンプレートオーサリングツール](./create-template.md)を使用します。

自分の用途に合ったメッセージ変換テンプレートを作成したら、そのテンプレートを手順 1 で作成したサーバーおよびテンプレートの構成に追加します。

## 手順 4：オーディエンスメタデータ構成の作成 {#create-audience-metadata-configuration}

一部の宛先では、Destination SDK は、宛先のオーディエンスをプログラムで作成、更新、削除するように、オーディエンスメタデータを構成する必要があります。 この設定をセットアップする必要がある場合やその方法について詳しくは、[オーディエンスメタデータ管理](./audience-metadata-management.md)を参照してください。

オーディエンスメタデータの構成を使用する場合は、手順 2 で作成した宛先構成に接続する必要があります。 オーディエンスメタデータ設定のインスタンス ID を、`audienceTemplateId` として宛先構成に追加します。

## 手順 5：認証の設定 {#set-up-authentication}

上記の宛先設定で `"authenticationRule": "CUSTOMER_AUTHENTICATION"` と `"authenticationRule": "PLATFORM_AUTHENTICATION"` のどちらを指定したかによって、`/destination` または `/credentials` のエンドポイントを使用して宛先の認証を設定できます。

* **最も一般的なケース**：宛先設定で `"authenticationRule": "CUSTOMER_AUTHENTICATION"` を選択し、宛先が OAuth 2 認証方式をサポートしている場合は、[OAuth 2 認証](./oauth2-authentication.md)をお読みください。
* `"authenticationRule": "PLATFORM_AUTHENTICATION"` を選択した場合は、[認証の構成](./authentication-configuration.md#when-to-use)を参照してください。

## 手順 6：宛先のテスト {#test-destination}

前の手順の構成エンドポイントを使用して宛先を設定した後、 [宛先テストツール](./test-destination.md)を使用して、Adobe Experience Platform と宛先の統合をテストすることができます。

宛先をテストするプロセスの一環として、Experience Platform UI を使用してセグメントを作成し、宛先に対してアクティブ化する必要があります。 Experience Platform でセグメントを作成する方法については、以下の 2 つのリソースを参照してください。

* [セグメントのドキュメントページを作成](https://experienceleague.adobe.com/docs/experience-platform/segmentation/ui/overview.html?lang=ja#create-segment)
* [セグメントの作成のビデオチュートリアル](https://experienceleague.adobe.com/docs/platform-learn/tutorials/segments/create-segments.html?lang=ja)

## 手順 7：宛先を公開する {#publish-destination}

宛先を設定してテストした後、[Destination Publishing API](./destination-publish-api.md) を使用して、構成をレビュー用にアドビに送信します。

## 手順 8：宛先のドキュメント化 {#document-destination}

独立系ソフトウェアベンダー（ISV）またはシステムインテグレーター（SI）が[製品化統合](./overview.md#productized-custom-integrations)を作成する場合は、 [セルフサービスドキュメント化プロセス](./docs-framework/documentation-instructions.md)を使用して、[Experience Platform の宛先カタログ](/help/destinations/catalog/overview.md)に宛先の製品ドキュメントページを作成します。
