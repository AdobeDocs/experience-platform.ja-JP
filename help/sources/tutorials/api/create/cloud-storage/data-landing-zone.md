---
keywords: Experience Platform;ホーム;人気のトピック;
solution: Experience Platform
title: フローサービス API を使用したデータランディングゾーンとAdobe Experience Platformの接続
topic-legacy: overview
type: Tutorial
description: フローサービス API を使用してAdobe Experience Platformをデータランディングゾーンに接続する方法を説明します。
exl-id: bdb60ed3-7c63-4a69-975a-c6f1508f319e
source-git-commit: ca7197036283ee15dbf60c113d361a5ea34d65c1
workflow-type: tm+mt
source-wordcount: '951'
ht-degree: 9%

---

# フローサービス API を使用して [!DNL Data Landing Zone] をAdobe Experience Platformに接続

[!DNL Data Landing Zone] は、Adobe Experience Platformでプロビジョニングされた一時ファイルストレージのクラウドベースのデータストレージ機能です。[!DNL Data Landing Zone] は、Platform へのデータの入出力にのみ使用されます。データは 7 日後に [!DNL Data Landing Zone] から自動的に削除されます。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して [!DNL Data Landing Zone] ソース接続を作成する手順を説明します。 このチュートリアルでは、[!DNL Data Landing Zone] を取得する方法と、資格情報を表示および更新する方法についても説明します。

## はじめに

このガイドでは、 Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [ソース](../../../../home.md):Experience Platformを使用すると、様々なソースからデータを取り込みながら、Platform サービスを使用して、受信データの構造化、ラベル付け、拡張をおこなうことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：Experience Platform は、単一の Platform インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

以下の節では、[!DNL Flow Service] API を使用して [!DNL Data Landing Zone] ソース接続を正しく作成するために知っておく必要がある追加情報を示します。

また、このチュートリアルでは、[Platform API の使用の手引き ](../../../../../landing/api-guide.md) を読み、Platform API に対する認証方法とドキュメントに記載されている呼び出し例を解釈する方法を学ぶ必要があります。

## 使用可能なランディングゾーンの取得

API を使用して [!DNL Data Landing Zone] にアクセスする最初の手順は、`type=user_drop_zone` をリクエストヘッダーの一部として指定しながら、[!DNL Connectors] API の `/landingzone` エンドポイントにGETリクエストを送信することです。

**API 形式**

```http
GET /connectors/landingzone?type=user_drop_zone
```

| ヘッダー | 説明 |
| --- | --- |
| `user_drop_zone` | `user_drop_zone` タイプを使用すると、API はランディングゾーンコンテナと、使用可能な他のタイプのコンテナを区別できます。 |

**リクエスト**

次のリクエストは、既存のランディングゾーンを取得します。

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/connectors/landingzone?type=user_drop_zone' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' 
```

**応答**

次の応答は、対応する `containerName` と `containerTTL` を含むランディングゾーンの情報を返します。

```json
{
    "containerName": "dlz-user-container",
    "containerTTL": "7"
}
```

| プロパティ | 説明 |
| --- | --- |
| `containerName` | 取得したランディングゾーンの名前。 |
| `containerTTL` | ランディングゾーン内のデータに適用される有効期間設定。 特定のランディングゾーン内のすべては、7 日後に削除されます。 |

## [!DNL Data Landing Zone] 資格情報を取得

[!DNL Data Landing Zone] の資格情報を取得するには、[!DNL Connectors] API の `/credentials` エンドポイントにGETリクエストを送信します。

**API 形式**

```http
GET /connectors/landingzone/credentials?type=user_drop_zone
```

**リクエスト**

次のリクエストの例では、既存のランディングゾーンの資格情報を取得します。

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/connectors/landingzone/credentials?type=user_drop_zone' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
```

**応答**

次の応答は、現在の `SASToken` と `SASUri`、およびランディングゾーンコンテナに対応する `storageAccountName` を含む、ランディングゾーンの資格情報情報を返します。

```json
{
    "containerName": "dlz-user-container",
    "SASToken": "sv=2020-04-08&si=dlz-ed86a61d-201f-4b50-b10f-a1bf173066fd&sr=c&sp=racwdlm&sig=4yTba8voU3L0wlcLAv9mZLdZ7NlMahbfYYPTMkQ6ZGU%3D",
    "storageAccountName": "dlblobstore99hh25i3dflek",
    "SASUri": "https://dlblobstore99hh25i3dflek.blob.core.windows.net/dlz-user-container?sv=2020-04-08&si=dlz-ed86a61d-201f-4b50-b10f-a1bf173066fd&sr=c&sp=racwdlm&sig=4yTba8voU3L0wlcLAv9mZLdZ7NlMahbfYYPTMkQ6ZGU%3D"
}
```

| プロパティ | 説明 |
| --- | --- |
| `containerName` | ランディングゾーンの名前。 |
| `SASToken` | ランディングゾーンの共有アクセス署名トークン。 この文字列には、リクエストを承認するために必要なすべての情報が含まれています。 |
| `SASUri` | ランディングゾーンの共有アクセス署名 URI。 この文字列は、認証先のランディングゾーンと、対応する SAS トークンの URI の組み合わせです。 |


## [!DNL Data Landing Zone] 資格情報の更新

`SASToken` を更新するには、[!DNL Connectors] API の `/credentials` エンドポイントにPOSTリクエストを送信します。

**API 形式**

```http
POST /connectors/landingzone/credentials?type=user_drop_zone&action=refresh
```

| ヘッダー | 説明 |
| --- | --- |
| `user_drop_zone` | `user_drop_zone` タイプを使用すると、API はランディングゾーンコンテナと、使用可能な他のタイプのコンテナを区別できます。 |
| `refresh` | `refresh` アクションを使用すると、ランディングゾーンの資格情報をリセットし、新しい `SASToken` を自動的に生成できます。 |

**リクエスト**

次のリクエストは、ランディングゾーンの資格情報を更新します。

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/connectors/landingzone/credentials?type=user_drop_zone&action=refresh' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
```

**応答**

次の応答は、`SASToken` と `SASUri` の更新された値を返します。

```json
{
    "containerName": "dlz-user-container",
    "SASToken": "sv=2020-04-08&si=dlz-9c4d03b8-a6ff-41be-9dcf-20123e717e99&sr=c&sp=racwdlm&sig=JbRMoDmFHQU4OWOpgrKdbZ1d%2BkvslO35%2FXTqBO%2FgbRA%3D",
    "storageAccountName": "dlblobstore99hh25i3dflek",
    "SASUri": "https://dlblobstore99hh25i3dflek.blob.core.windows.net/dlz-user-container?sv=2020-04-08&si=dlz-9c4d03b8-a6ff-41be-9dcf-20123e717e99&sr=c&sp=racwdlm&sig=JbRMoDmFHQU4OWOpgrKdbZ1d%2BkvslO35%2FXTqBO%2FgbRA%3D"
}
```

## ランディングゾーンのファイル構造とコンテンツの参照

[!DNL Flow Service] API の `connectionSpecs` エンドポイントにGETリクエストを送信することで、ランディングゾーンのファイル構造とコンテンツを調べることができます。

**API 形式**

```http
GET /connectionSpecs/{CONNECTION_SPEC_ID}/explore?objectType=root
```

| パラメーター | 説明 |
| --- | --- |
| `{CONNECTION_SPEC_ID}` | [!DNL Data Landing Zone] に対応する接続仕様 ID。 この固定 ID は次のとおりです。`26f526f2-58f4-4712-961d-e41bf1ccc0e8`. |

**リクエスト**

```shell
curl -X GET \
    'http://platform.adobe.io/data/foundation/flowservice/connectionSpecs/26f526f2-58f4-4712-961d-e41bf1ccc0e8/explore?objectType=root' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、クエリーされたディレクトリ内にあるファイルとフォルダーの配列を返します。 アップロードするファイルの `path` プロパティをメモしておきます。次の手順でファイルを指定して構造を検査する必要があります。

```json
[
    {
        "type": "file",
        "name": "account.csv",
        "path": "dlz-user-container/account.csv",
        "canPreview": true,
        "canFetchSchema": true
    },
    {
        "type": "file",
        "name": "data8.csv",
        "path": "dlz-user-container/data8.csv",
        "canPreview": true,
        "canFetchSchema": true
    },
    {
        "type": "folder",
        "name": "userdata1",
        "path": "dlz-user-container/userdata1/",
        "canPreview": false,
        "canFetchSchema": false
    }
]
```

## ランディングゾーンのファイル構造とコンテンツのプレビュー

ランディングゾーン内のファイルの構造を調べるには、ファイルのパスと型をクエリパラメーターとして指定しながら、GETリクエストを実行します。

**API 形式**

```http
GET /connectionSpecs/{CONNECTION_SPEC_ID}/explore?objectType=file&object={OBJECT}&fileType={FILE_TYPE}&preview={PREVIEW}
```

| パラメーター | 説明 | 例 |
| --- | --- | --- |
| `{CONNECTION_SPEC_ID}` | [!DNL Data Landing Zone] に対応する接続仕様 ID。 この固定 ID は次のとおりです。`26f526f2-58f4-4712-961d-e41bf1ccc0e8`. |
| `{OBJECT_TYPE}` | アクセスするオブジェクトのタイプ。 | `file` |
| `{OBJECT}` | アクセスするオブジェクトのパスと名前。 | `dlz-user-container/data8.csv` |
| `{FILE_TYPE}` | ファイルのタイプ。 | <ul><li>`delimited`</li><li>`json`</li><li>`parquet`</li></ul> |
| `{PREVIEW}` | ファイルのプレビューがサポートされるかどうかを定義する boolean 値です。 | </ul><li>`true`</li><li>`false`</li></ul> |

**リクエスト**

```shell
curl -X GET \
    'http://platform.adobe.io/data/foundation/flowservice/connectionSpecs/26f526f2-58f4-4712-961d-e41bf1ccc0e8/explore?objectType=file&object=dlz-user-container/data8.csv&fileType=delimited&preview=true' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、テーブル名とデータ型を含む、クエリされたファイルの構造を返します。

```json
{
    "format": "flat",
    "schema": {
        "columns": [
            {
                "name": "Id",
                "type": "string",
                "xdm": {
                    "type": "string"
                }
            },
            {
                "name": "FirstName",
                "type": "string",
                "xdm": {
                    "type": "string"
                }
            },
            {
                "name": "LastName",
                "type": "string",
                "xdm": {
                    "type": "string"
                }
            },
            {
                "name": "Email",
                "type": "string",
                "xdm": {
                    "type": "string"
                }
            },
            {
                "name": "Phone",
                "type": "string",
                "xdm": {
                    "type": "string"
                }
            }
        ]
    },
    "data": [
        {
            "Email": "rsmith@abc.com",
            "FirstName": "Richard",
            "Phone": "111111111",
            "Id": "12345",
            "LastName": "Smith"
        },
        {
            "Email": "morgan@bac.com",
            "FirstName": "Morgan",
            "Phone": "22222222222",
            "Id": "67890",
            "LastName": "Hart"
        }
    ]
}
```

## ソース接続の作成

ソース接続は、データの取り込み元の外部ソースへの接続を作成し、管理します。 ソース接続は、データソース、データ形式、データフローの作成に必要なソース接続 ID などの情報で構成されます。 ソース接続インスタンスは、テナントと IMS 組織に固有です。

ソース接続を作成するには、[!DNL Flow Service] API の `/sourceConnections` エンドポイントにPOSTリクエストを実行します。


**API 形式**

```http
POST /sourceConnections
```

**リクエスト**

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/sourceConnections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "Data Landing Zone source connection",
        "data": {
            "format": "delimited"
        },
        "params": {
            "path": "dlz-user-container/data8.csv"
        },
        "connectionSpec": {
            "id": "26f526f2-58f4-4712-961d-e41bf1ccc0e8",
            "version": "1.0"
        }
    }'
```

| プロパティ | 説明 |
| --- | --- |
| `name` | [!DNL Data Landing Zone] ソース接続の名前。 |
| `data.format` | Platform に取り込むデータの形式。 |
| `params.path` | Platform に取り込むファイルのパス。 |
| `connectionSpec.id` | [!DNL Data Landing Zone] に対応する接続仕様 ID。 この固定 ID は次のとおりです。`26f526f2-58f4-4712-961d-e41bf1ccc0e8`. |

**応答**

正常な応答は、新しく作成されたソース接続の一意の識別子 (`id`) を返します。 この ID は、次のチュートリアルでデータフローを作成する際に必要になります。

```json
{
    "id": "f5b46949-8c8d-4613-80cc-52c9c039e8b9",
    "etag": "\"1400d460-0000-0200-0000-613be3520000\""
}
```

## 次の手順

このチュートリアルでは、[!DNL Data Landing Zone] 資格情報を取得し、Platform に取り込むファイルを見つけるためのファイル構造を調べ、データを Platform に取り込むためのソース接続を作成しました。 次のチュートリアルに進みます。ここでは、 [!DNL Flow Service] API](../../collect/cloud-storage.md) を使用してクラウドストレージデータを Platform に取り込むためのデータフローを [ 作成する方法を学習します。
