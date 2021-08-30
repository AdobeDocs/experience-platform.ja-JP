---
keywords: Experience Platform；ホーム；人気のあるトピック；クラウドストレージ；クラウドストレージ
solution: Experience Platform
title: フローサービスAPIを使用したCloudストレージシステムの調査
topic-legacy: overview
description: このチュートリアルでは、フローサービスAPIを使用して、サードパーティのクラウドストレージシステムを調べます。
exl-id: ba1a9bff-43a6-44fb-a4e7-e6a45b7eeebd
source-git-commit: b4291b4f13918a1f85d73e0320c67dd2b71913fc
workflow-type: tm+mt
source-wordcount: '812'
ht-degree: 25%

---

# [!DNL Flow Service] APIを使用したクラウドストレージシステムの調査

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/)を使用して、サードパーティのクラウドストレージシステムを調べます。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../home.md): [!DNL Experience Platform] を使用すると、様々なソースからデータを取り込みながら、サービスを使用して受信データの構造化、ラベル付け、拡張をおこなうことがで [!DNL Platform] きます。
* [サンドボックス](../../../../sandboxes/home.md)：[!DNL Experience Platform] は、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

以下の節では、[!DNL Flow Service] APIを使用してクラウドストレージシステムに正常に接続するために必要な追加情報を示します。

### 接続IDの取得

[!DNL Platform] APIを使用してサードパーティのクラウドストレージを調査するには、有効な接続IDが必要です。 使用するストレージの接続がまだない場合は、次のチュートリアルを通じてストレージを作成できます。

* [[!DNL Amazon S3]](../create/cloud-storage/s3.md)
* [[!DNL Azure Blob]](../create/cloud-storage/blob.md)
* [[!DNL Azure Data Lake Storage Gen2]](../create/cloud-storage/adls-gen2.md)
* [[!DNL Azure File Storage]](../create/cloud-storage/azure-file-storage.md)
* [[!DNL FTP]](../create/cloud-storage/ftp.md)
* [[!DNL Google Cloud Storage]](../create/cloud-storage/google.md)
* [HDFS](../create/cloud-storage/hdfs.md)
* [[!DNL Oracle Object Storage]](../create/cloud-storage/oracle-object-storage.md)
* [[!DNL SFTP]](../create/cloud-storage/sftp.md)

### API 呼び出し例の読み取り

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。この中には、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください[!DNL Experience Platform]。

### 必須ヘッダーの値の収集

[!DNL Platform] API を呼び出すには、まず[認証チュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja#platform-apis)を完了する必要があります。次に示すように、すべての [!DNL Experience Platform] API 呼び出しに必要な各ヘッダーの値は認証チュートリアルで説明されています。

* `Authorization: Bearer {ACCESS_TOKEN}`
* `x-api-key: {API_KEY}`
* `x-gw-ims-org-id: {IMS_ORG}`

[!DNL Flow Service]に属するリソースを含む、[!DNL Experience Platform] のすべてのリソースは、特定の仮想サンドボックスに分離されます。[!DNL Platform] API へのすべてのリクエストには、操作がおこなわれるサンドボックスの名前を指定するヘッダーが必要です。

* `x-sandbox-name: {SANDBOX_NAME}`

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、メディアのタイプを指定する以下のような追加ヘッダーが必要です。

* `Content-Type: application/json`

## クラウドストレージの参照

クラウドストレージの接続IDを使用して、GETリクエストを実行することでファイルとディレクトリを調べることができます。 クラウドストレージを調査するためのGETリクエストを実行する場合、次の表に示すクエリパラメーターを含める必要があります。

| パラメーター | 説明 |
| --------- | ----------- |
| `objectType` | 調査するオブジェクトのタイプ。 この値は次のいずれかに設定します。 <ul><li>`folder`:特定のディレクトリの参照</li><li>`root`:ルートディレクトリを表示します。</li></ul> |
| `object` | このパラメーターは、特定のディレクトリを表示する場合にのみ必要です。 値は、調査するディレクトリのパスを表します。 |

次の呼び出しを使用して、[!DNL Platform]に取り込むファイルのパスを見つけます。

**API 形式**

```http
GET /connections/{CONNECTION_ID}/explore?objectType=root
GET /connections/{CONNECTION_ID}/explore?objectType=folder&object={PATH}
```

| パラメーター | 説明 |
| --- | --- |
| `{CONNECTION_ID}` | クラウドストレージソースコネクタの接続ID。 |
| `{PATH}` | ディレクトリのパス。 |

**リクエスト**

```shell
curl -X GET \
    'http://platform.adobe.io/data/foundation/flowservice/connections/{CONNECTION_ID}/explore?objectType=folder&object=/some/path/' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、クエリーされたディレクトリ内にあるファイルとフォルダーの配列を返します。 アップロードするファイルの`path`プロパティに注意してください。このプロパティは、次の手順で指定して構造を検査する必要があります。

```json
[
    {
        "type": "file",
        "name": "account.csv",
        "path": "/test-connectors/testFolder-fileIngestion/account.csv",
        "canPreview": true,
        "canFetchSchema": true
    },
    {
        "type": "file",
        "name": "profileData.json",
        "path": "/test-connectors/testFolder-fileIngestion/profileData.json",
        "canPreview": true,
        "canFetchSchema": true
    },
    {
        "type": "file",
        "name": "sampleprofile--3.parquet",
        "path": "/test-connectors/testFolder-fileIngestion/sampleprofile--3.parquet",
        "canPreview": true,
        "canFetchSchema": true
    }
]
```

## Inspectファイルの構造

クラウドストレージからGETファイルの構造を調べるには、ファイルのパスと型をクエリパラメーターとして指定しながら、データリクエストを実行します。

ファイルのパスとタイプを指定しながらGETリクエストを実行することで、クラウドストレージソースからデータファイルの構造を調べることができます。 また、CSV、TSV、圧縮JSON、区切り文字付きファイルなど、様々なファイルタイプを調べる場合は、クエリパラメーターの一部としてファイルタイプを指定します。

**API 形式**

```http
GET /connections/{CONNECTION_ID}/explore?objectType=file&object={FILE_PATH}&fileType={FILE_TYPE}&{QUERY_PARAMS}&preview=true
GET /connections/{CONNECTION_ID}/explore?objectType=file&object={FILE_PATH}&preview=true&fileType=delimited&columnDelimiter=\t
GET /connections/{CONNECTION_ID}/explore?objectType=file&object={FILE_PATH}&preview=true&fileType=delimited&compressionType=gzip;
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{CONNECTION_ID}` | クラウドストレージソースコネクタの接続ID。 |
| `{FILE_PATH}` | 検査するファイルへのパス。 |
| `{FILE_TYPE}` | ファイルのタイプ。 次のファイルタイプがサポートされています。<ul><li>区切り</code>:区切り文字区切り値。 DSVファイルはコンマで区切る必要があります。</li><li>JSON</code>:JavaScriptオブジェクト表記。 JSONファイルはXDMに準拠している必要がある</li><li>PARQUET</code>:Apache Parquet。 ParquetファイルはXDMに準拠している必要があります。</li></ul> |
| `{QUERY_PARAMS}` | 結果のフィルタリングに使用できるオプションのクエリパラメーター。 詳しくは、[クエリパラメーター](#query)の節を参照してください。 |

**リクエスト**

```shell
curl -X GET \
    'http://platform.adobe.io/data/foundation/flowservice/connections/{CONNECTION_ID}/explore?objectType=file&object=/aep-bootcamp/Adobe%20Pets%20Customer%2020190801%20EXP.json&fileType=json&preview=true' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、テーブル名とデータ型を含む、クエリされたファイルの構造を返します。

```json
[
    {
        "name": "Id",
        "type": "String"
    },
    {
        "name": "FirstName",
        "type": "String"
    },
    {
        "name": "LastName",
        "type": "String"
    },
    {
        "name": "Email",
        "type": "String"
    },
    {
        "name": "Phone",
        "type": "String"
    }
]
```

## クエリパラメーターの使用 {#query}

[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/)は、様々なファイルタイプをプレビューおよび検査するためのクエリパラメーターの使用をサポートしています。

| パラメーター | 説明 |
| --------- | ----------- |
| `columnDelimiter` | CSVファイルまたはTSVファイルを調べる列区切りとして指定した1文字の値。 パラメーターを指定しない場合、値はデフォルトでコンマ`(,)`になります。 |
| `compressionType` | 圧縮された区切りファイルまたはJSONファイルをプレビューするための必須のクエリパラメーター。 サポートされている圧縮ファイルは次のとおりです。 <ul><li>`bzip2`</li><li>`gzip`</li><li>`deflate`</li><li>`zipDeflate`</li><li>`tarGzip`</li><li>`tar`</li></ul> |

## 次の手順

このチュートリアルでは、クラウドストレージシステムを調べ、[!DNL Platform]に取り込むファイルのパスを見つけ、その構造を確認しました。 次のチュートリアルでこの情報を使用して、クラウドストレージからデータを[収集し、Platform](../collect/cloud-storage.md)に取り込むことができます。
