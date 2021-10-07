---
description: このページでは、「/authoring/destinations」 API エンドポイントを使用して実行できるすべての API 操作について説明します。
title: 宛先 API エンドポイントの操作
exl-id: 96755e9d-be62-432f-b985-91330575b395
source-git-commit: 76a596166edcdbf141b5ce5dc01557d2a0b4caf3
workflow-type: tm+mt
source-wordcount: '2405'
ht-degree: 5%

---

# 宛先エンドポイント API 操作 {#destination-configuration}

>[!IMPORTANT]
>
>**API エンドポイント**: `platform.adobe.io/data/core/activation/authoring/destinations`

このページでは、`/authoring/destinations` API エンドポイントを使用して実行できるすべての API 操作について説明します。 このエンドポイントでサポートされる機能の説明については、[ 宛先の設定 ](./destination-configuration.md) を参照してください。

## 宛先 API 操作の概要 {#get-started}

続行する前に、[ はじめに ](./getting-started.md) を参照して、必要な宛先オーサリング権限や必要なヘッダーの取得方法など、API を正しく呼び出すために知っておく必要がある重要な情報を確認してください。

## 宛先の設定の作成 {#create}

`/authoring/destinations` エンドポイントにPOSTリクエストを送信して、新しい宛先設定を作成できます。

**API 形式**


```http
POST /authoring/destinations
```

**リクエスト**

次のリクエストは、ペイロードで指定されたパラメーターで設定された新しい宛先設定を作成します。 以下のペイロードには、`/authoring/destinations` エンドポイントが受け入れるすべてのパラメーターが含まれています。 すべてのパラメーターを呼び出しに追加する必要はなく、API の要件に従ってテンプレートをカスタマイズできます。

```shell
curl -X POST https://platform.adobe.io/data/core/activation/authoring/destinations \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
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
| `name` | 文字列 | 宛先カタログ内の宛先のタイトルを示すExperience Platform |
| `description` | 文字列 | Adobeが宛先カードの宛先カタログで使用するExperience Platformの説明を入力します。 4～5 文以下を目指す。 |
| `status` | 文字列 | 宛先カードのライフサイクルステータスを示します。 指定できる値は、`TEST`、`PUBLISHED`、`DELETED` です。宛先を最初に設定する際は、`TEST` を使用します。 |
| `customerAuthenticationConfigurations` | 文字列 | サーバーへのExperience Platform顧客の認証に使用する設定を示します。 受け入れられる値については、以下の `authType` を参照してください。 |
| `customerAuthenticationConfigurations.authType` | 文字列 | 指定できる値は `OAUTH2, BEARER` です。 |
| `customerDataFields.name` | 文字列 | 紹介するカスタムフィールドの名前を指定します。 |
| `customerDataFields.type` | 文字列 | 導入するカスタムフィールドの種類を示します。 指定できる値は `string`、`object`、`integer` です。 |
| `customerDataFields.title` | 文字列 | ユーザーインターフェイスでユーザーに表示されるフィールドの名前をExperience Platformします |
| `customerDataFields.description` | 文字列 | カスタムフィールドの説明を入力します。 |
| `customerDataFields.isRequired` | Boolean | このフィールドが宛先設定ワークフローで必須かどうかを示します。 |
| `customerDataFields.enum` | 文字列 | カスタムフィールドをドロップダウンメニューとしてレンダリングし、ユーザーが使用できるオプションを一覧表示します。 |
| `customerDataFields.pattern` | 文字列 | 必要に応じて、カスタムフィールドのパターンを適用します。 正規表現を使用してパターンを適用します。 例えば、顧客 ID に数字やアンダースコアが含まれない場合は、このフィールドに `^[A-Za-z]+$` と入力します。 |
| `uiAttributes.documentationLink` | 文字列 | 宛先の [ 宛先カタログ ](https://experienceleague.adobe.com/docs/experience-platform/destinations/catalog/overview.html?lang=en#catalog) のドキュメントページを参照します。 `https://www.adobe.com/go/destinations-YOURDESTINATION-en` を使用します。`YOURDESTINATION` は宛先の名前です。 Moviestar という宛先の場合は、`https://www.adobe.com/go/destinations-moviestar-en` を使用します。 |
| `uiAttributes.category` | 文字列 | Adobe Experience Platformで宛先に割り当てられたカテゴリを指定します。 詳しくは、[ 宛先カテゴリ ](https://experienceleague.adobe.com/docs/experience-platform/rtcdp/destinations/destination-types.html?lang=en#destination-categories) を参照してください。 次のいずれかの値を使用します。`adobeSolutions, advertising, analytics, cdp, cloudStorage, crm, customerSuccess, database, dmp, ecommerce, email, emailMarketing, enrichment, livechat, marketingAutomation, mobile, personalization, protocols, social, streaming, subscriptions, surveys, tagManagers, voc, warehouses, payments`. |
| `uiAttributes.connectionType` | 文字列 | `Server-to-server` は現在、唯一のオプションです。 |
| `uiAttributes.frequency` | 文字列 | `Streaming` は現在、唯一のオプションです。 |
| `identityNamespaces.externalId.acceptsAttributes` | Boolean | 宛先が標準のプロファイル属性を受け入れるかどうかを示します。 通常、これらの属性はパートナーのドキュメントで強調表示されます。 |
| `identityNamespaces.externalId.acceptsCustomNamespaces` | Boolean | 顧客が宛先にカスタム名前空間を設定できるかどうかを示します。 |
| `identityNamespaces.externalId.allowedAttributesTransformation` | 文字列 | _サンプルの設定_&#x200B;では示されていません。例えば、[!DNL Platform] の顧客が属性としてプレーンな電子メールアドレスを持ち、プラットフォームがハッシュ化された電子メールのみを受け入れる場合に使用します。 ここで、適用する必要がある変換（例えば、E メールを小文字に変換し、ハッシュ化）を指定します。 |
| `identityNamespaces.externalId.acceptedGlobalNamespaces` | - | プラットフォームが [ 標準 ID 名前空間 ](https://experienceleague.adobe.com/docs/experience-platform/identity/namespaces.html?lang=en#standard-namespaces)（例えば、IDFA）を受け入れる場合に使用されるので、Platform ユーザーは、これらの ID 名前空間のみを選択するように制限できます。 <br> を使用する場合、を使用 `acceptedGlobalNamespaces`して、電子メールア `"requiredTransformation":"sha256(lower($))"` ドレスや電話番号を小文字にしてハッシュ化できます。 |
| `destinationDelivery.authenticationRule` | 文字列 | [!DNL Platform] ユーザーが宛先に接続する方法を示します。 指定できる値は `CUSTOMER_AUTHENTICATION`、`PLATFORM_AUTHENTICATION`、`NONE` です。<br> <ul><li>Platform のお客様が、ユーザー名とパスワード、ベアラートークン、または別の認証方法を使用してシステムにログインする場合は、`CUSTOMER_AUTHENTICATION` を使用します。 例えば、`customerAuthenticationConfigurations` で `authType: OAUTH2` または `authType:BEARER` も選択した場合は、このオプションを選択します。 </li><li> Adobeと宛先の間にグローバル認証システムがあり、`PLATFORM_AUTHENTICATION` 顧客が宛先に接続するための認証資格情報を提供する必要がない場合は、[!DNL Platform] を使用します。 この場合、[Credentials](./credentials-configuration.md) 設定を使用して credentials オブジェクトを作成する必要があります。 </li><li>宛先プラットフォームにデータを送信するために認証が必要ない場合は、`NONE` を使用します。 </li></ul> |
| `destinationDelivery.destinationServerId` | 文字列 | この宛先に使用する [ 宛先サーバーテンプレート ](./destination-server-api.md) の `instanceId`。 |
| `backfillHistoricalProfileData` | Boolean | 宛先に対してセグメントをアクティブ化した場合に、履歴プロファイルデータを書き出すかどうかを制御します。<br> <ul><li> `true`: [!DNL Platform] は、セグメントがアクティブ化される前にセグメントに適合した過去のユーザープロファイルを送信します。 </li><li> `false`: [!DNL Platform] には、セグメントがアクティブ化された後にセグメントに適合するユーザープロファイルのみが含まれます。 </li></ul> |
| `segmentMappingConfig.mapUserInput` | Boolean | 宛先のアクティベーションワークフローのセグメントマッピング ID をユーザーが入力するかどうかを制御します。 |
| `segmentMappingConfig.mapExperiencePlatformSegmentId` | Boolean | 宛先アクティベーションワークフローのセグメントマッピング ID がExperience Platformセグメント ID かどうかを制御します。 |
| `segmentMappingConfig.mapExperiencePlatformSegmentName` | Boolean | 宛先アクティベーションワークフローのセグメントマッピング ID がExperience Platformセグメント名かどうかを制御します。 |
| `segmentMappingConfig.audienceTemplateId` | Boolean | この宛先で使用される [ オーディエンスメタデータテンプレート ](./audience-metadata-api.md) の `instanceId`。 |
| `schemaConfig.profileFields` | 配列 | 上記の設定で示したように、定義済みの `profileFields` を追加すると、Experience Platform属性を宛先側の定義済み属性にマッピングするオプションが表示されます。 |
| `schemaConfig.profileRequired` | Boolean | 上記の設定例に示すように、Experience Platformから宛先のカスタム属性にプロファイル属性をマッピングできる場合は、`true` を使用します。 |
| `schemaConfig.segmentRequired` | Boolean | 常に `segmentRequired:true` を使用します。 |
| `schemaConfig.identityRequired` | Boolean | ユーザーが ID 名前空間をExperience Platformから目的のスキーマにマッピングできる場合は、`true` を使用します。 |
| `aggregation.aggregationType` | - | 「`BEST_EFFORT`」または「`CONFIGURABLE_AGGREGATION`」を選択します。上記の例の設定には、`BEST_EFFORT` 集計が含まれています。 `CONFIGURABLE_AGGREGATION` の例については、[ 宛先の設定 ](./destination-configuration.md#example-configuration) ドキュメントの例の設定を参照してください。 設定可能な集計に関連するパラメータを次の表に示します。 |
| `aggregation.bestEffortAggregation.maxUsersPerRequest` | 整数 | Experience Platformは、1 回の HTTP 呼び出しで、書き出された複数のプロファイルを集計できます。 1 回の HTTP 呼び出しでエンドポイントが受け取るプロファイルの最大数を指定します。 これは、ベストエフォートの集計です。 例えば、値 100 を指定すると、1 回の呼び出しで、Platform は 100 未満の任意の数のプロファイルを送信できます。 <br> サーバーが 1 回の要求で複数のユーザーを受け入れない場合、この値を 1 に設定します。 |
| `aggregation.bestEffortAggregation.splitUserById` | Boolean | 宛先への呼び出しを ID で分割する必要がある場合は、このフラグを使用します。 サーバーが呼び出しごとに 1 つの ID しか受け入れない場合、このフラグを `true` に設定します。 |
| `aggregation.configurableAggregation.splitUserById` | Boolean | 設定例のパラメータ [ ここ ](./destination-configuration.md#example-configuration) を参照してください。 宛先への呼び出しを ID で分割する必要がある場合は、このフラグを使用します。 サーバーが呼び出しごとに 1 つの ID しか受け入れない場合、このフラグを `true` に設定します。 |
| `aggregation.configurableAggregation.maxBatchAgeInSecs` | 整数 | *最大値：3600*&#x200B;を参照してください。設定例のパラメータ [ ここ ](./destination-configuration.md#example-configuration) を参照してください。 `maxNumEventsInBatch` と共に、エンドポイントに API 呼び出しを送信するまでExperience Platformが待機する時間を決定します。 <br> 例えば、両方のパラメーターに最大値を使用した場合、Experience Platformは、3600 秒または 10.000 の修飾済みプロファイルが存在するまで待ってから API 呼び出しをおこないます（いずれか最初に達成される方を待ちます）。 |
| `aggregation.configurableAggregation.maxNumEventsInBatch` | 整数 | *最大値：10000*&#x200B;を参照してください。設定例のパラメータ [ ここ ](./destination-configuration.md#example-configuration) を参照してください。 上の `maxBatchAgeInSecs` を参照してください。 |
| `aggregation.configurableAggregation.aggregationKey` | Boolean | 設定例のパラメータ [ ここ ](./destination-configuration.md#example-configuration) を参照してください。 次のパラメーターに基づいて、宛先にマッピングされた書き出し済みプロファイルを集計できます。<br> <ul><li>セグメント ID</li><li> セグメントのステータス </li><li> ID 名前空間 </li></ul> |
| `aggregation.configurableAggregation.aggregationKey.includeSegmentId` | Boolean | 設定例のパラメータ [ ここ ](./destination-configuration.md#example-configuration) を参照してください。 セグメント ID で宛先に書き出したプロファイルをグループ化する場合は、これを `true` に設定します。 |
| `aggregation.configurableAggregation.aggregationKey.includeSegmentStatus` | Boolean | 設定例のパラメータ [ ここ ](./destination-configuration.md#example-configuration) を参照してください。 宛先に書き出されたプロファイルをセグメント ID とセグメントステータスでグループ化する場合は、 `includeSegmentId:true` と `includeSegmentStatus:true` の両方を設定する必要があります。 |
| `aggregation.configurableAggregation.aggregationKey.includeIdentity` | Boolean | 設定例のパラメータ [ ここ ](./destination-configuration.md#example-configuration) を参照してください。 ID 名前空間で宛先に書き出したプロファイルをグループ化する場合は、これを `true` に設定します。 |
| `aggregation.configurableAggregation.aggregationKey.oneIdentityPerGroup` | Boolean | 設定例のパラメータ [ ここ ](./destination-configuration.md#example-configuration) を参照してください。 このパラメーターを使用して、書き出されたプロファイルを単一の ID（GAID、IDFA、電話番号、電子メールなど）のグループに集計するかどうかを指定します。 |
| `aggregation.configurableAggregation.aggregationKey.groups` | 文字列 | 設定例のパラメータ [ ここ ](./destination-configuration.md#example-configuration) を参照してください。 ID 名前空間のグループで、宛先に書き出したプロファイルをグループ化する場合は、ID グループのリストを作成します。 例えば、IDFA と GAID のモバイル識別子を含むプロファイルを、宛先への呼び出しと、この例の設定を使用して別の電子メールに組み合わせることができます。 |

{style=&quot;table-layout:auto&quot;}

**応答**

正常な応答は、HTTP ステータス 200 と、新しく作成した宛先設定の詳細を返します。

## 宛先設定のリスト {#retrieve-list}

IMS 組織のすべての宛先設定のリストを取得するには、`/authoring/destinations` エンドポイントにGETリクエストを送信します。

**API 形式**


```http
GET /authoring/destinations
```

**リクエスト**

次のリクエストは、IMS 組織とサンドボックス設定に基づいて、アクセス権のある宛先設定のリストを取得します。

```shell
curl -X GET https://platform.adobe.io/data/core/activation/authoring/destinations \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

次の応答は、使用した IMS 組織 ID とサンドボックス名に基づいて、HTTP ステータス 200 と、アクセス権のある宛先設定のリストを返します。 1 つの `instanceId` は、1 つの宛先のテンプレートに対応します。 応答は短く切り捨てられます。

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
| `name` | 文字列 | 宛先カタログ内の宛先のタイトルをExperience Platformします。 |
| `description` | 文字列 | Adobeが宛先カードの宛先カタログで使用するExperience Platformの説明を入力します。 4～5 文以下を目指す。 |
| `status` | 文字列 | 宛先カードのライフサイクルステータスを示します。 指定できる値は、`TEST`、`PUBLISHED`、`DELETED` です。宛先を最初に設定する際は、`TEST` を使用します。 |
| `customerAuthenticationConfigurations` | 文字列 | サーバーへのExperience Platform顧客の認証に使用する設定を示します。 受け入れられる値については、以下の `authType` を参照してください。 |
| `customerAuthenticationConfigurations.authType` | 文字列 | 指定できる値は `OAUTH2, BEARER` です。 |
| `customerDataFields.name` | 文字列 | 紹介するカスタムフィールドの名前を指定します。 |
| `customerDataFields.type` | 文字列 | 導入するカスタムフィールドの種類を示します。 指定できる値は `string`、`object`、`integer` です。 |
| `customerDataFields.title` | 文字列 | ユーザーインターフェイスでユーザーに表示されるフィールドの名前をExperience Platformします |
| `customerDataFields.description` | 文字列 | カスタムフィールドの説明を入力します。 |
| `customerDataFields.isRequired` | Boolean | このフィールドが宛先設定ワークフローで必須かどうかを示します。 |
| `customerDataFields.enum` | 文字列 | カスタムフィールドをドロップダウンメニューとしてレンダリングし、ユーザーが使用できるオプションを一覧表示します。 |
| `customerDataFields.pattern` | 文字列 | 必要に応じて、カスタムフィールドのパターンを適用します。 正規表現を使用してパターンを適用します。 例えば、顧客 ID に数字やアンダースコアが含まれない場合は、このフィールドに `^[A-Za-z]+$` と入力します。 |
| `uiAttributes.documentationLink` | 文字列 | 宛先の [ 宛先カタログ ](https://experienceleague.adobe.com/docs/experience-platform/destinations/catalog/overview.html?lang=en#catalog) のドキュメントページを参照します。 `https://www.adobe.com/go/destinations-YOURDESTINATION-en` を使用します。`YOURDESTINATION` は宛先の名前です。 Moviestar という宛先の場合は、`https://www.adobe.com/go/destinations-moviestar-en` |
| `uiAttributes.category` | 文字列 | Adobe Experience Platformで宛先に割り当てられたカテゴリを指定します。 詳しくは、[ 宛先カテゴリ ](https://experienceleague.adobe.com/docs/experience-platform/rtcdp/destinations/destination-types.html?lang=en#destination-categories) を参照してください。 次のいずれかの値を使用します。`adobeSolutions, advertising, analytics, cdp, cloudStorage, crm, customerSuccess, database, dmp, ecommerce, email, emailMarketing, enrichment, livechat, marketingAutomation, mobile, personalization, protocols, social, streaming, subscriptions, surveys, tagManagers, voc, warehouses, payments` |
| `uiAttributes.connectionType` | 文字列 | `Server-to-server` は現在、唯一のオプションです。 |
| `uiAttributes.frequency` | 文字列 | `Streaming` は現在、唯一のオプションです。 |
| `identityNamespaces.externalId.acceptsAttributes` | Boolean | 宛先が標準のプロファイル属性を受け入れるかどうかを示します。 通常、これらの属性はパートナーのドキュメントで強調表示されます。 |
| `identityNamespaces.externalId.acceptsCustomNamespaces` | Boolean | 顧客が宛先にカスタム名前空間を設定できるかどうかを示します。 詳しくは、Adobe Experience Platformの [ カスタム名前空間 ](https://experienceleague.adobe.com/docs/experience-platform/identity/namespaces.html?lang=en#manage-namespaces) を参照してください。 |
| `identityNamespaces.externalId.allowedAttributesTransformation` | 文字列 | _サンプルの設定_&#x200B;では示されていません。例えば、[!DNL Platform] の顧客が属性としてプレーンな電子メールアドレスを持ち、プラットフォームがハッシュ化された電子メールのみを受け入れる場合に使用します。 ここで、適用する必要がある変換（例えば、E メールを小文字に変換し、ハッシュ化）を指定します。 |
| `identityNamespaces.externalId.acceptedGlobalNamespaces` | - | プラットフォームが [ 標準 ID 名前空間 ](https://experienceleague.adobe.com/docs/experience-platform/identity/namespaces.html?lang=en#standard-namespaces)（例えば、IDFA）を受け入れる場合に使用されるので、Platform ユーザーは、これらの ID 名前空間のみを選択するように制限できます。 |
| `destinationDelivery.authenticationRule` | 文字列 | [!DNL Platform] ユーザーが宛先に接続する方法を示します。 指定できる値は `CUSTOMER_AUTHENTICATION`、`PLATFORM_AUTHENTICATION`、`NONE` です。<br> <ul><li>Platform のお客様が、ユーザー名とパスワード、ベアラートークン、または別の認証方法を使用してシステムにログインする場合は、`CUSTOMER_AUTHENTICATION` を使用します。 例えば、`customerAuthenticationConfigurations` で `authType: OAUTH2` または `authType:BEARER` も選択した場合は、このオプションを選択します。 </li><li> Adobeと宛先の間にグローバル認証システムがあり、`PLATFORM_AUTHENTICATION` 顧客が宛先に接続するための認証資格情報を提供する必要がない場合は、[!DNL Platform] を使用します。 この場合、[Credentials](./credentials-configuration.md) 設定を使用して credentials オブジェクトを作成する必要があります。 </li><li>宛先プラットフォームにデータを送信するために認証が必要ない場合は、`NONE` を使用します。 </li></ul> |
| `destinationDelivery.destinationServerId` | 文字列 | この宛先に使用する [ 宛先サーバーテンプレート ](./destination-server-api.md) の `instanceId`。 |
| `destConfigId` | 文字列 | このフィールドは自動的に生成され、入力は必要ありません。 |
| `backfillHistoricalProfileData` | Boolean | 宛先に対してセグメントをアクティブ化した場合に、履歴プロファイルデータを書き出すかどうかを制御します。<br> <ul><li> `true`: [!DNL Platform] は、セグメントがアクティブ化される前にセグメントに適合した過去のユーザープロファイルを送信します。 </li><li> `false`: [!DNL Platform] には、セグメントがアクティブ化された後にセグメントに適合するユーザープロファイルのみが含まれます。 </li></ul> |
| `segmentMappingConfig.mapUserInput` | Boolean | 宛先のアクティベーションワークフローのセグメントマッピング ID をユーザーが入力するかどうかを制御します。 |
| `segmentMappingConfig.mapExperiencePlatformSegmentId` | Boolean | 宛先アクティベーションワークフローのセグメントマッピング ID がExperience Platformセグメント ID かどうかを制御します。 |
| `segmentMappingConfig.mapExperiencePlatformSegmentName` | Boolean | 宛先アクティベーションワークフローのセグメントマッピング ID がExperience Platformセグメント名かどうかを制御します。 |
| `segmentMappingConfig.audienceTemplateId` | Boolean | この宛先で使用される [ オーディエンスメタデータテンプレート ](./audience-metadata-management.md) の `instanceId`。 オーディエンスメタデータテンプレートを設定するには、[ オーディエンスメタデータ API リファレンス ](./audience-metadata-api.md) を参照してください。 |

{style=&quot;table-layout:auto&quot;}

## 既存の宛先設定の更新 {#update}

既存の宛先設定を更新するには、`/authoring/destinations` エンドポイントにPUTリクエストを送信し、更新する宛先設定のインスタンス ID を指定します。 呼び出しの本文で、更新した宛先設定を指定します。

**API 形式**


```http
PUT /authoring/destinations/{INSTANCE_ID}
```

| パラメーター | 説明 |
| -------- | ----------- |
| `{INSTANCE_ID}` | 更新する宛先設定の ID。 |

**リクエスト**

次のリクエストは、ペイロードで指定されたパラメーターで設定された既存の宛先設定を更新します。 次の呼び出しの例では、以前に作成した設定 [ を更新し、GAID、IDFA、ハッシュ化された電子メール識別子を ID 名前空間として受け入れるようにしています。](./destination-configuration-api.md#create)

```shell
curl -X PUT https://platform.adobe.io/data/core/activation/authoring/destinations/b0780cb5-2bb7-4409-bf2c-c625ca818588 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
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

特定の宛先設定に関する詳細な情報を取得するには、`/authoring/destinations` エンドポイントにGETリクエストを送信し、取得する宛先設定のインスタンス ID を指定します。

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
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、HTTP ステータス 200 と、指定された宛先設定に関する詳細情報を返します。

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

`/authoring/destinations` エンドポイントにDELETEリクエストを送信し、リクエストパスで削除する宛先設定の ID を指定することで、指定した宛先設定を削除できます。

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
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
```

**応答**

正常な応答は、HTTP ステータス 200 と空の HTTP 応答を返します。

## API エラー処理

宛先 SDK API エンドポイントは、一般的なExperience PlatformAPI エラーメッセージの原則に従います。 Platform トラブルシューティングガイドの [API ステータスコード ](https://experienceleague.adobe.com/docs/experience-platform/landing/troubleshooting.html?lang=en#api-status-codes) および [ リクエストヘッダーのエラー ](https://experienceleague.adobe.com/docs/experience-platform/landing/troubleshooting.html?lang=en#request-header-errors) を参照してください。

## 次の手順

このドキュメントを読むと、`/authoring/destinations` API エンドポイントを使用して宛先を設定する方法がわかります。 [ 宛先 SDK を使用して宛先を設定する方法 ](./configure-destination-instructions.md) を読んで、この手順が宛先の設定プロセスにどのように適しているかを確認してください。
