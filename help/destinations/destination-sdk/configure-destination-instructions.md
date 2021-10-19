---
description: このページでは、デスティネーション SDK を使用してストリーミング出力先を設定する手順について説明します。
title: 目的の SDK を使用してストリーミング出力先を設定する方法
exl-id: d8aa7353-ba55-4a0d-81c4-ea2762387638
source-git-commit: a7c36f1a157b6020fede53e5c1074d966f26cf3d
workflow-type: tm+mt
source-wordcount: '670'
ht-degree: 0%

---

# 目的の SDK を使用してストリーミング出力先を設定する方法

## 概要 {#overview}

このページでは、 [ 送信先 sdk の設定オプション、 ](./configuration-options.md) およびその他のターゲット sdk 機能と API リファレンスドキュメントで、ストリーミング出力先を設定するための情報を使用する方法について説明し [ ](/help/destinations/destination-types.md#streaming-destinations) ます。 ステップは、次の順序で配置されます。

>[!NOTE]
>
>送信先 SDK を使用したバッチ出力先の設定は、現在サポートされていません。

## 前提条件 {#prerequisites}

以下の手順を実行する前に、移行先 SDK の「はじめに」ページを参照してください [ ](./getting-started.md) 。必要な Adobe i/o 認証資格情報や、宛先 sdk api を使用するためのその他の前提条件について詳しくは、こちらを参照してください。

## 送り先 SDK の設定オプションを使用して、宛先を設定する手順 {#steps}

![「ターゲット SDK エンドポイントの使用」の例を示します。](./assets/destination-sdk-steps.png)

## ステップ 1: サーバーとテンプレートの設定の作成 {#create-server-template-configuration}

最初に、エンドポイントを使用してサーバーとテンプレートの設定を作成し `/destinations-server` ます (「API リファレンスの読み取り」 [ ](./destination-server-api.md) )。 サーバーとテンプレートの設定について詳しくは、『リファレンスガイド』の [ 「サーバーとテンプレートの仕様」を参照してください ](./configuration-options.md#server-and-template) 。

次に例を示します。 パラメーターのメッセージ変換テンプレートは、 `requestBody.value` 手順3で「変換テンプレートを作成」に指定されていることに注意して [ ](./configure-destination-instructions.md#create-transformation-template) ください。

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

## ステップ 2: 宛先設定の作成 {#create-destination-configuration}

次に示すのは、API エンドポイントを使用して作成された宛先テンプレートの設定例です `/destinations` 。 このテンプレートについて詳しくは、宛先の設定を参照してください [ ](./destination-configuration.md) 。

手順1でサーバーとテンプレートの設定を「この宛先設定」に接続するには、サーバーとテンプレートの設定のインスタンス ID をここに追加し `destinationServerId` ます。

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
         "maxBatchAgeInSecs":360,
         "maxNumEventsInBatch":100
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

## ステップ 3: メッセージ変換テンプレートの作成-テンプレート言語を使用してメッセージ出力形式を指定します。 {#create-transformation-template}

出力先でサポートされているペイロードに基づいて、書き出されたデータのフォーマットを Adobe XDM フォーマットから出力先の形式に変換するテンプレートを作成する必要があります。 このセクションでは、テンプレート [ 言語を使用して id、属性、セグメントメンバーシップを変換する方法を示してい ](./message-format.md#using-templating) [ ます。また、 ](./create-template.md) アドビから提供されているテンプレートオーサリングツールを使用してください。

使用できるようにするメッセージ変形テンプレートを作成したら、手順1で作成したサーバー構成とテンプレート構成にそれを追加します。

## 手順 4: 対象ユーザー向けメタデータ設定の作成 {#create-audience-metadata-configuration}

一部の宛先については、移動先の SDK で、宛先の対象ユーザーをプログラムによって作成、更新、または削除するように、対象ユーザーのメタデータの設定を構成する必要があります。 [ ](./audience-metadata-management.md) この設定を設定する必要がある場合について詳しくは、対象ユーザー向けメタデータの管理とその方法を参照してください。

オーディエンスメタデータの設定を使用する場合は、手順2で作成したターゲットの設定に接続する必要があります。 対象ユーザーのメタデータ設定のインスタンス ID をという宛先設定に追加し `audienceTemplateId` ます。

## ステップ 5: 認証情報の設定/認証を設定する {#set-up-authentication}

上記の宛先設定を指定するか、またはそのエンドポイントを使用して、宛先に認証を設定するかに応じて、 `"authenticationRule": "CUSTOMER_AUTHENTICATION"` `"authenticationRule": "PLATFORM_AUTHENTICATION"` 宛先に対する認証を設定することができ `/destination` `/credentials` ます。

* **最も一般的なケース** : `"authenticationRule": "CUSTOMER_AUTHENTICATION"` 宛先設定で選択し、宛先で oauth 2 認証メソッドがサポートされている場合は、 [ oauth 2 認証を確認して ](./oauth2-authentication.md) ください。
* 「」を選択した場合は `"authenticationRule": "PLATFORM_AUTHENTICATION"` 、リファレンスマニュアルの「クリデンシャルの設定」を参照してください [ ](./credentials-configuration.md) 。

## 手順 6: 移動先のテスト {#test-destination}

前の手順で設定されているエンドポイントを使用して宛先を設定した後、移行先のテストツールを使用して、 [ ](./create-template.md) Adobe エクスペリエンスプラットフォームと宛先間の統合をテストすることができます。

出力先をテストするプロセスの一部として、経験 Platform の UI を使用してセグメントを作成し、宛先にアクティブにする必要があります。 ここでは、次の2つのリソースを参照してください。操作プラットフォームでセグメントを作成する方法について説明します。

* [セグメントドキュメントページの作成](https://experienceleague.adobe.com/docs/experience-platform/segmentation/ui/overview.html?lang=en#create-segment)
* [セグメントの作成ビデオウォークスルー](https://experienceleague.adobe.com/docs/platform-learn/tutorials/segments/create-segments.html?lang=en)

## 手順 7: 宛先のパブリッシュ {#publish-destination}

ターゲットの設定とテストが完了したら、宛先パブリッシュ API を使用して、 [ ](./destination-publish-api.md) レビュー用に Adobe に設定を送信します。

## 手順 8: 作成先をドキュメント化する {#document-destination}

独立系ソフトウェアベンダー (ISV) またはシステムインテグレーター (SI) が productized を作成している場合は [ ](./overview.md#productized-custom-integrations) 、セルフサービスのマニュアルを参照して、 [ ](./docs-framework/documentation-instructions.md) 宛先用の製品ドキュメントページを使用して、 [ リーグの出力先カタログを作成し ](/help/destinations/catalog/overview.md) ます。
