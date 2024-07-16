---
keywords: Experience Platform;ホーム;人気のトピック;
solution: Experience Platform
title: Flow Service API を使用したAdobe Experience Platformへのデータランディングゾーンの接続
type: Tutorial
description: Flow Service API を使用してAdobe Experience Platformを Data Landing Zone に接続する方法を説明します。
exl-id: bdb60ed3-7c63-4a69-975a-c6f1508f319e
source-git-commit: 0089aa0d6b765645840e6954c3957282c2ad972b
workflow-type: tm+mt
source-wordcount: '1300'
ht-degree: 18%

---

# Flow Service API を使用した [!DNL Data Landing Zone] のAdobe Experience Platformへの接続

>[!IMPORTANT]
>
>このページは、Experience Platformの [!DNL Data Landing Zone] *ソース* コネクタに固有のページです。 [!DNL Data Landing Zone] *宛先* コネクタへの接続について詳しくは、[[!DNL Data Landing Zone]  宛先ドキュメントページ ](/help/destinations/catalog/cloud-storage/data-landing-zone.md) を参照してください。

[!DNL Data Landing Zone] は、ファイルをAdobe Experience Platformに取り込むための、セキュリティで保護されたクラウドベースのファイルストレージ機能です。 データは、7 日後に [!DNL Data Landing Zone] から自動的に削除されます。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して [!DNL Data Landing Zone] ソース接続を作成する手順を説明します。 このチュートリアルでは、[!DNL Data Landing Zone] ールの取得方法、資格情報の表示と更新方法に関する手順も説明します。

## はじめに

このガイドは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ソース](../../../../home.md)：Experience Platform を使用すると、データを様々なソースから取得しながら、Platform サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：Experience Platform には、単一の Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

次の節では、[!DNL Flow Service] API を使用して [!DNL Data Landing Zone] ソース接続を正常に作成するために必要な追加情報を示しています。

また、このチュートリアルでは [Platform API の基本を学ぶ ](../../../../../landing/api-guide.md) に関するガイドを読んで、Platform API への認証方法と、ドキュメントに記載されている呼び出し例を解釈する方法も確認する必要があります。

## 使用可能なランディングゾーンの取得

API を使用して [!DNL Data Landing Zone] にアクセスする最初の手順は、[!DNL Connectors] API の `/landingzone` エンドポイントに対してGETリクエストを行い、その際にリクエストヘッダーの一部として `type=user_drop_zone` を指定することです。

**API 形式**

```http
GET /data/foundation/connectors/landingzone?type=user_drop_zone
```

| ヘッダー | 説明 |
| --- | --- |
| `user_drop_zone` | `user_drop_zone` タイプを使用すると、ランディングゾーンコンテナを、使用可能な他のタイプのコンテナと区別できます。 |

**リクエスト**

次のリクエストでは、既存のランディングゾーンが取得されます。

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

次の応答は、ランディングゾーンに関する情報（対応する `containerName` と `containerTTL` を含む）を返します。

```json
{
    "containerName": "dlz-user-container",
    "containerTTL": "7"
}
```

| プロパティ | 説明 |
| --- | --- |
| `containerName` | 取得したランディングゾーンの名前。 |
| `containerTTL` | ランディングゾーン内のデータに適用される有効期限（日数）。 特定のランディングゾーン内のはすべて、7 日後に削除されます。 |

## 資格情報 [!DNL Data Landing Zone] 取得

[!DNL Data Landing Zone] の資格情報を取得するには、[!DNL Connectors] API の `/credentials` エンドポイントにGETリクエストを実行します。

**API 形式**

```http
GET /data/foundation/connectors/landingzone/credentials?type=user_drop_zone
```

**リクエスト**

次のリクエスト例では、既存のランディングゾーンの資格情報を取得します。

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

次の応答では、現在の `SASToken`、`SASUri`、`storageAccountName`、有効期限など、データランディングゾーンの資格情報情報が返されます。

```json
{
    "containerName": "dlz-user-container",
    "SASToken": "sv=2020-04-08&si=dlz-ed86a61d-201f-4b50-b10f-a1bf173066fd&sr=c&sp=racwdlm&sig=4yTba8voU3L0wlcLAv9mZLdZ7NlMahbfYYPTMkQ6ZGU%3D",
    "storageAccountName": "dlblobstore99hh25i3dflek",
    "SASUri": "https://dlblobstore99hh25i3dflek.blob.core.windows.net/dlz-user-container?sv=2020-04-08&si=dlz-ed86a61d-201f-4b50-b10f-a1bf173066fd&sr=c&sp=racwdlm&sig=4yTba8voU3L0wlcLAv9mZLdZ7NlMahbfYYPTMkQ6ZGU%3D",
    "expiryDate": "2024-01-06"
}
```

| プロパティ | 説明 |
| --- | --- |
| `containerName` | ランディングゾーンの名前。 |
| `SASToken` | ランディングゾーンの共有アクセス署名トークン。 この文字列には、リクエストの認証に必要なすべての情報が含まれます。 |
| `SASUri` | ランディングゾーンの共有アクセス署名 URI。 この文字列は、認証対象のランディングゾーンの URI とそれに対応する SAS トークンの組み合わせです。 |
| `expiryDate` | SAS トークンの有効期限が切れる日付。 データランディングゾーンにデータをアップロードするためにアプリケーションで引き続き使用するには、有効期限の前にトークンを更新する必要があります。 指定された有効期限の前にトークンを手動で更新しない場合、GET資格情報の呼び出しが実行されると、自動的に更新され、新しいトークンが提供されます。 |


## 資格情報 [!DNL Data Landing Zone] 更新

[!DNL Connectors] API の `/credentials` エンドポイントにPOSTリクエストを行うことで、`SASToken` を更新できます。

**API 形式**

```http
POST /data/foundation/connectors/landingzone/credentials?type=user_drop_zone&action=refresh
```

| ヘッダー | 説明 |
| --- | --- |
| `user_drop_zone` | `user_drop_zone` タイプを使用すると、ランディングゾーンコンテナを、使用可能な他のタイプのコンテナと区別できます。 |
| `refresh` | `refresh` のアクションを使用すると、ランディングゾーンの資格情報をリセットし、新しい `SASToken` を自動的に生成できます。 |

**リクエスト**

次のリクエストは、ランディングゾーン資格情報を更新します。

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

次の応答は、`SASToken` および `SASUri` の更新された値を返します。

```json
{
    "containerName": "dlz-user-container",
    "SASToken": "sv=2020-04-08&si=dlz-9c4d03b8-a6ff-41be-9dcf-20123e717e99&sr=c&sp=racwdlm&sig=JbRMoDmFHQU4OWOpgrKdbZ1d%2BkvslO35%2FXTqBO%2FgbRA%3D",
    "storageAccountName": "dlblobstore99hh25i3dflek",
    "SASUri": "https://dlblobstore99hh25i3dflek.blob.core.windows.net/dlz-user-container?sv=2020-04-08&si=dlz-9c4d03b8-a6ff-41be-9dcf-20123e717e99&sr=c&sp=racwdlm&sig=JbRMoDmFHQU4OWOpgrKdbZ1d%2BkvslO35%2FXTqBO%2FgbRA%3D",
    "expiryDate": "2024-01-06"
}
```

## ランディングゾーンのファイル構造と内容の探索

[!DNL Flow Service] API の `connectionSpecs` エンドポイントに対してGETリクエストを実行することで、ランディングゾーンのファイル構造と内容を調べることができます。

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

応答が成功すると、クエリされたディレクトリ内にあるファイルとフォルダーの配列が返されます。 構造を検査するために次の手順でアップロードするファイルを指定する必要があるので、アップロードするファイルの `path` プロパティをメモします。

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

## ランディングゾーンファイルの構造と内容をプレビュー

ランディングゾーン内のファイルの構造を調べるには、GETリクエストを実行し、その際にファイルのパスとタイプをクエリパラメーターとして指定します。

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
| `{PREVIEW}` | ファイルのプレビューがサポートされているかどうかを定義するブール値。 | </ul><li>`true`</li><li>`false`</li></ul> |

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

応答が成功すると、ファイル名とデータタイプを含む、クエリされたファイルの構造が返されます。

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

### `determineProperties` を使用して、[!DNL Data Landing Zone] のファイルプロパティ情報を自動検出します

`determineProperties` パラメーターを使用すると、ソースの内容と構造を調べるためにGET呼び出しを行う際に、[!DNL Data Landing Zone] ータのファイル内容のプロパティ情報を自動検出できます。

#### `determineProperties` の使用例

次の表は、`determineProperties` クエリパラメーターを使用する際や、ファイルに関する情報を手動で指定する際に発生する可能性がある様々なシナリオの概要を説明しています。

| `determineProperties` | `queryParams` | 応答 |
| --- | --- | --- |
| True | なし | `determineProperties` がクエリパラメーターとして指定されている場合、ファイルプロパティの検出が行われ、応答は、ファイルタイプ、圧縮タイプ、列の区切り文字に関する情報を含んだ新しい `properties` キーを返します。 |
| なし | True | ファイルタイプ、圧縮タイプ、列区切り文字の値が `queryParams` の一部として手動で指定された場合、これらの値がスキーマの生成に使用され、同じプロパティが応答の一部として返されます。 |
| True | True | 両方のオプションが同時に実行された場合は、エラーが返されます。 |
| なし | なし | 2 つのオプションのどちらも指定されていない場合、応答のプロパティを取得する方法がないため、エラーが返されます。 |

**API 形式**

```http
GET /connectionSpecs/{CONNECTION_SPEC_ID}/explore?objectType=file&object={OBJECT}&fileType={FILE_TYPE}&preview={PREVIEW}&determineProperties=true
```

| パラメーター | 説明 | 例 |
| --- | --- | --- |
| `determineProperties` | このクエリパラメーターを使用すると、[!DNL Flow Service] API でファイルタイプ、圧縮タイプ、列の区切り文字に関する情報など、ファイルのプロパティに関する情報を検出できます。 | `true` |

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

応答が成功すると、クエリされたファイルの構造（ファイル名とデータタイプを含む）と、`fileType`、`compressionType`、`columnDelimiter` に関する情報を含む `properties` キーが返されます。

+++ここをクリック

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
| `properties.fileType` | クエリされたファイルの対応するファイルタイプ。 サポートされているファイルタイプは、`delimited`、`json`、`parquet` です。 |
| `properties.compressionType` | クエリされたファイルに使用される、対応する圧縮タイプ。 サポートされている圧縮タイプは次の通りです。 <ul><li>`bzip2`</li><li>`gzip`</li><li>`zipDeflate`</li><li>`tarGzip`</li><li>`tar`</li></ul> |
| `properties.columnDelimiter` | クエリされたファイルに使用される、対応する列区切り文字。 あらゆる単一の文字の値を、列の区切り文字として使用できます。デフォルト値はコンマ `(,)` です。 |


## ソース接続の作成

ソース接続は、データの取り込み元となる外部ソースへの接続を作成および管理します。ソース接続は、データソース、データ形式、データフローの作成に必要なソース接続 ID などの情報で構成されます。 ソース接続インスタンスは、テナントと組織に固有です。

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
| `name` | [!DNL Data Landing Zone] ソース接続の名前。 |
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

このチュートリアルでは、[!DNL Data Landing Zone] 資格情報を取得し、そのファイル構造を調べて、Platform に取り込むファイルを見つけ、Platform へのデータの取り込みを開始するためのソース接続を作成しました。 次のチュートリアルに進むことができます。ここでは、[API を使用してクラウドストレージデータを Platform に取り込むためのデータフローを作成 ](../../collect/cloud-storage.md) する方法を説明し  [!DNL Flow Service]  す。
