---
description: 「/authoring/destination-servers」エンドポイントを使用してAdobe Experience Platform Destination SDKの宛先サーバーの仕様を設定する方法について説明します。
title: Destination SDK
source-git-commit: 118ff85a9fceb8ee81dbafe2c381d365b813da29
workflow-type: tm+mt
source-wordcount: '2750'
ht-degree: 11%

---


# Destination SDK

宛先サーバーの仕様は、Adobe Experience Platformからデータを受信する宛先プラットフォームのタイプと、Platform と宛先の間の通信パラメーターを定義します。 例：

* A [ストリーミング](#streaming-example) 宛先サーバーの仕様は、Platform から HTTP メッセージを受信する HTTP サーバーエンドポイントを定義します。 エンドポイントに対する HTTP 呼び出しの形式を設定する方法については、 [テンプレート仕様](templating-specs.md) ページ。
* An [Amazon S3](#s3-example) 宛先サーバーの仕様を定義します [!DNL S3] バケット名と、Platform がファイルを書き出す場所のパス。
* An [SFTP](#sftp-example) 宛先サーバーの仕様では、Platform がファイルを書き出す SFTP サーバーのホスト名、ルートディレクトリ、通信ポート、暗号化タイプを定義します。

Destination SDKを使用して作成された統合で、このコンポーネントがどこに適合するかを把握するには、 [設定オプション](../configuration-options.md) ドキュメントを参照するか、次の宛先設定の概要ページを参照してください。

* [Destination SDK を使用したストリーミングの宛先の設定](../../guides/configure-destination-instructions.md#create-server-template-configuratiom)
* [Destination SDK を使用したファイルベースの宛先の設定](../../guides/configure-file-based-destination-instructions.md#create-server-file-configuration)

宛先サーバーの仕様は、 `/authoring/destination-servers` endpoint. このページに示すコンポーネントを設定できる API 呼び出しの詳細な例については、次の API リファレンスページを参照してください。

* [宛先サーバー設定の作成](../../authoring-api/destination-server/create-destination-server.md)
* [宛先サーバー設定の更新](../../authoring-api/destination-server/update-destination-server.md)

このページには、Destination SDKでサポートされるすべての宛先サーバーの種類と、その設定パラメーターがすべて表示されます。 宛先を作成する際に、パラメーター値を独自の値に置き換えます。

>[!IMPORTANT]
>
>Destination SDKでサポートされるすべてのパラメーター名と値は **大文字と小文字を区別**. 大文字と小文字の区別に関するエラーを避けるには、ドキュメントに示すように、パラメーターの名前と値を正確に使用してください。

## サポートされる統合のタイプ {#supported-integration-types}

このページで説明する機能をサポートする統合のタイプについて詳しくは、次の表を参照してください。

| 統合タイプ | 機能をサポート |
|---|---|
| リアルタイム（ストリーミング）統合 | ○ |
| ファイルベース（バッチ）の統合 | ○ |

条件 [作成中](../../authoring-api/destination-server/create-destination-server.md) または [更新中](../../authoring-api/destination-server/update-destination-server.md) 宛先サーバーの場合は、このページで説明するサーバータイプ設定の 1 つを使用します。 統合の要件に応じて、必ずこれらの例のサンプルパラメーター値を独自の値に置き換えてください。

## ハードコードされたフィールドとテンプレート化されたフィールド {#templatized-fields}

Destination SDKを通じて宛先サーバーを作成する場合、設定パラメータの値を設定にハードコーディングするか、テンプレート化されたフィールドを使用して定義できます。 テンプレート化されたフィールドを使用すると、ユーザー指定の値を Platform UI から読み取ることができます。

宛先サーバーのパラメーターには、2 つの設定可能なフィールドがあります。 これらのオプションは、ハードコードされた値とテンプレート化された値のどちらを使用するかを指定します。

| パラメーター | タイプ | 説明 |
|---|---|---|
| `templatingStrategy` | 文字列 | *必須。* を介してハードコードされた値が提供されるかどうかを定義します `value` フィールド、または UI でユーザーが設定できる値。 サポートされている値。 <ul><li>`NONE`:この値は、 `value` パラメーターを使用します（次の行を参照）。 例：`"value": "my-storage-bucket"`</li><li>`PEBBLE_V1`:ユーザーが UI でパラメーター値を指定する場合は、この値を使用します。 例：`"value": "{{customerData.bucket}}"`。 </li></ul> |
| `value` | 文字列 | *必須*。パラメータ値を定義します。 サポートされている値のタイプ： <ul><li>**ハードコードされた値**:ハードコードされた値 ( `"value": "my-storage-bucket"`) を使用できます。 値をハードコーディングする場合、 `templatingStrategy` は常に次のように設定する必要があります。 `NONE`.</li><li>**テンプレート化された値**:テンプレート化された値 ( `"value": "{{customerData.bucket}}"`) を使用します。 テンプレート化された値を使用する場合、 `templatingStrategy` は常に次のように設定する必要があります。 `PEBBLE_V1`.</li></ul> |

{style="table-layout:auto"}

### ハードコードされたフィールドとテンプレート化されたフィールドを使用するタイミング

作成する統合のタイプに応じて、ハードコードされたフィールドとテンプレート化されたフィールドの両方に、Destination SDKでの独自の使用方法があります。

**ユーザー入力なしでの宛先への接続**

ユーザーが [宛先に接続](../../../ui/connect-destination.md) Platform UI では、入力なしで宛先接続プロセスを処理する必要が生じる場合があります。

これをおこなうには、サーバー仕様で宛先プラットフォーム接続パラメーターをハードコードします。 宛先サーバー設定でハードコードされたパラメーター値を使用する場合、Adobe Experience Platformと宛先プラットフォーム間の接続は、ユーザーからの入力なしで処理されます。

次の例では、パートナーが、 `path.value` フィールドをハードコードしています。

```json
{
   "name":"Data Landing Zone destination server",
   "destinationServerType":"FILE_BASED_DLZ",
   "fileBasedDlzDestination":{
      "path":{
         "templatingStrategy":"NONE",
         "value":"Your/hardcoded/path/here"
      },
      "useCase": "Your use case"
   }
}
```

その結果、ユーザーが [宛先接続のチュートリアル](../../../ui/connect-destination.md)を返さない場合、 [認証手順](../../../ui/connect-destination.md#authenticate). 代わりに、次の画像に示すように、認証は Platform で処理されます。

![Platform と DLZ の宛先の間の認証画面を示す Ui 画像。](../../assets/functionality/destination-server/server-spec-hardcoded.png)

**ユーザー入力を使用して宛先に接続する**

API エンドポイントの選択やフィールド値の指定など、Platform UI での特定のユーザー入力に従って Platform と宛先間の接続を確立する必要がある場合は、サーバー仕様のテンプレート化されたフィールドを使用して、ユーザー入力を読み取り、宛先プラットフォームに接続できます。

次の例では、パートナーが [リアルタイム（ストリーミング）](#streaming-example) 統合と `url.value` フィールドは、テンプレート化されたパラメーターを使用します `{{customerData.region}}` ユーザーの入力に基づいて API エンドポイントの一部をパーソナライズする。

```json
{
   "name":"Templatized API endpoint example",
   "destinationServerType":"URL_BASED",
   "urlBasedDestination":{
      "url":{
         "templatingStrategy":"PEBBLE_V1",
         "value":"https://api.yourcompany.com/data/{{customerData.region}}/items"
      }
   }
}
```

ユーザーに Platform UI から値を選択するオプションを与えるには、 `region` また、 [宛先設定](../../authoring-api/destination-configuration/create-destination-configuration.md) を顧客データフィールドとして割り当てます。

```json
"customerDataFields":[
   {
      "name":"region",
      "title":"Region",
      "description":"Select an option",
      "type":"string",
      "isRequired":true,
      "readOnly":false,
      "enum":[
         "US",
         "EU"
      ]
   }
```

その結果、ユーザーが [宛先接続のチュートリアル](../../../ui/connect-destination.md)の場合、宛先プラットフォームに接続する前に、地域を選択する必要があります。 宛先に接続する際、テンプレート化されたフィールド `{{customerData.region}}` は、次の画像に示すように、UI でユーザーが選択した値に置き換えられます。

![領域セレクターを含む宛先接続画面を示す Ui 画像。](../../assets/functionality/destination-server/server-spec-template-region.png)

## リアルタイム（ストリーミング）宛先サーバ {#streaming-example}

このタイプの宛先サーバーを使用すると、HTTP リクエストを介してAdobe Experience Platformから宛先にデータを書き出すことができます。 サーバー設定には、メッセージを受信するサーバー（お客様側のサーバー）に関する情報が含まれています。

このプロセスは、ユーザーデータを一連の HTTP メッセージとして宛先プラットフォームに配信します。HTTP サーバー仕様のテンプレートとなるパラメーターは次のとおりです。

次のサンプルは、リアルタイム（ストリーミング）宛先の宛先サーバー設定の例を示しています。

```json
{
   "name":"Your destination server name",
   "destinationServerType":"URL_BASED",
   "urlBasedDestination":{
      "url":{
         "templatingStrategy":"PEBBLE_V1",
         "value":"{YOUR_API_ENDPOINT}"
      }
   }
}
```

| パラメーター | タイプ | 説明 |
|---|---|---|
| `name` | 文字列 | *必須。* サーバーのわかりやすい名前を表し、アドビにのみ表示されます。この名前は、パートナーや顧客には表示されません。例：`Moviestar destination server`。 |
| `destinationServerType` | 文字列 | *必須。* これを `URL_BASED` ストリーミング先用 |
| `templatingStrategy` | 文字列 | *必須。* <ul><li>用途 `PEBBLE_V1` ハードコードされた値の代わりに、テンプレート化されたフィールドを `value` フィールドに入力します。 次のようなエンドポイントがある場合は、このオプションを使用します。 `https://api.moviestar.com/data/{{customerData.region}}/items`：ユーザーは、Platform UI からエンドポイント領域を選択する必要があります。 </li><li> 用途 `NONE` テンプレート化された変換がAdobe側で必要でない場合（例えば、次のようなエンドポイントがある場合）。 `https://api.moviestar.com/data/items` </li></ul> |
| `value` | 文字列 | *必須。* Experience Platform が接続する API エンドポイントのアドレスを入力します。 |

{style="table-layout:auto"}

## [!DNL Amazon S3] 宛先サーバー {#s3-example}

この宛先サーバーを使用すると、Adobe Experience Platformデータを含むファイルをAmazon S3 ストレージに書き出すことができます。

以下のサンプルは、Amazon S3 の宛先の宛先サーバー設定の例を示しています。

```json
{
   "name":"Amazon S3 destination",
   "destinationServerType":"FILE_BASED_S3",
   "fileBasedS3Destination":{
      "bucket":{
         "templatingStrategy":"PEBBLE_V1",
         "value":"{{customerData.bucket}}"
      },
      "path":{
         "templatingStrategy":"PEBBLE_V1",
         "value":"{{customerData.path}}"
      }
   }
}
```

| パラメーター | タイプ | 説明 |
|---|---|---|
| `name` | 文字列 | 宛先サーバーの名前。 |
| `destinationServerType` | 文字列 | この値は、宛先プラットフォームに応じて設定します。 にファイルを書き出すには [!DNL Amazon S3] バケット：次の値に設定します。 `FILE_BASED_S3`. |
| `fileBasedS3Destination.bucket.templatingStrategy` | 文字列 | *必須*。この値を、 `bucket.value` フィールドに入力します。<ul><li>ユーザーがExperience PlatformUI に独自のバケット名を入力する場合は、この値をに設定します。 `PEBBLE_V1`. この場合、 `value` 値を [顧客データフィールド](../destination-configuration/customer-data-fields.md) ユーザーが入力した内容。 この使用例は、上記の例に示されています。</li><li>統合にハードコードされたバケット名（例： ）を使用する場合 `"bucket.value":"MyBucket"`の場合、この値を `NONE`.</li></ul> |
| `fileBasedS3Destination.bucket.value` | 文字列 | この宛先が使用する [!DNL Amazon S3] バケット名。これは、テンプレート化されたフィールドにすることができます。このフィールドは、 [顧客データフィールド](../destination-configuration/customer-data-fields.md) ユーザーによって入力される（上の例で示される）か、ハードコードされた値 ( 例： `"value":"MyBucket"`. |
| `fileBasedS3Destination.path.templatingStrategy` | 文字列 | *必須*。この値を、 `path.value` フィールドに入力します。<ul><li>ユーザーがExperience PlatformUI に独自のパスを入力する場合は、この値をに設定します。 `PEBBLE_V1`. この場合、 `path.value` 値を [顧客データフィールド](../destination-configuration/customer-data-fields.md) ユーザーが入力した内容。 この使用例は、上記の例に示されています。</li><li>統合にハードコードされたパス（例： ）を使用する場合 `"bucket.value":"/path/to/MyBucket"`の場合、この値を `NONE`.</li></ul> |
| `fileBasedS3Destination.path.value` | 文字列 | パス： [!DNL Amazon S3] この宛先で使用するバケット。 これは、テンプレート化されたフィールドにすることができます。このフィールドは、 [顧客データフィールド](../destination-configuration/customer-data-fields.md) ユーザーによって入力される（上の例で示される）か、ハードコードされた値 ( 例： `"value":"/path/to/MyBucket"`. |

{style="table-layout:auto"}

## [!DNL SFTP] 宛先サーバー {#sftp-example}

この宛先サーバーを使用すると、Adobe Experience Platformデータを含むファイルを [!DNL SFTP] ストレージサーバ。

以下のサンプルは、SFTP の宛先の宛先サーバー設定の例を示しています。

```json
{
   "name":"File-based SFTP destination server",
   "destinationServerType":"FILE_BASED_SFTP",
   "fileBasedSFTPDestination":{
      "rootDirectory":{
         "templatingStrategy":"PEBBLE_V1",
         "value":"{{customerData.rootDirectory}}"
      },
      "hostName":{
         "templatingStrategy":"PEBBLE_V1",
         "value":"{{customerData.hostName}}"
      },
      "port":22,
      "encryptionMode":"PGP"
   }
}
```

| パラメーター | タイプ | 説明 |
|---|---|---|
| `name` | 文字列 | 宛先サーバーの名前。 |
| `destinationServerType` | 文字列 | この値は、宛先プラットフォームに応じて設定します。 にファイルを書き出すには [!DNL SFTP] 宛先、これをに設定 `FILE_BASED_SFTP`. |
| `fileBasedSFTPDestination.rootDirectory.templatingStrategy` | 文字列 | *必須*。この値を、 `rootDirectory.value` フィールドに入力します。<ul><li>ユーザーがExperience PlatformUI に独自のルートディレクトリパスを入力する場合は、この値をに設定します。 `PEBBLE_V1`. この場合、 `rootDirectory.value` ユーザー指定の値を [顧客データフィールド](../destination-configuration/customer-data-fields.md) ユーザーが入力した内容。 この使用例は、上記の例に示されています。</li><li>統合にハードコードされたルートディレクトリパス（例： ）を使用する場合 `"rootDirectory.value":"Storage/MyDirectory"`の場合、この値を `NONE`.</li></ul> |
| `fileBasedSFTPDestination.rootDirectory.value` | 文字列 | 書き出したファイルをホストするディレクトリのパス。 これは、テンプレート化されたフィールドにすることができます。このフィールドは、 [顧客データフィールド](../destination-configuration/customer-data-fields.md) ユーザーによって入力される（上の例で示される）か、ハードコードされた値 ( 例： `"value":"Storage/MyDirectory"` |
| `fileBasedSFTPDestination.hostName.templatingStrategy` | 文字列 | *必須*。この値を、 `hostName.value` フィールドに入力します。<ul><li>ユーザーがExperience PlatformUI に独自のホスト名を入力する場合は、この値をに設定します。 `PEBBLE_V1`. この場合、 `hostName.value` ユーザー指定の値を [顧客データフィールド](../destination-configuration/customer-data-fields.md) ユーザーが入力した内容。 この使用例は、上記の例に示されています。</li><li>統合にハードコードされたホスト名（例： ）を使用する場合 `"hostName.value":"my.hostname.com"`の場合、この値を `NONE`.</li></ul> |
| `fileBasedSFTPDestination.hostName.value` | 文字列 | SFTP サーバーのホスト名。 これは、テンプレート化されたフィールドにすることができます。このフィールドは、 [顧客データフィールド](../destination-configuration/customer-data-fields.md) ユーザーによって入力される（上の例で示される）か、ハードコードされた値 ( 例： `"hostName.value":"my.hostname.com"`. |
| `port` | 整数 | SFTP ファイルサーバーのポート。 |
| `encryptionMode` | 文字列 | ファイルの暗号化を使用するかどうかを示します。 サポートされている値。 <ul><li>PGP</li><li>なし</li></ul> |

{style="table-layout:auto"}

## [!DNL Azure Data Lake Storage] ([!DNL ADLS]) 宛先サーバー {#adls-example}

この宛先サーバーを使用すると、Adobe Experience Platformデータを含むファイルを [!DNL Azure Data Lake Storage] アカウント

次のサンプルは、 [!DNL Azure Data Lake Storage] 宛先。

```json
{
   "name":"ADLS destination server",
   "destinationServerType":"FILE_BASED_ADLS_GEN2",
   "fileBasedAdlsGen2Destination":{
      "path":{
         "templatingStrategy":"PEBBLE_V1",
         "value":"{{customerData.path}}"
      }
   }
}
```

| パラメーター | タイプ | 説明 |
|---|---|---|
| `name` | 文字列 | 宛先接続の名前。 |
| `destinationServerType` | 文字列 | この値は、宛先プラットフォームに応じて設定します。 [!DNL Azure Data Lake Storage] の宛先の場合、これを `FILE_BASED_ADLS_GEN2` に設定します。 |
| `fileBasedAdlsGen2Destination.path.templatingStrategy` | 文字列 | *必須*。この値を、 `path.value` フィールドに入力します。<ul><li>ユーザーに [!DNL ADLS] Experience PlatformUI のフォルダーパス。この値を `PEBBLE_V1`. この場合、 `path.value` 値を [顧客データフィールド](../destination-configuration/customer-data-fields.md) ユーザーが入力した内容。 この使用例は、上記の例に示されています。</li><li>統合にハードコードされたパス（例： ）を使用する場合 `"abfs://<file_system>@<account_name>.dfs.core.windows.net/<path>/"`の場合、この値を `NONE`.</li></ul> |
| `fileBasedAdlsGen2Destination.path.value` | 文字列 | パス： [!DNL ADLS] ストレージフォルダー。 これは、テンプレート化されたフィールドにすることができます。このフィールドは、 [顧客データフィールド](../destination-configuration/customer-data-fields.md) ユーザーによって入力される（上の例で示される）か、ハードコードされた値 ( 例： `abfs://<file_system>@<account_name>.dfs.core.windows.net/<path>/`. |

{style="table-layout:auto"}

## [!DNL Azure Blob Storage] 宛先サーバー {#blob-example}

この宛先サーバーを使用すると、Adobe Experience Platformデータを含むファイルを [!DNL Azure Blob Storage] コンテナ。

次のサンプルは、 [!DNL Azure Blob Storage] 宛先。

```json
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
   }
}
```

| パラメーター | タイプ | 説明 |
|---|---|---|
| `name` | 文字列 | 宛先接続の名前。 |
| `destinationServerType` | 文字列 | この値は、宛先プラットフォームに応じて設定します。 [!DNL Azure Blob Storage] の宛先の場合、これを `FILE_BASED_AZURE_BLOB` に設定します。 |
| `fileBasedAzureBlobDestination.path.templatingStrategy` | 文字列 | *必須*。この値を、 `path.value` フィールドに入力します。<ul><li>ユーザーが独自の [!DNL Azure Blob] [ストレージアカウント URI](https://learn.microsoft.com/en-us/azure/storage/blobs/storage-blobs-introduction) Experience PlatformUI で、この値をに設定します。 `PEBBLE_V1`. この場合、 `path.value` 値を [顧客データフィールド](../destination-configuration/customer-data-fields.md) ユーザーが入力した内容。 この使用例は、上記の例に示されています。</li><li>統合にハードコードされたパス（例： ）を使用する場合 `"path.value": "https://myaccount.blob.core.windows.net/"`の場合、この値を `NONE`. |
| `fileBasedAzureBlobDestination.path.value` | 文字列 | パス： [!DNL Azure Blob] ストレージ。 これは、テンプレート化されたフィールドにすることができます。このフィールドは、 [顧客データフィールド](../destination-configuration/customer-data-fields.md) ユーザーによって入力される（上の例で示される）か、ハードコードされた値 ( 例： `https://myaccount.blob.core.windows.net/`. |
| `fileBasedAzureBlobDestination.container.templatingStrategy` | 文字列 | *必須*。この値を、 `container.value` フィールドに入力します。<ul><li>ユーザーが独自の [!DNL Azure Blob] [コンテナ名](https://learn.microsoft.com/en-us/azure/storage/blobs/storage-blobs-introduction) Experience PlatformUI で、この値をに設定します。 `PEBBLE_V1`. この場合、 `container.value` 値を [顧客データフィールド](../destination-configuration/customer-data-fields.md) ユーザーが入力した内容。 この使用例は、上記の例に示されています。</li><li>統合にハードコードされたコンテナ名（例： ）を使用する場合 `"path.value: myContainer"`の場合、この値を `NONE`. |
| `fileBasedAzureBlobDestination.container.value` | 文字列 | この宛先に使用する Azure Blob ストレージコンテナの名前。 これは、テンプレート化されたフィールドにすることができます。このフィールドは、 [顧客データフィールド](../destination-configuration/customer-data-fields.md) ユーザーによって入力される（上の例で示される）か、ハードコードされた値 ( 例： `myContainer`. |

{style="table-layout:auto"}

## [!DNL Data Landing Zone] ([!DNL DLZ]) 宛先サーバー {#dlz-example}

この宛先サーバーを使用すると、Platform データを含むファイルをに書き出すことができます。 [[!DNL Data Landing Zone]](../../../catalog/cloud-storage/data-landing-zone.md) ストレージ。

次のサンプルは、 [!DNL Data Landing Zone] ([!DNL DLZ]) の宛先に貼り付けます。

```json
{
   "name":"Data Landing Zone destination server",
   "destinationServerType":"FILE_BASED_DLZ",
   "fileBasedDlzDestination":{
      "path":{
         "templatingStrategy":"PEBBLE_V1",
         "value":"{{customerData.path}}"
      },
      "useCase": "Your use case"
   }
}
```

| パラメーター | タイプ | 説明 |
|---|---|---|
| `name` | 文字列 | 宛先接続の名前。 |
| `destinationServerType` | 文字列 | この値は、宛先プラットフォームに応じて設定します。 [!DNL Data Landing Zone] の宛先の場合、これを `FILE_BASED_DLZ` に設定します。 |
| `fileBasedDlzDestination.path.templatingStrategy` | 文字列 | *必須*。この値を、 `path.value` フィールドに入力します。<ul><li>ユーザーが独自の [!DNL Data Landing Zone] アカウントをExperience PlatformUI で、この値をに設定します。 `PEBBLE_V1`. この場合、 `path.value` 値を [顧客データフィールド](../destination-configuration/customer-data-fields.md) ユーザーが入力した内容。 この使用例は、上記の例に示されています。</li><li>統合にハードコードされたパス（例： ）を使用する場合 `"path.value": "https://myaccount.blob.core.windows.net/"`の場合、この値を `NONE`. |
| `fileBasedDlzDestination.path.value` | 文字列 | 書き出したファイルをホストする保存先フォルダーのパス。 |

{style="table-layout:auto"}

## [!DNL Google Cloud Storage] 宛先サーバー {#gcs-example}

この宛先サーバーを使用すると、Platform データを含むファイルを [!DNL Google Cloud Storage] アカウント

次のサンプルは、 [!DNL Google Cloud Storage] 宛先。

```json
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
   }
}
```

| パラメーター | タイプ | 説明 |
|---|---|---|
| `name` | 文字列 | 宛先接続の名前。 |
| `destinationServerType` | 文字列 | この値は、宛先プラットフォームに応じて設定します。 [!DNL Google Cloud Storage] の宛先の場合、これを `FILE_BASED_GOOGLE_CLOUD` に設定します。 |
| `fileBasedGoogleCloudStorageDestination.bucket.templatingStrategy` | 文字列 | *必須*。この値を、 `bucket.value` フィールドに入力します。<ul><li>ユーザーが独自の [!DNL Google Cloud Storage] Experience PlatformUI でのバケット名。この値をに設定します。 `PEBBLE_V1`. この場合、 `bucket.value` 値を [顧客データフィールド](../destination-configuration/customer-data-fields.md) ユーザーが入力した内容。 この使用例は、上記の例に示されています。</li><li>統合にハードコードされたバケット名（例： ）を使用する場合 `"bucket.value": "my-bucket"`の場合、この値を `NONE`. |
| `fileBasedGoogleCloudStorageDestination.bucket.value` | 文字列 | この宛先が使用する [!DNL Google Cloud Storage] バケット名。これは、テンプレート化されたフィールドにすることができます。このフィールドは、 [顧客データフィールド](../destination-configuration/customer-data-fields.md) ユーザーによって入力される（上の例で示される）か、ハードコードされた値 ( 例： `"value": "my-bucket"`. |
| `fileBasedGoogleCloudStorageDestination.path.templatingStrategy` | 文字列 | *必須*。この値を、 `path.value` フィールドに入力します。<ul><li>ユーザーが独自の [!DNL Google Cloud Storage] Experience PlatformUI のバケットのパス。この値をに設定します。 `PEBBLE_V1`. この場合、 `path.value` 値を [顧客データフィールド](../destination-configuration/customer-data-fields.md) ユーザーが入力した内容。 この使用例は、上記の例に示されています。</li><li>統合にハードコードされたパス（例： ）を使用する場合 `"path.value": "/path/to/my-bucket"`の場合、この値を `NONE`.</li></ul> |
| `fileBasedGoogleCloudStorageDestination.path.value` | 文字列 | パス： [!DNL Google Cloud Storage] この宛先で使用するフォルダー。 これは、テンプレート化されたフィールドにすることができます。このフィールドは、 [顧客データフィールド](../destination-configuration/customer-data-fields.md) ユーザーによって入力される（上の例で示される）か、ハードコードされた値 ( 例： `"value": "/path/to/my-bucket"`. |

{style="table-layout:auto"}

## 次の手順 {#next-steps}

この記事を読んだ後、宛先サーバーの仕様と、その構成方法をより深く理解する必要があります。

その他の宛先サーバーコンポーネントの詳細については、次の記事を参照してください。

* [テンプレート仕様](templating-specs.md)
* [メッセージの形式](message-format.md)
* [ファイル形式設定](file-formatting.md)
