---
description: このページでは、Destination SDK を使用してストリーミングの宛先を設定する手順について説明します。
title: Destination SDK を使用したストリーミングの宛先の設定
exl-id: d8aa7353-ba55-4a0d-81c4-ea2762387638
source-git-commit: d6402f22ff50963b06c849cf31cc25267ba62bb1
workflow-type: tm+mt
source-wordcount: '820'
ht-degree: 65%

---

# Destination SDK を使用したストリーミングの宛先の設定

## 概要 {#overview}

このページでは、[Destination SDK の設定オプション](../functionality/configuration-options.md)や、他の Destination SDK 機能と API リファレンスのドキュメントに記載されている情報の利用方して[ストリーミング宛先](../../destination-types.md#streaming-destinations)を設定する方法について説明します。以下にその手順を、順を追って説明します。

## 前提条件 {#prerequisites}

以下に示す手順に進む前に、[Destination SDK の概要](../getting-started.md)ページを参照して、Adobe I/O 認証に必要な認証情報や、Destination SDK API を使用するためのその他の前提条件について確認してください。これは、パートナーシップと権限の前提条件を満たし、宛先の開発を開始する準備が整っていることを前提としています。

## Destination SDK の構成オプションを使用して宛先を設定する手順 {#steps}

![Destination SDK エンドポイントの使用手順の図解](../assets/guides/destination-sdk-steps.png)

## 手順 1：サーバーとテンプレートの構成を作成する {#create-server-template-configuration}

開始者 [サーバーとテンプレートの設定の作成](../authoring-api/destination-server/create-destination-server.md) の使用 `/destinations-server` endpoint.

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

以下に、`/destinations` API エンドポイントを使用して作成された、宛先テンプレートの構成の例を示します。 詳しくは、 [宛先設定の作成](../authoring-api/destination-configuration/create-destination-configuration.md) を参照してください。

手順 1 のサーバーとテンプレート設定をこの宛先構成に接続するには、サーバーのインスタンス ID とテンプレート設定を、`destinationServerId` として追加します。

>[!IMPORTANT]
>
>正しく設定されたリアルタイム（ストリーミング）の宛先を作成するには、次の手順を実行します。 *必須* 少なくとも 1 つのターゲット id をに追加する `identityNamespaces`、以下に示すように。 ターゲット ID が設定されていない場合、ユーザーはアクティベーションワークフローの[マッピング手順](../../ui/activate-segment-streaming-destinations.md#mapping)以降に進むことができません。

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
   "audienceMetadataConfig":{
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

宛先がサポートするペイロードに基づいて、AdobeXDM 形式から書き出されたデータの形式を、宛先でサポートされる形式に変換するテンプレートを作成する必要があります。 「 」セクションのテンプレートの例を参照してください [ID、属性、オーディエンスのメンバーシップ変換にテンプレート言語を使用する](../functionality/destination-server/message-format.md#using-templating) また、 [テンプレートオーサリングツール](../testing-api/streaming-destinations/create-template.md) Adobe

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
   "audienceMetadataConfig":{
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


## 手順 5：認証の設定 {#set-up-authentication}

上記の宛先設定で `"authenticationRule": "CUSTOMER_AUTHENTICATION"` と `"authenticationRule": "PLATFORM_AUTHENTICATION"` のどちらを指定したかによって、`/destination` または `/credentials` のエンドポイントを使用して宛先の認証を設定できます。

選択した場合 `"authenticationRule": "CUSTOMER_AUTHENTICATION"` 宛先の設定で、宛先が OAuth 2 認証方式をサポートしている場合は、 [OAuth 2 認証](../functionality/destination-configuration/oauth2-authentication.md).

選択した場合 `"authenticationRule": "PLATFORM_AUTHENTICATION"`を使用する場合は、 [資格情報設定](../credentials-api/create-credential-configuration.md).

## 手順 6：宛先のテスト {#test-destination}

前の手順の構成エンドポイントを使用して宛先を設定した後、 [宛先テストツール](../testing-api/streaming-destinations/streaming-destination-testing-overview.md)を使用して、Adobe Experience Platform と宛先の統合をテストすることができます。

宛先をテストするプロセスの一環として、Experience Platform UI を使用してセグメントを作成し、宛先に対してアクティブ化する必要があります。 Experience Platformでオーディエンスを作成する方法については、以下の 2 つのリソースを参照してください。

* [オーディエンスドキュメントの作成ページ](/help/segmentation/ui/overview.md#create-segment)
* [オーディエンスのビデオチュートリアルの作成](https://experienceleague.adobe.com/docs/platform-learn/tutorials/segments/create-segments.html?lang=ja)

## 手順 7：宛先を公開する {#publish-destination}

>[!NOTE]
>
>独自の用途でプライベートな宛先を作成し、他の顧客が使用できるように宛先カタログに公開しようとしない場合は、この手順は不要です。

宛先を設定してテストした後、[Destination Publishing API](../publishing-api/create-publishing-request.md) を使用して、構成をレビュー用にアドビに送信します。

## 手順 8：宛先のドキュメント化 {#document-destination}

>[!NOTE]
>
>独自の用途でプライベートな宛先を作成し、他の顧客が使用できるように宛先カタログに公開しようとしない場合は、この手順は不要です。

独立系ソフトウェアベンダー（ISV）またはシステムインテグレータ（SI）で[製品化統合](../overview.md#productized-custom-integrations)を作成する場合、[セルフサービスドキュメント化プロセス](../docs-framework/documentation-instructions.md)を使用して、宛先の製品ドキュメントページを [Experience Platform 宛先カタログ](/help/destinations/catalog/overview.md)に作成します。

## 手順 9:Adobeの確認用に宛先を送信 {#submit-for-review}

>[!NOTE]
>
>独自の用途でプライベートな宛先を作成し、他の顧客が使用できるように宛先カタログに公開しようとしない場合は、この手順は不要です。

最後に、宛先をExperience Platformカタログに公開してすべてのExperience Platformの顧客に表示できるようにするには、Adobeの確認のために宛先を正式に送信する必要があります。 次の方法に関する完全な情報を見つけます。 [送信してレビュー用に生産済みのDestination SDKで作成](../guides/submit-destination.md).
