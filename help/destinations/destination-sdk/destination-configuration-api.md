---
description: このページでは、API エンドポイント /authoring/destinations を使用して実行できるすべての API 操作について説明します。
title: 宛先 API エンドポイントの操作
exl-id: 96755e9d-be62-432f-b985-91330575b395
source-git-commit: 301cef53644e813c3fd43e7f2dbaf730c9e5fc11
workflow-type: tm+mt
source-wordcount: '2571'
ht-degree: 96%

---

# 宛先エンドポイント API の操作 {#destination-configuration}

>[!IMPORTANT]
>
>**API エンドポイント**：`platform.adobe.io/data/core/activation/authoring/destinations`

このページでは、`/authoring/destinations` API エンドポイントを使用して実行できるすべての API の操作について説明します。このエンドポイントでサポートされる機能については、[宛先設定](./destination-configuration.md)を参照してください。

## 宛先 API 操作の概要 {#get-started}

続ける前に「[はじめる前に](./getting-started.md)」を参照し、必要な宛先オーサリング権限および必要なヘッダーの取得方法など、API の呼び出しを正常に行うために必要となる重要な情報を確認してください。

## ストリーミング宛先の設定を作成する {#create}

`/authoring/destinations` エンドポイントに POST リクエストを実行することで、新しい宛先設定を作成できます。

**API 形式**

```http
POST /authoring/destinations
```

**リクエスト**

次のリクエストは、ペイロード内のパラメーター設定に基づいて、新しいストリーミング宛先設定を作成します。以下のペイロードには、`/authoring/destinations` エンドポイントで使用可能なストリーミング宛先のすべてのパラメーターが含まれます。呼び出しにすべてのパラメーターを追加する必要はなく、テンプレートは API 要件に応じてカスタマイズできることに注意してください。

```shell
curl -X POST https://platform.adobe.io/data/core/activation/authoring/destinations \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d '
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
         "pattern":"^[A-Za-z]+$"
      }
   ],
   "uiAttributes":{
      "documentationLink":"https://www.adobe.com/go/destinations-moviestar-en",
      "category":"mobile",
      "connectionType":"Server-to-server",
      "frequency":"Streaming"
   },
   "identityNamespaces":{
      "external_id":{
         "acceptsAttributes":true,
         "acceptsCustomNamespaces":true,
         "acceptedGlobalNamespaces":{
            "Email":{
            }
         }
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
   "schemaConfig":{
      "profileFields":[
         {
            "name":"a_custom_attribute",
            "title":"a_custom_attribute",
            "description":"This is a fixed attribute on your destination side that customers can map profile attributes to. For example, the phoneNumber value in Experience Platform could be phoneNo on your side.",
            "type":"string",
            "isRequired":false,
            "readOnly":false,
            "hidden":false
         }
      ],
      "profileRequired":true,
      "segmentRequired":true,
      "identityRequired":true
   },
   "aggregation":{
      "aggregationType":"BEST_EFFORT",
      "bestEffortAggregation":{
         "maxUsersPerRequest":10,
         "splitUserById":false
      }
   },
   "destinationDelivery":[
      {
         "authenticationRule":"CUSTOMER_AUTHENTICATION",
         "destinationServerId":"9c77000a-4559-40ae-9119-a04324a3ecd4"
      }
   ],
   "backfillHistoricalProfileData":true
}
```

| パラメーター | タイプ | 説明 |
|---------|----------|------|
| `name` | 文字列 | Experience Platform カタログでご使用の宛先のタイトルを示します |
| `description` | 文字列 | アドビが宛先カードの Experience Platform 宛先カタログで使用する説明を入力します。4 ～ 5 文以下を目指します。 |
| `status` | 文字列 | 宛先カードのライフサイクルステータスを示します。 指定できる値は、`TEST`、`PUBLISHED`、`DELETED` です。最初に宛先を設定する際は `TEST` を使用します。 |
| `customerAuthenticationConfigurations` | 文字列 | Experience Platform の顧客をサーバーで認証するために使用される構成を示します。 使用可能な値については、下記の `authType` を参照してください。 |
| `customerAuthenticationConfigurations.authType` | 文字列 | ストリーミング宛先でサポートしている値は次の通りです。 <ul><li>`OAUTH2`</li><li>`BEARER`</li></ul> ファイルベースの宛先でサポートしている値は次の通りです。 <ul><li>`S3`</li><li>`AZURE_CONNECTION_STRING`</li><li>`AZURE_SERVICE_PRINCIPAL`</li><li>`SFTP_WITH_SSH_KEY`</li><li>`SFTP_WITH_PASSWORD`</li></ul> |
| `customerDataFields.name` | 文字列 | 導入するカスタムフィールドの名前を記入します。 |
| `customerDataFields.type` | 文字列 | 導入するカスタムフィールドのタイプを示します。 指定できる値は、`string`、`object`、`integer` です。 |
| `customerDataFields.title` | 文字列 | Experience Platform ユーザーインターフェイスで顧客に表示されるフィールドの名前を示します |
| `customerDataFields.description` | 文字列 | カスタムフィールドの説明を入力します。 |
| `customerDataFields.isRequired` | ブール値 | このフィールドが宛先設定ワークフローで必須かどうかを示します。 |
| `customerDataFields.enum` | 文字列 | カスタムフィールドをドロップダウンメニューとしてレンダリングし、ユーザーが使用できるオプションを一覧表示します。 |
| `customerDataFields.pattern` | 文字列 | 必要に応じて、カスタムフィールドのパターンを適用します。正規表現を使用して、パターンを適用します。 例えば、顧客 ID に数字やアンダースコアが含まれない場合は、このフィールドで `^[A-Za-z]+$` を入力します。 |
| `uiAttributes.documentationLink` | 文字列 | 宛先用の[宛先のカタログ](https://experienceleague.adobe.com/docs/experience-platform/destinations/catalog/overview.html?lang=ja#catalog)にあるドキュメントページを参照します。`https://www.adobe.com/go/destinations-YOURDESTINATION-en` を使用します。ここでは、`YOURDESTINATION` は宛先の名前です。Moviestar という宛先の場合、`https://www.adobe.com/go/destinations-moviestar-en` を使用します。。このリンクは、Adobeが宛先をライブに設定し、ドキュメントが公開された後にのみ機能します。 |
| `uiAttributes.category` | 文字列 | Adobe Experience Platform で宛先に割り当てられたカテゴリを参照します。 詳しくは、[宛先のカテゴリ](https://experienceleague.adobe.com/docs/experience-platform/rtcdp/destinations/destination-types.html?lang=ja#destination-categories)をお読みください。次のいずれかの値を使用します：`adobeSolutions, advertising, analytics, cdp, cloudStorage, crm, customerSuccess, database, dmp, ecommerce, email, emailMarketing, enrichment, livechat, marketingAutomation, mobile, personalization, protocols, social, streaming, subscriptions, surveys, tagManagers, voc, warehouses, payments` |
| `uiAttributes.connectionType` | 文字列 | `Server-to-server` は現在利用可能な唯一のオプションです。 |
| `uiAttributes.frequency` | 文字列 | `Streaming` は現在唯一の利用可能なオプションです。 |
| `identityNamespaces.externalId.acceptsAttributes` | ブール値 | 宛先が標準のプロファイル属性を受け入れるかどうかを示します。 通常、これらの属性はパートナーのドキュメントでハイライト表示されます。 |
| `identityNamespaces.externalId.acceptsCustomNamespaces` | ブール値 | 顧客が宛先でカスタム名前空間を設定できるかどうかを示します。 |
| `identityNamespaces.externalId.transformation` | 文字列 | _サンプル設定には表示されません_。例えば、[!DNL Platform] の顧客がプレーンなメールアドレスを属性として持っており、プラットフォームがハッシュ化されたメールのみを受け取る場合に使用します。ここで、適用する必要のある変換（例えば、メールを小文字に変換してからハッシュ化するなど）を行います。 |
| `identityNamespaces.externalId.acceptedGlobalNamespaces` | - | プラットフォームが[標準 ID 名前空間](https://experienceleague.adobe.com/docs/experience-platform/identity/namespaces.html?lang=ja#standard-namespaces)（例えば IDFA）を受け入れる場合に使用します。これにより、Platform ユーザーがこれらの ID 名前空間のみを選択するように制限できます。<br> `acceptedGlobalNamespaces` を使用する場合、`"requiredTransformation":"sha256(lower($))"` を使用すれば、メールアドレスまたは電話番号を小文字に変換してハッシュ化できます。 |
| `destinationDelivery.authenticationRule` | 文字列 | [!DNL Platform] の顧客が宛先に接続する方法を示します。使用できる値は `CUSTOMER_AUTHENTICATION`、`PLATFORM_AUTHENTICATION`、`NONE`、<br> です。 <ul><li>Platform の顧客がユーザー名とパスワード、ベアラートークン、または他の認証方法を使用してシステムにログインする場合は、`CUSTOMER_AUTHENTICATION` を使用します。例えば、`customerAuthenticationConfigurations` で `authType: OAUTH2` や `authType:BEARER` も選択した場合、このオプションを選択することになります。 </li><li> アドビと接続先との間にグローバル認証システムがあり、[!DNL Platform] の顧客が接続先に認証資格情報を提供する必要がない場合は、`PLATFORM_AUTHENTICATION` を使用してください。この場合、[資格情報](./credentials-configuration-api.md)の構成を使用して、資格情報オブジェクトを作成する必要があります。 </li><li>宛先プラットフォームにデータを送信するために認証が必要ない場合は、`NONE` を使用します。 </li></ul> |
| `destinationDelivery.destinationServerId` | 文字列 | この宛先に使用される[宛先サーバーテンプレート](./destination-server-api.md)の `instanceId`。 |
| `backfillHistoricalProfileData` | ブール値 | 宛先に対してセグメントをアクティブ化する際に、履歴プロファイルデータを書き出すかどうかを制御します。 <br> <ul><li> `true`：[!DNL Platform] は、セグメントがアクティブ化される前に、セグメントに適格となる履歴ユーザープロファイルを送信します。 </li><li> `false`：[!DNL Platform] には、セグメントが有効化された後にセグメントに適格となるユーザーのプロファイルのみが含まれます。 </li></ul> |
| `segmentMappingConfig.mapUserInput` | ブール値 | 宛先のアクティベーションワークフローのセグメントマッピング ID をユーザーが入力するかどうかを制御します。 |
| `segmentMappingConfig.mapExperiencePlatformSegmentId` | ブール値 | 宛先のアクティベーションワークフローのセグメントマッピング ID が Adobe Experience Platform のセグメント ID かどうかを制御します。 |
| `segmentMappingConfig.mapExperiencePlatformSegmentName` | ブール値 | 宛先のアクティベーションワークフローのセグメントマッピング ID が Adobe Experience Platform のセグメント名かどうかを制御します。 |
| `segmentMappingConfig.audienceTemplateId` | ブール値 | この宛先に使用する[オーディエンスメタデータテンプレート](./audience-metadata-api.md) の `instanceId`。 |
| `schemaConfig.profileFields` | 配列 | 上記の設定に示すように、定義済みの `profileFields` を追加する際、ユーザーは Adobe Experience Platform 属性を宛先側の定義済み属性にマッピングするオプションを選択できます。 |
| `schemaConfig.profileRequired` | ブール値 | 上記の設定例に示すように、ユーザーが Experience Platform から宛先側のカスタム属性にプロファイル属性をマッピングできる場合、`true` を使用します。 |
| `schemaConfig.segmentRequired` | ブール値 | 常に `segmentRequired:true` を使用します。 |
| `schemaConfig.identityRequired` | ブール値 | ユーザーが、Adobe Experience Platform から目的のスキーマに ID 名前空間をマッピングできるようにする場合、`true` を使用します。 |
| `aggregation.aggregationType` | - | `BEST_EFFORT` または `CONFIGURABLE_AGGREGATION` を選択します。上記の設定例には `BEST_EFFORT` 集計が含まれています。`CONFIGURABLE_AGGREGATION` の例については、[宛先設定](./destination-configuration.md#example-configuration)に関するドキュメントの設定例を参照してください。設定可能な集計に関連するパラメーターは、この表の後続の行で説明します。 |
| `aggregation.bestEffortAggregation.maxUsersPerRequest` | 整数 | Adobe Experience Platformは、1 回の HTTP 呼び出しで書き出された複数のプロファイルを集計できます。 1 回の HTTP 呼び出しでエンドポイントが受け取るプロファイルの最大数を指定します。 これはベストエフォートの集計であることに注意してください。例えば、値 100 を指定した場合、Platform が 1 回の呼び出しで送信するプロファイルの数は、100 未満の任意の数になります。<br> サーバーが 1 回のリクエストで複数のユーザーを受け入れない場合、この値を 1 に設定します。 |
| `aggregation.bestEffortAggregation.splitUserById` | ブール値 | 宛先への呼び出しを ID で分割する必要がある場合は、このフラグを使用します。サーバーが呼び出しごとに 1 つの ID しか受け入れない場合、特定の名前空間に対してこのフラグを `true` に設定します。 |
| `aggregation.configurableAggregation.splitUserById` | ブール値 | [こちら](./destination-configuration.md#example-configuration)で設定例のパラメーターを参照してください。宛先への呼び出しを ID で分割する必要がある場合は、このフラグを使用します。サーバーが呼び出しごとに 1 つの ID しか受け入れない場合、特定の名前空間に対してこのフラグを `true` に設定します。 |
| `aggregation.configurableAggregation.maxBatchAgeInSecs` | 整数 | <ul><li>*最小値：1800*</li><li>*最大値：3600*</li><li>[こちら](./destination-configuration.md#example-configuration)で設定例のパラメーターを参照してください。使用可能な最小値と最大値の間の値を設定します。 と `maxNumEventsInBatch`の値が 0 の場合、このパラメーターは、Experience Platformがエンドポイントに API 呼び出しを送信するまで待機する時間を決定します。 <br> 例えば、両方のパラメーターに最大値を使用した場合、Experience Platform は 3600 秒待機するか認定済みプロファイルが 10.000 個になるまで（いずれか早い方）待ってから、API 呼び出しを行います。 </li></ul> |
| `aggregation.configurableAggregation.maxNumEventsInBatch` | 整数 | <ul><li>*最小値：1000*</li><li>*最大値：10000*</li><li>[こちら](./destination-configuration.md#example-configuration)で設定例のパラメーターを参照してください。使用可能な最小値と最大値の間の値を設定します。 このパラメーターの詳細については、 `maxBatchAgeInSecs` ちょうど上に</li></ul> |
| `aggregation.configurableAggregation.aggregationKey` | ブール値 | [こちら](./destination-configuration.md#example-configuration)で設定例のパラメーターを参照してください。以下のパラメーターに基づいて、宛先にマッピングされた書き出し済みプロファイルを集計できます。<br> <ul><li>セグメント ID</li><li> セグメントのステータス </li><li> ID 名前空間 </li></ul> |
| `aggregation.configurableAggregation.aggregationKey.includeSegmentId` | ブール値 | [こちら](./destination-configuration.md#example-configuration)で設定例のパラメーターを参照してください。グループプロファイルをセグメント ID でご使用の宛先に書き出す場合、これを `true` に設定します。 |
| `aggregation.configurableAggregation.aggregationKey.includeSegmentStatus` | ブール値 | [こちら](./destination-configuration.md#example-configuration)で設定例のパラメーターを参照してください。宛先に書き出されたプロファイルをセグメント ID とセグメントステータスでグループ化する場合は、`includeSegmentId:true` および `includeSegmentStatus:true` の両方を設定する必要があります。 |
| `aggregation.configurableAggregation.aggregationKey.includeIdentity` | ブール値 | [こちら](./destination-configuration.md#example-configuration)で設定例のパラメーターを参照してください。グループプロファイルを ID 名前空間でご使用の宛先に書き出す場合、これを `true` に設定します。 |
| `aggregation.configurableAggregation.aggregationKey.oneIdentityPerGroup` | ブール値 | [こちら](./destination-configuration.md#example-configuration)で設定例のパラメーターを参照してください。このパラメーターを使用して、書き出されたプロファイルを単一の ID のグループ（GAID、IDFA、電話番号、メールなど）に集計するかどうかを指定します。 |
| `aggregation.configurableAggregation.aggregationKey.groups` | 文字列 | [ここ](./destination-configuration.md#example-configuration)で設定例のパラメーターを参照してください。宛先に書き出されたグループプロファイルを ID 名前空間のグループ別にグループ化する場合は、ID グループのリストを作成します。例えば、IDFA および GAID モバイル識別子を含むプロファイルを、例の設定を使用して、宛先への呼び出しと、別のメールへと組み合わせることができます。 |

{style=&quot;table-layout:auto&quot;}

**応答**

成功時の応答は、HTTP ステータス 200 と共に、新しく作成された宛先の詳細を返します。

## ファイルベースの宛先の設定を作成する {#create-file-based}

`/authoring/destinations` エンドポイントに POST リクエストを実行することで、新しい宛先設定を作成できます。

**API 形式**

```http
POST /authoring/destinations
```

**リクエスト**

次のリクエストは、ペイロードで指定されたパラメーターによって設定された、新しい [!DNL Amazon S3] ファイルベースの宛先を作成します。以下のペイロードには、`/authoring/destinations` エンドポイントで利用可能なファイルベースの宛先のパラメーターをすべて含みます。呼び出しにすべてのパラメーターを追加する必要はなく、テンプレートは API 要件に応じてカスタマイズできることに注意してください。

```shell
curl -X POST https://platform.adobe.io/data/core/activation/authoring/destinations \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d '
{
        "name": "S3 Destination with CSV Options",
        "description": "S3 Destination with CSV Options",
        "releaseNotes": "S3 Destination with CSV Options",
        "status": "TEST",
        "customerAuthenticationConfigurations": [
            {
                "authType": "S3"
            }
        ],
        "customerEncryptionConfigurations": [
            {
                "encryptionAlgo": ""
            }
        ],
        "customerDataFields": [
            {
                "name": "bucket",
                "title": "Select S3 Bucket",
                "description": "Select S3 Bucket",
                "type": "string",
                "isRequired": true,
                "readOnly": false,
                "hidden": false
            },
            {
                "name": "path",
                "title": "S3 path",
                "description": "Select S3 Bucket",
                "type": "string",
                "isRequired": true,
                "pattern": "^[A-Za-z]+$",
                "readOnly": false,
                "hidden": false
            },
            {
                "name": "sep",
                "title": "Select separator for each field and value",
                "description": "Select for each field and value",
                "type": "string",
                "isRequired": false,
                "readOnly": false,
                "hidden": false
            },
            {
                "name": "encoding",
                "title": "Specify encoding (charset) of saved CSV files",
                "description": "Select encoding of csv files",
                "type": "string",
                "enum": ["UTF-8", "UTF-16"],
                "isRequired": false,
                "readOnly": false,
                "hidden": false
            },
            {
                "name": "quote",
                "title": "Select a single character used for escaping quoted values",
                "description": "Select single charachter for escaping quoted values",
                "type": "string",
                "isRequired": false,
                "readOnly": false,
                "hidden": false
            },
            {
                "name": "quoteAll",
                "title": "Quote All",
                "description": "Select flag for escaping quoted values",
                "type": "string",
                "enum" : ["true","false"],
                "default": "true",
                "isRequired": true,
                "readOnly": false,
                "hidden": false
            },
             {
                "name": "escape",
                "title": "Select a single character used for escaping quotes",
                "description": "Select a single character used for escaping quotes inside an already quoted value",
                "type": "string",
                "isRequired": false,
                "readOnly": false,
                "hidden": false
            },
            {
                "name": "escapeQuotes",
                "title": "Escape quotes",
                "description": "A flag indicating whether values containing quotes should always be enclosed in quotes",
                "type": "string",
                "enum" : ["true","false"],
                "isRequired": false,
                "default": "true",
                "readOnly": false,
                "hidden": false
            },
            {
                "name": "header",
                "title": "header",
                "description": "Writes the names of columns as the first line.",
                "type": "string",
                "isRequired": false,
                "enum" : ["true","false"],
                "readOnly": false,
                "default": "true",
                "hidden": false
            },
            {
                "name": "ignoreLeadingWhiteSpace",
                "title": "Ignore leading white space",
                "description": "A flag indicating whether or not leading whitespaces from values being written should be skipped.",
                "type": "string",
                "isRequired": false,
                "enum" : ["true","false"],
                "readOnly": false,
                "default": "true",
                "hidden": false
            },
            {
                "name": "nullValue",
                "title": "Select the string representation of a null value",
                "description": "Sets the string representation of a null value. ",
                "type": "string",
                "isRequired": false,
                "readOnly": false,
                "hidden": false
            },
            {
                "name": "dateFormat",
                "title": "Date format",
                "description": "Select the string that indicates a date format. ",
                "type": "string",
                "default": "yyyy-MM-dd",
                "isRequired": false,
                "readOnly": false,
                "hidden": false
            },
             {
                "name": "charToEscapeQuoteEscaping",
                "title": "Char to escape quote escaping",
                "description": "Sets a single character used for escaping the escape for the quote character",
                "type": "string",
                "isRequired": false,
                "readOnly": false,
                "hidden": false
            },
            {
                "name": "emptyValue",
                "title": "Select the string representation of an empty value",
                "description": "Select the string representation of an empty value",
                "type": "string",
                "isRequired": false,
                "readOnly": false,
                "default": "",
                "hidden": false
            },
            {
                "name": "compression",
                "title": "Select compression",
                "description": "Select compressiont",
                "type": "string",
                "isRequired": true,
                "readOnly": false,
                "enum" : ["SNAPPY","GZIP","DEFLATE", "NONE"]
            },
            {
                "name": "fileType",
                "title": "Select a fileType",
                "description": "Select fileType",
                "type": "string",
                "isRequired": true,
                "readOnly": false,
                "hidden": false,
                "enum" :["csv", "json", "parquet"],
                "default" : "csv"
            }
        ],
        "uiAttributes": {
            "documentationLink": "https://www.adobe.io/apis/experienceplatform.html",
            "category": "S3",
            "iconUrl": "https://dc5tqsrhldvnl.cloudfront.net/2/90048/da276e30c730ce6cd666c8ca78360df21.png",
            "connectionType": "S3",
            "flowRunsSupported": true,
            "monitoringSupported": true,
            "frequency": "Batch"
        },
        "destinationDelivery": [
            {
                "deliveryMatchers" : [
                    {
                        "type" : "SOURCE",
                        "value" : [
                            "batch"
                        ]
                    }
                ],
                "authenticationRule": "CUSTOMER_AUTHENTICATION",
                "destinationServerId": "{{destinationServerId}}"
            }
        ],
        "schemaConfig" : {
            "profileRequired" : true,
            "segmentRequired" : true,
            "identityRequired" : true
        },
        "batchConfig":{
            "allowMandatoryFieldSelection": true,
            "allowJoinKeyFieldSelection": true,
            "defaultExportMode": "DAILY_FULL_EXPORT",
            "allowedExportMode":[
                "DAILY_FULL_EXPORT",
                "FIRST_FULL_THEN_INCREMENTAL"
            ],
            "allowedScheduleFrequency":[
                "DAILY",
                "EVERY_3_HOURS",
                "EVERY_6_HOURS",
                "EVERY_8_HOURS",
                "EVERY_12_HOURS",
                "ONCE",
                "EVERY_HOUR"
            ],
            "defaultFrequency":"DAILY",
            "defaultStartTime":"00:00"
        },
        "backfillHistoricalProfileData": true
    }
```

上記のすべてのパラメーターについて詳しくは、[ファイルベースの宛先設定](file-based-destination-configuration.md)を参照してください。

**応答**

成功時の応答は、HTTP ステータス 200 と共に、新しく作成された宛先の詳細を返します。

## 宛先設定のリスト {#retrieve-list}

IMS 組織のすべての宛先設定のリストを取得するには、`/authoring/destinations` エンドポイントに GET リクエストを作成します。

**API 形式**


```http
GET /authoring/destinations
```

**リクエスト**

次のリクエストは、IMS 組織とサンドボックス設定に基づいて、自身がアクセス権を持つ宛先設定のリストを取得します。

```shell
curl -X GET https://platform.adobe.io/data/core/activation/authoring/destinations \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

次の応答は、使用した IMS 組織 ID とサンドボックス名に基づいて、HTTP ステータス 200 と、アクセス権のある宛先設定のリストを返します。1 つの `instanceId` は、1 つの宛先のテンプレートに対応します。簡潔にするために、応答は切り捨てられます。

```json
{
   "items":[
      {
         "instanceId":"b0780cb5-2bb7-4409-bf2c-c625ca818588",
         "createdDate":"2020-10-28T06:14:09.784471Z",
         "lastModifiedDate":"2021-06-28T06:14:09.784471Z",
         "imsOrg":"AC3428435BF324E90A49402A@AdobeOrg",
         "sandboxName":"prod",
         "sandboxId":"r5g6660-c5da-11e9-93d4-6d5fc3a66a8e",
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
               "pattern":"^[A-Za-z]+$"
            }
         ],
         "uiAttributes":{
            "documentationLink":"https://www.adobe.com/go/destinations-moviestar-en",
            "category":"mobile",
            "connectionType":"Server-to-server",
            "frequency":"Streaming"
         },
         "identityNamespaces":{
            "external_id":{
               "acceptsAttributes":true,
               "acceptsCustomNamespaces":true,
               "acceptedGlobalNamespaces":{
                  "Email":{
                     
                  }
               }
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
         "schemaConfig":{
            "profileFields":[
               {
                  "name":"a_custom_attribute",
                  "title":"a_custom_attribute",
                  "description":"This is a fixed attribute on your destination side that customers can map profile attributes to. For example, the phoneNumber value in Experience Platform could be phoneNo on your side.",
                  "type":"string",
                  "isRequired":false,
                  "readOnly":false,
                  "hidden":false
               }
            ],
            "profileRequired":true,
            "segmentRequired":true,
            "identityRequired":true
         },
         "aggregation":{
            "aggregationType":"BEST_EFFORT",
            "bestEffortAggregation":{
               "maxUsersPerRequest":10,
               "splitUserById":false
            }
         },
         "destinationDelivery":[
            {
               "authenticationRule":"CUSTOMER_AUTHENTICATION",
               "destinationServerId":"9c77000a-4559-40ae-9119-a04324a3ecd4"
            }
         ],
         "destConfigId":"410631b8-f6b3-4b7c-82da-7998aa3f327c",
         "backfillHistoricalProfileData":true
      }
   ]
}
    
```

| パラメーター | タイプ | 説明 |
|---------|----------|------|
| `name` | 文字列 | Experience Platform カタログ内の宛先のタイトルを示します。 |
| `description` | 文字列 | アドビが宛先カードの Experience Platform 宛先カタログで使用する説明を入力します。4 ～ 5 文以下を目指します。 |
| `status` | 文字列 | 宛先カードのライフサイクルステータスを示します。 指定できる値は、`TEST`、`PUBLISHED`、`DELETED` です。最初に宛先を設定する際は `TEST` を使用します。 |
| `customerAuthenticationConfigurations` | 文字列 | Experience Platform の顧客をサーバーで認証するために使用される構成を示します。 使用可能な値については、下記の `authType` を参照してください。 |
| `customerAuthenticationConfigurations.authType` | 文字列 | 指定できる値は、`OAUTH2, BEARER` です。 |
| `customerDataFields.name` | 文字列 | 導入するカスタムフィールドの名前を記入します。 |
| `customerDataFields.type` | 文字列 | 導入するカスタムフィールドのタイプを示します。 指定できる値は、`string`、`object`、`integer` です。 |
| `customerDataFields.title` | 文字列 | Experience Platform ユーザーインターフェイスで顧客に表示されるフィールドの名前を示します |
| `customerDataFields.description` | 文字列 | カスタムフィールドの説明を入力します。 |
| `customerDataFields.isRequired` | ブール値 | このフィールドが宛先設定ワークフローで必須かどうかを示します。 |
| `customerDataFields.enum` | 文字列 | カスタムフィールドをドロップダウンメニューとしてレンダリングし、ユーザーが使用できるオプションを一覧表示します。 |
| `customerDataFields.pattern` | 文字列 | 必要に応じて、カスタムフィールドのパターンを適用します。正規表現を使用して、パターンを適用します。 例えば、顧客 ID に数字やアンダースコアが含まれない場合は、このフィールドで `^[A-Za-z]+$` を入力します。 |
| `uiAttributes.documentationLink` | 文字列 | 宛先用の[宛先のカタログ](https://experienceleague.adobe.com/docs/experience-platform/destinations/catalog/overview.html?lang=en#catalog)にあるドキュメントページを参照します。`https://www.adobe.com/go/destinations-YOURDESTINATION-en` を使用します。ここでは、`YOURDESTINATION` は宛先の名前です。Moviestar という宛先の場合、`https://www.adobe.com/go/destinations-moviestar-en` を使用します。。このリンクは、Adobeが宛先をライブに設定し、ドキュメントが公開された後にのみ機能します。 |
| `uiAttributes.category` | 文字列 | Adobe Experience Platform で宛先に割り当てられたカテゴリを参照します。 詳しくは、[宛先カテゴリ](https://experienceleague.adobe.com/docs/experience-platform/rtcdp/destinations/destination-types.html?lang=en#destination-categories)を参照してください。次のいずれかの値を使用します。`adobeSolutions, advertising, analytics, cdp, cloudStorage, crm, customerSuccess, database, dmp, ecommerce, email, emailMarketing, enrichment, livechat, marketingAutomation, mobile, personalization, protocols, social, streaming, subscriptions, surveys, tagManagers, voc, warehouses, payments` |
| `uiAttributes.connectionType` | 文字列 | `Server-to-server` は現在利用可能な唯一のオプションです。 |
| `uiAttributes.frequency` | 文字列 | `Streaming` は現在唯一の利用可能なオプションです。 |
| `identityNamespaces.externalId.acceptsAttributes` | ブール値 | 宛先が標準のプロファイル属性を受け入れるかどうかを示します。 通常、これらの属性はパートナーのドキュメントでハイライト表示されます。 |
| `identityNamespaces.externalId.acceptsCustomNamespaces` | ブール値 | 顧客が宛先でカスタム名前空間を設定できるかどうかを示します。詳しくは、Adobe Experience Platform の[カスタム名前空間](https://experienceleague.adobe.com/docs/experience-platform/identity/namespaces.html?lang=ja#manage-namespaces)を参照してください。 |
| `identityNamespaces.externalId.transformation` | 文字列 | _サンプル設定には表示されません_。例えば、[!DNL Platform] の顧客がプレーンなメールアドレスを属性として持っており、プラットフォームがハッシュ化されたメールのみを受け取る場合に使用します。ここで、適用する必要のある変換（例えば、メールを小文字に変換してからハッシュ化するなど）を行います。 |
| `identityNamespaces.externalId.acceptedGlobalNamespaces` | - | プラットフォームが[標準 ID 名前空間](https://experienceleague.adobe.com/docs/experience-platform/identity/namespaces.html?lang=en#standard-namespaces)（例えば IDFA）を受け入れる場合に使用します。これにより、Platform ユーザーがこれらの ID 名前空間のみを選択するように制限できます。 |
| `destinationDelivery.authenticationRule` | 文字列 | [!DNL Platform] の顧客が宛先に接続する方法を示します。使用できる値は `CUSTOMER_AUTHENTICATION`、`PLATFORM_AUTHENTICATION`、`NONE`、<br> です。 <ul><li>Platform の顧客がユーザー名とパスワード、ベアラートークン、または他の認証方法を使用してシステムにログインする場合は、`CUSTOMER_AUTHENTICATION` を使用します。例えば、`customerAuthenticationConfigurations` で `authType: OAUTH2` や `authType:BEARER` も選択した場合、このオプションを選択することになります。 </li><li> アドビと接続先との間にグローバル認証システムがあり、[!DNL Platform] の顧客が接続先に認証資格情報を提供する必要がない場合は、`PLATFORM_AUTHENTICATION` を使用してください。この場合、[資格情報](./authentication-configuration.md)の構成を使用して、資格情報オブジェクトを作成する必要があります。 </li><li>宛先プラットフォームにデータを送信するために認証が必要ない場合は、`NONE` を使用します。 </li></ul> |
| `destinationDelivery.destinationServerId` | 文字列 | この宛先に使用される[宛先サーバーテンプレート](./destination-server-api.md)の `instanceId` です。 |
| `destConfigId` | 文字列 | このフィールドは自動的に生成され、入力は不要です。 |
| `backfillHistoricalProfileData` | ブール値 | 宛先に対してセグメントをアクティブ化する際に、履歴プロファイルデータを書き出すかどうかを制御します。 <br> <ul><li> `true`：[!DNL Platform] は、セグメントがアクティブ化される前に、セグメントに適格となる履歴ユーザープロファイルを送信します。 </li><li> `false`：[!DNL Platform] には、セグメントが有効化された後にセグメントに適格となるユーザーのプロファイルのみが含まれます。 </li></ul> |
| `segmentMappingConfig.mapUserInput` | ブール値 | 宛先のアクティベーションワークフローのセグメントマッピング ID をユーザーが入力するかどうかを制御します。 |
| `segmentMappingConfig.mapExperiencePlatformSegmentId` | ブール値 | 宛先のアクティベーションワークフローのセグメントマッピング ID が Adobe Experience Platform のセグメント ID かどうかを制御します。 |
| `segmentMappingConfig.mapExperiencePlatformSegmentName` | ブール値 | 宛先のアクティベーションワークフローのセグメントマッピング ID が Adobe Experience Platform のセグメント名かどうかを制御します。 |
| `segmentMappingConfig.audienceTemplateId` | ブール値 | この宛先に使用される[オーディエンスメタデータテンプレート](./audience-metadata-management.md)の `instanceId`。オーディエンスのメタデータテンプレートを設定するには、[オーディエンスメタデータ API リファレンス](./audience-metadata-api.md)を参照してください。 |

{style=&quot;table-layout:auto&quot;}

## 既存の宛先設定の更新 {#update}

既存の宛先設定を更新するには、`/authoring/destinations` エンドポイントに PUT リクエストを実行し、更新したい宛先設定のインスタンス ID を指定します。呼び出しの本文で、更新した宛先設定を指定します。

**API 形式**


```http
PUT /authoring/destinations/{INSTANCE_ID}
```

| パラメーター | 説明 |
| -------- | ----------- |
| `{INSTANCE_ID}` | 更新する宛先設定の ID。 |

**リクエスト**

次のリクエストは、ペイロード内のパラメーター設定に基づいて、既存の宛先設定を更新します。以下の呼び出しの例では、[以前に作成した](./destination-configuration-api.md#create)設定を更新しています。これにより、GAID、IDFA、ハッシュ化された電子メール ID を ID 名前空間として受け入れるようになります。

```shell
curl -X PUT https://platform.adobe.io/data/core/activation/authoring/destinations/b0780cb5-2bb7-4409-bf2c-c625ca818588 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d '
{
   "instanceId":"b0780cb5-2bb7-4409-bf2c-c625ca818588",
   "createdDate":"2020-10-28T06:14:09.784471Z",
   "lastModifiedDate":"2021-04-28T06:14:09.784471Z",
   "imsOrg":"AC3428435BF324E90A49402A@AdobeOrg",
   "sandboxName":"prod",
   "sandboxId":"r5g6660-c5da-11e9-93d4-6d5fc3a66a8e",
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
         "pattern":"^[A-Za-z]+$"
      }
   ],
   "uiAttributes":{
      "documentationLink":"https://www.adobe.com/go/destinations-moviestar-en",
      "category":"mobile",
      "connectionType":"Server-to-server",
      "frequency":"Streaming"
   },
   "identityNamespaces":{
      "external_id":{
         "acceptsAttributes":true,
         "acceptsCustomNamespaces":true,
         "acceptedGlobalNamespaces":{
            "Email":{
            }
         }
      },
      "another_id":{
         "acceptsAttributes":true,
         "acceptsCustomNamespaces":true
      },
      "gaid":{
         "acceptsAttributes":true,
         "acceptsCustomNamespaces":true,
         "acceptedGlobalNamespaces":{
            "GAID":{
               
            }
         }
      },
      "idfa":{
         "acceptsAttributes":true,
         "acceptsCustomNamespaces":true,
         "acceptedGlobalNamespaces":{
            "IDFA":{
               
            }
         }
      },
      "email_lc_sha256":{
         "acceptsAttributes":true,
         "acceptsCustomNamespaces":true,
         "transformation":"sha256(lower($))",
         "acceptedGlobalNamespaces":{
            "Email":{
               "requiredTransformation":"sha256(lower($))"
            },
            "Email_LC_SHA256":{
               
            }
         }
      }
   },
   "segmentMappingConfig":{
      "mapExperiencePlatformSegmentName":false,
      "mapExperiencePlatformSegmentId":false,
      "mapUserInput":false,
      "audienceTemplateId":"cbf90a70-96b4-437b-86be-522fbdaabe9c"
   },
   "schemaConfig":{
      "profileFields":[
         {
            "name":"a_custom_attribute",
            "title":"a_custom_attribute",
            "description":"This is a fixed attribute on your destination side that customers can map profile attributes to. For example, the phoneNumber value in Experience Platform could be phoneNo on your side.",
            "type":"string",
            "isRequired":false,
            "readOnly":false,
            "hidden":false
         }
      ],
      "profileRequired":true,
      "segmentRequired":true,
      "identityRequired":true
   },
   "aggregation":{
      "aggregationType":"BEST_EFFORT",
      "bestEffortAggregation":{
         "maxUsersPerRequest":10,
         "splitUserById":false
      }
   },
   "destinationDelivery":[
      {
         "authenticationRule":"CUSTOMER_AUTHENTICATION",
         "destinationServerId":"9c77000a-4559-40ae-9119-a04324a3ecd4"
      }
   ],
   "backfillHistoricalProfileData":true
}
```

## 特定の宛先設定の取得 {#get}

`/authoring/destinations` エンドポイントに GET リクエストを実行し、取得する宛先設定のインスタンス ID を指定することで、特定の宛先設定に関する詳細な情報を取得できます。

**API 形式**


```http
GET /authoring/destinations/{INSTANCE_ID}
```

| パラメーター | 説明 |
| -------- | ----------- |
| `{INSTANCE_ID}` | 取得する宛先設定の ID。 |

**リクエスト**

```shell
curl -X GET https://platform.adobe.io/data/core/activation/authoring/destinations/b0780cb5-2bb7-4409-bf2c-c625ca818588 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

リクエストが成功した場合は、HTTP ステータス 200と指定した宛先設定の詳細が返されます。

```json
{
   "instanceId":"b0780cb5-2bb7-4409-bf2c-c625ca818588",
   "createdDate":"2020-10-28T06:14:09.784471Z",
   "lastModifiedDate":"2021-06-04T06:14:09.784471Z",
   "imsOrg":"AC3428435BF324E90A49402A@AdobeOrg",
   "sandboxName":"prod",
   "sandboxId":"r5g6660-c5da-11e9-93d4-6d5fc3a66a8e",
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
         "pattern":"^[A-Za-z]+$"
      }
   ],
   "uiAttributes":{
      "documentationLink":"https://www.adobe.com/go/destinations-moviestar-en",
      "category":"mobile",
      "connectionType":"Server-to-server",
      "frequency":"Streaming"
   },
   "identityNamespaces":{
      "external_id":{
         "acceptsAttributes":true,
         "acceptsCustomNamespaces":true,
         "acceptedGlobalNamespaces":{
            "Email":{
               
            }
         }
      },
      "another_id":{
         "acceptsAttributes":true,
         "acceptsCustomNamespaces":true
      },
      "gaid":{
         "acceptsAttributes":true,
         "acceptsCustomNamespaces":true,
         "acceptedGlobalNamespaces":{
            "GAID":{
               
            }
         }
      },
      "idfa":{
         "acceptsAttributes":true,
         "acceptsCustomNamespaces":true,
         "acceptedGlobalNamespaces":{
            "IDFA":{
               
            }
         }
      },
      "email_lc_sha256":{
         "acceptsAttributes":true,
         "acceptsCustomNamespaces":true,
         "transformation":"sha256(lower($))",
         "acceptedGlobalNamespaces":{
            "Email":{
               "requiredTransformation":"sha256(lower($))"
            },
            "Email_LC_SHA256":{
               
            }
         }
      }
   },
   "segmentMappingConfig":{
      "mapExperiencePlatformSegmentName":false,
      "mapExperiencePlatformSegmentId":false,
      "mapUserInput":false,
      "audienceTemplateId":"cbf90a70-96b4-437b-86be-522fbdaabe9c"
   },
   "schemaConfig":{
      "profileFields":[
         {
            "name":"a_custom_attribute",
            "title":"a_custom_attribute",
            "description":"This is a fixed attribute on your destination side that customers can map profile attributes to. For example, the phoneNumber value in Experience Platform could be phoneNo on your side.",
            "type":"string",
            "isRequired":false,
            "readOnly":false,
            "hidden":false
         }
      ],
      "profileRequired":true,
      "segmentRequired":true,
      "identityRequired":true
   },
   "aggregation":{
      "aggregationType":"BEST_EFFORT",
      "bestEffortAggregation":{
         "maxUsersPerRequest":10,
         "splitUserById":false
      }
   },
   "destinationDelivery":[
      {
         "authenticationRule":"CUSTOMER_AUTHENTICATION",
         "destinationServerId":"9c77000a-4559-40ae-9119-a04324a3ecd4"
      }
   ],
   "backfillHistoricalProfileData":true
}
```


## 特定の宛先設定の削除 {#delete}

`/authoring/destinations` エンドポイントに DELETE リクエストを実行し、リクエストパスで削除する宛先設定の ID を指定することで、指定した宛先設定を削除できます。

**API 形式**

```http
DELETE /authoring/destinations/{INSTANCE_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{INSTANCE_ID}` | 削除する宛先設定の `id`。 |

**リクエスト**

```shell
curl -X DELETE https://platform.adobe.io/data/core/activation/authoring/destinations/b0780cb5-2bb7-4409-bf2c-c625ca818588 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
```

**応答**

リクエストが成功した場合は、空の HTTP 応答とともに HTTP ステータス 200 が返されます。

## API エラー処理

Destination SDK API エンドポイントは、一般的な Experience Platform API エラーメッセージの原則に従います。Platform トラブルシューティングガイドの [API ステータスコード](../../landing/troubleshooting.md#api-status-codes)および[リクエストヘッダーエラー](../../landing/troubleshooting.md#request-header-errors)を参照してください。

## 次の手順

このドキュメントでは、`/authoring/destinations` API エンドポイントを使用して宛先を設定する方法を確認しました。[Destination SDK を使用して宛先を設定する方法](./configure-destination-instructions.md)を参照して、宛先を設定するプロセスにおいてこのステップが当てはまる箇所を把握してください。
