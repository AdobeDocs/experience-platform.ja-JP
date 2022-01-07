---
description: このページでは、「/authoring/destinations」 API エンドポイントを使用して実行できるすべての API 操作について説明します。
title: 宛先 API エンドポイントの操作
exl-id: 96755e9d-be62-432f-b985-91330575b395
source-git-commit: 6dd8a94e46b9bee6d1407e7ec945a722d8d7ecdb
workflow-type: tm+mt
source-wordcount: '2387'
ht-degree: 5%

---

# 宛先エンドポイント API 操作 {#destination-configuration}

>[!IMPORTANT]
>
>**API エンドポイント**: `platform.adobe.io/data/core/activation/authoring/destinations`

このページでは、 `/authoring/destinations` API エンドポイント。 このエンドポイントでサポートされる機能については、 [宛先設定](./destination-configuration.md).

## 宛先 API 操作の概要 {#get-started}

続行する前に、 [入門ガイド](./getting-started.md) を参照してください。

## 宛先の設定を作成する {#create}

新しい宛先設定を作成するには、 `/authoring/destinations` endpoint.

**API 形式**


```http
POST /authoring/destinations
```

**リクエスト**

次のリクエストは、ペイロードで指定されたパラメーターで設定された新しい宛先設定を作成します。 以下のペイロードには、 `/authoring/destinations` endpoint. API 要件に従って、呼び出しにすべてのパラメーターを追加する必要はなく、テンプレートをカスタマイズできることに注意してください。

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
| `name` | 文字列 | 宛先カタログ内の宛先のタイトルをExperience Platformします |
| `description` | 文字列 | Adobeが宛先カードの宛先カタログで使用するExperience Platformを指定します。 4～5 文以下を目指します。 |
| `status` | 文字列 | 宛先カードのライフサイクルステータスを示します。 指定できる値は、`TEST`、`PUBLISHED`、`DELETED` です。用途 `TEST` を設定します。 |
| `customerAuthenticationConfigurations` | 文字列 | サーバーへのExperience Platform顧客の認証に使用する設定を示します。 詳しくは、 `authType` を参照してください。 |
| `customerAuthenticationConfigurations.authType` | 文字列 | 指定できる値は次のとおりです。 `OAUTH2, BEARER`. |
| `customerDataFields.name` | 文字列 | 紹介するカスタムフィールドの名前を指定します。 |
| `customerDataFields.type` | 文字列 | 導入するカスタムフィールドのタイプを示します。 指定できる値は次のとおりです。 `string`, `object`, `integer` |
| `customerDataFields.title` | 文字列 | Experience Platformユーザーインターフェイスの顧客に表示されるフィールドの名前を示します |
| `customerDataFields.description` | 文字列 | カスタムフィールドの説明を入力します。 |
| `customerDataFields.isRequired` | ブール値 | このフィールドが宛先設定ワークフローで必須かどうかを示します。 |
| `customerDataFields.enum` | 文字列 | カスタムフィールドをドロップダウンメニューとしてレンダリングし、ユーザーが使用できるオプションを一覧表示します。 |
| `customerDataFields.pattern` | 文字列 | 必要に応じて、カスタムフィールドのパターンを適用します。 正規表現を使用して、パターンを適用します。 例えば、顧客 ID に数字やアンダースコアが含まれない場合は、 `^[A-Za-z]+$` を選択します。 |
| `uiAttributes.documentationLink` | 文字列 | ページの [宛先カタログ](https://experienceleague.adobe.com/docs/experience-platform/destinations/catalog/overview.html?lang=en#catalog) を設定します。 用途 `https://www.adobe.com/go/destinations-YOURDESTINATION-en`で、 `YOURDESTINATION` は、宛先の名前です。 Moviestar という宛先の場合、 `https://www.adobe.com/go/destinations-moviestar-en`. |
| `uiAttributes.category` | 文字列 | Adobe Experience Platformで宛先に割り当てられたカテゴリを指します。 詳しくは、 [宛先カテゴリ](https://experienceleague.adobe.com/docs/experience-platform/rtcdp/destinations/destination-types.html?lang=en#destination-categories). 次のいずれかの値を使用します。 `adobeSolutions, advertising, analytics, cdp, cloudStorage, crm, customerSuccess, database, dmp, ecommerce, email, emailMarketing, enrichment, livechat, marketingAutomation, mobile, personalization, protocols, social, streaming, subscriptions, surveys, tagManagers, voc, warehouses, payments`. |
| `uiAttributes.connectionType` | 文字列 | `Server-to-server` は現在唯一の利用可能なオプションです。 |
| `uiAttributes.frequency` | 文字列 | `Streaming` は現在唯一の利用可能なオプションです。 |
| `identityNamespaces.externalId.acceptsAttributes` | ブール値 | 宛先が標準のプロファイル属性を受け入れるかどうかを示します。 通常、これらの属性はパートナーのドキュメントで強調表示されます。 |
| `identityNamespaces.externalId.acceptsCustomNamespaces` | ブール値 | 顧客が宛先にカスタム名前空間を設定できるかどうかを示します。 |
| `identityNamespaces.externalId.allowedAttributesTransformation` | 文字列 | _サンプル設定には表示されません_. 例えば、 [!DNL Platform] のお客様は、属性としてプレーンな電子メールアドレスを持っており、プラットフォームはハッシュ化された電子メールのみを受け取ります。 適用が必要な変換（例えば、E メールを小文字に変換し、ハッシュ化）を指定する場所です。 |
| `identityNamespaces.externalId.acceptedGlobalNamespaces` | - | プラットフォームがを受け入れる場合に使用します [標準 id 名前空間](https://experienceleague.adobe.com/docs/experience-platform/identity/namespaces.html?lang=en#standard-namespaces) （例えば IDFA）を使用して、Platform ユーザーがこれらの ID 名前空間を選択するように制限できます。 <br> を使用する場合、 `acceptedGlobalNamespaces`を使用する場合、 `"requiredTransformation":"sha256(lower($))"` を小文字にし、ハッシュ化した電子メールアドレスまたは電話番号に変換します。 |
| `destinationDelivery.authenticationRule` | 文字列 | 方法を示します [!DNL Platform] のお客様が宛先に接続します。 指定できる値は次のとおりです。 `CUSTOMER_AUTHENTICATION`, `PLATFORM_AUTHENTICATION`, `NONE`. <br> <ul><li>用途 `CUSTOMER_AUTHENTICATION` Platform のお客様が、ユーザー名とパスワード、ベアラートークンまたはその他の認証方法を使用してシステムにログインした場合。 例えば、このオプションを選択する場合は、 `authType: OAUTH2` または `authType:BEARER` in `customerAuthenticationConfigurations`. </li><li> 用途 `PLATFORM_AUTHENTICATION` Adobeと宛先の間にグローバル認証システムがあり、 [!DNL Platform] のお客様は、宛先に接続するための認証資格情報を提供する必要はありません。 この場合、 [資格情報](./credentials-configuration-api.md) 設定。 </li><li>用途 `NONE` 宛先プラットフォームにデータを送信するために認証が必要ない場合に使用します。 </li></ul> |
| `destinationDelivery.destinationServerId` | 文字列 | この `instanceId` の [宛先サーバーテンプレート](./destination-server-api.md) この宛先に使用されます。 |
| `backfillHistoricalProfileData` | ブール値 | 宛先に対してセグメントをアクティブ化する際に、履歴プロファイルデータを書き出すかどうかを制御します。 <br> <ul><li> `true`: [!DNL Platform] は、セグメントがアクティブ化される前に、そのセグメントの対象として認定された過去のユーザープロファイルを送信します。 </li><li> `false`: [!DNL Platform] には、セグメントがアクティブ化された後にセグメントに適合するユーザープロファイルのみが含まれます。 </li></ul> |
| `segmentMappingConfig.mapUserInput` | ブール値 | 宛先のアクティベーションワークフローのセグメントマッピング ID をユーザーが入力するかどうかを制御します。 |
| `segmentMappingConfig.mapExperiencePlatformSegmentId` | ブール値 | 宛先のアクティベーションワークフローのセグメントマッピング ID がExperience Platformセグメント ID かどうかを制御します。 |
| `segmentMappingConfig.mapExperiencePlatformSegmentName` | ブール値 | 宛先アクティベーションワークフローのセグメントマッピング ID がExperience Platformセグメント名かどうかを制御します。 |
| `segmentMappingConfig.audienceTemplateId` | ブール値 | この `instanceId` の [オーディエンスメタデータテンプレート](./audience-metadata-api.md) この宛先に使用されます。 |
| `schemaConfig.profileFields` | 配列 | 定義済みの `profileFields` 上記の設定で示すように、ユーザーは、Experience Platform属性を宛先側の事前定義済み属性にマッピングするオプションを持ちます。 |
| `schemaConfig.profileRequired` | ブール値 | 用途 `true` 上記の設定例に示すように、ユーザーがExperience Platformから宛先側のカスタム属性にプロファイル属性をマッピングできる場合。 |
| `schemaConfig.segmentRequired` | ブール値 | 常に使用 `segmentRequired:true`. |
| `schemaConfig.identityRequired` | ブール値 | 用途 `true` id 名前空間をExperience Platformから目的のスキーマにマッピングできる場合。 |
| `aggregation.aggregationType` | - | 「`BEST_EFFORT`」または「`CONFIGURABLE_AGGREGATION`」を選択します。上記の設定例には以下が含まれます。 `BEST_EFFORT` 集計。 例： `CONFIGURABLE_AGGREGATION`( [宛先設定](./destination-configuration.md#example-configuration) 文書。 設定可能な集計に関連するパラメーターを、この表で説明します。 |
| `aggregation.bestEffortAggregation.maxUsersPerRequest` | 整数 | Experience Platformは、1 回の HTTP 呼び出しで、書き出された複数のプロファイルを集計できます。 1 回の HTTP 呼び出しでエンドポイントが受け取るプロファイルの最大数を指定します。 これはベストエフォートの集計です。 例えば、値 100 を指定した場合、Platform は 1 回の呼び出しで 100 未満の任意の数のプロファイルを送信できます。 <br> サーバーが 1 回のリクエストで複数のユーザーを受け入れない場合、この値を 1 に設定します。 |
| `aggregation.bestEffortAggregation.splitUserById` | ブール値 | 宛先への呼び出しを ID で分割する必要がある場合は、このフラグを使用します。 このフラグをに設定します。 `true` サーバーが呼び出しごとに 1 つの id しか受け入れない場合、特定の名前空間に対して。 |
| `aggregation.configurableAggregation.splitUserById` | ブール値 | 設定例の「パラメーター」を参照してください。 [ここ](./destination-configuration.md#example-configuration). 宛先への呼び出しを ID で分割する必要がある場合は、このフラグを使用します。 このフラグをに設定します。 `true` サーバーが呼び出しごとに 1 つの id しか受け入れない場合、特定の名前空間に対して。 |
| `aggregation.configurableAggregation.maxBatchAgeInSecs` | 整数 | *最大値：3600*. 設定例の「パラメーター」を参照してください。 [ここ](./destination-configuration.md#example-configuration). と `maxNumEventsInBatch`に値を指定する場合、Experience Platformがエンドポイントに API 呼び出しを送信するまで待機する時間を決定します。 <br> 例えば、両方のパラメーターに最大値を使用した場合、Experience Platformは 3600 秒または 10.000 個の認定済みプロファイルが存在するまで待ってから API 呼び出しをおこないます（いずれか最初に待ちます）。 |
| `aggregation.configurableAggregation.maxNumEventsInBatch` | 整数 | *最大値：10000*. 設定例の「パラメーター」を参照してください。 [ここ](./destination-configuration.md#example-configuration). 詳しくは、 `maxBatchAgeInSecs` ちょうど上に |
| `aggregation.configurableAggregation.aggregationKey` | ブール値 | 設定例の「パラメーター」を参照してください。 [ここ](./destination-configuration.md#example-configuration). 以下のパラメーターに基づいて、宛先にマッピングされた書き出し済みプロファイルを集計できます。 <br> <ul><li>セグメント ID</li><li> セグメントのステータス </li><li> id 名前空間 </li></ul> |
| `aggregation.configurableAggregation.aggregationKey.includeSegmentId` | ブール値 | 設定例の「パラメーター」を参照してください。 [ここ](./destination-configuration.md#example-configuration). これを `true` を選択します。 |
| `aggregation.configurableAggregation.aggregationKey.includeSegmentStatus` | ブール値 | 設定例の「パラメーター」を参照してください。 [ここ](./destination-configuration.md#example-configuration). 次の両方を設定する必要があります `includeSegmentId:true` および `includeSegmentStatus:true` 宛先に書き出されたプロファイルをセグメント ID とセグメントステータスでグループ化する場合は、を選択します。 |
| `aggregation.configurableAggregation.aggregationKey.includeIdentity` | ブール値 | 設定例の「パラメーター」を参照してください。 [ここ](./destination-configuration.md#example-configuration). これを `true` を追加します。 |
| `aggregation.configurableAggregation.aggregationKey.oneIdentityPerGroup` | ブール値 | 設定例の「パラメーター」を参照してください。 [ここ](./destination-configuration.md#example-configuration). このパラメーターを使用して、書き出されたプロファイルを単一の ID のグループ（GAID、IDFA、電話番号、電子メールなど）に集計するかどうかを指定します。 |
| `aggregation.configurableAggregation.aggregationKey.groups` | 文字列 | 設定例の「パラメーター」を参照してください。 [ここ](./destination-configuration.md#example-configuration). 宛先に書き出されたプロファイルを ID 名前空間のグループ別にグループ化する場合は、ID グループのリストを作成します。 例えば、IDFA および GAID モバイル識別子を含むプロファイルを、例の設定を使用して、宛先への呼び出しと、別の電子メールへと組み合わせることができます。 |

{style=&quot;table-layout:auto&quot;}

**応答**

正常な応答は、HTTP ステータス 200 と、新しく作成された宛先設定の詳細を返します。

## 宛先設定のリスト {#retrieve-list}

IMS 組織のすべての宛先設定のリストを取得するには、にGETリクエストをおこないます `/authoring/destinations` endpoint.

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

次の応答は、使用した IMS 組織 ID とサンドボックス名に基づいて、HTTP ステータス 200 と、アクセス権のある宛先設定のリストを返します。 1 `instanceId` は、1 つの宛先のテンプレートに対応します。 簡潔にするために、応答は切り捨てられます。

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
| `description` | 文字列 | Adobeが宛先カードの宛先カタログで使用するExperience Platformを指定します。 4～5 文以下を目指します。 |
| `status` | 文字列 | 宛先カードのライフサイクルステータスを示します。 指定できる値は、`TEST`、`PUBLISHED`、`DELETED` です。用途 `TEST` を設定します。 |
| `customerAuthenticationConfigurations` | 文字列 | サーバーへのExperience Platform顧客の認証に使用する設定を示します。 詳しくは、 `authType` を参照してください。 |
| `customerAuthenticationConfigurations.authType` | 文字列 | 指定できる値は次のとおりです。 `OAUTH2, BEARER`. |
| `customerDataFields.name` | 文字列 | 紹介するカスタムフィールドの名前を指定します。 |
| `customerDataFields.type` | 文字列 | 導入するカスタムフィールドのタイプを示します。 指定できる値は次のとおりです。 `string`, `object`, `integer` |
| `customerDataFields.title` | 文字列 | Experience Platformユーザーインターフェイスの顧客に表示されるフィールドの名前を示します |
| `customerDataFields.description` | 文字列 | カスタムフィールドの説明を入力します。 |
| `customerDataFields.isRequired` | ブール値 | このフィールドが宛先設定ワークフローで必須かどうかを示します。 |
| `customerDataFields.enum` | 文字列 | カスタムフィールドをドロップダウンメニューとしてレンダリングし、ユーザーが使用できるオプションを一覧表示します。 |
| `customerDataFields.pattern` | 文字列 | 必要に応じて、カスタムフィールドのパターンを適用します。 正規表現を使用して、パターンを適用します。 例えば、顧客 ID に数字やアンダースコアが含まれない場合は、 `^[A-Za-z]+$` を選択します。 |
| `uiAttributes.documentationLink` | 文字列 | ページの [宛先カタログ](https://experienceleague.adobe.com/docs/experience-platform/destinations/catalog/overview.html?lang=en#catalog) を設定します。 用途 `https://www.adobe.com/go/destinations-YOURDESTINATION-en`で、 `YOURDESTINATION` は、宛先の名前です。 Moviestar という宛先の場合、 `https://www.adobe.com/go/destinations-moviestar-en` |
| `uiAttributes.category` | 文字列 | Adobe Experience Platformで宛先に割り当てられたカテゴリを指します。 詳しくは、 [宛先カテゴリ](https://experienceleague.adobe.com/docs/experience-platform/rtcdp/destinations/destination-types.html?lang=en#destination-categories). 次のいずれかの値を使用します。 `adobeSolutions, advertising, analytics, cdp, cloudStorage, crm, customerSuccess, database, dmp, ecommerce, email, emailMarketing, enrichment, livechat, marketingAutomation, mobile, personalization, protocols, social, streaming, subscriptions, surveys, tagManagers, voc, warehouses, payments` |
| `uiAttributes.connectionType` | 文字列 | `Server-to-server` は現在唯一の利用可能なオプションです。 |
| `uiAttributes.frequency` | 文字列 | `Streaming` は現在唯一の利用可能なオプションです。 |
| `identityNamespaces.externalId.acceptsAttributes` | ブール値 | 宛先が標準のプロファイル属性を受け入れるかどうかを示します。 通常、これらの属性はパートナーのドキュメントで強調表示されます。 |
| `identityNamespaces.externalId.acceptsCustomNamespaces` | ブール値 | 顧客が宛先にカスタム名前空間を設定できるかどうかを示します。 詳細を表示 [カスタム名前空間](https://experienceleague.adobe.com/docs/experience-platform/identity/namespaces.html?lang=en#manage-namespaces) Adobe Experience Platform |
| `identityNamespaces.externalId.allowedAttributesTransformation` | 文字列 | _サンプル設定には表示されません_. 例えば、 [!DNL Platform] のお客様は、属性としてプレーンな電子メールアドレスを持っており、プラットフォームはハッシュ化された電子メールのみを受け取ります。 適用が必要な変換（例えば、E メールを小文字に変換し、ハッシュ化）を指定する場所です。 |
| `identityNamespaces.externalId.acceptedGlobalNamespaces` | - | プラットフォームがを受け入れる場合に使用します [標準 id 名前空間](https://experienceleague.adobe.com/docs/experience-platform/identity/namespaces.html?lang=en#standard-namespaces) （例えば IDFA）を使用して、Platform ユーザーがこれらの ID 名前空間を選択するように制限できます。 |
| `destinationDelivery.authenticationRule` | 文字列 | 方法を示します [!DNL Platform] のお客様が宛先に接続します。 指定できる値は次のとおりです。 `CUSTOMER_AUTHENTICATION`, `PLATFORM_AUTHENTICATION`, `NONE`. <br> <ul><li>用途 `CUSTOMER_AUTHENTICATION` Platform のお客様が、ユーザー名とパスワード、ベアラートークンまたはその他の認証方法を使用してシステムにログインした場合。 例えば、このオプションを選択する場合は、 `authType: OAUTH2` または `authType:BEARER` in `customerAuthenticationConfigurations`. </li><li> 用途 `PLATFORM_AUTHENTICATION` Adobeと宛先の間にグローバル認証システムがあり、 [!DNL Platform] のお客様は、宛先に接続するための認証資格情報を提供する必要はありません。 この場合、 [資格情報](./authentication-configuration.md) 設定。 </li><li>用途 `NONE` 宛先プラットフォームにデータを送信するために認証が必要ない場合に使用します。 </li></ul> |
| `destinationDelivery.destinationServerId` | 文字列 | この `instanceId` の [宛先サーバーテンプレート](./destination-server-api.md) この宛先に使用されます。 |
| `destConfigId` | 文字列 | このフィールドは自動的に生成され、入力は不要です。 |
| `backfillHistoricalProfileData` | ブール値 | 宛先に対してセグメントをアクティブ化する際に、履歴プロファイルデータを書き出すかどうかを制御します。 <br> <ul><li> `true`: [!DNL Platform] は、セグメントがアクティブ化される前に、そのセグメントの対象として認定された過去のユーザープロファイルを送信します。 </li><li> `false`: [!DNL Platform] には、セグメントがアクティブ化された後にセグメントに適合するユーザープロファイルのみが含まれます。 </li></ul> |
| `segmentMappingConfig.mapUserInput` | ブール値 | 宛先のアクティベーションワークフローのセグメントマッピング ID をユーザーが入力するかどうかを制御します。 |
| `segmentMappingConfig.mapExperiencePlatformSegmentId` | ブール値 | 宛先のアクティベーションワークフローのセグメントマッピング ID がExperience Platformセグメント ID かどうかを制御します。 |
| `segmentMappingConfig.mapExperiencePlatformSegmentName` | ブール値 | 宛先アクティベーションワークフローのセグメントマッピング ID がExperience Platformセグメント名かどうかを制御します。 |
| `segmentMappingConfig.audienceTemplateId` | ブール値 | この `instanceId` の [オーディエンスメタデータテンプレート](./audience-metadata-management.md) この宛先に使用されます。 オーディエンスメタデータテンプレートを設定するには、 [オーディエンスメタデータ API リファレンス](./audience-metadata-api.md). |

{style=&quot;table-layout:auto&quot;}

## 既存の宛先設定の更新 {#update}

既存の宛先設定を更新するには、 `/authoring/destinations` エンドポイントを作成し、更新する宛先設定のインスタンス ID を指定します。 呼び出しの本文で、更新した宛先設定を指定します。

**API 形式**


```http
PUT /authoring/destinations/{INSTANCE_ID}
```

| パラメーター | 説明 |
| -------- | ----------- |
| `{INSTANCE_ID}` | 更新する宛先設定の ID。 |

**リクエスト**

次のリクエストは、ペイロードで指定されたパラメーターで設定された既存の宛先設定を更新します。 以下の呼び出しの例では、設定を更新しています。 [以前に作成済み](./destination-configuration-api.md#create) を追加しました。

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

特定の宛先設定に関する詳細な情報を取得するには、 `/authoring/destinations` エンドポイントを作成し、取得する宛先設定のインスタンス ID を指定します。

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

指定した宛先設定を削除するには、 `/authoring/destinations` エンドポイントを探し、リクエストパスで削除する宛先設定の ID を指定します。

**API 形式**

```http
DELETE /authoring/destinations/{INSTANCE_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{INSTANCE_ID}` | この `id` 削除する宛先設定の情報です。 |

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

Destination SDKAPI エンドポイントは、一般的なExperience PlatformAPI エラーメッセージの原則に従います。 参照： [API ステータスコード](../../landing/troubleshooting.md#api-status-codes) および [リクエストヘッダーエラー](../../landing/troubleshooting.md#request-header-errors) （Platform トラブルシューティングガイド）を参照してください。

## 次の手順

このドキュメントを読んだ後、 `/authoring/destinations` API エンドポイント。 読み取り [宛先の設定にDestination SDKを使用する方法](./configure-destination-instructions.md) を参照して、この手順が宛先を設定するプロセスに適した場所を把握します。
