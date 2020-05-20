---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: データアクセスの概要
topic: tutorial
translation-type: tm+mt
source-git-commit: 4817162fe2b7cbf4ae4c1ed325db2af31da5b5d3
workflow-type: tm+mt
source-wordcount: '1367'
ht-degree: 2%

---


# Data Access APIを使用したクエリデータセットデータ

このドキュメントでは、Adobe Experience PlatformのData Access APIを使用して、データセット内に保存されたデータの検索、アクセス、ダウンロードの方法を説明するチュートリアルを順を追って説明します。 また、ページングや部分的なダウンロードなど、Data Access APIの独自の機能の一部についても紹介します。

## はじめに

このチュートリアルでは、データセットの作成と設定の方法について説明します。 詳しくは、 [データセット作成のチュートリアル](../../catalog/datasets/create.md) （英語）を参照してください。

以下の節では、プラットフォームAPIを正しく呼び出すために知る必要がある追加情報について説明します。

### サンプルAPI呼び出しの読み取り

このチュートリアルでは、リクエストをフォーマットする方法を示すAPI呼び出しの例を提供します。 例えば、パス、必須のヘッダー、適切にフォーマットされた要求ペイロードなどです。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される表記について詳しくは、Experience PlatformトラブルシューティングガイドのAPI呼び出し例の読み [方に関する節を参照してください](../../landing/troubleshooting.md#how-do-i-format-an-api-request) 。

### 必要なヘッダーの値の収集

プラットフォームAPIを呼び出すには、まず [認証チュートリアルを完了する必要があります](../../tutorials/authentication.md)。 次に示すように、認証チュートリアルで、すべてのExperience Platform API呼び出しに必要な各ヘッダーの値を指定します。

- 認証： 無記名 `{ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id: `{IMS_ORG}`

エクスペリエンスプラットフォームのすべてのリソースは、特定の仮想サンドボックスに分離されています。 プラットフォームAPIへのすべてのリクエストには、操作が実行されるサンドボックスの名前を指定するヘッダーが必要です。

- x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE] プラットフォームのサンドボックスについて詳しくは、「 [サンドボックスの概要に関するドキュメント](../../sandboxes/home.md)」を参照してください。

ペイロード(POST、PUT、PATCH)を含むすべてのリクエストには、次の追加のヘッダーが必要です。

- Content-Type: application/json

## シーケンス図

このチュートリアルでは、次のシーケンス図に示す手順に従って、Data Access APIのコア機能に焦点を当てます。</br>
![](../images/sequence_diagram.png)

カタログAPIを使用すると、バッチやファイルに関する情報を取得できます。 Data Access APIを使用すると、ファイルのサイズに応じて、HTTP経由でこれらのファイルにアクセスしたり、ファイルの全部または一部をダウンロードしたりできます。

## データの検索

Data Access APIを使用する前に、アクセスするデータの場所を特定する必要があります。 カタログAPIには、組織のメタデータを参照し、アクセスするバッチまたはファイルのIDを取得するために使用できるエンドポイントが2つあります。

- `GET /batches`: 組織内のバッチのリストを戻します
- `GET /dataSetFiles`: 組織内のファイルのリストを返します

カタログAPIのエンドポイントの包括的なリストについては、『 [APIリファレンス](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/catalog.yaml)』を参照してください。

## IMS組織でのバッチのリストの取得

カタログAPIを使用して、組織の下のバッチのリストを返すことができます。

**API形式**

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

応答には、IMS組織に関連するすべてのバッチのリストを含むオブジェクトが含まれ、各最上位レベルの値はバッチを表します。 個々のバッチオブジェクトには、その特定のバッチの詳細が含まれます。 下の応答は最小限に抑えられ、空き容量が確保されました。

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

### バッチのリストのフィルタリング

特定の使用事例に関連するデータを取得するために、フィルターは特定のバッチを見つける必要がある場合が多くあります。 返される応答をフィルターするために、 `GET /batches` リクエストにパラメーターを追加できます。 以下のリクエストは、特定のデータセット内で指定された時間が経過した後に作成されたすべてのバッチを、いつ作成されたかで並べ替えて返します。

**API形式**

```http
GET /batches?createdAfter={START_TIMESTAMP}&dataSet={DATASET_ID}&sort={SORT_BY}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{START_TIMESTAMP}` | ミリ秒単位の開始タイムスタンプ（例：1514836799000）。 |
| `{DATASET_ID}` | データセットの識別子。 |
| `{SORT_BY}` | 指定された値で応答を並べ替えます。 例えば、作成日を降順に `desc:created` 並べ替えます。 |

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

パラメーターとフィルターーの完全なリストについては、 [カタログAPIリファレンスを参照してください](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/catalog.yaml)。

## 特定のバッチに属するすべてのファイルのリストを取得する

アクセスするバッチのIDが決まったので、Data Access APIを使用して、そのバッチに属するファイルのリストを取得できます。

**API形式**

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
| `data._links.self.href` | このファイルにアクセスするURLです。 |

応答には、指定したバッチ内のすべてのファイルをリストするデータ配列が含まれます。 ファイルは、そのファイルIDによって参照されます。ファイルIDは、 `dataSetFileId` フィールドの下にあります。

## ファイルIDを使用したファイルへのアクセス

一意のファイルIDを取得したら、Data Access APIを使用して、ファイル名、バイト単位のサイズ、ファイルをダウンロードするためのリンクなど、ファイルに関する特定の詳細にアクセスできます。

**API形式**

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

ファイルIDが個々のファイルを指すかディレクトリを指すかによって、返されるデータ配列には、そのディレクトリに属するファイルの1つのエントリまたはリストが含まれる場合があります。 各ファイル要素には、ファイル名、バイト単位のサイズ、ファイルをダウンロードするためのリンクなどの詳細が含まれます。

**ケース1: ファイルIDは単一のファイルを指します**

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
| `_links.self.href` | ファイルをダウンロードするURLです。 |

**ケース2: ファイルIDがディレクトリを指す**

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
| `data._links.self.href` | 関連ファイルをダウンロードするURL。 |

この応答は、IDとIDを持つ2つの異なるファイルを含むディレクトリ `{FILE_ID_2}` を返し `{FILE_ID_3}`ます。 このシナリオでは、各ファイルにアクセスするには、各ファイルのURLに従う必要があります。

## ファイルのメタデータの取得

HEADリクエストを作成して、ファイルのメタデータを取得できます。 これにより、サイズ（バイト単位）やファイル形式を含むファイルのメタデータヘッダが返されます。

**API形式**

```http
HEAD /files/{FILE_ID}?path={FILE_NAME}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{FILE_ID}` | ファイルの識別子。 |
| `{FILE_NAME`} | ファイル名(例：プロファイル.パーケット) |

**リクエスト**

```shell
curl -I 'https://platform.adobe.io/data/foundation/export/files/8dcedb36-1cb2-4496-9a38-7b2041114b56-1?path=profiles.parquet' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

応答ヘッダーには、次のような、照会されたファイルのメタデータが含まれます。
- `Content-Length`: ペイロードのサイズをバイト単位で示します
- `Content-Type`: ファイルの種類を示します。

## ファイルのコンテンツへのアクセス

データアクセスAPIを使用してファイルのコンテンツにアクセスすることもできます。

**API形式**

```shell
GET /files/{FILE_ID}?path={FILE_NAME}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{FILE_ID}` | ファイルの識別子。 |
| `{FILE_NAME`} | ファイル名(例：プロファイル.パーケット)。 |

**リクエスト**

```shell
curl -X GET 'https://platform.adobe.io/data/foundation/export/files/8dcedb36-1cb2-4496-9a38-7b2041114b56-1?path=profiles.parquet' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、ファイルの内容を返します。

## ファイルの一部のコンテンツのダウンロード

Data Access APIを使用すると、チャンク単位でファイルをダウンロードできます。 範囲ヘッダーは、ファイルから特定のバイト範囲をダウンロードする `GET /files/{FILE_ID}` 要求時に指定できます。 範囲を指定しない場合、APIはデフォルトでファイル全体をダウンロードします。

[前の節のHEADの例では](#retrieve-the-metadata-of-a-file) 、特定のファイルのサイズをバイト単位で示しています。

**API形式**

```http
GET /files/{FILE_ID}?path={FILE_NAME}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{FILE_ID} ` | ファイルの識別子。 |
| `{FILE_NAME}` | ファイル名(例：プロファイル.パーケット) |

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
| `Range: bytes=0-99` | ダウンロードするバイトの範囲を指定します。 この値を指定しない場合、APIはファイル全体をダウンロードします。 この例では、最初の100バイトがダウンロードされます。 |

**応答**

応答本体には、ファイルの最初の100バイト（要求の「Range」ヘッダーで指定）と、HTTPステータス206（部分的な内容）が含まれます。 この応答には、次のヘッダーも含まれます。

- Content-Length: 100（返されるバイト数）
- コンテンツタイプ： application/parket（パーケファイルが要求されたので、応答コンテンツタイプはparket）
- Content-Range: バイト0 ～ 99/249058(合計バイト数の中から要求された範囲(0 ～ 99))(249058))

## API応答のページネーションの設定

データアクセスAPI内の応答はページ分割されます。 デフォルトでは、1ページあたりの最大エントリ数は100です。 ページングパラメーターを使用して、デフォルトの動作を変更できます。

- `limit`: 「limit」パラメーターを使用して、必要に応じて1ページあたりのエントリ数を指定できます。
- `start`: オフセットは、「開始」クエリパラメータで設定できます。
- `&`: アンパサンドを使用すると、1回の呼び出しで複数のパラメーターを組み合わせることができます。

**API形式**

```http
GET /batches/{BATCH_ID}/files?start={OFFSET}
GET /batches/{BATCH_ID}/files?limit={LIMIT}
GET /batches/{BATCH_ID}/files?start={OFFSET}&limit={LIMIT}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{BATCH_ID}` | アクセスしようとしているバッチのバッチ識別子。 |
| `{OFFSET}` | 結果配列を開始するための指定されたインデックス(例：開始=0) |
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

応答には、リクエストパラメーターで指定された、単一の要素を持つ `"data"` 配列が含まれ `limit=1`ます。 この要素は、リクエストの `start=0` パラメーターで指定された、最初に使用可能なファイルの詳細を含むオブジェクトです（ゼロベースの番号では、最初の要素は「0」になります）。

この `_links.next.href` 値には応答の次のページへのリンクが含まれます。このリンクから、 `start` パラメーターの詳細がわかり `start=1`ます。

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
