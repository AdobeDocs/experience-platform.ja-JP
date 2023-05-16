---
description: このページでは、Adobe Experience Platform Destination SDKを介した宛先サーバーの作成に使用される API 呼び出しの例を示します。
title: 宛先サーバー設定の作成
source-git-commit: 118ff85a9fceb8ee81dbafe2c381d365b813da29
workflow-type: tm+mt
source-wordcount: '1623'
ht-degree: 52%

---


# 宛先サーバー設定の作成

宛先サーバーを作成することは、Destination SDKを使用して独自の宛先を作成する最初の手順です。 宛先サーバーには、 [server](../../functionality/destination-server/server-specs.md) および [テンプレート](../../functionality/destination-server/templating-specs.md) 仕様 [メッセージ形式](../../functionality/destination-server/message-format.md)、および [ファイル形式](../../functionality/destination-server/file-formatting.md) オプション（ファイルベースの宛先の場合）

このページの例では、API リクエストとペイロードを示しています。この API リクエストを使用して、 `/authoring/destination-servers` API エンドポイント。

このエンドポイントを通じて設定できる機能について詳しくは、次の記事を参照してください。

* [Destination SDK](../../../destination-sdk/functionality/destination-server/server-specs.md)
* [Destination SDK](../../../destination-sdk/functionality/destination-server/templating-specs.md)
* [メッセージの形式](../../../destination-sdk/functionality/destination-server/message-format.md)
* [ファイル形式設定](../../../destination-sdk/functionality/destination-server/file-formatting.md)

>[!IMPORTANT]
>
>Destination SDKでサポートされるすべてのパラメーター名と値は **大文字と小文字を区別**. 大文字と小文字の区別に関するエラーを避けるには、ドキュメントに示すように、パラメーターの名前と値を正確に使用してください。

## 宛先サーバー API 操作の概要 {#get-started}

続行する前に「[はじめる前に](../../getting-started.md)」を参照し、必要な宛先オーサリング権限および必要なヘッダーの取得方法など、API の呼び出しを正常に行うために必要となる重要な情報を確認してください。

## 宛先サーバー設定の作成 {#create}

新しい宛先サーバー設定を作成するには、 `POST` にリクエスト `/authoring/destination-servers` endpoint.

>[!TIP]
>
>**API エンドポイント**：`platform.adobe.io/data/core/activation/authoring/destination-servers`

**API 形式**

```http
POST /authoring/destination-servers
```

作成する宛先の種類に応じて、少し異なる種類の宛先サーバーを設定する必要があります。 Destination SDKでサポートされるすべての宛先の種類については、次の「宛先サーバーの例」タブのを参照してください。

以下のサンプルペイロードには、各宛先サーバータイプでサポートされるすべてのパラメーターが含まれています。 リクエストにすべてのパラメーターを含める必要はありません。 ペイロードは、ニーズに応じてカスタマイズできます。

下の各タブを選択し、対応する API リクエストを表示します。

>[!BEGINTABS]

>[!TAB リアルタイム（ストリーミング）]

**リアルタイム（ストリーミング）宛先サーバーの作成**

リアルタイム（ストリーミング）API ベースの統合を設定する際は、次に示すようなリアルタイム（ストリーミング）の宛先サーバーを作成する必要があります。

+++リクエスト

```shell
curl -X POST https://platform.adobe.io/data/core/activation/authoring/destination-servers \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d '
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
         "value":"{ \"attributes\": [ {% for ns in [\"external_id\", \"yourdestination_id\"] %} {% if input.profile.identityMap[ns] is not empty and first_namespace_encountered %} , {% endif %} {% set first_namespace_encountered = true %} {% for identity in input.profile.identityMap[ns]%} { \"{{ ns }}\": \"{{ identity.id }}\" {% if input.profile.segmentMembership.ups is not empty %} , \"AEPSegments\": { \"add\": [ {% for segment in input.profile.segmentMembership.ups %} {% if segment.value.status == \"realized\" or segment.value.status == \"existing\" %} {% if added_segment_found %} , {% endif %} {% set added_segment_found = true %} \"{{ destination.segmentAliases[segment.key] }}\" {% endif %} {% endfor %} ], \"remove\": [ {% for segment in input.profile.segmentMembership.ups %} {% if segment.value.status == \"exited\" %} {% if removed_segment_found %} , {% endif %} {% set removed_segment_found = true %} \"{{ destination.segmentAliases[segment.key] }}\" {% endif %} {% endfor %} ] } {% set removed_segment_found = false %} {% set added_segment_found = false %} {% endif %} {% if input.profile.attributes is not empty %} , {% endif %} {% for attribute in input.profile.attributes %} \"{{ attribute.key }}\": {% if attribute.value is empty %} null {% else %} \"{{ attribute.value.value }}\" {% endif %} {% if not loop.last%} , {% endif %} {% endfor %} } {% if not loop.last %} , {% endif %} {% endfor %} {% endfor %} ] }"
      },
      "contentType":"application/json"
   }
}
```

| パラメーター | タイプ | 説明 |
| -------- | ----------- | ----------- |
| `name` | 文字列 | *必須。* サーバーのわかりやすい名前を表し、アドビにのみ表示されます。この名前は、パートナーや顧客には表示されません。例えば `Moviestar destination server` です。 |
| `destinationServerType` | 文字列 | *必須。* に設定 `URL_BASED` ( リアルタイム（ストリーミング）宛先の場合 ) |
| `urlBasedDestination.url.templatingStrategy` | 文字列 | *必須。* <ul><li>以下の `value` フィールドの URL をアドビが変換する必要がある場合は、`PEBBLE_V1` を使用します。次のようなエンドポイントがある場合は、このオプションを使用します。 `https://api.moviestar.com/data/{{customerData.region}}/items`( `region` 部品は顧客によって異なる場合があります。 この場合は、 `region` as a [顧客データフィールド](../../functionality/destination-configuration/customer-data-fields.md) 内 [宛先設定](../destination-configuration/create-destination-configuration.md. </li><li> アドビ側での変換が不要な場合は、`NONE` を使用します。例えば、次のようなエンドポイントがある場合です。`https://api.moviestar.com/data/items`</li></ul> |
| `urlBasedDestination.url.value` | 文字列 | *必須。* Experience Platform が接続する API エンドポイントのアドレスを入力します。 |
| `httpTemplate.httpMethod` | 文字列 | *必須。* サーバーへの呼び出しでアドビが使用するメソッド。オプションは、`GET`、`PUT`、`POST`、`DELETE`、`PATCH` です。 |
| `httpTemplate.requestBody.templatingStrategy` | 文字列 | *必須。* `PEBBLE_V1` を使用します。 |
| `httpTemplate.requestBody.value` | 文字列 | *必須。* この文字列は、Platform 顧客のデータをサービスが想定する形式に変換する、文字エスケープバージョンです。<br> <ul><li> テンプレートの記述方法について詳しくは、[テンプレートセクションの使用](../../functionality/destination-server/message-format.md#using-templating)を参照してください。 </li><li> 文字のエスケープについて詳しくは、[RFC JSON 規格の第 7 節](https://tools.ietf.org/html/rfc8259#section-7)を参照してください。 </li><li> 単純な変換の例については、[プロファイル属性](../../functionality/destination-server/message-format.md#attributes)の変換を参照してください。 </li></ul> |
| `httpTemplate.contentType` | 文字列 | *必須。* サーバーが受け入れるコンテンツタイプ。この値は `application/json` である可能性が高いです。 |

{style="table-layout:auto"}

+++

+++応答

リクエストが成功した場合は、新しく作成した宛先サーバー設定の詳細を含む HTTP ステータス 200 が返されます。

+++

>[!TAB Amazon S3]

**Amazon S3 の宛先サーバーの作成**

次を作成する必要があります： [!DNL Amazon S3] ファイルベースのを設定する際に、次に示すような宛先サーバー [!DNL Amazon S3] 宛先。

+++リクエスト

```shell
curl -X POST https://platform.adobe.io/data/core/activation/authoring/destination-servers \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d '
{
    "name": "S3 destination",
    "destinationServerType": "FILE_BASED_S3",
    "fileBasedS3Destination": {
        "bucket": {
            "templatingStrategy": "PEBBLE_V1",
            "value": "{{customerData.bucket}}"
        },
        "path": {
            "templatingStrategy": "PEBBLE_V1",
            "value": "{{customerData.path}}"
        }
    },
    "fileConfigurations": {
        "compression": {
            "templatingStrategy": "PEBBLE_V1",
            "value": "{{customerData.compression}}"
        },
        "fileType": {
            "templatingStrategy": "PEBBLE_V1",
            "value": "{{customerData.fileType}}"
        },
        "csvOptions": {
            "quote": {
                "templatingStrategy": "NONE",
                "value": "\""
            },
            "quoteAll": {
                "templatingStrategy": "NONE",
                "value": "false"
            },
            "escape": {
                "templatingStrategy": "NONE",
                "value": "\\"
            },
            "escapeQuotes": {
                "templatingStrategy": "NONE",
                "value": "true"
            },
            "header": {
                "templatingStrategy": "NONE",
                "value": "true"
            },
            "ignoreLeadingWhiteSpace": {
                "templatingStrategy": "NONE",
                "value": "true"
            },
            "ignoreTrailingWhiteSpace": {
                "templatingStrategy": "NONE",
                "value": "true"
            },
            "nullValue": {
                "templatingStrategy": "NONE",
                "value": ""
            },
            "dateFormat": {
                "templatingStrategy": "NONE",
                "value": "yyyy-MM-dd"
            },
            "timestampFormat": {
                "templatingStrategy": "NONE",
                "value": "yyyy-MM-dd'T':mm:ss[.SSS][XXX]"
            },
            "charToEscapeQuoteEscaping": {
                "templatingStrategy": "NONE",
                "value": "\\"
            },
            "emptyValue": {
                "templatingStrategy": "NONE",
                "value": ""
            }
        }
    }
}
```

| パラメーター | タイプ | 説明 |
|---|---|---|
| `name` | 文字列 | 宛先接続の名前。 |
| `destinationServerType` | 文字列 | この値は、宛先プラットフォームに応じて設定します。 [!DNL Amazon S3] の場合は、これを `FILE_BASED_S3` に設定します。 |
| `fileBasedS3Destination.bucket.templatingStrategy` | 文字列 | *必須。* `PEBBLE_V1` を使用します。 |
| `fileBasedS3Destination.bucket.value` | 文字列 | この宛先が使用する [!DNL Amazon S3] バケット名。 |
| `fileBasedS3Destination.path.templatingStrategy` | 文字列 | *必須。* `PEBBLE_V1` を使用します。 |
| `fileBasedS3Destination.path.value` | 文字列 | 書き出したファイルをホストする保存先フォルダーのパス。 |
| `fileConfigurations` | なし | 詳しくは、 [ファイルフォーマット設定](../../functionality/destination-server/file-formatting.md) を参照してください。 |

{style="table-layout:auto"}

+++

+++応答

リクエストが成功した場合は、新しく作成した宛先サーバー設定の詳細を含む HTTP ステータス 200 が返されます。

+++

>[!TAB SFTP]

**の作成 [!DNL SFTP] 宛先サーバー**

次を作成する必要があります： [!DNL SFTP] ファイルベースのを設定する際に、次に示すような宛先サーバー [!DNL SFTP] 宛先。

+++リクエスト

```shell
curl -X POST https://platform.adobe.io/data/core/activation/authoring/destination-servers \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d '
{
   "name":"File-based SFTP destination server",
   "destinationServerType":"FILE_BASED_SFTP",
   "fileBasedSftpDestination":{
      "rootDirectory":{
         "templatingStrategy":"PEBBLE_V1",
         "value":"{{customerData.rootDirectory}}"
      }, 
      "port": 22,
      "encryptionMode" : "PGP"
   },
    "fileConfigurations": {
        "compression": {
            "templatingStrategy": "PEBBLE_V1",
            "value": "{{customerData.compression}}"
        },
        "fileType": {
            "templatingStrategy": "PEBBLE_V1",
            "value": "{{customerData.fileType}}"
        },
        "csvOptions": {
            "quote": {
                "templatingStrategy": "NONE",
                "value": "\""
            },
            "quoteAll": {
                "templatingStrategy": "NONE",
                "value": "false"
            },
            "escape": {
                "templatingStrategy": "NONE",
                "value": "\\"
            },
            "escapeQuotes": {
                "templatingStrategy": "NONE",
                "value": "true"
            },
            "header": {
                "templatingStrategy": "NONE",
                "value": "true"
            },
            "ignoreLeadingWhiteSpace": {
                "templatingStrategy": "NONE",
                "value": "true"
            },
            "ignoreTrailingWhiteSpace": {
                "templatingStrategy": "NONE",
                "value": "true"
            },
            "nullValue": {
                "templatingStrategy": "NONE",
                "value": ""
            },
            "dateFormat": {
                "templatingStrategy": "NONE",
                "value": "yyyy-MM-dd"
            },
            "timestampFormat": {
                "templatingStrategy": "NONE",
                "value": "yyyy-MM-dd'T':mm:ss[.SSS][XXX]"
            },
            "charToEscapeQuoteEscaping": {
                "templatingStrategy": "NONE",
                "value": "\\"
            },
            "emptyValue": {
                "templatingStrategy": "NONE",
                "value": ""
            }
        }
    }
}
```

| パラメーター | タイプ | 説明 |
|---|---|---|
| `name` | 文字列 | 宛先接続の名前。 |
| `destinationServerType` | 文字列 | この値は、宛先プラットフォームに応じて設定します。 [!DNL SFTP] の宛先の場合、これを `FILE_BASED_SFTP` に設定します。 |
| `fileBasedSftpDestination.rootDirectory.templatingStrategy` | 文字列 | *必須。* `PEBBLE_V1` を使用します。 |
| `fileBasedSftpDestination.rootDirectory.value` | 文字列 | 宛先ストレージのルートディレクトリ。 |
| `fileBasedSftpDestination.hostName.templatingStrategy` | 文字列 | *必須。* `PEBBLE_V1` を使用します。 |
| `fileBasedSftpDestination.hostName.value` | 文字列 | 宛先ストレージのホスト名。 |
| `port` | 整数 | SFTP ファイルサーバーのポート。 |
| `encryptionMode` | 文字列 | ファイルの暗号化を使用するかどうかを示します。 サポートされている値。 <ul><li>PGP</li><li>なし</li></ul> |
| `fileConfigurations` | なし | 詳しくは、 [ファイルフォーマット設定](../../functionality/destination-server/file-formatting.md) を参照してください。 |

{style="table-layout:auto"}

+++

+++応答

リクエストが成功した場合は、新しく作成した宛先サーバー設定の詳細を含む HTTP ステータス 200 が返されます。

+++

>[!TAB Azure Data Lake Storage]

**の作成 [!DNL Azure Data Lake Storage] 宛先サーバー**

次を作成する必要があります： [!DNL Azure Data Lake Storage] ファイルベースのを設定する際に、次に示すような宛先サーバー [!DNL Azure Data Lake Storage] 宛先。

+++リクエスト

```shell
curl -X POST https://platform.adobe.io/data/core/activation/authoring/destination-servers \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d '
{
   "name":"ADLS destination server",
   "destinationServerType":"FILE_BASED_ADLS_GEN2",
   "fileBasedAdlsGen2Destination":{
      "path":{
         "templatingStrategy":"PEBBLE_V1",
         "value":"{{customerData.path}}"
      }
   },
  "fileConfigurations": {
        "compression": {
            "templatingStrategy": "PEBBLE_V1",
            "value": "{{customerData.compression}}"
        },
        "fileType": {
            "templatingStrategy": "PEBBLE_V1",
            "value": "{{customerData.fileType}}"
        },
        "csvOptions": {
            "quote": {
                "templatingStrategy": "NONE",
                "value": "\""
            },
            "quoteAll": {
                "templatingStrategy": "NONE",
                "value": "false"
            },
            "escape": {
                "templatingStrategy": "NONE",
                "value": "\\"
            },
            "escapeQuotes": {
                "templatingStrategy": "NONE",
                "value": "true"
            },
            "header": {
                "templatingStrategy": "NONE",
                "value": "true"
            },
            "ignoreLeadingWhiteSpace": {
                "templatingStrategy": "NONE",
                "value": "true"
            },
            "ignoreTrailingWhiteSpace": {
                "templatingStrategy": "NONE",
                "value": "true"
            },
            "nullValue": {
                "templatingStrategy": "NONE",
                "value": ""
            },
            "dateFormat": {
                "templatingStrategy": "NONE",
                "value": "yyyy-MM-dd"
            },
            "timestampFormat": {
                "templatingStrategy": "NONE",
                "value": "yyyy-MM-dd'T':mm:ss[.SSS][XXX]"
            },
            "charToEscapeQuoteEscaping": {
                "templatingStrategy": "NONE",
                "value": "\\"
            },
            "emptyValue": {
                "templatingStrategy": "NONE",
                "value": ""
            }
        }
    }
}
```

| パラメーター | タイプ | 説明 |
|---|---|---|
| `name` | 文字列 | 宛先接続の名前。 |
| `destinationServerType` | 文字列 | この値は、宛先プラットフォームに応じて設定します。 [!DNL Azure Data Lake Storage] の宛先の場合、これを `FILE_BASED_ADLS_GEN2` に設定します。 |
| `fileBasedAdlsGen2Destination.path.templatingStrategy` | 文字列 | *必須。* `PEBBLE_V1` を使用します。 |
| `fileBasedAdlsGen2Destination.path.value` | 文字列 | 書き出したファイルをホストする保存先フォルダーのパス。 |
| `fileConfigurations` | なし | 詳しくは、 [ファイルフォーマット設定](../../functionality/destination-server/file-formatting.md) を参照してください。 |

{style="table-layout:auto"}

+++

+++応答

リクエストが成功した場合は、新しく作成した宛先サーバー設定の詳細を含む HTTP ステータス 200 が返されます。

+++

>[!TAB Azure Blob ストレージ]

**の作成 [!DNL Azure Blob Storage] 宛先サーバー**

次を作成する必要があります： [!DNL Azure Blob Storage] ファイルベースのを設定する際に、次に示すような宛先サーバー [!DNL Azure Blob Storage] 宛先。

+++リクエスト

```shell
curl -X POST https://platform.adobe.io/data/core/activation/authoring/destination-servers \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d '
{
   "name":"Blob destination server",
   "destinationServerType":"FILE_BASED_AZURE_BLOB",
   "fileBasedAzureBlobDestination":{
      "path":{
         "templatingStrategy":"PEBBLE_V1",
         "value":"{{customerData.path}}"
      },
      "container":{
         "templatingStrategy":"PEBBLE_V1",
         "value":"{{customerData.container}}"
      }
   },
  "fileConfigurations": {
        "compression": {
            "templatingStrategy": "PEBBLE_V1",
            "value": "{{customerData.compression}}"
        },
        "fileType": {
            "templatingStrategy": "PEBBLE_V1",
            "value": "{{customerData.fileType}}"
        },
        "csvOptions": {
            "quote": {
                "templatingStrategy": "NONE",
                "value": "\""
            },
            "quoteAll": {
                "templatingStrategy": "NONE",
                "value": "false"
            },
            "escape": {
                "templatingStrategy": "NONE",
                "value": "\\"
            },
            "escapeQuotes": {
                "templatingStrategy": "NONE",
                "value": "true"
            },
            "header": {
                "templatingStrategy": "NONE",
                "value": "true"
            },
            "ignoreLeadingWhiteSpace": {
                "templatingStrategy": "NONE",
                "value": "true"
            },
            "ignoreTrailingWhiteSpace": {
                "templatingStrategy": "NONE",
                "value": "true"
            },
            "nullValue": {
                "templatingStrategy": "NONE",
                "value": ""
            },
            "dateFormat": {
                "templatingStrategy": "NONE",
                "value": "yyyy-MM-dd"
            },
            "timestampFormat": {
                "templatingStrategy": "NONE",
                "value": "yyyy-MM-dd'T':mm:ss[.SSS][XXX]"
            },
            "charToEscapeQuoteEscaping": {
                "templatingStrategy": "NONE",
                "value": "\\"
            },
            "emptyValue": {
                "templatingStrategy": "NONE",
                "value": ""
            }
        }
    }
}
```

| パラメーター | タイプ | 説明 |
|---|---|---|
| `name` | 文字列 | 宛先接続の名前。 |
| `destinationServerType` | 文字列 | この値は、宛先プラットフォームに応じて設定します。 [!DNL Azure Blob Storage] の宛先の場合、これを `FILE_BASED_AZURE_BLOB` に設定します。 |
| `fileBasedAzureBlobDestination.path.templatingStrategy` | 文字列 | *必須。* `PEBBLE_V1` を使用します。 |
| `fileBasedAzureBlobDestination.path.value` | 文字列 | 書き出したファイルをホストする保存先フォルダーのパス。 |
| `fileBasedAzureBlobDestination.container.templatingStrategy` | 文字列 | *必須。* `PEBBLE_V1` を使用します。 |
| `fileBasedAzureBlobDestination.container.value` | 文字列 | この宛先で使用される [!DNL Azure Blob Storage] コンテナ名。 |
| `fileConfigurations` | なし | 詳しくは、 [ファイルフォーマット設定](../../functionality/destination-server/file-formatting.md) を参照してください。 |

{style="table-layout:auto"}

+++

+++応答

リクエストが成功した場合は、新しく作成した宛先サーバー設定の詳細を含む HTTP ステータス 200 が返されます。

+++

>[!TAB データランディングゾーン (DLZ)]

**の作成 [!DNL Data Landing Zone (DLZ)] 宛先サーバー**

次を作成する必要があります： [!DNL Data Landing Zone (DLZ)] ファイルベースのを設定する際に、次に示すような宛先サーバー [!DNL Data Landing Zone (DLZ)] 宛先。

+++リクエスト

```shell
curl -X POST https://platform.adobe.io/data/core/activation/authoring/destination-servers \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d '
{
   "name":"DLZ destination server",
   "destinationServerType":"FILE_BASED_DLZ",
   "fileBasedDlzDestination":{
      "path":{
         "templatingStrategy":"PEBBLE_V1",
         "value":"{{customerData.path}}"
      },
      "useCase": "Your use case"
   },
   "fileConfigurations": {
        "compression": {
            "templatingStrategy": "PEBBLE_V1",
            "value": "{{customerData.compression}}"
        },
        "fileType": {
            "templatingStrategy": "PEBBLE_V1",
            "value": "{{customerData.fileType}}"
        },
        "csvOptions": {
            "quote": {
                "templatingStrategy": "NONE",
                "value": "\""
            },
            "quoteAll": {
                "templatingStrategy": "NONE",
                "value": "false"
            },
            "escape": {
                "templatingStrategy": "NONE",
                "value": "\\"
            },
            "escapeQuotes": {
                "templatingStrategy": "NONE",
                "value": "true"
            },
            "header": {
                "templatingStrategy": "NONE",
                "value": "true"
            },
            "ignoreLeadingWhiteSpace": {
                "templatingStrategy": "NONE",
                "value": "true"
            },
            "ignoreTrailingWhiteSpace": {
                "templatingStrategy": "NONE",
                "value": "true"
            },
            "nullValue": {
                "templatingStrategy": "NONE",
                "value": ""
            },
            "dateFormat": {
                "templatingStrategy": "NONE",
                "value": "yyyy-MM-dd"
            },
            "timestampFormat": {
                "templatingStrategy": "NONE",
                "value": "yyyy-MM-dd'T':mm:ss[.SSS][XXX]"
            },
            "charToEscapeQuoteEscaping": {
                "templatingStrategy": "NONE",
                "value": "\\"
            },
            "emptyValue": {
                "templatingStrategy": "NONE",
                "value": ""
            }
        }
    }
}
```

| パラメーター | タイプ | 説明 |
|---|---|---|
| `name` | 文字列 | 宛先接続の名前。 |
| `destinationServerType` | 文字列 | この値は、宛先プラットフォームに応じて設定します。 [!DNL Data Landing Zone] の宛先の場合、これを `FILE_BASED_DLZ` に設定します。 |
| `fileBasedDlzDestination.path.templatingStrategy` | 文字列 | *必須。*`PEBBLE_V1` を使用します。 |
| `fileBasedDlzDestination.path.value` | 文字列 | 書き出したファイルをホストする保存先フォルダーのパス。 |
| `fileConfigurations` | なし | 詳しくは、 [ファイルフォーマット設定](../../functionality/destination-server/file-formatting.md) を参照してください。 |

{style="table-layout:auto"}

+++

+++応答

リクエストが成功した場合は、新しく作成した宛先サーバー設定の詳細を含む HTTP ステータス 200 が返されます。

+++

>[!TAB Google Cloud Storage]

**の作成 [!DNL Google Cloud Storage] 宛先サーバー**

次を作成する必要があります： [!DNL Google Cloud Storage] ファイルベースのを設定する際に、次に示すような宛先サーバー [!DNL Google Cloud Storage] 宛先。

+++リクエスト

```shell
curl -X POST https://platform.adobe.io/data/core/activation/authoring/destination-servers \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d '
{
   "name":"Google Cloud Storage Server",
   "destinationServerType":"FILE_BASED_GOOGLE_CLOUD",
   "fileBasedGoogleCloudStorageDestination":{
      "bucket":{
         "templatingStrategy":"PEBBLE_V1",
         "value":"{{customerData.bucket}}"
      },
      "path":{
         "templatingStrategy":"PEBBLE_V1",
         "value":"{{customerData.path}}"
      }
   },
  "fileConfigurations": {
        "compression": {
            "templatingStrategy": "PEBBLE_V1",
            "value": "{{customerData.compression}}"
        },
        "fileType": {
            "templatingStrategy": "PEBBLE_V1",
            "value": "{{customerData.fileType}}"
        },
        "csvOptions": {
            "quote": {
                "templatingStrategy": "NONE",
                "value": "\""
            },
            "quoteAll": {
                "templatingStrategy": "NONE",
                "value": "false"
            },
            "escape": {
                "templatingStrategy": "NONE",
                "value": "\\"
            },
            "escapeQuotes": {
                "templatingStrategy": "NONE",
                "value": "true"
            },
            "header": {
                "templatingStrategy": "NONE",
                "value": "true"
            },
            "ignoreLeadingWhiteSpace": {
                "templatingStrategy": "NONE",
                "value": "true"
            },
            "ignoreTrailingWhiteSpace": {
                "templatingStrategy": "NONE",
                "value": "true"
            },
            "nullValue": {
                "templatingStrategy": "NONE",
                "value": ""
            },
            "dateFormat": {
                "templatingStrategy": "NONE",
                "value": "yyyy-MM-dd"
            },
            "timestampFormat": {
                "templatingStrategy": "NONE",
                "value": "yyyy-MM-dd'T':mm:ss[.SSS][XXX]"
            },
            "charToEscapeQuoteEscaping": {
                "templatingStrategy": "NONE",
                "value": "\\"
            },
            "emptyValue": {
                "templatingStrategy": "NONE",
                "value": ""
            }
        }
    }
}
```

| パラメーター | タイプ | 説明 |
|---|---|---|
| `name` | 文字列 | 宛先接続の名前。 |
| `destinationServerType` | 文字列 | この値は、宛先プラットフォームに応じて設定します。 [!DNL Google Cloud Storage] の宛先の場合、これを `FILE_BASED_GOOGLE_CLOUD` に設定します。 |
| `fileBasedGoogleCloudStorageDestination.bucket.templatingStrategy` | 文字列 | *必須。*`PEBBLE_V1` を使用します。 |
| `fileBasedGoogleCloudStorageDestination.bucket.value` | 文字列 | この宛先が使用する [!DNL Google Cloud Storage] バケット名。 |
| `fileBasedGoogleCloudStorageDestination.path.templatingStrategy` | 文字列 | *必須。* `PEBBLE_V1` を使用します。 |
| `fileBasedGoogleCloudStorageDestination.path.value` | 文字列 | 書き出したファイルをホストする保存先フォルダーのパス。 |
| `fileConfigurations` | なし | 詳しくは、 [ファイルフォーマット設定](../../functionality/destination-server/file-formatting.md) を参照してください。 |

{style="table-layout:auto"}

+++

+++応答

リクエストが成功した場合は、新しく作成した宛先サーバー設定の詳細を含む HTTP ステータス 200 が返されます。

+++

>[!TAB 動的スキーマサーバー]

**動的スキーマサーバーの作成**

独自の API エンドポイントからプロファイルスキーマを取得する宛先を設定する際には、次に示すような動的スキーマサーバーを作成する必要があります。 動的スキーマは、静的スキーマとは異なり、 `profileFields` 配列。 代わりに、動的スキーマは、スキーマ設定を取得する独自の API に接続する動的スキーマサーバーを使用します。

+++リクエスト

```shell
curl -X POST https://platform.adobe.io/data/core/activation/authoring/destination-servers \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d '
{
   "name":"Dynamic Schema Server",
   "destinationServerType":"URL_BASED",
   "urlBasedDestination":{
      "url":{
         "templatingStrategy":"PEBBLE_V1",
         "value":"https://YOUR_API_ENDPOINT/"
      }
   },
   "httpTemplate":{
      "httpMethod":"GET"
   },
   "responseFields":[
      {
         "templatingStrategy":"PEBBLE_V1",
         "value":"{\n    \"type\":\"object\",\n    \"title\": \"Contact Schema\",\n    \"properties\": {\n        {% for setDefinition in response.body.items %}\n            \"{{setDefinition.key}}\": {\n                \"title\" : \"{{setDefinition.name.value}}\",\n                \"type\" : \"object\",\n                \"properties\": {\n                    {% for attribute in setDefinition.attributes %}\n                        \"{{attribute.key}}\": {\n                            \"title\" : \"{{attribute.name.value}}\",\n                            \"type\" : \"string\"\n                        }\n                        {% if not loop.last %},{%endif%}\n                    {% endfor %}\n                }\n            }\n            {% if not loop.last %},{%endif%}\n        {% endfor %}\n    }\n}",
         "name":"schema"
      }
   ]
}
```

| パラメーター | タイプ | 説明 |
| -------- | ----------- | ----------- |
| `name` | 文字列 | *必須。* 動的スキーマサーバーのわかりやすい名前を表し、Adobeにのみ表示されます。 |
| `destinationServerType` | 文字列 | *必須。* に設定 `URL_BASED` 動的スキーマサーバー用。 |
| `urlBasedDestination.url.templatingStrategy` | 文字列 | *必須。* <ul><li>以下の `value` フィールドの URL をアドビが変換する必要がある場合は、`PEBBLE_V1` を使用します。次のようなエンドポイントがある場合は、このオプションを使用します。`https://api.moviestar.com/data/{{customerData.region}}/items` </li><li> アドビ側での変換が不要な場合は、`NONE` を使用します。例えば、次のようなエンドポイントがある場合です。`https://api.moviestar.com/data/items`</li></ul> |
| `urlBasedDestination.url.value` | 文字列 | *必須。* Experience Platformが接続する API エンドポイントのアドレスを入力し、アクティベーションワークフローのマッピング手順で、ターゲットフィールドとして入力するスキーマフィールドを取得します。 |
| `httpTemplate.httpMethod` | 文字列 | *必須。* サーバーへの呼び出しでアドビが使用するメソッド。動的スキーマサーバーの場合は、 `GET`. |
| `responseFields.templatingStrategy` | 文字列 | *必須。* `PEBBLE_V1` を使用します。 |
| `responseFields.value` | 文字列 | *必須。* この文字列は、パートナー API から受け取った応答を、Platform UI に表示されるパートナースキーマに変換する、文字エスケープされた変換テンプレートです。 <br> <ul><li> テンプレートの記述方法について詳しくは、[テンプレートセクションの使用](../../functionality/destination-server/message-format.md#using-templating)を参照してください。 </li><li> 文字のエスケープについて詳しくは、[RFC JSON 規格の第 7 節](https://tools.ietf.org/html/rfc8259#section-7)を参照してください。 </li><li> 単純な変換の例については、[プロファイル属性](../../functionality/destination-server/message-format.md#attributes)の変換を参照してください。 </li></ul> |

{style="table-layout:auto"}

+++

+++応答

リクエストが成功した場合は、新しく作成した宛先サーバー設定の詳細を含む HTTP ステータス 200 が返されます。

+++

>[!ENDTABS]

## API エラー処理 {#error-handling}

Destination SDK API エンドポイントは、一般的な Experience Platform API エラーメッセージの原則に従います。Platform トラブルシューティングガイドの [API ステータスコード](../../../../landing/troubleshooting.md#api-status-codes)および[リクエストヘッダーエラー](../../../../landing/troubleshooting.md#request-header-errors)を参照してください。

## 次の手順 {#next-steps}

このドキュメントを読んだ後、Destination SDKを使用して新しい宛先サーバーを作成する方法がわかりました。 `/authoring/destination-servers` API エンドポイント。

このエンドポイントで実行できる操作について詳しくは、次の記事を参照してください。

* [宛先サーバー設定の取得](retrieve-destination-server.md)
* [宛先サーバー設定の更新](update-destination-server.md)
* [宛先サーバー設定の削除](delete-destination-server.md)

このエンドポイントが宛先オーサリングプロセスに適した場所を理解するには、次の記事を参照してください。

* [Destination SDK を使用したストリーミングの宛先の設定](../../guides/configure-destination-instructions.md#create-server-template-configuration)
* [Destination SDK を使用したファイルベースの宛先の設定](../../guides/configure-file-based-destination-instructions.md#create-server-file-configuration)