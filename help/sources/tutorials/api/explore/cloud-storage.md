---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Flow Service APIを使用したクラウドストレージシステムの調査
topic: overview
translation-type: tm+mt
source-git-commit: 7cd9bec7336d0e1d9f3036cf862633f498002af8

---


# Flow Service APIを使用したクラウドストレージシステムの調査

フローサービスは、Adobe Experience Platform内の様々な異なるソースから顧客データを収集し、一元化するために使用します。 このサービスは、サポートされるすべてのソースを接続できるユーザーインターフェイスとRESTful APIを提供します。

このチュートリアルでは、Flow Service APIを使用して、サードパーティのクラウドストレージシステムを調べます。

## はじめに

このガイドでは、Adobe Experience Platformの次のコンポーネントに関する作業を理解している必要があります。

* [資料](../../../home.md):エクスペリエンスプラットフォームを使用すると、様々なソースからデータを取り込みながら、プラットフォームサービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../sandboxes/home.md):Experience Platformは、デジタルエクスペリエンスアプリケーションの開発と発展を支援するために、単一のプラットフォームインスタンスを別々の仮想環境に分割する仮想サンドボックスを提供します。

次の節では、フローサービスAPIを使用してクラウドストレージシステムに正常に接続するために知っておく必要がある追加情報を示します。

### ベース接続の取得

Platform APIを使用してサードパーティのクラウドストレージを調べるには、有効なベース接続IDが必要です。 操作するストレージのベース接続がまだない場合は、次のチュートリアルを使用して作成できます。

* [Amazon S3](../create/cloud-storage/s3.md)
* [Azure BLOB](../create/cloud-storage/blob.md)
* [Azure Data LakeストレージGen2](../create/cloud-storage/adls-gen2.md)
* [Google Cloud Store](../create/cloud-storage/google.md)
* [SFTP](../create/cloud-storage/sftp.md)

### サンプルAPI呼び出しの読み取り

このチュートリアルでは、リクエストをフォーマットする方法を示すAPI呼び出しの例を示します。 これには、パス、必須ヘッダー、適切にフォーマットされたリクエストペイロードが含まれます。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される表記について詳しくは、エクスペリエンスプラットフォームのトラブルシューテ [ィングガイドのAPI呼び出し例の読み方に関する節](../../../../landing/troubleshooting.md#how-do-i-format-an-api-request) （英語のみ）を参照してください。

### 必要なヘッダーの値の収集

プラットフォームAPIを呼び出すには、まず認証チュートリアルを完了する必要 [があります](../../../../tutorials/authentication.md)。 次に示すように、認証チュートリアルで、すべてのエクスペリエンスプラットフォームAPI呼び出しで必要な各ヘッダーの値を入力します。

* 認証：無記名 `{ACCESS_TOKEN}`
* x-api-key: `{API_KEY}`
* x-gw-ims-org-id: `{IMS_ORG}`

フローサービスに属するリソースを含む、エクスペリエンスプラットフォームのすべてのリソースは、特定の仮想サンドボックスに分離されます。 プラットフォームAPIへのすべてのリクエストには、操作が行われるサンドボックスの名前を指定するヘッダーが必要です。

* x-sandbox-name: `{SANDBOX_NAME}`

ペイロード(POST、PUT、PATCH)を含むすべての要求には、追加のメディアタイプヘッダーが必要です。

* コンテンツタイプ： `application/json`

## クラウドストレージ

クラウドストレージのベース接続を使用して、GETリクエストを実行することで、ファイルやディレクトリを調べることができます。 クラウドストレージを調査するGETリクエストを実行する場合、次の表に示すクエリパラメーターを含める必要があります。

| パラメーター | 説明 |
| --------- | ----------- |
| `objectType` | 調査するオブジェクトのタイプ。 この値を次のいずれかに設定します。 <ul><li>`folder`:特定のディレクトリの参照</li><li>`root`:ルートディレクトリを表示します。</li></ul> |
| `object` | このパラメーターは、特定のディレクトリを表示する場合にのみ必要です。 この値は、調査するディレクトリのパスを表します。 |

次の呼び出しを使用して、プラットフォームに取り込むファイルのパスを探します。

**API形式**

```http
GET /connections/{BASE_CONNECTION_ID}/explore?objectType=root
GET /connections/{BASE_CONNECTION_ID}/explore?objectType=folder&object={PATH}
```

| パラメーター | 説明 |
| --- | --- |
| `{BASE_CONNECTION_ID}` | クラウドストレージベース接続のID。 |
| `{PATH}` | ディレクトリのパス。 |

**リクエスト**

```shell
curl -X GET \
    'http://platform.adobe.io/data/foundation/flowservice/connections/{BASE_CONNECTION_ID}/explore?objectType=folder&object=/some/path/' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、クエリーされたディレクトリ内で見つかったファイルとフォルダの配列を返します。 アップロードするフ `path` ァイルのプロパティを控えておきます。次の手順でその構造を確認する必要があるので注意してください。

```json
[
    {
        "type": "File",
        "name": "data.csv",
        "path": "/some/path/data.csv"
    },
    {
        "type": "Folder",
        "name": "foobar",
        "path": "/some/path/foobar"
    }
]
```

## ファイルの構造を検査する

クラウドストレージのデータファイルの構造を調べるには、ファイルのパスをパラメーターとして指定しながら、GETリクエストを実行します。

**API形式**

```http
GET /connections/{BASE_CONNECTION_ID}/explore?objectType=file&object={FILE_PATH}&fileType={FILE_TYPE}
```

| パラメーター | 説明 |
| --- | --- |
| `{BASE_CONNECTION_ID}` | クラウドストレージベース接続のID。 |
| `{FILE_PATH}` | ファイルへのパス。 |
| `{FILE_TYPE}` | ファイルのタイプ。 次のファイルタイプがサポートされています。<ul><li>区切り</code>:区切り文字区切り値。 DSVファイルはコンマで区切る必要があります。</li><li>JSON</code>:JavaScriptオブジェクト表記法を参照してください。 JSONファイルはXDMに準拠している必要があります</li><li>パーケ</code>:Apache Parket。 パーケファイルはXDMに準拠している必要があります。</li></ul> |

**リクエスト**

```shell
curl -X GET \
    'http://platform.adobe.io/data/foundation/flowservice/connections/{BASE_CONNECTION_ID}/explore?objectType=file&object=/some/path/data.csv&fileType=DELIMITED' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、クエリーされたファイルの構造（テーブル名とデータ型を含む）を返します。

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

## 次の手順

このチュートリアルに従って、クラウドストレージシステムを調べ、プラットフォームに取り込むファイルのパスを見つけ、その構造を確認しました。 次のチュートリアルでこの情報を使用して、クラウド [ストレージからデータを収集し、Platformに取り込むことができます](../collect/cloud-storage.md)。