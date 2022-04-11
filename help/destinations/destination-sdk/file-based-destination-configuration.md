---
description: この構成により、宛先名、カテゴリ、説明、ロゴなどの基本情報を示すことができます。 また、この構成での設定は、Experience Platform ユーザーが宛先に対して認証する方法、Experience Platform ユーザーインターフェイスに表示される方法、宛先に書き出すことができる ID も決定します。
title: （ベータ版）Destination SDK のファイルベースの宛先設定オプション
source-git-commit: 5186e90b850f1e75ec358fa01bfb8a5edac29277
workflow-type: ht
source-wordcount: '1899'
ht-degree: 100%

---

# （ベータ版）ファイルベースの宛先設定 {#destination-configuration}

## 概要 {#overview}

>[!IMPORTANT]
>
>現在、Adobe Experience Platform Destination SDK でのファイルベースの宛先のサポートはベータ版です。ドキュメントと機能は変更される場合があります。

この設定を使用すると、宛先名、カテゴリ、説明など、ファイルベースの宛先に関する重要な情報を指定できます。また、この構成での設定は、Experience Platform ユーザーが宛先に対して認証する方法、Experience Platform ユーザーインターフェイスに表示される方法、宛先に書き出すことができる ID も決定します。

宛先サーバーが動作するために必須である他の構成（宛先サーバーとオーディエンスメタデータ）も、この構成に接続されます。この 2 つの構成を参照する方法については、[下記の節](./destination-configuration.md#connecting-all-configurations)をお読みください。

このドキュメントで説明する機能は、`/authoring/destinations` API エンドポイントを用いて構成することができます。エンドポイントで実行できる操作の完全なリストには、[宛先 API エンドポイントの操作](./destination-configuration-api.md)をお読みください。

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
| `name` | 文字列 | Experience Platform カタログ内の宛先のタイトルを示します。 |
| `description` | 文字列 | Experience Platform 宛先カタログで、宛先カードの説明を提供します。4 ～ 5 文以下を目指します。 |
| `status` | 文字列 | 宛先カードのライフサイクルステータスを示します。 指定できる値は、`TEST`、`PUBLISHED`、`DELETED` です。最初に宛先を設定するときは、`TEST` を使用します。 |
| `maxProfileAttributes` | 文字列 | 顧客が宛先にエクスポートできるプロファイル属性の最大数を示します。デフォルト値は `2000` です。 |
| `maxIdentityAttributes` | 文字列 | 顧客が宛先にエクスポートできる ID 名前空間の最大数を示します。デフォルト値は `10` です。 |

{style=&quot;table-layout:auto&quot;}

## 顧客認証の構成 {#customer-authentication-configurations}

宛先設定のこのセクションは、Experience Platform ユーザーインターフェイスで[新しい宛先の設定](/help/destinations/ui/connect-destination.md)ページを生成し、ユーザーが持つ宛先のアカウントに Experience Platform を接続します。

```json
"customerAuthenticationConfigurations": [
        {
            "authType": "S3"
        }
    ],
```

`authType` フィールドに指定する[認証オプション](authentication-configuration.md##supported-authentication-types)に応じて、ユーザーに対して Experience Platform ページが次のように生成されます。

### Amazon S3 認証

Amazon S3 認証タイプを設定する際に、ユーザーは S3 資格情報を入力する必要があります。

![S3 認証を使用した UI レンダリング](assets/s3-authentication-ui.png)

### Azure Blob 認証  {#blob}

Azure Blob 認証タイプを設定する際に、ユーザーは接続文字列を入力する必要があります。

![Blob 認証を使用した UI レンダリング](assets/blob-authentication-ui.png)

### パスワード認証を使用した SFTP

パスワード認証タイプで SFTP を設定する際に、ユーザーは SFTP のユーザー名とパスワード、SFTP ドメインとポート（デフォルトポートは 22）を入力する必要があります。

![パスワード認証を使用した SFTP での UI レンダリング](assets/sftp-password-authentication-ui.png)

### SSH キー認証を使用した SFTP

SSH キー認証タイプで SFTP を設定する際に、ユーザーは SFTP のユーザー名と SSH キー、および SFTP ドメインとポート（デフォルトポートは 22）を入力する必要があります。

![SSH キー認証を使用した SFTP での UI レンダリング](assets/sftp-key-authentication-ui.png)

## 顧客データフィールド {#customer-data-fields}

Experience Platform UI で宛先に接続する際に、このセクションを使用して、宛先に固有のカスタムフィールドに入力するようユーザーに求めます。

次の例では、宛先の名前を入力し、[!DNL Amazon S3] バケット名とフォルダーパス、そして圧縮タイプとファイル形式を指定するように `customerDataFields` でユーザーに求めます。

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
| `name` | 文字列 | 導入するカスタムフィールドの名前を記入します。 |
| `title` | 文字列 | Experience Platform のユーザーインターフェースで顧客に表示される、フィールドの名前を示します。 |
| `description` | 文字列 | カスタムフィールドの説明を入力します。 |
| `type` | 文字列 | 導入するカスタムフィールドのタイプを示します。 指定できる値は、`string`、`object`、`integer` です。 |
| `isRequired` | ブール値 | このフィールドが宛先設定ワークフローで必須かどうかを示します。 |
| `pattern` | 文字列 | 必要に応じて、カスタムフィールドのパターンを適用します。正規表現を使用して、パターンを適用します。 例えば、顧客 ID に数字やアンダースコアが含まれない場合は、このフィールドに `^[A-Za-z]+$` を入力します。 |
| `enum` | 文字列 | カスタムフィールドをドロップダウンメニューとしてレンダリングし、ユーザーが使用できるオプションを一覧表示します。 |
| `default` | 文字列 | デフォルト値を `enum` リストから定義します。 |

{style=&quot;table-layout:auto&quot;}

## UI 属性 {#ui-attributes}

この節では、Adobe Experience Platform ユーザーインターフェイスで宛先に対してアドビが使用する、上記の設定の UI 要素について説明します。

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
| `documentationLink` | 文字列 | 宛先については、[宛先カタログ](https://experienceleague.adobe.com/docs/experience-platform/destinations/catalog/overview.html?lang=ja#catalog)にあるドキュメントページを参照してください。`http://www.adobe.com/go/destinations-YOURDESTINATION-en` を使用します。`YOURDESTINATION` は宛先の名前です。Moviestar という宛先の場合は、`http://www.adobe.com/go/destinations-moviestar-en` となります。 |
| `category` | 文字列 | Adobe Experience Platform で宛先に割り当てられたカテゴリを参照します。 詳しくは、[宛先のカテゴリ](https://experienceleague.adobe.com/docs/experience-platform/destinations/destination-types.html?lang=ja)を参照してください。次のいずれかの値を使用します。`adobeSolutions, advertising, analytics, cdp, cloudStorage, crm, customerSuccess, database, dmp, ecommerce, email, emailMarketing, enrichment, livechat, marketingAutomation, mobile, personalization, protocols, social, streaming, subscriptions, surveys, tagManagers, voc, warehouses, payments` |
| `iconUrl` | 文字列 | 宛先カタログのカードに表示するアイコンをホストした URL。 |
| `connectionType` | 文字列 | 宛先に応じた接続のタイプ。サポートされている値。 <ul><li>`Azure Blob`</li><li>`Azure Data Lake Storage`</li><li>`S3`</li><li>`SFTP`</li></ul> |
| `flowRunsSupported` | ブール値 | 宛先接続を[フロー実行 UI](../../dataflows/ui/monitor-destinations.md#monitoring-destinations-dashboard) に含めるかどうかを示します。この項目を `true` に設定すると、次のように動作します。 <ul><li>**[!UICONTROL 前回のデータフロー実行日]**&#x200B;および&#x200B;**[!UICONTROL 前回のデータフロー実行ステータス]**&#x200B;が宛先の参照ページに表示されます。</li><li>「**[!UICONTROL データフローの実行]**」タブおよび「**[!UICONTROL アクティベーションデータ]**」タブが宛先ビューページに表示されます。</li></ul> |
| `monitoringSupported` | ブール値 | 宛先接続を[モニタリング UI](../ui/destinations-workspace.md#browse) に含めるかどうかを示します。この項目を `true` に設定すると、「**[!UICONTROL モニタリングで表示]**」オプションが宛先の参照ページに表示されます。 |
| `frequency` | 文字列 | 宛先でサポートされているデータ書き出しのタイプを指します。 ファイルベースの宛先は `Batch` に設定します。 |

{style=&quot;table-layout:auto&quot;}

## 宛先配信 {#destination-delivery}

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
| `authenticationRule` | 文字列 | [!DNL Platform] の顧客が宛先に接続する方法を示します。指定できる値は、`CUSTOMER_AUTHENTICATION`、`PLATFORM_AUTHENTICATION`、`NONE`です。<br> <ul><li>Platform の顧客が次のいずれかの方法でシステムにログインする場合、`CUSTOMER_AUTHENTICATION` を使用します。 <ul><li>`"authType": "S3"`</li><li>`"authType":"AZURE_CONNECTION_STRING"`</li><li>`"authType":"AZURE_SERVICE_PRINCIPAL"`</li><li>`"authType":"SFTP_WITH_SSH_KEY"`</li><li>`"authType":"SFTP_WITH_PASSWORD"`</li></ul> </li><li> Adobe と宛先の間にグローバル認証システムがあり、宛先に接続するための認証資格情報を [!DNL Platform] の顧客が提供する必要がない場合は、`PLATFORM_AUTHENTICATION` を使用します。この場合、[資格情報](./credentials-configuration-api.md)設定を使用して、資格情報オブジェクトを作成する必要があります。 </li><li>宛先プラットフォームにデータを送信するために認証が必要ない場合は、`NONE` を使用します。 </li></ul> |
| `destinationServerId` | 文字列 | この宛先に使用した[宛先サーバー設定](./destination-server-api.md)の `instanceId`。 |

{style=&quot;table-layout:auto&quot;}

## セグメントマッピングの構成 {#segment-mapping}

宛先設定の当該セクションは、セグメント名や ID などのセグメントメタデータを Experience Platform と宛先間で共有する方法に関する内容です。

このセクションでは、`audienceTemplateId` を通じて、この設定と[オーディエンスメタデータ設定](./audience-metadata-management.md)を結び付けています。

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
| `mapExperiencePlatformSegmentName` | ブール値 | 宛先のアクティベーションワークフローのセグメントマッピング ID が Adobe Experience Platform のセグメント名かどうかを制御します。 |
| `mapExperiencePlatformSegmentId` | ブール値 | 宛先のアクティベーションワークフローのセグメントマッピング ID が Adobe Experience Platform のセグメント ID かどうかを制御します。 |
| `mapUserInput` | ブール値 | 宛先のアクティベーションワークフローのセグメントマッピング ID をユーザーが入力するかどうかを制御します。 |
| `audienceTemplateId` | ブール値 | この宛先に使用した[オーディエンスメタデータテンプレート](./audience-metadata-management.md)の `instanceId`。オーディエンスメタデータテンプレートを設定するには、[オーディエンスメタデータ API リファレンス](./audience-metadata-api.md)を参照してください。 |

## マッピングステップのスキーマ構成 {#schema-configuration}

`schemaConfig` のパラメーターを使用して、宛先の有効化ワークフローのマッピング手順を有効にします。次に説明するパラメーターを使用すると、Experience Platform ユーザーがプロファイル属性や ID をファイルベースの宛先にマッピングできるかどうかを決定できます。

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
| `profileFields` | 配列 | 定義済みの `profileFields` を追加すると、Experience Platform ユーザーは、Platform 属性を宛先の事前定義済み属性にマッピングするオプションを選択できます。 |
| `profileRequired` | ブール値 | 前述の設定例に示すように、ユーザーが Experience Platform のプロファイル属性を宛先側のカスタム属性にマッピングできる場合は、`true` を使用します。 |
| `segmentRequired` | ブール値 | 常に `segmentRequired:true` を使用します。 |
| `identityRequired` | ブール値 | ユーザーが Experience Platform の ID 名前空間を目的のスキーマにマッピングできる場合は、`true` を使用します。 |

{style=&quot;table-layout:auto&quot;}

### マッピング手順での動的スキーマ設定 {#dynamic-schema-configuration}

Adobe Experience Platform Destination SDK は、パートナー定義のスキーマをサポートします。パートナー定義のスキーマを使用すると、[ストリーミング宛先](destination-configuration.md#schema-configuration)ワークフローと同様に、ユーザーはプロファイル属性と ID を宛先のパートナーが定義したカスタムスキーマにマッピングできます。

`dynamicSchemaConfig` のパラメーターを使用して、Platform プロファイル属性や ID のマッピング先となる独自のスキーマを定義します。

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
| `profileRequired` | ブール値 | 前述の設定例に示すように、ユーザーが Experience Platform のプロファイル属性を宛先のカスタム属性にマッピングできる場合は、`true` を使用します。 |
| `segmentRequired` | ブール値 | 常に `segmentRequired:true` を使用します。 |
| `identityRequired` | ブール値 | ユーザーが Experience Platform の ID 名前空間を目的のスキーマにマッピングできる場合は、`true` を使用します。 |
| `destinationServerId` | 文字列 | この宛先に使用した[宛先サーバー設定](./destination-server-api.md)の `instanceId`。 |
| `authenticationRule` | 文字列 | [!DNL Platform] の顧客が宛先に接続する方法を示します。指定できる値は、`CUSTOMER_AUTHENTICATION`、`PLATFORM_AUTHENTICATION`、`NONE`です。<br> <ul><li>Platform の顧客が次のいずれかの方法でシステムにログインする場合は、`CUSTOMER_AUTHENTICATION` を使用します。 <ul><li>`"authType": "S3"`</li><li>`"authType":"AZURE_CONNECTION_STRING"`</li><li>`"authType":"AZURE_SERVICE_PRINCIPAL"`</li><li>`"authType":"SFTP_WITH_SSH_KEY"`</li><li>`"authType":"SFTP_WITH_PASSWORD"`</li></ul> </li><li> アドビと宛先の間にグローバル認証システムがあり、宛先に接続するための認証資格情報を [!DNL Platform] の顧客が提供する必要がない場合は、`PLATFORM_AUTHENTICATION` を使用します。この場合、[資格情報](./credentials-configuration-api.md)設定を使用して、資格情報オブジェクトを作成する必要があります。 </li><li>宛先プラットフォームにデータを送信するために認証が必要ない場合は、`NONE` を使用します。 </li></ul> |
| `value` | 文字列 | マッピング手順で、Experience Platform ユーザーインターフェイスに表示されるスキーマの名前。 |
| `responseFormat` | 文字列 | カスタムスキーマを定義する際は、常に `SCHEMA` に設定します。 |

{style=&quot;table-layout:auto&quot;}

## ID と属性 {#identities-and-attributes}

このセクションのパラメーターは、宛先がどの ID を受け入れるかを決定します。この設定では、Experience Platform ユーザーインターフェースの[マッピング手順](/help/destinations/ui/activate-segment-streaming-destinations.md#mapping)で、ユーザーが XDM スキーマから宛先のスキーマに ID や属性をマッピングする際に、ターゲットの ID や属性も入力されます。


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

顧客がどの [!DNL Platform] ID を宛先にエクスポートできるかを指定する必要があります。例としては、[!DNL Experience Cloud ID]、ハッシュ化されたメール，デバイス ID（[!DNL IDFA]、[!DNL GAID]）があります。これらの値は、[!DNL Platform] ID 名前空間であり、顧客が宛先から ID 名前空間にマッピングできます。また、宛先でサポートされている ID に顧客がカスタム名前空間をマッピングできるかどうかを指定することもできます。

ID 名前空間は、[!DNL Platform] と宛先の間で 1 対 1 の応答を必要としません。
例えば、顧客は [!DNL Platform] [!DNL IDFA] 名前空間を [!DNL IDFA] 名前空間に宛先からマッピングできます。または、同じ [!DNL Platform] [!DNL IDFA] 名前空間を [!DNL Customer ID] 名前空間に宛先でマッピングできます。

## バッチ設定 {#batch-configuration}

この節では、Adobe Experience Platform ユーザーインターフェイスの宛先に対してアドビが使用する、上記の設定のファイルのエクスポート設定について説明します。

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
| `allowMandatoryFieldSelection` | ブール値 | `true` に設定すると、顧客は必須のプロファイル属性を指定できます。デフォルト値は `false` です。詳しくは、[必須の属性](../ui/activate-batch-profile-destinations.md#mandatory-attributes)を参照してください。 |
| `allowDedupeKeyFieldSelection` | ブール値 | `true` に設定すると、顧客は重複排除キーを指定できます。デフォルト値は `false` です。詳しくは、[重複排除キー](../ui/activate-batch-profile-destinations.md#deduplication-keys)を参照してください。 |
| `defaultExportMode` | 列挙 | デフォルトのファイルのエクスポートモードを定義します。サポートされている値。<ul><li>`DAILY_FULL_EXPORT`</li><li>`FIRST_FULL_THEN_INCREMENTAL`</li></ul><br> デフォルト値は `DAILY_FULL_EXPORT` です。ファイルエクスポートのスケジューリングについて詳しくは、[バッチ有効化ドキュメント](../ui/activate-batch-profile-destinations.md#scheduling)を参照してください。 |
| `allowedExportModes` | リスト | 顧客が使用できるファイルのエクスポートモードを定義します。サポートされている値。<ul><li>`DAILY_FULL_EXPORT`</li><li>`FIRST_FULL_THEN_INCREMENTAL`</li></ul> |
| `allowedScheduleFrequency` | リスト | 顧客が使用できるファイルエクスポートの頻度を定義します。サポートされている値。<ul><li>`ONCE`</li><li>`EVERY_3_HOURS`</li><li>`EVERY_6_HOURS`</li><li>`EVERY_8_HOURS`</li><li>`EVERY_12_HOURS`</li><li>`DAILY`</li></ul> |
| `defaultFrequency` | 列挙 | デフォルトのファイルエクスポートの頻度を定義します。サポートされている値は次のとおりです。<ul><li>`ONCE`</li><li>`EVERY_3_HOURS`</li><li>`EVERY_6_HOURS`</li><li>`EVERY_8_HOURS`</li><li>`EVERY_12_HOURS`</li><li>`DAILY`</li></ul> <br> デフォルト値は `DAILY` です。 |
| `defaultStartTime` | 文字列 | ファイルエクスポートのデフォルトの開始時間を定義します。24 時間のファイル形式を使用します。デフォルト値は「00:00」です。 |

## プロファイル選定履歴 {#profile-backfill}

宛先の設定で `backfillHistoricalProfileData` パラメーターを使用すると、過去のプロファイル資格を宛先にエクスポートする必要があるかどうかを指定できます。

```json
   "backfillHistoricalProfileData":true
```

| パラメーター | タイプ | 説明 |
|---------|----------|------|
| `backfillHistoricalProfileData` | ブール値 | 宛先に対してセグメントを有効化する際に、過去のプロファイルデータをエクスポートするかどうかを制御します。<br> <ul><li> `true`：[!DNL Platform] は、セグメントが有効化される前に、そのセグメントに資格のある過去のユーザープロファイルを送信します。 </li><li> `false`：[!DNL Platform] には、セグメントがアクティブ化された後にセグメントに適合するユーザープロファイルのみが含まれます。 </li></ul> |

## この構成が宛先に必要なすべての情報をどのように接続するか {#connecting-all-configurations}

一部の宛先設定は、[宛先サーバー](./server-and-file-configuration.md)または[オーディエンスメタデータ設定](./audience-metadata-management.md)を介して設定する必要があります。ここで説明する宛先の構成は、以下のように他の 2 つの構成を参照することで、これらの設定をすべて接続するものです。

* `destinationServerId` を使用して、目的の宛先に対して設定された宛先サーバーおよびテンプレート設定を参照します。
* `audienceMetadataId` を使用して、目的の宛先に対して設定されたオーディエンスメタデータを参照します。
