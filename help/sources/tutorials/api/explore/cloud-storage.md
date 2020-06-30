---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Flow Service APIを使用したクラウドストレージシステムの調査
topic: overview
translation-type: tm+mt
source-git-commit: fc5cdaa661c47e14ed5412868f3a54fd7bd2b451
workflow-type: tm+mt
source-wordcount: '682'
ht-degree: 2%

---


# APIを使用したクラウドストレージシステムの調査 [!DNL Flow Service]

[!DNL Flow Service] は、Adobe Experience Platform内のさまざまな異なるソースから顧客データを収集し、一元化するために使用します。 このサービスは、ユーザーインターフェイスとRESTful APIを提供し、サポートされるすべてのソースを接続できます。

このチュートリアルでは、 [!DNL Flow Service] APIを使用してサードパーティのクラウドストレージシステムを調査します。

## はじめに

このガイドでは、次のAdobe Experience Platformのコンポーネントについて、十分に理解している必要があります。

* [ソース](../../../home.md): [!DNL Experience Platform] 様々なソースからデータを取り込むことができ、 [!DNL Platform] サービスを使用してデータの構造化、ラベル付け、および入力データの拡張を行うことができます。
* [サンドボックス](../../../../sandboxes/home.md): [!DNL Experience Platform] は、1つの [!DNL Platform] インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

次の節では、 [!DNL Flow Service] APIを使用してクラウドストレージシステムに正常に接続するために知っておく必要がある追加情報について説明します。

### ベース接続の取得

APIを使用してサードパーティのクラウドストレージを調査するに [!DNL Platform] は、有効なベース接続IDが必要です。 操作するストレージの基本接続がまだない場合は、次のチュートリアルを使用して作成できます。

* [Amazon S3](../create/cloud-storage/s3.md)
* [Azure BLOB](../create/cloud-storage/blob.md)
* [Azure Data LakeストレージGen2](../create/cloud-storage/adls-gen2.md)
* [Google Cloud Store](../create/cloud-storage/google.md)
* [SFTP](../create/cloud-storage/sftp.md)

### サンプルAPI呼び出しの読み取り

このチュートリアルでは、リクエストをフォーマットする方法を示すAPI呼び出しの例を提供します。 例えば、パス、必須のヘッダー、適切にフォーマットされた要求ペイロードなどです。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される規則について詳しくは、トラブルシューティングガイドのAPI呼び出し例 [を読む方法に関する節](../../../../landing/troubleshooting.md#how-do-i-format-an-api-request) を参照して [!DNL Experience Platform] ください。

### 必要なヘッダーの値の収集

APIを呼び出すには、まず [!DNL Platform] 認証チュートリアルを完了する必要があり [ます](../../../../tutorials/authentication.md)。 次に示すように、認証チュートリアルで、すべての [!DNL Experience Platform] API呼び出しに必要な各ヘッダーの値を指定する

* 認証： 無記名 `{ACCESS_TOKEN}`
* x-api-key: `{API_KEY}`
* x-gw-ims-org-id: `{IMS_ORG}`

に属するリソース [!DNL Experience Platform]を含む、のすべてのリソースは、特定の仮想サンドボックスに分離され [!DNL Flow Service]ます。 APIへのすべてのリクエストには、操作が実行されるサンドボックスの名前を指定するヘッダーが必要で [!DNL Platform] す。

* x-sandbox-name: `{SANDBOX_NAME}`

ペイロード(POST、PUT、PATCH)を含むすべての要求には、追加のメディアタイプヘッダーが必要です。

* Content-Type: `application/json`

## クラウドストレージの利用

クラウドストレージの基本接続を使用して、GETリクエストを実行することで、ファイルやディレクトリを調べることができます。 クラウドストレージを調査するGETリクエストを実行する場合、次の表に示すクエリパラメーターを含める必要があります。

| パラメーター | 説明 |
| --------- | ----------- |
| `objectType` | 調査するオブジェクトのタイプ。 この値は次のいずれかに設定します。 <ul><li>`folder`: 特定のディレクトリの参照</li><li>`root`: ルートディレクトリを調べます。</li></ul> |
| `object` | このパラメーターは、特定のディレクトリを表示する場合にのみ必要です。 この値は、調査するディレクトリのパスを表します。 |

次の呼び出しを使用して、に取り込むファイルのパスを探しま [!DNL Platform]す。

**API形式**

```http
GET /connections/{BASE_CONNECTION_ID}/explore?objectType=root
GET /connections/{BASE_CONNECTION_ID}/explore?objectType=folder&object={PATH}
```

| パラメーター | 説明 |
| --- | --- |
| `{BASE_CONNECTION_ID}` | クラウドストレージベースの接続のID。 |
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

正常な応答は、照会されたディレクトリ内のファイルとフォルダの配列を返します。 アップロードするファイルの `path` プロパティを控えておきます。次の手順でその構造を調べる必要があります。

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

クラウドストレージーからデータファイルの構造を検査するには、ファイルのパスをクエリーパラメーターとして指定しながら、GETリクエストを実行します。

**API形式**

```http
GET /connections/{BASE_CONNECTION_ID}/explore?objectType=file&object={FILE_PATH}&fileType={FILE_TYPE}
```

| パラメーター | 説明 |
| --- | --- |
| `{BASE_CONNECTION_ID}` | クラウドストレージベースの接続のID。 |
| `{FILE_PATH}` | ファイルへのパス。 |
| `{FILE_TYPE}` | ファイルの種類です。 次のファイルタイプがサポートされています。<ul><li>区切り文字</code>: 区切り文字区切り値。 DSVファイルはコンマで区切る必要があります。</li><li>JSON</code>: JavaScriptオブジェクト表記を参照してください。 JSONファイルはXDMに準拠している必要があります</li><li>パーケ</code>: Apacheパーケー。 パーケファイルはXDMに準拠している必要があります。</li></ul> |

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

## 次の手順

このチュートリアルに従って、クラウドストレージシステムを調べ、に取り込むファイルのパスを見つけ、その構造を確認し [!DNL Platform]ました。 次のチュートリアルでこの情報を使用して、クラウドストレージからデータを [収集し、Platformに取り込むことができます](../collect/cloud-storage.md)。