---
description: この設定を使用すると、宛先名、カテゴリ、説明、ロゴなどの基本情報を指定できます。 また、この設定では、Experience Platformユーザーが宛先に対して認証する方法、Experience Platformユーザーインターフェイスでの表示方法、宛先に書き出すことができる ID も決定します。
title: （ベータ版）Destination SDKのファイルベースの宛先設定オプション
source-git-commit: 5186e90b850f1e75ec358fa01bfb8a5edac29277
workflow-type: tm+mt
source-wordcount: '1899'
ht-degree: 6%

---

# （ベータ版）ファイルベースの宛先設定 {#destination-configuration}

## 概要 {#overview}

>[!IMPORTANT]
>
>現在、Adobe Experience Platform Destination SDKでのファイルベースの宛先のサポートはベータ版です。 ドキュメントと機能は変更される場合があります。

この設定を使用すると、宛先名、カテゴリ、説明など、ファイルベースの宛先に関する重要な情報を指定できます。 また、この設定では、Experience Platformユーザーが宛先に対して認証する方法、Experience Platformユーザーインターフェイスでの表示方法、宛先に書き出すことができる ID も決定します。

また、この設定は、宛先サーバーとオーディエンスメタデータの機能に必要な他の設定も、この設定に接続します。 2 つの設定を [下のセクション](./destination-configuration.md#connecting-all-configurations).

このドキュメントで説明する機能は、 `/authoring/destinations` API エンドポイント。 読み取り [宛先 API エンドポイントの操作](./destination-configuration-api.md) エンドポイントで実行できる操作の完全なリストについては、を参照してください。

## Amazon S3 の宛先の設定例 {#batch-example-configuration}

```json
{
   "name":"S3 Destination with CSV Options",
   "description":"S3 Destination with CSV Options",
   "status":"TEST",
   "maxProfileAttributes": "2000",
   "maxIdentityAttributes": "10",
   "customerAuthenticationConfigurations":[
      {
         "authType":"S3"
      }
   ],
   "customerEncryptionConfigurations":[],
   "customerDataFields":[
      {
         "name":"bucket",
         "title":"Amazon S3 bucket name",
         "description":"Enter your Amazon S3 bucket name",
         "type":"string",
         "isRequired":true,
         "readOnly":false,
         "hidden":false
      },
      {
         "name":"path",
         "title":"Amazon S3 bucket path",
         "description":"Enter your S3 bucket path",
         "type":"string",
         "isRequired":true,
         "pattern":"^[A-Za-z]+$",
         "readOnly":false,
         "hidden":false
      },
      {
         "name":"sep",
         "title":"Enter a separator for each field and value",
         "description":"Enter a separator character for each field and value",
         "type":"string",
         "isRequired":false,
         "readOnly":false,
         "hidden":false
      },
      {
         "name":"encoding",
         "title":"Specify encoding (charset) of saved CSV files",
         "description":"Select encoding of csv files",
         "type":"string",
         "enum":[
            "UTF-8",
            "UTF-16"
         ],
         "isRequired":false,
         "readOnly":false,
         "hidden":false
      },
      {
         "name":"quote",
         "title":"Select a single character used for escaping quoted values",
         "description":"Select single charachter for escaping quoted values",
         "type":"string",
         "isRequired":false,
         "readOnly":false,
         "hidden":false
      },
      {
         "name":"quoteAll",
         "title":"Escape all quoted values",
         "description":"Select whether to escape all quoted values",
         "type":"string",
         "enum":[
            "true",
            "false"
         ],
         "default":"true",
         "isRequired":true,
         "readOnly":false,
         "hidden":false
      },
      {
         "name":"escape",
         "title":"Select a single character used for escaping quotes",
         "description":"Select a single character used for escaping quotes inside an already quoted value",
         "type":"string",
         "isRequired":false,
         "readOnly":false,
         "hidden":false
      },
      {
         "name":"escapeQuotes",
         "title":"Escape quotes",
         "description":"A flag indicating whether values containing quotes should always be enclosed in quotes",
         "type":"string",
         "enum":[
            "true",
            "false"
         ],
         "isRequired":false,
         "default":"true",
         "readOnly":false,
         "hidden":false
      },
      {
         "name":"header",
         "title":"header",
         "description":"Writes the names of columns as the first line.",
         "type":"string",
         "isRequired":false,
         "enum":[
            "true",
            "false"
         ],
         "readOnly":false,
         "default":"true",
         "hidden":false
      },
      {
         "name":"ignoreLeadingWhiteSpace",
         "title":"Ignore leading white space",
         "description":"A flag indicating whether or not leading whitespaces from values being written should be skipped.",
         "type":"string",
         "isRequired":false,
         "enum":[
            "true",
            "false"
         ],
         "readOnly":false,
         "default":"true",
         "hidden":false
      },
      {
         "name":"nullValue",
         "title":"Select the string representation of a null value",
         "description":"Sets the string representation of a null value. ",
         "type":"string",
         "isRequired":false,
         "readOnly":false,
         "hidden":false
      },
      {
         "name":"dateFormat",
         "title":"Date format",
         "description":"Select the string that indicates a date format. ",
         "type":"string",
         "default":"yyyy-MM-dd",
         "isRequired":false,
         "readOnly":false,
         "hidden":false
      },
      {
         "name":"charToEscapeQuoteEscaping",
         "title":"Char to escape quote escaping",
         "description":"Sets a single character used for escaping the escape for the quote character",
         "type":"string",
         "isRequired":false,
         "readOnly":false,
         "hidden":false
      },
      {
         "name":"emptyValue",
         "title":"Select the string representation of an empty value",
         "description":"Select the string representation of an empty value",
         "type":"string",
         "isRequired":false,
         "readOnly":false,
         "default":"",
         "hidden":false
      },
      {
         "name":"compression",
         "title":"Select compression",
         "description":"Select compressiont",
         "type":"string",
         "isRequired":true,
         "readOnly":false,
         "enum":[
            "SNAPPY",
            "GZIP",
            "DEFLATE",
            "NONE"
         ]
      },
      {
         "name":"fileType",
         "title":"Select a fileType",
         "description":"Select fileType",
         "type":"string",
         "isRequired":true,
         "readOnly":false,
         "hidden":false,
         "enum":[
            "csv",
            "json",
            "parquet"
         ],
         "default":"csv"
      }
   ],
   "uiAttributes":{
      "documentationLink":"http://www.adobe.com/go/YOURDESTINATION-en",
      "category":"S3",
      "iconUrl":"https://dc5tqsrhldvnl.cloudfront.net/2/90048/da276e30c730ce6cd666c8ca78360df21.png",
      "connectionType":"S3",
      "flowRunsSupported":true,
      "monitoringSupported":true,
      "frequency":"Batch"
   },
   "destinationDelivery":[
      {
         "deliveryMatchers":[
            {
               "type":"SOURCE",
               "value":[
                  "batch"
               ]
            }
         ],
         "authenticationRule":"CUSTOMER_AUTHENTICATION",
         "destinationServerId":"{{destinationServerId}}"
      }
   ],
   "identityNamespaces": {
        "adobe_id": {
            "acceptsAttributes": true,
            "acceptsCustomNamespaces": true
        },
        "mobile_id": {
            "acceptsAttributes": true,
            "acceptsCustomNamespaces": true
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
              "name":"phoneNo",
              "title":"phoneNo",
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
   "batchConfig":{
      "allowMandatoryFieldSelection":true,
      "allowJoinKeyFieldSelection":true,
      "defaultExportMode":"DAILY_FULL_EXPORT",
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
   "backfillHistoricalProfileData":true
}
```

| パラメーター | タイプ | 説明 |
|---------|----------|------|
| `name` | 文字列 | 宛先カタログ内の宛先のタイトルをExperience Platformします。 |
| `description` | 文字列 | 宛先カタログで宛先カードの説明をExperience Platformします。 4～5 文以下を目指します。 |
| `status` | 文字列 | 宛先カードのライフサイクルステータスを示します。 指定できる値は、`TEST`、`PUBLISHED`、`DELETED` です。用途 `TEST` を設定します。 |
| `maxProfileAttributes` | 文字列 | 顧客が宛先に書き出すことができるプロファイル属性の最大数を示します。 デフォルト値は `2000` です。 |
| `maxIdentityAttributes` | 文字列 | 顧客が宛先に書き出すことができる ID 名前空間の最大数を示します。 デフォルト値は `10` です。 |

{style=&quot;table-layout:auto&quot;}

## 顧客認証設定 {#customer-authentication-configurations}

宛先設定のこのセクションは、 [新しい宛先の設定](/help/destinations/ui/connect-destination.md) ページを開きます。このページでは、Experience Platformが、宛先のアカウントにExperience Platformを接続できます。

```json
"customerAuthenticationConfigurations": [
        {
            "authType": "S3"
        }
    ],
```

対象に応じて [認証オプション](authentication-configuration.md##supported-authentication-types) 次に示す `authType` 」フィールドに値を入力すると、Experience Platformページが次のように生成されます。

### Amazon S3 認証

Amazon S3 認証タイプを設定する場合、ユーザーは S3 資格情報を入力する必要があります。

![S3 認証を使用した UI レンダリング](assets/s3-authentication-ui.png)

### Azure Blob 認証  {#blob}

Azure BLOB 認証タイプを設定する場合、ユーザーは接続文字列を入力する必要があります。

![BLOB 認証を使用した UI レンダリング](assets/blob-authentication-ui.png)

### パスワード認証付き SFTP

パスワード認証タイプで SFTP を設定する場合、ユーザーは SFTP のユーザー名とパスワード、SFTP ドメインとポート（デフォルトポートは 22）を入力する必要があります。

![SFTP での UI レンダリング（パスワード認証）](assets/sftp-password-authentication-ui.png)

### SSH キー認証を使用した SFTP

SFTP（SSH キー認証タイプ）を設定する場合、ユーザーは SFTP ユーザー名と SSH キー、および SFTP ドメインとポート（デフォルトポートは 22）を入力する必要があります。

![SSH キー認証を使用した SFTP での UI レンダリング](assets/sftp-key-authentication-ui.png)

## 顧客データフィールド {#customer-data-fields}

このセクションを使用して、Experience PlatformUI で宛先に接続する際に、宛先に固有のカスタムフィールドに入力するようユーザーに求めます。

次の例では、 `customerDataFields` ユーザーは、宛先の名前を入力し、 [!DNL Amazon S3] バケット名とフォルダーパス、および圧縮タイプとファイル形式。

```json
 "customerDataFields":[
      {
         "name":"bucket",
         "title":"Amazon S3 bucket name",
         "description":"Enter your Amazon S3 bucket name",
         "type":"string",
         "isRequired":true,
         "readOnly":false,
         "hidden":false
      },
      {
         "name":"path",
         "title":"Amazon S3 bucket path",
         "description":"Enter your S3 bucket path",
         "type":"string",
         "isRequired":true,
         "pattern":"^[A-Za-z]+$",
         "readOnly":false,
         "hidden":false
      },
      {
         "name":"sep",
         "title":"Enter a separator for each field and value",
         "description":"Enter a separator character for each field and value",
         "type":"string",
         "isRequired":false,
         "readOnly":false,
         "hidden":false
      },
      {
         "name":"encoding",
         "title":"Specify encoding (charset) of saved CSV files",
         "description":"Select encoding of csv files",
         "type":"string",
         "enum":[
            "UTF-8",
            "UTF-16"
         ],
         "isRequired":false,
         "readOnly":false,
         "hidden":false
      },
      {
         "name":"quote",
         "title":"Select a single character used for escaping quoted values",
         "description":"Select single charachter for escaping quoted values",
         "type":"string",
         "isRequired":false,
         "readOnly":false,
         "hidden":false
      },
      {
         "name":"quoteAll",
         "title":"Escape all quoted values",
         "description":"Select whether to escape all quoted values",
         "type":"string",
         "enum":[
            "true",
            "false"
         ],
         "default":"true",
         "isRequired":true,
         "readOnly":false,
         "hidden":false
      },
      {
         "name":"escape",
         "title":"Select a single character used for escaping quotes",
         "description":"Select a single character used for escaping quotes inside an already quoted value",
         "type":"string",
         "isRequired":false,
         "readOnly":false,
         "hidden":false
      },
      {
         "name":"escapeQuotes",
         "title":"Escape quotes",
         "description":"A flag indicating whether values containing quotes should always be enclosed in quotes",
         "type":"string",
         "enum":[
            "true",
            "false"
         ],
         "isRequired":false,
         "default":"true",
         "readOnly":false,
         "hidden":false
      },
      {
         "name":"header",
         "title":"header",
         "description":"Writes the names of columns as the first line.",
         "type":"string",
         "isRequired":false,
         "enum":[
            "true",
            "false"
         ],
         "readOnly":false,
         "default":"true",
         "hidden":false
      },
      {
         "name":"ignoreLeadingWhiteSpace",
         "title":"Ignore leading white space",
         "description":"A flag indicating whether or not leading whitespaces from values being written should be skipped.",
         "type":"string",
         "isRequired":false,
         "enum":[
            "true",
            "false"
         ],
         "readOnly":false,
         "default":"true",
         "hidden":false
      },
      {
         "name":"nullValue",
         "title":"Select the string representation of a null value",
         "description":"Sets the string representation of a null value. ",
         "type":"string",
         "isRequired":false,
         "readOnly":false,
         "hidden":false
      },
      {
         "name":"dateFormat",
         "title":"Date format",
         "description":"Select the string that indicates a date format. ",
         "type":"string",
         "default":"yyyy-MM-dd",
         "isRequired":false,
         "readOnly":false,
         "hidden":false
      },
      {
         "name":"charToEscapeQuoteEscaping",
         "title":"Char to escape quote escaping",
         "description":"Sets a single character used for escaping the escape for the quote character",
         "type":"string",
         "isRequired":false,
         "readOnly":false,
         "hidden":false
      },
      {
         "name":"emptyValue",
         "title":"Select the string representation of an empty value",
         "description":"Select the string representation of an empty value",
         "type":"string",
         "isRequired":false,
         "readOnly":false,
         "default":"",
         "hidden":false
      },
      {
         "name":"compression",
         "title":"Select compression",
         "description":"Select compressiont",
         "type":"string",
         "isRequired":true,
         "readOnly":false,
         "enum":[
            "SNAPPY",
            "GZIP",
            "DEFLATE",
            "NONE"
         ]
      },
      {
         "name":"fileType",
         "title":"Select a fileType",
         "description":"Select fileType",
         "type":"string",
         "isRequired":true,
         "readOnly":false,
         "hidden":false,
         "enum":[
            "csv",
            "json",
            "parquet"
         ],
         "default":"csv"
      }
```

| パラメーター | タイプ | 説明 |
|---------|----------|------|
| `name` | 文字列 | 紹介するカスタムフィールドの名前を指定します。 |
| `title` | 文字列 | ユーザーインターフェイスのユーザーに表示されるフィールドの名前をExperience Platformします。 |
| `description` | 文字列 | カスタムフィールドの説明を入力します。 |
| `type` | 文字列 | 導入するカスタムフィールドのタイプを示します。 指定できる値は次のとおりです。 `string`, `object`, `integer`. |
| `isRequired` | ブール値 | このフィールドが宛先設定ワークフローで必須かどうかを示します。 |
| `pattern` | 文字列 | 必要に応じて、カスタムフィールドのパターンを適用します。 正規表現を使用して、パターンを適用します。 例えば、顧客 ID に数字やアンダースコアが含まれない場合は、 `^[A-Za-z]+$` を選択します。 |
| `enum` | 文字列 | カスタムフィールドをドロップダウンメニューとしてレンダリングし、ユーザーが使用できるオプションを一覧表示します。 |
| `default` | 文字列 | デフォルト値を `enum` リスト。 |

{style=&quot;table-layout:auto&quot;}

## UI 属性 {#ui-attributes}

この節では、Adobe Experience Platformユーザーインターフェイスの宛先にAdobeが使用する必要がある、上記の設定の UI 要素について説明します。

```json
"uiAttributes":{
      "documentationLink":"http://www.adobe.com/go/YOURDESTINATION-en",
      "category":"S3",
      "iconUrl":"https://dc5tqsrhldvnl.cloudfront.net/2/90048/da276e30c730ce6cd666c8ca78360df21.png",
      "connectionType":"S3",
      "flowRunsSupported":true,
      "monitoringSupported":true,
      "frequency":"Batch"
   }
```

| パラメーター | タイプ | 説明 |
|---------|----------|------|
| `documentationLink` | 文字列 | ページの [宛先カタログ](https://experienceleague.adobe.com/docs/experience-platform/destinations/catalog/overview.html?lang=en#catalog) を設定します。 用途 `http://www.adobe.com/go/destinations-YOURDESTINATION-en`で、 `YOURDESTINATION` は、宛先の名前です。 Moviestar という宛先の場合、 `http://www.adobe.com/go/destinations-moviestar-en` |
| `category` | 文字列 | Adobe Experience Platformで宛先に割り当てられたカテゴリを指します。 詳しくは、 [宛先カテゴリ](https://experienceleague.adobe.com/docs/experience-platform/destinations/destination-types.html). 次のいずれかの値を使用します。 `adobeSolutions, advertising, analytics, cdp, cloudStorage, crm, customerSuccess, database, dmp, ecommerce, email, emailMarketing, enrichment, livechat, marketingAutomation, mobile, personalization, protocols, social, streaming, subscriptions, surveys, tagManagers, voc, warehouses, payments`. |
| `iconUrl` | 文字列 | 宛先カタログカードに表示するアイコンをホストした URL。 |
| `connectionType` | 文字列 | 宛先に応じた接続のタイプ。 サポートされている値： <ul><li>`Azure Blob`</li><li>`Azure Data Lake Storage`</li><li>`S3`</li><li>`SFTP`</li></ul> |
| `flowRunsSupported` | ブール値 | 宛先接続が [フロー実行 UI](../../dataflows/ui/monitor-destinations.md#monitoring-destinations-dashboard). これを `true`: <ul><li>この **[!UICONTROL 前回のデータフロー実行日]** および **[!UICONTROL 前回のデータフロー実行ステータス]** がリンク先の参照ページに表示されます。</li><li>この **[!UICONTROL データフローの実行]** および **[!UICONTROL アクティベーションデータ]** タブが宛先ビューページに表示されます。</li></ul> |
| `monitoringSupported` | ブール値 | 宛先接続が [UI の監視](../ui/destinations-workspace.md#browse). これを `true`、 **[!UICONTROL 監視で表示]** オプションが表示されます。 |
| `frequency` | 文字列 | 宛先でサポートされているデータ書き出しのタイプを指します。 に設定 `Batch` （ファイルベースの宛先の場合） |

{style=&quot;table-layout:auto&quot;}

## 宛先の配信 {#destination-delivery}

```json
 "destinationDelivery":[
      {
         "deliveryMatchers":[
            {
               "type":"SOURCE",
               "value":[
                  "batch"
               ]
            }
         ],
         "authenticationRule":"CUSTOMER_AUTHENTICATION",
         "destinationServerId":"{{destinationServerId}}"
      }
   ]
```

| パラメーター | タイプ | 説明 |
|---------|----------|------|
| `authenticationRule` | 文字列 | 方法を示します [!DNL Platform] のお客様が宛先に接続します。 指定できる値は次のとおりです。 `CUSTOMER_AUTHENTICATION`, `PLATFORM_AUTHENTICATION`, `NONE`. <br> <ul><li>用途 `CUSTOMER_AUTHENTICATION` Platform のお客様が、次のいずれかの方法でシステムにログインする場合。 <ul><li>`"authType": "S3"`</li><li>`"authType":"AZURE_CONNECTION_STRING"`</li><li>`"authType":"AZURE_SERVICE_PRINCIPAL"`</li><li>`"authType":"SFTP_WITH_SSH_KEY"`</li><li>`"authType":"SFTP_WITH_PASSWORD"`</li></ul> </li><li> 用途 `PLATFORM_AUTHENTICATION` Adobeと宛先の間にグローバル認証システムがあり、 [!DNL Platform] のお客様は、宛先に接続するための認証資格情報を提供する必要はありません。 この場合、 [資格情報](./credentials-configuration-api.md) 設定。 </li><li>用途 `NONE` 宛先プラットフォームにデータを送信するために認証が必要ない場合に使用します。 </li></ul> |
| `destinationServerId` | 文字列 | この `instanceId` の [宛先サーバーの設定](./destination-server-api.md) この宛先に使用されます。 |

{style=&quot;table-layout:auto&quot;}

## セグメントマッピングの設定 {#segment-mapping}

宛先設定のこのセクションは、セグメント名や ID などのセグメントメタデータをExperience Platformと宛先の間で共有する方法に関連しています。

を通じて `audienceTemplateId`の場合、この節では、この設定を [オーディエンスメタデータ設定](./audience-metadata-management.md).

```json
   "segmentMappingConfig":{
       "mapExperiencePlatformSegmentName":false,
       "mapExperiencePlatformSegmentId":false,
       "mapUserInput":false,
       "audienceTemplateId":"cbf90a70-96b4-437b-86be-522fbdaabe9c"
   },
```

| パラメーター | タイプ | 説明 |
|---------|----------|------|
| `mapExperiencePlatformSegmentName` | ブール値 | 宛先アクティベーションワークフローのセグメントマッピング ID がExperience Platformセグメント名かどうかを制御します。 |
| `mapExperiencePlatformSegmentId` | ブール値 | 宛先のアクティベーションワークフローのセグメントマッピング ID がExperience Platformセグメント ID かどうかを制御します。 |
| `mapUserInput` | ブール値 | 宛先のアクティベーションワークフローのセグメントマッピング ID をユーザーが入力するかどうかを制御します。 |
| `audienceTemplateId` | ブール値 | この `instanceId` の [オーディエンスメタデータテンプレート](./audience-metadata-management.md) この宛先に使用されます。 オーディエンスメタデータテンプレートを設定するには、 [オーディエンスメタデータ API リファレンス](./audience-metadata-api.md). |

## マッピング手順のスキーマ設定 {#schema-configuration}

のパラメーターを使用します。 `schemaConfig` ：宛先のアクティベーションワークフローのマッピング手順を有効にします。 以下に説明するパラメーターを使用すると、Experience Platformユーザーがプロファイル属性や ID をファイルベースの宛先にマッピングできるかどうかを判断できます。

```json
"schemaConfig":{
      "profileFields":[
           {
              "name":"phoneNo",
              "title":"phoneNo",
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
```

| パラメーター | タイプ | 説明 |
|---------|----------|------|
| `profileFields` | 配列 | 定義済みの `profileFields`の場合、Experience Platformユーザーは、Platform 属性を宛先の事前定義済み属性にマッピングするオプションがあります。 |
| `profileRequired` | ブール値 | 用途 `true` 上記の設定例に示すように、ユーザーがExperience Platformから宛先側のカスタム属性にプロファイル属性をマッピングできる場合。 |
| `segmentRequired` | ブール値 | 常に使用 `segmentRequired:true`. |
| `identityRequired` | ブール値 | 用途 `true` ( ユーザーが、id 名前空間をExperience Platformから目的のスキーマにマッピングできる場合 )。 |

{style=&quot;table-layout:auto&quot;}

### マッピング手順での動的スキーマ設定 {#dynamic-schema-configuration}

Adobe Experience Platform Destination SDKは、パートナー定義のスキーマをサポートします。 パートナー定義のスキーマを使用すると、プロファイル属性と ID を、 [ストリーミング先](destination-configuration.md#schema-configuration) ワークフロー。

のパラメーターを使用します。  `dynamicSchemaConfig` を使用して、Platform プロファイル属性や id のマッピング先となる独自のスキーマを定義します。

```json
"schemaConfig":{
   "dynamicSchemaConfig":{
      "dynamicEnum": {
         "authenticationRule":"CUSTOMER_AUTHENTICATION",
         "destinationServerId":"{{destinationServerId}}",
         "value": "Schema Name",
         "responseFormat": "SCHEMA"
      }
   },
   "profileRequired":true,
   "segmentRequired":true,
   "identityRequired":true
}
```

| パラメーター | タイプ | 説明 |
|---------|----------|------|
| `profileRequired` | ブール値 | 用途 `true` 上記の設定例に示すように、ユーザーがExperience Platformから宛先側のカスタム属性にプロファイル属性をマッピングできる場合。 |
| `segmentRequired` | ブール値 | 常に使用 `segmentRequired:true`. |
| `identityRequired` | ブール値 | 用途 `true` ( ユーザーが、id 名前空間をExperience Platformから目的のスキーマにマッピングできる場合 )。 |
| `destinationServerId` | 文字列 | この `instanceId` の [宛先サーバーの設定](./destination-server-api.md) この宛先に使用されます。 |
| `authenticationRule` | 文字列 | 方法を示します [!DNL Platform] のお客様が宛先に接続します。 指定できる値は次のとおりです。 `CUSTOMER_AUTHENTICATION`, `PLATFORM_AUTHENTICATION`, `NONE`. <br> <ul><li>用途 `CUSTOMER_AUTHENTICATION` Platform のお客様が、次のいずれかの方法でシステムにログインする場合。 <ul><li>`"authType": "S3"`</li><li>`"authType":"AZURE_CONNECTION_STRING"`</li><li>`"authType":"AZURE_SERVICE_PRINCIPAL"`</li><li>`"authType":"SFTP_WITH_SSH_KEY"`</li><li>`"authType":"SFTP_WITH_PASSWORD"`</li></ul> </li><li> 用途 `PLATFORM_AUTHENTICATION` Adobeと宛先の間にグローバル認証システムがあり、 [!DNL Platform] のお客様は、宛先に接続するための認証資格情報を提供する必要はありません。 この場合、 [資格情報](./credentials-configuration-api.md) 設定。 </li><li>用途 `NONE` 宛先プラットフォームにデータを送信するために認証が必要ない場合に使用します。 </li></ul> |
| `value` | 文字列 | マッピング手順のExperience Platformユーザーインターフェイスに表示されるスキーマの名前。 |
| `responseFormat` | 文字列 | 常にに設定 `SCHEMA` カスタムスキーマを定義する際に使用します。 |

{style=&quot;table-layout:auto&quot;}

## ID と属性 {#identities-and-attributes}

この節のパラメーターは、宛先が受け入れる ID を決定します。 また、この設定により、 [マッピング手順](/help/destinations/ui/activate-segment-streaming-destinations.md#mapping) を使用します。


```json
"identityNamespaces": {
        "adobe_id": {
            "acceptsAttributes": true,
            "acceptsCustomNamespaces": true
        },
        "mobile_id": {
            "acceptsAttributes": true,
            "acceptsCustomNamespaces": true
        }
    },
```

どちらを指定するかを指定する必要があります [!DNL Platform] id 顧客は、を宛先に書き出すことができます。 以下に例を示します。 [!DNL Experience Cloud ID]，ハッシュ化された電子メール，デバイス ID ([!DNL IDFA], [!DNL GAID]) をクリックします。 これらの値は次のとおりです。 [!DNL Platform] ユーザーが宛先の id 名前空間にマッピングできる id 名前空間。 また、顧客が宛先でサポートされている ID にカスタム名前空間をマッピングできるかどうかを指定することもできます。

ID 名前空間では、 [!DNL Platform] および宛先
例えば、お客様は [!DNL Platform] [!DNL IDFA] 名前空間を [!DNL IDFA] 名前空間を宛先から作成するか、同じ名前空間をマッピングできます。 [!DNL Platform] [!DNL IDFA] 名前空間を [!DNL Customer ID] 名前空間を使用します。

## バッチ設定 {#batch-configuration}

この節では、Adobe Experience Platformユーザーインターフェイスの宛先でAdobeが使用する必要がある、上記の設定のファイル書き出し設定について説明します。

```json
 "batchConfig":{
      "allowMandatoryFieldSelection":true,
      "allowDedupeKeyFieldSelection":true,
      "defaultExportMode":"DAILY_FULL_EXPORT",
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
   }
```

| パラメーター | タイプ | 説明 |
|---------|----------|------|
| `allowMandatoryFieldSelection` | ブール値 | に設定 `true` を使用すると、顧客は必須のプロファイル属性を指定できます。 デフォルト値は `false` です。詳しくは、 [必須の属性](../ui/activate-batch-profile-destinations.md#mandatory-attributes) を参照してください。 |
| `allowDedupeKeyFieldSelection` | ブール値 | に設定 `true` ：顧客が重複排除キーを指定できるようにします。 デフォルト値は `false` です。詳しくは、 [重複排除キー](../ui/activate-batch-profile-destinations.md#deduplication-keys) を参照してください。 |
| `defaultExportMode` | Enum | デフォルトのファイル書き出しモードを定義します。 サポートされている値：<ul><li>`DAILY_FULL_EXPORT`</li><li>`FIRST_FULL_THEN_INCREMENTAL`</li></ul><br>デフォルト値は `DAILY_FULL_EXPORT` です。詳しくは、 [バッチアクティベーションドキュメント](../ui/activate-batch-profile-destinations.md#scheduling) を参照してください。 |
| `allowedExportModes` | リスト | のお客様が使用できるファイルエクスポートモードを定義します。 サポートされている値：<ul><li>`DAILY_FULL_EXPORT`</li><li>`FIRST_FULL_THEN_INCREMENTAL`</li></ul> |
| `allowedScheduleFrequency` | リスト | 顧客が使用できるファイル書き出し頻度を定義します。 サポートされている値：<ul><li>`ONCE`</li><li>`EVERY_3_HOURS`</li><li>`EVERY_6_HOURS`</li><li>`EVERY_8_HOURS`</li><li>`EVERY_12_HOURS`</li><li>`DAILY`</li></ul> |
| `defaultFrequency` | Enum | デフォルトのファイル書き出し頻度を定義します。サポートされている値は次のとおりです。<ul><li>`ONCE`</li><li>`EVERY_3_HOURS`</li><li>`EVERY_6_HOURS`</li><li>`EVERY_8_HOURS`</li><li>`EVERY_12_HOURS`</li><li>`DAILY`</li></ul> <br>デフォルト値は `DAILY` です。 |
| `defaultStartTime` | 文字列 | ファイルエクスポートのデフォルトの開始時間を定義します。 24 時間形式のファイルを使用します。 デフォルト値は「00:00」です。 |

## 過去のプロファイルの選定 {#profile-backfill}

以下を使用して、 `backfillHistoricalProfileData` パラメーターを使用して、過去のプロファイル選定を宛先に書き出す必要があるかどうかを指定します。

```json
   "backfillHistoricalProfileData":true
```

| パラメーター | タイプ | 説明 |
|---------|----------|------|
| `backfillHistoricalProfileData` | ブール値 | 宛先に対してセグメントをアクティブ化する際に、履歴プロファイルデータを書き出すかどうかを制御します。 <br> <ul><li> `true`: [!DNL Platform] は、セグメントがアクティブ化される前に、そのセグメントの対象として認定された過去のユーザープロファイルを送信します。 </li><li> `false`: [!DNL Platform] には、セグメントがアクティブ化された後にセグメントに適合するユーザープロファイルのみが含まれます。 </li></ul> |

## この設定により、宛先に必要なすべての情報が接続される仕組み {#connecting-all-configurations}

一部の宛先設定は、 [宛先サーバー](./server-and-file-configuration.md) または [オーディエンスメタデータ設定](./audience-metadata-management.md). ここで説明する宛先設定では、次のように、他の 2 つの設定を参照することで、これらの設定をすべて接続します。

* 以下を使用： `destinationServerId` を使用して、目的の宛先に対して設定された宛先サーバーおよびテンプレート設定を参照します。
* 以下を使用： `audienceMetadataId` を使用して、目的の宛先に対して設定されたオーディエンスメタデータを参照します。
