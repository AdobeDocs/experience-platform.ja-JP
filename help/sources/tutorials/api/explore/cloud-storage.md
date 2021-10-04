---
keywords: Experience Platform；ホーム；人気のあるトピック；クラウドストレージ；クラウドストレージ
solution: Experience Platform
title: フローサービス API を使用した Cloud ストレージシステムの調査
topic-legacy: overview
description: このチュートリアルでは、フローサービス API を使用して、サードパーティのクラウドストレージシステムを調べます。
exl-id: ba1a9bff-43a6-44fb-a4e7-e6a45b7eeebd
source-git-commit: b4291b4f13918a1f85d73e0320c67dd2b71913fc
workflow-type: tm+mt
source-wordcount: '812'
ht-degree: 27%

---

# [!DNL Flow Service] API を使用したクラウドストレージシステムの調査

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用してサードパーティのクラウドストレージシステムを調べます。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../home.md): [!DNL Experience Platform] を使用すると、様々なソースからデータを取り込みながら、サービスを使用して、受信データの構造化、ラベル付け、強化をおこなうことがで [!DNL Platform] きます。
* [サンドボックス](../../../../sandboxes/home.md)：[!DNL Experience Platform] は、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

以下の節では、[!DNL Flow Service] API を使用してクラウドストレージシステムに正常に接続するために知っておく必要がある追加情報を示します。

### 接続 ID の取得

[!DNL Platform] API を使用してサードパーティのクラウドストレージを調べるには、有効な接続 ID が必要です。 使用するストレージの接続がまだない場合は、次のチュートリアルを通じてストレージを作成できます。

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

クラウドストレージの接続 ID を使用して、GETリクエストを実行することで、ファイルとディレクトリを調べることができます。 クラウドストレージを調査するためのGETリクエストを実行する場合、次の表に示すクエリーパラメーターを含める必要があります。

| パラメーター | 説明 |
| --------- | ----------- |
| `objectType` | 調査するオブジェクトのタイプ。 この値は次のいずれかに設定します。 <ul><li>`folder`:特定のディレクトリの参照</li><li>`root`:ルートディレクトリを表示します。</li></ul> |
| `object` | このパラメーターは、特定のディレクトリを表示する場合にのみ必要です。 この値は、調査するディレクトリのパスを表します。 |

次の呼び出しを使用して、[!DNL Platform] に取り込むファイルのパスを探します。

**API 形式**

```http
GET /connections/{CONNECTION_ID}/explore?objectType=root
GET /connections/{CONNECTION_ID}/explore?objectType=folder&object={PATH}
```

| パラメーター | 説明 |
| --- | --- |
| `{CONNECTION_ID}` | クラウドストレージソースコネクタの接続 ID。 |
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

正常な応答は、クエリーされたディレクトリ内にあるファイルとフォルダーの配列を返します。 アップロードするファイルの `path` プロパティをメモしておきます。次の手順で指定して構造を確認する必要があります。

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

クラウドストレージソースからGETファイルの構造を調べるには、ファイルのパスとタイプを指定してデータリクエストを実行します。 また、CSV、TSV、圧縮 JSON、区切り形式のファイルなど、様々なファイルタイプを調べる場合は、クエリパラメーターの一部にファイルタイプを指定します。

**API 形式**

```http
GET /connections/{CONNECTION_ID}/explore?objectType=file&object={FILE_PATH}&fileType={FILE_TYPE}&{QUERY_PARAMS}&preview=true
GET /connections/{CONNECTION_ID}/explore?objectType=file&object={FILE_PATH}&preview=true&fileType=delimited&columnDelimiter=\t
GET /connections/{CONNECTION_ID}/explore?objectType=file&object={FILE_PATH}&preview=true&fileType=delimited&compressionType=gzip;
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{CONNECTION_ID}` | クラウドストレージソースコネクタの接続 ID。 |
| `{FILE_PATH}` | 検査するファイルのパス。 |
| `{FILE_TYPE}` | ファイルのタイプ。 次のファイルタイプがサポートされています。<ul><li>区切り </code>:区切り文字区切り値。 DSV ファイルはコンマで区切る必要があります。</li><li>JSON</code>:JavaScript オブジェクト表記。 JSON ファイルは XDM に準拠している必要がある</li><li>PARQUET</code>:Apache Parquet。 Parquet ファイルは XDM に準拠している必要があります。</li></ul> |
| `{QUERY_PARAMS}` | 結果のフィルタリングに使用できるオプションのクエリパラメーター。 詳しくは、[ クエリパラメータ ](#query) の節を参照してください。 |

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

[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) では、様々なファイルタイプをプレビューおよび検査するためのクエリパラメーターの使用がサポートされています。

| パラメーター | 説明 |
| --------- | ----------- |
| `columnDelimiter` | CSV または TSV ファイルを調べる列区切り文字として指定した 1 文字の値。 パラメーターを指定しない場合、値のデフォルトはコンマ `(,)` です。 |
| `compressionType` | 圧縮された区切りファイルまたは JSON ファイルをプレビューするための必須のクエリパラメーター。 サポートされる圧縮ファイルは次のとおりです。 <ul><li>`bzip2`</li><li>`gzip`</li><li>`deflate`</li><li>`zipDeflate`</li><li>`tarGzip`</li><li>`tar`</li></ul> |

## 次の手順

このチュートリアルでは、クラウドストレージシステムを調べ、[!DNL Platform] に取り込むファイルのパスを見つけ、その構造を確認しました。 次のチュートリアルでこの情報を使用して、クラウドストレージからデータを収集し、Platform](../collect/cloud-storage.md) に取り込むことができます。[
