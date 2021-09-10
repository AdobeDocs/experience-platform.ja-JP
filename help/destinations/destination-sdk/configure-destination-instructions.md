---
description: このページでは、宛先SDKの「設定オプション」の参照情報を使用して、宛先SDKを使用して宛先を設定する方法について説明します。
seo-description: This page describes how to use the reference information in Configuration options for the Destinations SDK to configure your destination using Destination SDK.
seo-title: How to use Destination SDK to configure your destination
title: 宛先SDKを使用した宛先の設定方法
exl-id: d8aa7353-ba55-4a0d-81c4-ea2762387638
source-git-commit: bd65cfa557fb42d23022578b98bc5482e8bd50b1
workflow-type: tm+mt
source-wordcount: '568'
ht-degree: 0%

---

# 宛先SDKを使用した宛先の設定方法

## 概要 {#overview}

このページでは、宛先SDK](./configuration-options.md)の[設定オプションの参照情報を使用して宛先を設定する方法について説明します。 手順は、以下の順番でレイアウトされます。

## 前提条件 {#prerequisites}

以下の手順に進む前に、[宛先SDKの概要](./getting-started.md)ページを読み、宛先SDK APIで動作するために必要なAdobe I/O認証資格情報の取得およびその他の前提条件を確認してください。

## 宛先SDKの設定オプションを使用して宛先を設定する手順 {#steps}

![宛先SDKエンドポイントの使用手順の説明](./assets/destination-sdk-steps.png)

## 手順1:サーバーとテンプレートの設定の作成 {#create-server-template-configuration}

まず、`/destinations-server`エンドポイントを使用してサーバーとテンプレートの設定を作成します（[APIリファレンス](./destination-server-api.md)を読み取ります）。 サーバーとテンプレートの設定について詳しくは、リファレンスの節の[サーバーとテンプレートの仕様](./configuration-options.md#server-and-template)を参照してください。

次に設定例を示します。 `requestBody.value`パラメーター内のメッセージ変換テンプレートは、手順3、[変換テンプレートの作成](./configure-destination-instructions.md#create-transformation-template)で指定します。

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

## 手順2:宛先設定の作成 {#create-destination-configuration}

以下に、`/destinations` APIエンドポイントを使用して作成した宛先テンプレートの設定例を示します。 このテンプレートについて詳しくは、[宛先の設定](./destination-configuration.md)を参照してください。

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
         "maxBatchAgeInSecs":360,
         "maxNumEventsInBatch":100
      }
   },
   "destinationDelivery":[
      {
         "authenticationRule":"CUSTOMER_AUTHENTICATION",
         "destinationServerId":"9c77000a-4559-40ae-9119-a04324a3ecd4"
      }
   ],
   "inputSchemaId":"cc8621770a9243b98aba4df79898b1ed"
}
```

## 手順3:メッセージ変換テンプレートの作成 — テンプレート言語を使用してメッセージ出力形式を指定します。 {#create-transformation-template}

宛先がサポートするペイロードに基づいて、AdobeXDM形式から書き出されたデータの形式を、宛先でサポートされる形式に変換するテンプレートを作成する必要があります。 [ID、属性、セグメントメンバーシップ変換のテンプレート言語の使用](./message-format.md#using-templating)の節のテンプレート例を参照し、Adobeが提供する[テンプレートオーサリングツール](./create-template.md)を使用します。

## 手順4:オーディエンスメタデータ設定の作成 {#create-audience-metadata-configuration}

一部の宛先では、宛先SDKは、宛先のオーディエンスをプログラムで作成、更新、削除するように、オーディエンスメタデータテンプレートを設定する必要があります。 この設定をおこなう必要があるタイミングと方法については、[Audience metadata management](./audience-metadata-management.md)を参照してください。

## 手順5:資格情報の設定の作成/認証の設定 {#set-up-authentication}

上記の宛先設定で`"authenticationRule": "CUSTOMER_AUTHENTICATION"`と`"authenticationRule": "PLATFORM_AUTHENTICATION"`のどちらを指定したかに応じて、`/destination`または`/credentials`エンドポイントを使用して、宛先の認証を設定できます。

* **最も一般的なケース**:「 」を選択し、 `"authenticationRule": "CUSTOMER_AUTHENTICATION"` 宛先がOAuth 2認証方法をサポートしている場合は、「  [OAuth 2認証](./oauth2-authentication.md)」を読み取ります。
* `"authenticationRule": "PLATFORM_AUTHENTICATION"`を選択した場合は、リファレンスドキュメントの[資格情報の設定](./credentials-configuration.md)を参照してください。

## 手順6:宛先のテスト {#test-destination}

前の手順のテンプレートを使用して宛先を設定したら、[宛先テストツール](./create-template.md)を使用して、Adobe Experience Platformと宛先の統合をテストできます。

宛先をテストするプロセスの一部として、Experience PlatformUIを使用してセグメントを作成し、宛先に対してアクティブ化する必要があります。 Experience Platformでセグメントを作成する方法については、以下の2つのリソースを参照してください。

* [セグメントドキュメントの作成ページ](https://experienceleague.adobe.com/docs/experience-platform/segmentation/ui/overview.html?lang=en#create-segment)
* [セグメントのビデオチュートリアルの作成](https://experienceleague.adobe.com/docs/platform-learn/tutorials/segments/create-segments.html?lang=en)


## 手順7:宛先の公開 {#publish-destination}

宛先の設定とテストが完了したら、[宛先公開API](./destination-publish-api.md)を使用して、確認用に設定をAdobeに送信します。

## 手順8:宛先のドキュメント化 {#document-destination}

[製品化されたExperience League](./overview.md#productized-custom-integrations)を作成するIndependent Software Vendor(ISV)またはSystem Integrator(SI)の場合、[セルフサービスドキュメントプロセス](./docs-framework/documentation-instructions.md)を使用して、 [統合先カタログ](/help/destinations/catalog/overview.md)に製品ドキュメントページを作成します。
