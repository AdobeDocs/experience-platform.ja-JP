---
description: このページでは、「/authoring/destinations」 APIエンドポイントを使用して実行できるすべてのAPI操作について説明します。
title: 宛先APIエンドポイントの操作
source-git-commit: 19307fba8f722babe5b6d57e80735ffde00fc851
workflow-type: tm+mt
source-wordcount: '2360'
ht-degree: 5%

---

# 宛先エンドポイントAPI操作 {#destination-configuration}

>[!IMPORTANT]
>
>**API エンドポイント**: `platform.adobe.io/data/core/activation/authoring/destinations`

このページでは、`/authoring/destinations` APIエンドポイントを使用して実行できるすべてのAPI操作について説明します。 このエンドポイントでサポートされる機能については、[宛先の設定](./destination-configuration.md)を参照してください。

## 宛先API操作の概要 {#get-started}

続行する前に、[はじめに](./getting-started.md)を参照し、必要な宛先オーサリング権限や必要なヘッダーの取得方法など、APIを正しく呼び出すために必要な重要な情報を確認してください。

## 宛先の設定の作成 {#create}

`/authoring/destinations`エンドポイントにPOSTリクエストを送信して、新しい宛先設定を作成できます。

**API 形式**


```http
POST /authoring/destinations
```

**リクエスト**

次のリクエストは、ペイロードで指定されたパラメーターによって設定された新しい宛先設定を作成します。 以下のペイロードには、`/authoring/destinations`エンドポイントで受け入れられるすべてのパラメーターが含まれます。 APIの要件に従って、呼び出しにすべてのパラメーターを追加する必要はなく、テンプレートがカスタマイズ可能であることに注意してください。

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
   "aggregation":{
      "aggregationType":"CONFIGURABLE_AGGREGATION",
      "configurableAggregation":{
         "splitUserById":true,
         "maxBatchAgeInSecs":0,
         "maxNumEventsInBatch":0,
         "aggregationKey":{
            "includeSegmentId":true,
            "includeSegmentStatus":true,
            "includeIdentity":true,
            "oneIdentityPerGroup":false,
            "groups":[
               {
                  "namespaces":[
                     "IDFA",
                     "GAID"
                  ]
               },
               {
                  "namespaces":[
                     "EMAIL"
                  ]
               }
            ]
         }
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
| `description` | 文字列 | Adobeが宛先カードの宛先カタログで使用するExperience Platformの説明を入力します。 4～5文以下を目指す。 |
| `status` | 文字列 | 宛先カードのライフサイクルステータスを示します。 指定できる値は、`TEST`、`PUBLISHED`、`DELETED` です。宛先を最初に設定する際には、`TEST`を使用します。 |
| `customerAuthenticationConfigurations` | 文字列 | サーバーに対するExperience Platform顧客の認証に使用される設定を示します。 使用可能な値については、以下の`authType`を参照してください。 |
| `customerAuthenticationConfigurations.authType` | 文字列 | 指定できる値は`OAUTH2, BEARER`です。 |
| `customerDataFields.name` | 文字列 | 導入するカスタムフィールドの名前を指定します。 |
| `customerDataFields.type` | 文字列 | 導入するカスタムフィールドのタイプを示します。 指定できる値は`string`、`object`、`integer`です。 |
| `customerDataFields.title` | 文字列 | Experience Platformユーザーインターフェイスの顧客に表示されるフィールドの名前を示します |
| `customerDataFields.description` | 文字列 | カスタムフィールドの説明を入力します。 |
| `customerDataFields.isRequired` | Boolean | 宛先設定ワークフローでこのフィールドが必須かどうかを示します。 |
| `customerDataFields.enum` | 文字列 | カスタムフィールドをドロップダウンメニューとしてレンダリングし、ユーザーが使用できるオプションを一覧表示します。 |
| `customerDataFields.pattern` | 文字列 | 必要に応じて、カスタムフィールドのパターンを適用します。 正規表現を使用してパターンを適用します。 例えば、顧客IDに数値やアンダースコアが含まれない場合は、このフィールドに`^[A-Za-z]+$`と入力します。 |
| `uiAttributes.documentationLink` | 文字列 | 宛先の[宛先カタログ](https://experienceleague.adobe.com/docs/experience-platform/destinations/catalog/overview.html?lang=en#catalog)のドキュメントページを参照します。 `http://www.adobe.com/go/destinations-YOURDESTINATION-en`を使用します。`YOURDESTINATION`は宛先の名前です。 Moviestarという宛先の場合は、`http://www.adobe.com/go/destinations-moviestar-en`を使用します。 |
| `uiAttributes.category` | 文字列 | Adobe Experience Platformで宛先に割り当てられたカテゴリを参照します。 詳しくは、[宛先カテゴリ](https://experienceleague.adobe.com/docs/experience-platform/rtcdp/destinations/destination-types.html?lang=en#destination-categories)を参照してください。 次のいずれかの値を使用します。`adobeSolutions, advertising, analytics, cdp, cloudStorage, crm, customerSuccess, database, dmp, ecommerce, email, emailMarketing, enrichment, livechat, marketingAutomation, mobile, personalization, protocols, social, streaming, subscriptions, surveys, tagManagers, voc, warehouses, payments`. |
| `uiAttributes.connectionType` | 文字列 | `Server-to-server` は現在、唯一のオプションです。 |
| `uiAttributes.frequency` | 文字列 | `Streaming` は現在、唯一のオプションです。 |
| `identityNamespaces.externalId.acceptsAttributes` | Boolean | 宛先が標準のプロファイル属性を受け入れるかどうかを示します。 通常、これらの属性はパートナーのドキュメントで強調表示されます。 |
| `identityNamespaces.externalId.acceptsCustomNamespaces` | Boolean | 顧客が宛先にカスタム名前空間を設定できるかどうかを示します。 |
| `identityNamespaces.externalId.allowedAttributesTransformation` | 文字列 | _サンプルの設定_&#x200B;では示されていません。例えば、[!DNL Platform]の顧客が属性としてプレーンなEメールアドレスを持ち、プラットフォームがハッシュ化されたEメールのみを受け入れる場合に使用します。 ここで、適用する必要がある変換（例えば、Eメールを小文字に変換し、ハッシュ化）を指定します。 |
| `identityNamespaces.externalId.acceptedGlobalNamespaces` | - | _サンプルの設定_&#x200B;では示されていません。プラットフォームが[標準のID名前空間](https://experienceleague.adobe.com/docs/experience-platform/identity/namespaces.html?lang=en#standard-namespaces)（IDFAなど）を受け入れる場合に使用されるので、PlatformユーザーがこれらのID名前空間を選択するように制限できます。 <br> を使用する場合、を使用 `acceptedGlobalNamespaces`して、電子メールアド `"requiredTransformation":"sha256(lower($))"` レスや電話番号を小文字およびハッシュ化できます。このパラメーターの使用方法を確認するには、[宛先設定の更新](./destination-configuration-api.md#update)の下のセクションで設定を参照してください。 |
| `destinationDelivery.authenticationRule` | 文字列 | [!DNL Platform]ユーザーが宛先に接続する方法を示します。 指定できる値は`CUSTOMER_AUTHENTICATION`、`PLATFORM_AUTHENTICATION`、`NONE`です。<br> <ul><li>Platformのお客様が、ユーザー名とパスワード、ベアラートークン、または別の認証方法でシステムにログインする場合は、`CUSTOMER_AUTHENTICATION`を使用します。 例えば、`customerAuthenticationConfigurations`で`authType: OAUTH2`または`authType:BEARER`も選択した場合は、このオプションを選択します。 </li><li> Adobeと宛先の間にグローバル認証システムがあり、[!DNL Platform]のお客様が宛先に接続するための認証資格情報を提供する必要がない場合は、`PLATFORM_AUTHENTICATION`を使用します。 この場合、[Credentials](./credentials-configuration.md)設定を使用してcredentialsオブジェクトを作成する必要があります。 </li><li>宛先プラットフォームにデータを送信するために認証が必要ない場合は、`NONE`を使用します。 </li></ul> |
| `destinationDelivery.destinationServerId` | 文字列 | この宛先に使用される[宛先サーバーテンプレート](./destination-server-api.md)の`instanceId`。 |
| `backfillHistoricalProfileData` | Boolean | 宛先に対してセグメントをアクティブ化した場合に、履歴プロファイルデータを書き出すかどうかを制御します。<br> <ul><li> `true`: [!DNL Platform] は、セグメントがアクティブ化される前にセグメントに適合した過去のユーザープロファイルを送信します。 </li><li> `false`: [!DNL Platform] には、セグメントがアクティブ化された後にセグメントに適合するユーザープロファイルのみが含まれます。 </li></ul> |
| `segmentMappingConfig.mapUserInput` | Boolean | 宛先のアクティベーションワークフローのセグメントマッピングIDをユーザーが入力するかどうかを制御します。 |
| `segmentMappingConfig.mapExperiencePlatformSegmentId` | Boolean | 宛先のアクティベーションワークフローのセグメントマッピングIDがExperience PlatformセグメントIDかどうかを制御します。 |
| `segmentMappingConfig.mapExperiencePlatformSegmentName` | Boolean | 宛先のアクティベーションワークフローのセグメントマッピングIDがExperience Platformセグメント名かどうかを制御します。 |
| `segmentMappingConfig.audienceTemplateId` | Boolean | この宛先に使用される[オーディエンスメタデータテンプレート](./audience-metadata-api.md)の`instanceId`。 |
| `schemaConfig.profileFields` | 配列 | 上記の設定で示すように、定義済みの`profileFields`を追加すると、Experience Platform属性を宛先側の定義済みの属性にマッピングするオプションが表示されます。 |
| `schemaConfig.profileRequired` | Boolean | 上記の設定例に示すように、Experience Platformから宛先側のカスタム属性にプロファイル属性をマッピングできる場合は、`true`を使用します。 |
| `schemaConfig.segmentRequired` | Boolean | 常に`segmentRequired:true`を使用します。 |
| `schemaConfig.identityRequired` | Boolean | ユーザーがID名前空間をExperience Platformから目的のスキーマにマッピングできる場合は、`true`を使用します。 |
| `aggregation.aggregationType` | - | 「`BEST_EFFORT`」または「`CONFIGURABLE_AGGREGATION`」を選択します。上記の例の設定には両方の集計タイプが含まれていますが、必要なのは、宛先に対していずれかを選択することだけです。 |
| `aggregation.bestEffortAggregation.maxUsersPerRequest` | 整数 | Experience Platformは、1回のHTTP呼び出しで、書き出された複数のプロファイルを集計できます。 1回のHTTP呼び出しでエンドポイントが受け取るプロファイルの最大数を指定します。 これは、ベストエフォートの集計です。 例えば、値100を指定すると、Platformは1回の呼び出しで100未満の任意の数のプロファイルを送信できます。 <br> サーバーがリクエストごとに複数のユーザーを受け入れない場合、この値を1に設定します。 |
| `aggregation.bestEffortAggregation.splitUserById` | Boolean | 宛先への呼び出しをIDで分割する必要がある場合は、このフラグを使用します。 サーバーが呼び出しごとに1つのIDしか受け入れない場合、このフラグを`true`に設定します。 |
| `aggregation.configurableAggregation.splitUserById` | Boolean | 宛先への呼び出しをIDで分割する必要がある場合は、このフラグを使用します。 サーバーが呼び出しごとに1つのIDしか受け入れない場合、このフラグを`true`に設定します。 |
| `aggregation.configurableAggregation.maxBatchAgeInSecs` | 整数 | *最大値：3600*&#x200B;を参照してください。`maxNumEventsInBatch`と共に、エンドポイントにAPI呼び出しを送信するまでExperience Platformが待機する時間を決定します。 <br> 例えば、両方のパラメーターに最大値を使用する場合、Experience Platformは、API呼び出しをおこなう前に、3600秒または10.000個の認定済みプロファイルが存在するまで待機します（いずれか最初に達成される方を待ちます）。 |
| `aggregation.configurableAggregation.maxNumEventsInBatch` | 整数 | *最大値：10000*&#x200B;を参照してください。上の`maxBatchAgeInSecs`を参照してください。 |
| `aggregation.configurableAggregation.aggregationKey` | Boolean | 次のパラメーターに基づいて、宛先にマッピングされた書き出し済みプロファイルを集計できます。<br> <ul><li>セグメントID</li><li> セグメントステータス </li><li> id名前空間 </li></ul> |
| `aggregation.configurableAggregation.aggregationKey.includeSegmentId` | Boolean | セグメントIDで宛先に書き出したプロファイルをグループ化する場合は、これを`true`に設定します。 |
| `aggregation.configurableAggregation.aggregationKey.includeSegmentStatus` | Boolean | セグメントIDとセグメントステータスで宛先に書き出したプロファイルをグループ化する場合は、 `includeSegmentId:true`と`includeSegmentStatus:true`の両方を設定する必要があります。 |
| `aggregation.configurableAggregation.aggregationKey.includeIdentity` | Boolean | ID名前空間で宛先に書き出したプロファイルをグループ化する場合は、これを`true`に設定します。 |
| `aggregation.configurableAggregation.aggregationKey.oneIdentityPerGroup` | Boolean | このパラメーターを使用して、書き出されたプロファイルを単一のID（GAID、IDFA、電話番号、電子メールなど）のグループに集計するかどうかを指定します。 |
| `aggregation.configurableAggregation.aggregationKey.groups` | 文字列 | ID名前空間のグループ別に、宛先に書き出されたプロファイルをグループ化する場合は、IDグループのリストを作成します。 例えば、この例の設定を使用して、IDFAおよびGAIDのモバイル識別子を含むプロファイルを、宛先への呼び出しと、電子メールの呼び出しに組み合わせることができます。 |

{style=&quot;table-layout:auto&quot;}

**応答**

正常な応答は、HTTPステータス200と、新しく作成された宛先設定の詳細を返します。

## 宛先設定のリスト {#retrieve-list}

IMS組織のすべての宛先設定のリストを取得するには、`/authoring/destinations`エンドポイントにGETリクエストを送信します。

**API 形式**


```http
GET /authoring/destinations
```

**リクエスト**

次のリクエストは、IMS組織とサンドボックス設定に基づいて、アクセス権のある宛先設定のリストを取得します。

```shell
curl -X GET https://platform.adobe.io/data/core/activation/authoring/destinations \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

次の応答は、使用したIMS組織IDとサンドボックス名に基づいて、HTTPステータス200と、アクセス権のある宛先設定のリストを返します。 1つの`instanceId`は、1つの宛先のテンプレートに対応します。 簡潔にするために応答は切り捨てられます。

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
         "aggregation":{
            "aggregationType":"CONFIGURABLE_AGGREGATION",
            "configurableAggregation":{
               "splitUserById":true,
               "maxBatchAgeInSecs":0,
               "maxNumEventsInBatch":0,
               "aggregationKey":{
                  "includeSegmentId":true,
                  "includeSegmentStatus":true,
                  "includeIdentity":true,
                  "oneIdentityPerGroup":false,
                  "groups":[
                     {
                        "namespaces":[
                           "IDFA",
                           "GAID"
                        ]
                     },
                     {
                        "namespaces":[
                           "EMAIL"
                        ]
                     }
                  ]
               }
            }
         },
         "destinationDelivery":[
            {
               "authenticationRule":"CUSTOMER_AUTHENTICATION",
               "destinationServerId":"9c77000a-4559-40ae-9119-a04324a3ecd4"
            }
         ],
         "inputSchemaId":"cc8621770a9243b98aba4df79898b1ed",
         "destConfigId":"410631b8-f6b3-4b7c-82da-7998aa3f327c",
         "backfillHistoricalProfileData":true
      }
   ]
}
    
```

| パラメーター | タイプ | 説明 |
|---------|----------|------|
| `name` | 文字列 | 宛先カタログ内の宛先のタイトルをExperience Platformします。 |
| `description` | 文字列 | Adobeが宛先カードの宛先カタログで使用するExperience Platformの説明を入力します。 4～5文以下を目指す。 |
| `status` | 文字列 | 宛先カードのライフサイクルステータスを示します。 指定できる値は、`TEST`、`PUBLISHED`、`DELETED` です。宛先を最初に設定する際には、`TEST`を使用します。 |
| `customerAuthenticationConfigurations` | 文字列 | サーバーに対するExperience Platform顧客の認証に使用される設定を示します。 使用可能な値については、以下の`authType`を参照してください。 |
| `customerAuthenticationConfigurations.authType` | 文字列 | 指定できる値は`OAUTH2, BEARER`です。 |
| `customerDataFields.name` | 文字列 | 導入するカスタムフィールドの名前を指定します。 |
| `customerDataFields.type` | 文字列 | 導入するカスタムフィールドのタイプを示します。 指定できる値は`string`、`object`、`integer`です。 |
| `customerDataFields.title` | 文字列 | Experience Platformユーザーインターフェイスの顧客に表示されるフィールドの名前を示します |
| `customerDataFields.description` | 文字列 | カスタムフィールドの説明を入力します。 |
| `customerDataFields.isRequired` | Boolean | 宛先設定ワークフローでこのフィールドが必須かどうかを示します。 |
| `customerDataFields.enum` | 文字列 | カスタムフィールドをドロップダウンメニューとしてレンダリングし、ユーザーが使用できるオプションを一覧表示します。 |
| `customerDataFields.pattern` | 文字列 | 必要に応じて、カスタムフィールドのパターンを適用します。 正規表現を使用してパターンを適用します。 例えば、顧客IDに数値やアンダースコアが含まれない場合は、このフィールドに`^[A-Za-z]+$`と入力します。 |
| `uiAttributes.documentationLink` | 文字列 | 宛先の[宛先カタログ](https://experienceleague.adobe.com/docs/experience-platform/destinations/catalog/overview.html?lang=en#catalog)のドキュメントページを参照します。 `http://www.adobe.com/go/destinations-YOURDESTINATION-en`を使用します。`YOURDESTINATION`は宛先の名前です。 Moviestarという宛先の場合、`http://www.adobe.com/go/destinations-moviestar-en` |
| `uiAttributes.category` | 文字列 | Adobe Experience Platformで宛先に割り当てられたカテゴリを参照します。 詳しくは、[宛先カテゴリ](https://experienceleague.adobe.com/docs/experience-platform/rtcdp/destinations/destination-types.html?lang=en#destination-categories)を参照してください。 次のいずれかの値を使用します。`adobeSolutions, advertising, analytics, cdp, cloudStorage, crm, customerSuccess, database, dmp, ecommerce, email, emailMarketing, enrichment, livechat, marketingAutomation, mobile, personalization, protocols, social, streaming, subscriptions, surveys, tagManagers, voc, warehouses, payments` |
| `uiAttributes.connectionType` | 文字列 | `Server-to-server` は現在、唯一のオプションです。 |
| `uiAttributes.frequency` | 文字列 | `Streaming` は現在、唯一のオプションです。 |
| `identityNamespaces.externalId.acceptsAttributes` | Boolean | 宛先が標準のプロファイル属性を受け入れるかどうかを示します。 通常、これらの属性はパートナーのドキュメントで強調表示されます。 |
| `identityNamespaces.externalId.acceptsCustomNamespaces` | Boolean | 顧客が宛先にカスタム名前空間を設定できるかどうかを示します。 |
| `identityNamespaces.externalId.allowedAttributesTransformation` | 文字列 | _サンプルの設定_&#x200B;では示されていません。例えば、[!DNL Platform]の顧客が属性としてプレーンなEメールアドレスを持ち、プラットフォームがハッシュ化されたEメールのみを受け入れる場合に使用します。 ここで、適用する必要がある変換（例えば、Eメールを小文字に変換し、ハッシュ化）を指定します。 |
| `identityNamespaces.externalId.acceptedGlobalNamespaces` | - | _サンプルの設定_&#x200B;では示されていません。プラットフォームが[標準のID名前空間](https://experienceleague.adobe.com/docs/experience-platform/identity/namespaces.html?lang=en#standard-namespaces)（IDFAなど）を受け入れる場合に使用されるので、PlatformユーザーがこれらのID名前空間を選択するように制限できます。 |
| `destinationDelivery.authenticationRule` | 文字列 | [!DNL Platform]ユーザーが宛先に接続する方法を示します。 指定できる値は`CUSTOMER_AUTHENTICATION`、`PLATFORM_AUTHENTICATION`、`NONE`です。<br> <ul><li>Platformのお客様が、ユーザー名とパスワード、ベアラートークン、または別の認証方法でシステムにログインする場合は、`CUSTOMER_AUTHENTICATION`を使用します。 例えば、`customerAuthenticationConfigurations`で`authType: OAUTH2`または`authType:BEARER`も選択した場合は、このオプションを選択します。 </li><li> Adobeと宛先の間にグローバル認証システムがあり、[!DNL Platform]のお客様が宛先に接続するための認証資格情報を提供する必要がない場合は、`PLATFORM_AUTHENTICATION`を使用します。 この場合、[Credentials](./credentials-configuration.md)設定を使用してcredentialsオブジェクトを作成する必要があります。 </li><li>宛先プラットフォームにデータを送信するために認証が必要ない場合は、`NONE`を使用します。 </li></ul> |
| `destinationDelivery.destinationServerId` | 文字列 | この宛先に使用される[宛先サーバーテンプレート](./destination-server-api.md)の`instanceId`。 |
| `inputSchemaId` | 文字列 | このフィールドは自動的に生成され、入力は必要ありません。 |
| `destConfigId` | 文字列 | このフィールドは自動的に生成され、入力は必要ありません。 |
| `backfillHistoricalProfileData` | Boolean | 宛先に対してセグメントをアクティブ化した場合に、履歴プロファイルデータを書き出すかどうかを制御します。<br> <ul><li> `true`: [!DNL Platform] は、セグメントがアクティブ化される前にセグメントに適合した過去のユーザープロファイルを送信します。 </li><li> `false`: [!DNL Platform] には、セグメントがアクティブ化された後にセグメントに適合するユーザープロファイルのみが含まれます。 </li></ul> |
| `segmentMappingConfig.mapUserInput` | Boolean | 宛先のアクティベーションワークフローのセグメントマッピングIDをユーザーが入力するかどうかを制御します。 |
| `segmentMappingConfig.mapExperiencePlatformSegmentId` | Boolean | 宛先のアクティベーションワークフローのセグメントマッピングIDがExperience PlatformセグメントIDかどうかを制御します。 |
| `segmentMappingConfig.mapExperiencePlatformSegmentName` | Boolean | 宛先のアクティベーションワークフローのセグメントマッピングIDがExperience Platformセグメント名かどうかを制御します。 |
| `segmentMappingConfig.audienceTemplateId` | Boolean | この宛先に使用される[オーディエンスメタデータテンプレート](./audience-metadata-management.md)の`instanceId`。 オーディエンスメタデータテンプレートを設定するには、[オーディエンスメタデータAPIリファレンス](./audience-metadata-api.md)をお読みください。 |


## 既存の宛先設定の更新 {#update}

既存の宛先設定を更新するには、`/authoring/destinations`エンドポイントにPUTリクエストを送信し、更新する宛先設定のインスタンスIDを指定します。 呼び出しの本文で、更新された宛先設定を指定します。

**API 形式**


```http
PUT /authoring/destinations/{INSTANCE_ID}
```

| パラメーター | 説明 |
| -------- | ----------- |
| `{INSTANCE_ID}` | 更新する宛先設定のID。 |

**リクエスト**

次のリクエストは、ペイロードで指定されたパラメーターで設定された既存の宛先設定を更新します。 次の呼び出しの例では、以前に作成した設定[を更新し、GAID、IDFA、ハッシュ化された電子メール識別子をID名前空間として受け入れるようにしています。](./destination-configuration-api.md#create)

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
   "aggregation":{
      "aggregationType":"CONFIGURABLE_AGGREGATION",
      "configurableAggregation":{
         "splitUserById":true,
         "maxBatchAgeInSecs":0,
         "maxNumEventsInBatch":0,
         "aggregationKey":{
            "includeSegmentId":true,
            "includeSegmentStatus":true,
            "includeIdentity":true,
            "oneIdentityPerGroup":false,
            "groups":[
               {
                  "namespaces":[
                     "IDFA",
                     "GAID"
                  ]
               },
               {
                  "namespaces":[
                     "EMAIL"
                  ]
               }
            ]
         }
      }
   },
   "destinationDelivery":[
      {
         "authenticationRule":"CUSTOMER_AUTHENTICATION",
         "destinationServerId":"9c77000a-4559-40ae-9119-a04324a3ecd4"
      }
   ],
   "inputSchemaId":"cc8621770a9243b98aba4df79898b1ed",
   "backfillHistoricalProfileData":true
}
```

## 特定の宛先設定の取得 {#get}

特定の宛先設定に関する詳細な情報を取得するには、`/authoring/destinations`エンドポイントにGETリクエストを送信し、取得する宛先設定のインスタンスIDを指定します。

**API 形式**


```http
GET /authoring/destinations/{INSTANCE_ID}
```

| パラメーター | 説明 |
| -------- | ----------- |
| `{INSTANCE_ID}` | 取得する宛先設定のID。 |

**リクエスト**

```shell
curl -X GET https://platform.adobe.io/data/core/activation/authoring/destinations/b0780cb5-2bb7-4409-bf2c-c625ca818588 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、HTTPステータス200と、指定された宛先設定に関する詳細情報を返します。

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
   "aggregation":{
      "aggregationType":"CONFIGURABLE_AGGREGATION",
      "configurableAggregation":{
         "splitUserById":true,
         "maxBatchAgeInSecs":0,
         "maxNumEventsInBatch":0,
         "aggregationKey":{
            "includeSegmentId":true,
            "includeSegmentStatus":true,
            "includeIdentity":true,
            "oneIdentityPerGroup":false,
            "groups":[
               {
                  "namespaces":[
                     "IDFA",
                     "GAID"
                  ]
               },
               {
                  "namespaces":[
                     "EMAIL"
                  ]
               }
            ]
         }
      }
   },
   "destinationDelivery":[
      {
         "authenticationRule":"CUSTOMER_AUTHENTICATION",
         "destinationServerId":"9c77000a-4559-40ae-9119-a04324a3ecd4"
      }
   ],
   "inputSchemaId":"cc8621770a9243b98aba4df79898b1ed",
   "backfillHistoricalProfileData":true
}
```


## 特定の宛先設定の削除 {#delete}

`/authoring/destinations`エンドポイントにDELETEリクエストを送信し、リクエストパスで削除する宛先設定のIDを指定することで、指定した宛先設定を削除できます。

**API 形式**

```http
DELETE /authoring/destinations/{INSTANCE_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{INSTANCE_ID}` | 削除する宛先設定の`id`。 |

**リクエスト**

```shell
curl -X DELETE https://platform.adobe.io/data/core/activation/authoring/destinations/b0780cb5-2bb7-4409-bf2c-c625ca818588 \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
```

**応答**

正常な応答は、HTTPステータス200と空のHTTP応答を返します。

## APIエラー処理

宛先SDK APIエンドポイントは、一般的なExperience PlatformAPIエラーメッセージの原則に従います。 Platformトラブルシューティングガイドの[APIステータスコード](https://experienceleague.adobe.com/docs/experience-platform/landing/troubleshooting.html?lang=en#api-status-codes)および[リクエストヘッダーエラー](https://experienceleague.adobe.com/docs/experience-platform/landing/troubleshooting.html?lang=en#request-header-errors)を参照してください。

## 次の手順

このドキュメントを読むと、`/authoring/destinations` APIエンドポイントを使用して宛先を設定する方法がわかります。 [宛先SDKを使用して宛先](./configure-destination-instructions.md)を設定する方法を読み、この手順が宛先の設定プロセスにどのように適合するかを理解してください。
