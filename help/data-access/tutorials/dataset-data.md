---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: データアクセスの概要
topic: tutorial
translation-type: tm+mt
source-git-commit: cb5df9b44486bda84f08805f1077d6097e3666e2
workflow-type: tm+mt
source-wordcount: '1332'
ht-degree: 74%

---


# APIを使用したクエリデータセット [!DNL Data Access] データ

This document provides a step-by-step tutorial that covers how to locate, access, and download data stored within a dataset using the [!DNL Data Access] API in Adobe Experience Platform. You will also be introduced to some of the unique features of the [!DNL Data Access] API, such as paging and partial downloads.

## はじめに

このチュートリアルでは、データセットの作成方法と設定方法について説明します。詳しくは、「[データセ ット作成のチュートリアル](../../catalog/datasets/create.md)」を参照してください。

以下の節では、Platform API を正しく呼び出すために知っておく必要がある追加情報を示します。

### API 呼び出し例の読み取り

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。この中には、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。[!DNL Experience Platform]

### 必須ヘッダーの値の収集

In order to make calls to [!DNL Platform] APIs, you must first complete the [authentication tutorial](../../tutorials/authentication.md). Completing the authentication tutorial provides the values for each of the required headers in all [!DNL Experience Platform] API calls, as shown below:

- Authorization: Bearer `{ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id: `{IMS_ORG}`

All resources in [!DNL Experience Platform] are isolated to specific virtual sandboxes. All requests to [!DNL Platform] APIs require a header that specifies the name of the sandbox the operation will take place in:

- x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE]
>
>For more information on sandboxes in [!DNL Platform], see the [sandbox overview documentation](../../sandboxes/home.md).

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、以下のような追加ヘッダーが必要です。

- Content-Type: application/json

## シーケンス図

This tutorial follows the steps outlined in the sequence diagram below, highlighting the core functionality of the [!DNL Data Access] API.</br>
![](../images/sequence_diagram.png)

The [!DNL Catalog] API allows you to retrieve information regarding batches and files. The [!DNL Data Access] API allows you to access and download these files over HTTP as either full or partial downloads, depending on the size of the file.

## データの検索

Before you can begin to use the [!DNL Data Access] API, you need to identify the location of the data that you want to access. In the [!DNL Catalog] API, there are two endpoints that you can use to browse an organization&#39;s metadata and retrieve the ID of a batch or file that you want to access:

- `GET /batches`：組織内のバッチのリストを返します
- `GET /dataSetFiles`：組織内のファイルのリストを返します

For a comprehensive list of endpoints in the [!DNL Catalog] API, please refer to the [API Reference](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/catalog.yaml).

## IMS 組織内のバッチのリストを取得する

Using the [!DNL Catalog] API, you can return a list of batches under your organization:

**API 形式**

```http
GET /batches
```

**リクエスト**

```shell
curl -X GET 'https://platform.adobe.io/data/foundation/catalog/batches/' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

応答には、IMS 組織に関連するすべてのバッチのリストを含むオブジェクトが含まれ、各最上位の値はバッチを表します。個々のバッチオブジェクトには、その特定のバッチの詳細が含まれます。以下の応答は、スペースのために最小化されています。

```json
{
    "{BATCH_ID_1}": {
        "imsOrg": "{IMS_ORG}",
        "created": 1516640135526,
        "createdClient": "{CREATED_CLIENT}",
        "createdUser": "{CREATED_BY}",
        "updatedUser": "{CREATED_BY}",
        "updated": 1516640135526,
        "status": "processing",
        "version": "1.0.0",
        "availableDates": {}
    },
    "{BATCH_ID_2}": {
    ...
    }
}
```

### バッチのリストのフィルター

特定の使用事例に関連するデータを取得するために、特定のバッチを検索するためにフィルターが必要になることがよくあります。`GET /batches`リクエストにパラメーターを追加して、返された応答をフィルターすることができます。以下のリクエストは、特定のデータセット内で、指定した時間が経過した後に作成されたすべてのバッチを、いつ作成されたかで並べ替えて返します。

**API 形式**

```http
GET /batches?createdAfter={START_TIMESTAMP}&dataSet={DATASET_ID}&sort={SORT_BY}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{START_TIMESTAMP}` | 開始のタイムスタンプ（ミリ秒）（例：1514836799000）。 |
| `{DATASET_ID}` | データセット識別子。 |
| `{SORT_BY}` | 指定された値で応答を並べ替えます。たとえば、`desc:created`は作成日を降順に並べ替えてオブジェクトをソートします。 |

**リクエスト**

```shell
curl -X GET 'https://platform.adobe.io/data/foundation/catalog/batches?createdAfter=1521053542579&dataSet=5cd9146b21dae914b71f654f&orderBy=desc:created' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

```json
{   "{BATCH_ID_3}": {
        "imsOrg": "{IMS_ORG}",
        "relatedObjects": [
            {
                "id": "5c01a91863540f14cd3d0439",
                "type": "dataSet"
            },
            {
                "id": "00998255b4a148a2bfd4804c2f327324",
                "type": "batch"
            }
        ],
        "status": "success",
        "metrics": {
            "recordsFailed": 0,
            "recordsWritten": 2,
            "startTime": 1550791835809,
            "endTime": 1550791994636
        },
        "errors": [],
        "created": 1550791457173,
        "createdClient": "{CLIENT_CREATED}",
        "createdUser": "{CREATED_BY}",
        "updatedUser": "{CREATED_BY}",
        "updated": 1550792060301,
        "version": "1.0.116"
    },
    "{BATCH_ID_4}": {
        "imsOrg": "{IMS_ORG}",
        "status": "success",
        "relatedObjects": [
            {
                "type": "batch",
                "id": "00aff31a9ae84a169d69b886cc63c063"
            },
            {
                "type": "dataSet",
                "id": "5bfde8c5905c5a000082857d"
            }
        ],
        "metrics": {
            "startTime": 1544571333876,
            "endTime": 1544571358291,
            "recordsRead": 4,
            "recordsWritten": 4
        },
        "errors": [],
        "created": 1544571077325,
        "createdClient": "{CLIENT_CREATED}",
        "createdUser": "{CREATED_BY}",
        "updatedUser": "{CREATED_BY}",
        "updated": 1544571368776,
        "version": "1.0.3"
    }
}
```

パラメーターとフィルターの完全なリストについては、「[カタログ API リファレンス](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/catalog.yaml)」を参照してください。

## 特定のバッチに属するすべてのファイルのリストを取得する

Now that you have the ID of the batch that you want to access, you can use the [!DNL Data Access] API to get a list of files belonging to that batch.

**API 形式**

```http
GET /batches/{BATCH_ID}/files
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{BATCH_ID}` | アクセスしようとしているバッチのバッチ識別子。 |

**リクエスト**

```shell
curl -X GET 'https://platform.adobe.io/data/foundation/export/batches/5c6f332168966814cd81d3d3/files' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

```json
{
    "data": [
        {
            "dataSetFileId": "8dcedb36-1cb2-4496-9a38-7b2041114b56-1",
            "dataSetViewId": "5cc6a9b60d4a5914b7940a7f",
            "version": "1.0.0",
            "created": "1558522305708",
            "updated": "1558522305708",
            "isValid": false,
            "_links": {
                "self": {
                    "href": "https://platform.adobe.io:443/data/foundation/export/files/8dcedb36-1cb2-4496-9a38-7b2041114b56-1"
                }
            }
        }
    ],
    "_page": {
        "limit": 100,
        "count": 1
    }
}
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `data._links.self.href` | このファイルにアクセスする URL。 |

応答には、指定したバッチ内のすべてのファイルをリストするデータ配列が含まれます。ファイルは、そのファイル ID によって参照されます。このファイルは、`dataSetFileId`フィールドの下にあります。

## ファイル ID を使用したファイルへのアクセス

Once you have a unique file ID, you can use the [!DNL Data Access] API to access the specific details about the file, including its name, size in bytes, and a link to download it.

**API 形式**

```http
GET /files/{FILE_ID}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{FILE_ID}` | アクセスするファイルの識別子。 |

**リクエスト**

```shell
curl -X GET 'https://platform.adobe.io/data/foundation/export/files/8dcedb36-1cb2-4496-9a38-7b2041114b56-1' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

ファイル ID が個々のファイルを指すかディレクトリを指すかに応じて、返されるデータ配列には、そのディレクトリに属するファイルの 1 つのエントリまたはリストが含まれる場合があります。各ファイル要素には、ファイル名、サイズ（バイト単位）、ファイルをダウンロードするためのリンクなどの詳細が含まれます。

**ケース 1：ファイル ID が単一のファイルを指す**

**応答** 

```json
{
    "data": [
        {
            "name": "{FILE_NAME}.parquet",
            "length": "249058",
            "_links": {
                "self": {
                    "href": "https://platform.adobe.io/data/foundation/export/files/{FILE_ID_1}?path={FILE_NAME_1}.parquet"
                }
            }
        }
    ],
    "_page": {
        "limit": 100,
        "count": 1
    }
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{FILE_NAME}.parquet` | ファイルの名前。 |
| `_links.self.href` | ファイルをダウンロードする URL。 |

**ケース 2：ファイル ID がディレクトリを指す**

**応答** 

```json
{
    "data": [
        {
            "dataSetFileId": "{FILE_ID_2}",
            "dataSetViewId": "460590b01ba38afd1",
            "version": "1.0.0",
            "created": "150151267347",
            "updated": "150151267347",
            "isValid": true,
            "_links": {
                "self": {
                    "href": "https://platform.adobe.io/data/foundation/export/files/{FILE_ID_2}"
                }
            }
        },
        {
            "dataSetFileId": "{FILE_ID_3}",
            "dataSetViewId": "460590b01ba38afd1",
            "version": "1.0.0",
            "created": "150151267685",
            "updated": "150151267685",
            "isValid": true,
            "_links": {
                "self": {
                    "href": "https://platform.adobe.io/data/foundation/export/files/{FILE_ID_3}"
                }
            }
        }
    ],
    "_page": {
        "limit": 100,
        "count": 2
    }
}
```

| プロパティ | 説明 |
| -------- | ----------- | 
| `data._links.self.href` | 関連ファイルをダウンロードする URL。 |

この応答は、ID `{FILE_ID_2}`と`{FILE_ID_3}`を持つ 2 つの異なるファイルを含むディレクトリを返します。このシナリオでは、各ファイルにアクセスするには、各ファイルの URL に従う必要があります。

## ファイルのメタデータの取得

HEAD リクエストをおこなうことで、ファイルのメタデータを取得できます。これは、サイズ（バイト単位）やファイル形式を含む、ファイルのメタデータヘッダーを返します。

**API 形式**

```http
HEAD /files/{FILE_ID}?path={FILE_NAME}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{FILE_ID}` | ファイルの識別子。 |
| `{FILE_NAME}` | ファイル名（例：profiles.parquet） |

**リクエスト**

```shell
curl -I 'https://platform.adobe.io/data/foundation/export/files/8dcedb36-1cb2-4496-9a38-7b2041114b56-1?path=profiles.parquet' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

応答ヘッダーには、次のような、クエリされたファイルのメタデータが含まれます。
- `Content-Length`：ペイロードのサイズをバイト単位で示します
- `Content-Type`：ファイルのタイプを示します。

## ファイルのコンテンツへのアクセス

You can also access the contents of a file using the [!DNL Data Access] API.

**API 形式**

```shell
GET /files/{FILE_ID}?path={FILE_NAME}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{FILE_ID}` | ファイルの識別子。 |
| `{FILE_NAME}` | ファイル名（例：profiles.parquet）。 |

**リクエスト**

```shell
curl -X GET 'https://platform.adobe.io/data/foundation/export/files/8dcedb36-1cb2-4496-9a38-7b2041114b56-1?path=profiles.parquet' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

成功した場合は、ファイルの内容が返されます。

## ファイルの一部コンテンツのダウンロード

The [!DNL Data Access] API allows for downloading files in chunks. 範囲ヘッダーは、ファイルから特定のバイト範囲をダウンロードする`GET /files/{FILE_ID}`リクエスト時に指定できます。範囲を指定しない場合、API はデフォルトでファイル全体をダウンロードします。

[前の節](#retrieve-the-metadata-of-a-file)の HEAD の例では、特定のファイルのサイズをバイト単位で示しています。

**API 形式**

```http
GET /files/{FILE_ID}?path={FILE_NAME}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{FILE_ID} ` | ファイルの識別子。 |
| `{FILE_NAME}` | ファイル名（例：profiles.parquet） |

**リクエスト**

```shell
curl -X GET 'https://platform.adobe.io/data/foundation/export/files/8dcedb36-1cb2-4496-9a38-7b2041114b56-1?path=profiles.parquet' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Range: bytes=0-99'
```

| プロパティ | 説明 |
| -------- | ----------- | 
| `Range: bytes=0-99` | ダウンロードするバイトの範囲を指定します。範囲を指定しない場合、API はファイル全体をダウンロードします。この例では、最初の 100 バイトがダウンロードされます。 |

**応答** 

応答本体には、（リクエストの「範囲」ヘッダーで指定された）ファイルの最初の 100 バイトと、HTTP ステータス 206（部分的な内容）が含まれます。応答には、次のヘッダーも含まれます。

- コンテンツの長さ：100（返されるバイト数）
- Content-type: application/parquet（パケットファイルがリクエストされたので、応答コンテンツタイプはパケット）
- コンテンツ範囲：バイト 0～99／249058（合計バイト数（249058）のうち、リクエストされた範囲（0-99））

## API 応答のページネーションの設定

Responses within the [!DNL Data Access] API are paginated. デフォルトでは、1 ページあたりの最大エントリ数は 100 です。ページングパラメーターを使用して、デフォルトの動作を変更できます。

- `limit`：「limit」パラメーターを使用して、必要に応じて 1 ページあたりのエントリ数を指定できます。
- `start`：オフセットは、「開始」クエリパラメーターで設定できます。
- `&`：アンパサンドを使用すると、1 回の呼び出しで複数のパラメーターを組み合わせることができます。

**API 形式**

```http
GET /batches/{BATCH_ID}/files?start={OFFSET}
GET /batches/{BATCH_ID}/files?limit={LIMIT}
GET /batches/{BATCH_ID}/files?start={OFFSET}&limit={LIMIT}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{BATCH_ID}` | アクセスしようとしているバッチのバッチ識別子。 |
| `{OFFSET}` | 結果配列を開始するインデックス（例：開始= 0） |
| `{LIMIT}` | 結果配列に返される結果の数を制御します（例：limit=1） |

**リクエスト**

```shell
curl -X GET 'https://platform.adobe.io/data/foundation/export/batches/5c102cac7c7ebc14cd6b098e/files?start=0&limit=1' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**:

応答には、リクエストパラメーター`limit=1`で指定された単一の要素を持つ`"data"`配列が含まれます。この要素は、リクエストの`start=0`パラメーターで指定された、最初に使用可能なファイルの詳細を含むオブジェクトです（0 から始まる番号では、最初の要素は「0」になります）。

この`_links.next.href`値には、応答の次のページへのリンクが含まれています。このリンクで、`start`パラメーターの詳細`start=1`がわかります。

```json
{
    "data": [
        {
            "dataSetFileId": "{FILE_ID_1}",
            "dataSetViewId": "5a9f264c2aa0cf01da4d82fa",
            "version": "1.0.0",
            "created": "1521053793635",
            "updated": "1521053793635",
            "isValid": false,
            "_links": {
                "self": {
                    "href": "https://platform.adobe.io/data/foundation/export/files/{FILE_ID_1}"
                }
            }
        }
    ],
    "_page": {
        "limit": 1,
        "count": 6
    },
    "_links": {
        "next": {
            "href": "https://platform.adobe.io/data/foundation/export/batches/5c102cac7c7ebc14cd6b098e/files?start=1&limit=1"
        },
        "page": {
            "href": "https://platform.adobe.io/data/foundation/export/batches/5c102cac7c7ebc14cd6b098e/files?start=0&limit=1",
            "templated": true
        }
    }
}
```
