---
keywords: Experience Platform;ホーム;人気のトピック;
solution: Experience Platform
title: フローサービス API を使用したデータランディングゾーンとAdobe Experience Platformの接続
topic-legacy: overview
type: Tutorial
description: フローサービス API を使用してAdobe Experience Platformをデータランディングゾーンに接続する方法を説明します。
exl-id: bdb60ed3-7c63-4a69-975a-c6f1508f319e
source-git-commit: 47a94b00e141b24203b01dc93834aee13aa6113c
workflow-type: tm+mt
source-wordcount: '1223'
ht-degree: 20%

---

# 接続 [!DNL Data Landing Zone] フローサービス API を使用してAdobe Experience Platformに送信

[!DNL Data Landing Zone] は、Adobe Experience Platformでプロビジョニングされた一時ファイルストレージのクラウドベースのデータストレージ機能です。 データは [!DNL Data Landing Zone] 7 日後

このチュートリアルでは、 [!DNL Data Landing Zone] を使用したソース接続 [[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/). このチュートリアルでは、 [!DNL Data Landing Zone]を更新し、資格情報を表示および更新します。

## はじめに

このガイドは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ソース](../../../../home.md)：Experience Platform を使用すると、データを様々なソースから取得しながら、Platform サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：Experience Platform には、単一の Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

以下の節では、 [!DNL Data Landing Zone] を使用したソース接続 [!DNL Flow Service] API

また、このチュートリアルでは、 [Platform API の概要](../../../../../landing/api-guide.md) を参照して、Platform API に対する認証方法と、ドキュメントに記載されている呼び出し例の解釈方法について確認してください。

## 使用可能なランディングゾーンの取得

API を使用してにアクセスする際の最初の手順 [!DNL Data Landing Zone] は、 `/landingzone` エンドポイント [!DNL Connectors] を提供する際の API `type=user_drop_zone` をリクエストヘッダーの一部として追加する必要があります。

**API 形式**

```http
GET /connectors/landingzone?type=user_drop_zone
```

| ヘッダー | 説明 |
| --- | --- |
| `user_drop_zone` | この `user_drop_zone` タイプを使用すると、API はランディングゾーンコンテナを、使用可能な他のタイプのコンテナと区別できます。 |

**リクエスト**

次のリクエストは、既存のランディングゾーンを取得します。

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/connectors/landingzone?type=user_drop_zone' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' 
```

**応答**

次の応答は、ランディングゾーンに関する情報 ( 対応する `containerName` および `containerTTL`.

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

## 取得 [!DNL Data Landing Zone] 資格情報

の資格情報を取得するには [!DNL Data Landing Zone]、 `/credentials` エンドポイント [!DNL Connectors] API

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
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
```

**応答**

次の応答は、現在の `SASToken` および `SASUri`、および `storageAccountName` ランディングゾーンコンテナに対応する

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
| `SASToken` | ランディングゾーンの共有アクセス署名トークン。 この文字列には、リクエストの承認に必要な情報がすべて含まれています。 |
| `SASUri` | ランディングゾーンの共有アクセス署名 URI です。 この文字列は、認証先のランディングゾーンへの URI と、対応する SAS トークンの組み合わせです。 |


## 更新 [!DNL Data Landing Zone] 資格情報

次の項目を更新： `SASToken` に対してPOSTリクエストを行う `/credentials` エンドポイント [!DNL Connectors] API

**API 形式**

```http
POST /connectors/landingzone/credentials?type=user_drop_zone&action=refresh
```

| ヘッダー | 説明 |
| --- | --- |
| `user_drop_zone` | この `user_drop_zone` タイプを使用すると、API はランディングゾーンコンテナを、使用可能な他のタイプのコンテナと区別できます。 |
| `refresh` | この `refresh` 「 」アクションを使用すると、ランディングゾーンの資格情報をリセットし、新しい `SASToken`. |

**リクエスト**

次のリクエストは、ランディングゾーンの資格情報を更新します。

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/connectors/landingzone/credentials?type=user_drop_zone&action=refresh' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
```

**応答**

次の応答は、 `SASToken` および `SASUri`.

```json
{
    "containerName": "dlz-user-container",
    "SASToken": "sv=2020-04-08&si=dlz-9c4d03b8-a6ff-41be-9dcf-20123e717e99&sr=c&sp=racwdlm&sig=JbRMoDmFHQU4OWOpgrKdbZ1d%2BkvslO35%2FXTqBO%2FgbRA%3D",
    "storageAccountName": "dlblobstore99hh25i3dflek",
    "SASUri": "https://dlblobstore99hh25i3dflek.blob.core.windows.net/dlz-user-container?sv=2020-04-08&si=dlz-9c4d03b8-a6ff-41be-9dcf-20123e717e99&sr=c&sp=racwdlm&sig=JbRMoDmFHQU4OWOpgrKdbZ1d%2BkvslO35%2FXTqBO%2FgbRA%3D"
}
```

## ランディングゾーンのファイル構造とコンテンツの参照

に対してGETリクエストをおこなうことで、ランディングゾーンのファイル構造とコンテンツを確認できます `connectionSpecs` エンドポイント [!DNL Flow Service] API

**API 形式**

```http
GET /connectionSpecs/{CONNECTION_SPEC_ID}/explore?objectType=root
```

| パラメーター | 説明 |
| --- | --- |
| `{CONNECTION_SPEC_ID}` | [!DNL Data Landing Zone] に対応する接続仕様 ID。 この修正済み ID は `26f526f2-58f4-4712-961d-e41bf1ccc0e8` です。 |

**リクエスト**

```shell
curl -X GET \
    'http://platform.adobe.io/data/foundation/flowservice/connectionSpecs/26f526f2-58f4-4712-961d-e41bf1ccc0e8/explore?objectType=root' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、クエリされたディレクトリ内で見つかったファイルとフォルダーの配列を返します。 次の項目をメモします。 `path` プロパティを指定してファイルの構造を検査する必要があるので、アップロードするファイルのプロパティを設定します。

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

## ランディングゾーンのファイル構造とコンテンツをプレビュー

ランディングゾーン内のファイルの構造を調べるには、ファイルのパスと型をクエリパラメーターとして指定し、GETリクエストを実行します。

**API 形式**

```http
GET /connectionSpecs/{CONNECTION_SPEC_ID}/explore?objectType=file&object={OBJECT}&fileType={FILE_TYPE}&preview={PREVIEW}
```

| パラメーター | 説明 | 例 |
| --- | --- | --- |
| `{CONNECTION_SPEC_ID}` | [!DNL Data Landing Zone] に対応する接続仕様 ID。 この修正済み ID は `26f526f2-58f4-4712-961d-e41bf1ccc0e8` です。 |
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
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、クエリされたファイルの構造（ファイル名とデータ型を含む）を返します。

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

### 用途 `determineProperties` ファイルのプロパティ情報を自動検出するには [!DNL Data Landing Zone]

以下を使用して、 `determineProperties` パラメータを使用して、 [!DNL Data Landing Zone] GET呼び出しを実行してソースのコンテンツと構造を調べる場合。

#### `determineProperties` 使用例

次の表に、 `determineProperties` クエリパラメーターを使用するか、手動でファイルに関する情報を指定します。

| `determineProperties` | `queryParams` | 応答 |
| --- | --- | --- |
| True | N/A | If `determineProperties` がクエリパラメーターとして指定された場合、ファイルのプロパティの検出が発生し、応答が新しい `properties` ファイルタイプ、圧縮タイプおよび列区切り文字に関する情報を含むキー。 |
| 該当なし | True | ファイルタイプ、圧縮タイプ、列区切りの値が `queryParams`の場合、スキーマの生成に使用され、同じプロパティが応答の一部として返されます。 |
| True | True | 両方のオプションが同時に実行された場合は、エラーが返されます。 |
| なし | なし | 2 つのオプションのどちらも指定されていない場合は、応答のプロパティを取得する方法がないので、エラーが返されます。 |

**API 形式**

```http
GET /connectionSpecs/{CONNECTION_SPEC_ID}/explore?objectType=file&object={OBJECT}&fileType={FILE_TYPE}&preview={PREVIEW}&determineProperties=true
```

| パラメーター | 説明 | 例 |
| --- | --- | --- |
| `determineProperties` | このクエリパラメーターを使用すると、 [!DNL Flow Service] ファイルタイプ、圧縮タイプ、列区切り文字に関する情報など、ファイルのプロパティに関する情報を検出する API です。 | `true` |

**リクエスト**

```shell
curl -X GET \
    'https://platform.adobe.io/data/foundation/flowservice/connectionSpecs/26f526f2-58f4-4712-961d-e41bf1ccc0e8/explore?objectType=file&object=dlz-user-container/garageWeek/file1&preview=true&determineProperties=true' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、クエリされたファイルの構造と、ファイル名、データ型、および `properties` キー、次の情報を含む `fileType`, `compressionType`、および `columnDelimiter`.

+++クリック

```json
{
    "properties": {
        "fileType": "delimited",
        "compressionType": "tarGzip",
        "columnDelimiter": "~"
    },
    "format": "flat",
    "schema": {
        "columns": [
            {
                "name": "id",
                "type": "string",
                "xdm": {
                    "type": "string"
                }
            },
            {
                "name": "firstName",
                "type": "string",
                "xdm": {
                    "type": "string"
                }
            },
            {
                "name": "lastName",
                "type": "string",
                "xdm": {
                    "type": "string"
                }
            },
            {
                "name": "email",
                "type": "string",
                "xdm": {
                    "type": "string"
                }
            },
            {
                "name": "birthday",
                "type": "string",
                "xdm": {
                    "type": "string"
                }
            }
        ]
    },
    "data": [
        {
            "birthday": "1313-0505-19731973",
            "firstName": "Yvonne",
            "lastName": "Thilda",
            "id": "100",
            "email": "Yvonne.Thilda@yopmail.com"
        },
        {
            "birthday": "1515-1212-19731973",
            "firstName": "Mary",
            "lastName": "Pillsbury",
            "id": "101",
            "email": "Mary.Pillsbury@yopmail.com"
        },
        {
            "birthday": "0505-1010-19751975",
            "firstName": "Corene",
            "lastName": "Joeann",
            "id": "102",
            "email": "Corene.Joeann@yopmail.com"
        },
        {
            "birthday": "2727-0303-19901990",
            "firstName": "Dari",
            "lastName": "Greenwald",
            "id": "103",
            "email": "Dari.Greenwald@yopmail.com"
        },
        {
            "birthday": "1717-0404-19651965",
            "firstName": "Lucy",
            "lastName": "Magdalen",
            "id": "199",
            "email": "Lucy.Magdalen@yopmail.com"
        }
    ]
}
```

+++

| プロパティ | 説明 |
| --- | --- |
| `properties.fileType` | クエリされたファイルの対応するファイルタイプ。 次のファイルタイプがサポートされています。 `delimited`, `json`、および `parquet`. |
| `properties.compressionType` | 問い合わせたファイルに使用される、対応する圧縮タイプ。 サポートされている圧縮タイプは次のとおりです。 <ul><li>`bzip2`</li><li>`gzip`</li><li>`zipDeflate`</li><li>`tarGzip`</li><li>`tar`</li></ul> |
| `properties.columnDelimiter` | クエリされたファイルに使用される、対応する列区切り文字。 あらゆる単一の文字の値を、列の区切り文字として使用できます。デフォルト値はコンマです。 `(,)`. |


## ソース接続の作成

ソース接続は、データの取り込み元となる外部ソースへの接続を作成および管理します。ソース接続は、データソース、データ形式、データフローの作成に必要なソース接続 ID などの情報で構成されます。 ソース接続インスタンスは、テナントと IMS 組織に固有です。

ソース接続を作成するには、[!DNL Flow Service] API の `/sourceConnections` エンドポイントに POST リクエストを実行します。


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
    -H 'x-gw-ims-org-id: {ORG_ID}' \
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
| `name` | お客様の [!DNL Data Landing Zone] ソース接続。 |
| `data.format` | Platform に取り込むデータの形式。 |
| `params.path` | Platform に取り込むファイルへのパス。 |
| `connectionSpec.id` | [!DNL Data Landing Zone] に対応する接続仕様 ID。 この修正済み ID は `26f526f2-58f4-4712-961d-e41bf1ccc0e8` です。 |

**応答**

リクエストが成功した場合は、新しく作成されたソース接続の一意の ID（`id`）が返されます。この ID は、次のチュートリアルでデータフローを作成する際に必要です。

```json
{
    "id": "f5b46949-8c8d-4613-80cc-52c9c039e8b9",
    "etag": "\"1400d460-0000-0200-0000-613be3520000\""
}
```

## 次の手順

このチュートリアルに従うことで、 [!DNL Data Landing Zone] 資格情報は、Platform に取り込むファイルを見つけるためにファイル構造を調べ、ソース接続を作成してデータの Platform への取り込みを開始します。 次のチュートリアルに進み、次の方法を学ぶことができます。 [データフローを作成し、 [!DNL Flow Service] API](../../collect/cloud-storage.md).
