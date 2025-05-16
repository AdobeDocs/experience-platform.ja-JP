---
description: Destination SDKで作成された宛先に対するオーディエンスタイプの設定方法を説明します。
title: オーディエンスデータタイプの設定
exl-id: c56fb0f9-adb2-4fb2-ab06-c0398d828600
source-git-commit: 5d84ea1baa96c288d9d37606122e0a41880478b9
workflow-type: tm+mt
source-wordcount: '735'
ht-degree: 7%

---

# オーディエンスデータタイプの設定

Destination SDKで宛先コネクタを作成する場合、宛先に書き出すオーディエンスのタイプを定義できます。 適切なオーディエンスデータタイプを設定すると、マーケティングキャンペーン、アカウントベースの戦略、データ分析のどれに関するものであっても、宛先が意図したユースケースに適したデータを確実に受け取ることができます。

以下のオーディエンスデータタイプを確認して、それらの違いを学び、統合に必要なタイプを特定します。 次に、様々なオーディエンスタイプを書き出すように宛先を設定する方法について、このページの後述の節を参照してください。

| オーディエンスデータタイプ | 説明 | ユースケース |
|---------|----------|---------|
| [ 人物オーディエンス ](../../../../segmentation/types/people-audiences.md) | 顧客プロファイルに基づき、マーケティングキャンペーンの対象となる人物のグループを指定できます。 | 頻繁な購入、買い物かごの放棄 |
| [ アカウントオーディエンス ](../../../../segmentation/types/account-audiences.md) | アカウントベースのマーケティング戦略では、特定の組織内の個人をターゲットに設定します。 | B2B マーケティング |
| [ 見込み客オーディエンス ](../../../../segmentation/types/prospect-audiences.md) | まだ顧客ではないものの、ターゲットオーディエンスと特性を共有する個人をターゲットに設定します。 | サードパーティデータを使用した予測 |
| [ データセットの書き出し ](../../../../catalog/datasets/overview.md) | Adobe Experience Platform Data Lake に保存された構造化データのコレクション。 | レポート、データサイエンスワークフロー |

サポートされているオーディエンスデータタイプは、作成した宛先のタイプによって異なります。
以下の表を参照して、どの宛先タイプがどのオーディエンスデータタイプをサポートしているかを理解してください。

| 宛先のタイプ | 人物オーディエンス | アカウントオーディエンス | 見込み客オーディエンス | データセット |
|---------|----------|---------|---------|---------|
| ストリーミング | ✓ | ✓ | X | X |
| ファイルベース | ✓ | ✓ | ✓ | ✓ |

{style="table-layout:auto"}

## `sources` 配列 {#sources}

`sources` 配列は、宛先がサポートするオーディエンスデータのタイプを指定します。 アカウントオーディエンス、見込み客オーディエンスおよびデータセットの書き出しには必要ですが、デフォルトでサポートされているので、人物オーディエンスには必要ありません。

```json
"sources":[
   "ACCOUNTS" // Specifies that this destination supports account audiences
]
```

`sources` 配列は、次の値を受け入れます。

* `"ACCOUNTS"`：宛先がアカウントオーディエンスの書き出しをサポートしていることを指定します。
* `"UNIFIED_PROFILE_PROSPECTS"`：宛先が見込み客オーディエンスの書き出しをサポートすることを指定します。
* `"DATASETS"`：宛先がデータセットの書き出しをサポートしていることを指定します。

宛先に書き出すオーディエンスタイプに応じて、以下の節で宛先設定の例を確認します。

## 人物オーディエンスのエクスポート {#people-audiences}

人物オーディエンスは、すべての宛先タイプに対してデフォルトでサポートされており、特定の `sources` 値は必要ありません。 人物オーディエンスをサポートする宛先を作成するには、`sources` 配列がデフォルトの動作なので、この配列を使用する必要はありません。

+++ 人物オーディエンスをサポートしたストリーミング宛先設定の例

これは、人物オーディエンスを書き出すストリーミング宛先の例です。 設定に `sources` 配列がないことに注意してください。」

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

+++

## アカウントオーディエンスの書き出し {#account}

アカウントベースのマーケティングを行う [!DNL B2B] しい宛先を設定する場合は、宛先にアカウントオーディエンスサポートを追加することを検討してください。 例えば、アカウントベースのオーディエンスを使用して、[!DNL Chief Operating Officer (COO)] または [!DNL Chief Marketing Officer (CMO)] という役職のユーザーの連絡先情報を持たないすべてのアカウントのレコードを取得できます。

アカウントオーディエンスの書き出しをサポートする宛先を作成するには、以下の設定スニペットを [ 宛先設定 ](../../authoring-api/destination-configuration/create-destination-configuration.md) に追加します。

```json
"sources":[
   "ACCOUNTS" // Specifies that this destination supports account audiences
] 
```

+++ アカウントオーディエンスをサポートしたストリーミング宛先設定の例

```shell {line-numbers="true" highlight="12-14"}
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
   "sources":[
      "ACCOUNTS"
   ],
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

+++

## 見込み客オーディエンスのエクスポート {#prospect}

まだ顧客ではないが、ターゲットオーディエンスと特性を共有する個人をターゲットにする場合は、見込み客オーディエンスサポートを宛先に追加することを検討してください。 見込み客プロファイルを使用すると、信頼できるサードパーティパートナーの属性で顧客プロファイルを補完できます。 詳しくは、この [ 見込み客のユースケース ](../../../../rtcdp/partner-data/prospecting.md) を参照してください。

見込み客オーディエンスの書き出しをサポートする宛先を作成するには、以下の設定スニペットを [ 宛先設定 ](../../authoring-api/destination-configuration/create-destination-configuration.md) に追加します。


```json
"sources":[
   "UNIFIED_PROFILE_PROSPECTS" // Specifies that this destination supports prospect audiences
] 
```

+++ 見込み客オーディエンスをサポートするストリーミング宛先設定の例

```shell {line-numbers="true" highlight="12-14"}
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
   "sources":[
      "UNIFIED_PROFILE_PROSPECTS"
   ],
   "customerAuthenticationConfigurations":[
      {
         "authType":"BEARER"
      }
   ],
   "customerDataFields":[
      {
         "name":"bucketName",
         "title":"Enter the name of your Amazon S3 bucket",
         "description":"Amazon S3 bucket name",
         "type":"string",
         "isRequired":true,
         "pattern":"(?=^.{3,63}$)(?!^(\\d+\\.)+\\d+$)(^(([a-z0-9]|[a-z0-9][a-z0-9\\-]*[a-z0-9])\\.)*([a-z0-9]|[a-z0-9][a-z0-9\\-]*[a-z0-9])$)",
         "readOnly":false,
         "hidden":false
      },
      {
         "name":"path",
         "title":"Enter the path to your S3 bucket folder",
         "description":"Enter the path to your S3 bucket folder",
         "type":"string",
         "isRequired":true,
         "pattern":"^[0-9a-zA-Z\\/\\!\\-_\\.\\*\\''\\(\\)]*((\\%SEGMENT_(NAME|ID)\\%)?\\/?)+$",
         "readOnly":false,
         "hidden":false
      },
      {
         "name":"compression",
         "title":"Compression format",
         "description":"Select the desired file compression format.",
         "type":"string",
         "isRequired":true,
         "readOnly":false,
         "enum":[
            "GZIP",
            "NONE"
         ]
      },
      {
         "name":"fileType",
         "title":"File type",
         "description":"Select the exported file type.",
         "type":"string",
         "isRequired":true,
         "readOnly":false,
         "hidden":false,
         "enum":[
            "json",
            "parquet"
         ],
         "default":"parquet"
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

+++

## データセットを書き出し {#datasets}

オーディエンスの関心や選定別にグループ化または構造化されていない未加工のデータセットを書き出す場合は、宛先にデータセット書き出しサポートを追加することを検討してください。 このデータは、レポート、データサイエンスワークフロー、その他の多くのユースケースに使用できます。 例えば、管理者、データエンジニアまたはアナリストは、Experience Platformからデータをエクスポートして Data Warehouse と同期したり、[!DNL BI] Analysis Tools や外部の Cloud [!DNL ML] ツールで使用したり、システムに長期保存のニーズに応じて保存したりできます。

データセットの書き出しをサポートする宛先を作成するには、以下の設定スニペットを [ 宛先設定 ](../../authoring-api/destination-configuration/create-destination-configuration.md) に追加します。

```json
"sources":[
   "DATASETS" // Specifies that this destination supports dataset exports
]
```

+++ データセット書き出しのサポートを含んだファイルベースの宛先設定の例

```shell {line-numbers="true" highlight="12-14"}
curl -X POST https://platform.adobe.io/data/core/activation/authoring/destinations \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d '
{
   "name":"Amazon S3 destination with dataset export capability",
   "description":"Amazon S3 destination with dataset export capability",
   "status":"TEST",
   "sources":[
      "DATASETS"
   ],
   "customerAuthenticationConfigurations":[
      {
         "authType":"S3"
      }
   ],
   "customerDataFields":[
      {
         "name":"bucketName",
         "title":"Enter the name of your Amazon S3 bucket",
         "description":"Amazon S3 bucket name",
         "type":"string",
         "isRequired":true,
         "pattern":"(?=^.{3,63}$)(?!^(\\d+\\.)+\\d+$)(^(([a-z0-9]|[a-z0-9][a-z0-9\\-]*[a-z0-9])\\.)*([a-z0-9]|[a-z0-9][a-z0-9\\-]*[a-z0-9])$)",
         "readOnly":false,
         "hidden":false
      },
      {
         "name":"path",
         "title":"Enter the path to your S3 bucket folder",
         "description":"Enter the path to your S3 bucket folder",
         "type":"string",
         "isRequired":true,
         "pattern":"^[0-9a-zA-Z\\/\\!\\-_\\.\\*\\''\\(\\)]*((\\%SEGMENT_(NAME|ID)\\%)?\\/?)+$",
         "readOnly":false,
         "hidden":false
      },
      {
         "name":"compression",
         "title":"Compression format",
         "description":"Select the desired file compression format.",
         "type":"string",
         "isRequired":true,
         "readOnly":false,
         "enum":[
            "GZIP",
            "NONE"
         ]
      },
      {
         "name":"fileType",
         "title":"File type",
         "description":"Select the exported file type.",
         "type":"string",
         "isRequired":true,
         "readOnly":false,
         "hidden":false,
         "enum":[
            "json",
            "parquet"
         ],
         "default":"parquet"
      }
   ],
   "uiAttributes":{
      "documentationLink":"https://www.adobe.com/go/destinations-dataset-export-en",
      "category":"cloudStorage",
      "frequency":"Batch",
      "monitoringSupported":true,
      "flowRunsSupported":true
   },
   "segmentMappingConfig":{
      "mapExperiencePlatformSegmentName":false,
      "mapExperiencePlatformSegmentId":false,
      "mapUserInput":false,
      "audienceTemplateId":"19c8fc89-9b73-4e0f-893e-549410b23f39"
   },
   "aggregation":{
      "aggregationType":"BEST_EFFORT"
   },
   "destinationDelivery":[
      {
         "authenticationRule":"PLATFORM_AUTHENTICATION",
         "authenticationId":"2fff57e1-6603-4927-8a9c-147c90839bdb",
         "destinationServerId":"e59de90a-6df6-471a-bd55-11f7bdb52fae"
      }
   ],
   "inputSchemaId":"70d0bd4734cc49c98d48c4b162e9a1b7",
   "schemaConfig":{
      "useCustomerSchemaForAttributeMapping":false,
      "requiredMappingsOnly":false,
      "profileRequired":false,
      "segmentRequired":false,
      "identityRequired":false
   },
   "batchConfig":{
      "allowMandatoryFieldSelection":false,
      "autoSelectJoinKeyOnPartnerSchemaSelection":false,
      "joinKeyTitle":"DEDUPLICATION KEY",
      "defaultExportMode":"FIRST_FULL_THEN_INCREMENTAL",
      "allowedExportModes":[
         
      ],
      "allowedScheduleFrequency":[
         
      ],
      "defaultFrequency":"EVERY_HOUR",
      "defaultStartTime":"00:00",
      "filenameConfig":{
         "allowedFilenameAppendOptions":[
            
         ],
         "defaultFilenameAppendOptions":[
            
         ],
         "defaultFilename":""
      },
      "datasetBatchConfig":{
         "allowedFoldernameAppendOptions":[
            "DESTINATION",
            "DATASET_ID",
            "DATASET_NAME",
            "EXPORT_TIME",
            "DESTINATION_INSTANCE_ID",
            "DESTINATION_INSTANCE_NAME",
            "ORGANIZATION_NAME",
            "SANDBOX_NAME",
            "DATETIME",
            "CUSTOM_TEXT"
         ],
         "defaultFoldernameAppendOptions":[
            "DATASET_ID",
            "EXPORT_TIME"
         ],
         "allowedExportModes":[
            "DAILY_FULL_EXPORT",
            "FIRST_FULL_THEN_INCREMENTAL"
         ],
         "allowedScheduleFrequency":[
            "DAILY",
            "EVERY_3_HOURS",
            "EVERY_6_HOURS",
            "EVERY_12_HOURS",
            "EVERY_8_HOURS",
            "ONCE"
         ]
      },
      "allowDedupKeyFieldSelection":false
   },
   "maxProfileAttributes":9000,
   "maxIdentityAttributes":1000,
   "destConfigId":"069280b7-40fc-490c-85c4-3bcae17b5441",
   "backfillHistoricalProfileData":true
}
```

+++

## 次の手順 {#next-steps}

この記事を読むことで、宛先に対するオーディエンスデータタイプの設定方法について、理解を深めることができました。

その他の宛先コンポーネントについて詳しくは、以下の記事を参照してください。

* [顧客認証設定](customer-authentication.md)
* [OAuth2 認証](oauth2-authorization.md)
* [顧客データフィールド](customer-data-fields.md)
* [UI 属性](ui-attributes.md)
* [スキーマ設定](schema-configuration.md)
* [ID 名前空間設定](identity-namespace-configuration.md)
* [サポートされるマッピング設定](supported-mapping-configurations.md)
* [宛先配信](destination-delivery.md)
* [オーディエンスメタデータ設定](audience-metadata-configuration.md)
* [集計ポリシー](aggregation-policy.md)
* [バッチ設定](batch-configuration.md)
* [プロファイル選定履歴](historical-profile-qualifications.md)
