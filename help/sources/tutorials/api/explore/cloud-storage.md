---
keywords: Experience Platform；ホーム；人気の高いトピック；クラウドストレージ；クラウドストレージ
solution: Experience Platform
title: Flow Service APIを使用したCloudストレージシステムの調査
topic-legacy: overview
description: このチュートリアルでは、Flow Service APIを使用して、サードパーティのクラウドストレージシステムを調査します。
exl-id: ba1a9bff-43a6-44fb-a4e7-e6a45b7eeebd
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '820'
ht-degree: 18%

---

# [!DNL Flow Service] APIを使用したクラウドストレージシステムの調査

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/flow-service.yaml)を使用して、サードパーティのクラウドストレージシステムを調査します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../home.md): [!DNL Experience Platform] 様々なソースからデータを取り込むことができ、 [!DNL Platform] サービスを使用してデータの構造化、ラベル付け、および入力データの拡張を行うことができます。
* [サンドボックス](../../../../sandboxes/home.md): [!DNL Experience Platform] は、1つの [!DNL Platform] インスタンスを個別の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

[!DNL Flow Service] APIを使用してクラウドストレージシステムに正しく接続するために必要な追加情報については、以下の節で説明します。

### 接続IDの取得

[!DNL Platform] APIを使用してサードパーティのクラウドストレージを調査するには、有効な接続IDが必要です。 操作するストレージにまだ接続していない場合は、次のチュートリアルを使用して接続を作成できます。

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

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。この中には、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。サンプル API 呼び出しのドキュメントで使用されている規則については、[!DNL Experience Platform] トラブルシューテングガイドの[サンプル API 呼び出しの読み方](../../../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。

### 必須ヘッダーの値の収集

[!DNL Platform] API を呼び出すには、まず[認証チュートリアル](https://www.adobe.com/go/platform-api-authentication-en)を完了する必要があります。次に示すように、すべての [!DNL Experience Platform] API 呼び出しに必要な各ヘッダーの値は認証チュートリアルで説明されています。

* `Authorization: Bearer {ACCESS_TOKEN}`
* `x-api-key: {API_KEY}`
* `x-gw-ims-org-id: {IMS_ORG}`

[!DNL Experience Platform]内のすべてのリソース（[!DNL Flow Service]に属するリソースを含む）は、特定の仮想サンドボックスに分離されます。 [!DNL Platform] APIへのすべてのリクエストには、操作が行われるサンドボックスの名前を指定するヘッダーが必要です。

* `x-sandbox-name: {SANDBOX_NAME}`

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、メディアのタイプを指定する以下のような追加ヘッダーが必要です。

* `Content-Type: application/json`

## クラウドストレージの利用

クラウドストレージの接続IDを使用して、GETリクエストを実行することで、ファイルやディレクトリを調べることができます。 クラウドストレージを調査するためのGETリクエストを実行する場合は、次の表に示すクエリパラメーターを含める必要があります。

| パラメーター | 説明 |
| --------- | ----------- |
| `objectType` | 調査するオブジェクトのタイプ。 この値は次のいずれかに設定します。 <ul><li>`folder`:特定のディレクトリの参照</li><li>`root`:ルートディレクトリを調べます。</li></ul> |
| `object` | このパラメーターは、特定のディレクトリを表示する場合にのみ必要です。 この値は、調査するディレクトリのパスを表します。 |

次の呼び出しを使用して、[!DNL Platform]に取り込むファイルのパスを探します。

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

正常な応答は、照会されたディレクトリ内のファイルとフォルダの配列を返します。 次の手順でファイルの構造を調べる必要があるので、アップロードするファイルの`path`プロパティを控えておいてください。

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

## ファイルの構造をInspectにする

クラウドストレージーからGETファイルの構造を検査するには、ファイルのパスを指定し、クエリーパラメーターとして入力しながらデータリクエストを実行します。

ファイルのパスと種類を指定しながらGETリクエストを実行すると、クラウドストレージソースからデータファイルの構造を調べることができます。 また、CSV、TSV、圧縮JSON、区切り形式のファイルなど、様々なファイルタイプを調べる場合は、それらのファイルタイプをクエリーパラメーターの一部として指定します。

**API 形式**

```http
GET /connections/{CONNECTION_ID}/explore?objectType=file&object={FILE_PATH}&fileType={FILE_TYPE}&{QUERY_PARAMS}&preview=true
GET /connections/{CONNECTION_ID}/explore?objectType=file&object={FILE_PATH}&preview=true&fileType=delimited&columnDelimiter=\t
GET /connections/{CONNECTION_ID}/explore?objectType=file&object={FILE_PATH}&preview=true&fileType=delimited&compressionType=gzip;
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{CONNECTION_ID}` | クラウドストレージソースコネクタの接続ID。 |
| `{FILE_PATH}` | 検査するファイルへのパスです。 |
| `{FILE_TYPE}` | ファイルの種類です。 次のファイルタイプがサポートされています。<ul><li>DELIMITED</code>:区切り文字区切り値。 DSVファイルはコンマで区切る必要があります。</li><li>JSON</code>:JavaScriptオブジェクト表記を参照してください。 JSONファイルはXDMに準拠している必要があります</li><li>PARKET</code>:Apacheパーケー。 パーケファイルはXDMに準拠している必要があります。</li></ul> |
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

成功した応答は、クエリー対象のファイルの構造（テーブル名とデータ型を含む）を返します。

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

[[!DNL Flow Service] API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/flow-service.yaml)は、異なるファイルタイプのプレビューや検査を行うためのクエリパラメーターの使用をサポートしています。

| パラメーター | 説明 |
| --------- | ----------- |
| `columnDelimiter` | CSVファイルまたはTSVファイルを検査するための列区切り文字として指定した1文字の値。 パラメーターを指定しない場合、値のデフォルトはコンマ`(,)`です。 |
| `compressionType` | 圧縮された区切りファイルまたはJSONファイルをプレビューするために必要なクエリパラメーター。 サポートされる圧縮ファイルは次のとおりです。 <ul><li>`bzip2`</li><li>`gzip`</li><li>`deflate`</li><li>`zipDeflate`</li><li>`tarGzip`</li><li>`tar`</li></ul> |

## 次の手順

このチュートリアルに従って、クラウドストレージシステムを調べ、[!DNL Platform]に取り込むファイルのパスを見つけ、その構造を確認しました。 この情報は、次のチュートリアルで使用して、[クラウドストレージからデータを収集し、Platform](../collect/cloud-storage.md)に取り込むことができます。
