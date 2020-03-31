---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: データアクセスの概要
topic: tutorial
translation-type: tm+mt
source-git-commit: 4817162fe2b7cbf4ae4c1ed325db2af31da5b5d3

---


# クエリデータセットデータのデータアクセスAPIを使用

このドキュメントでは、Adobe Experience PlatformのデータアクセスAPIを使用して、データセット内に保存されたデータを検索、アクセス、ダウンロードする手順を説明するチュートリアルを紹介します。 また、ページングや部分的なダウンロードなど、Data Access APIの固有の機能の一部も紹介します。

## はじめに

このチュートリアルでは、データセットの作成方法と設定方法について説明します。 詳しくは、データセ [ット作成のチュートリアル](../../catalog/datasets/create.md) （英語のみ）を参照してください。

以下の節では、プラットフォームAPIを正しく呼び出すために知っておく必要がある追加情報を示します。

### サンプルAPI呼び出しの読み取り

このチュートリアルでは、リクエストをフォーマットする方法を示すAPI呼び出しの例を示します。 これには、パス、必須ヘッダー、適切にフォーマットされたリクエストペイロードが含まれます。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される表記について詳しくは、エクスペリエンスプラットフォームのトラブルシューテ [ィングガイドのAPI呼び出し例の読み方に関する節](../../landing/troubleshooting.md#how-do-i-format-an-api-request) （英語のみ）を参照してください。

### 必要なヘッダーの値の収集

プラットフォームAPIを呼び出すには、まず認証チュートリアルを完了する必要 [があります](../../tutorials/authentication.md)。 次に示すように、認証チュートリアルで、すべてのエクスペリエンスプラットフォームAPI呼び出しで必要な各ヘッダーの値を入力します。

- 認証：無記名 `{ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id: `{IMS_ORG}`

エクスペリエンスプラットフォームのすべてのリソースは、特定の仮想サンドボックスに分離されます。 プラットフォームAPIへのすべてのリクエストには、操作が行われるサンドボックスの名前を指定するヘッダーが必要です。

- x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE] プラットフォームのサンドボックスについて詳しくは、サンドボックスの概要ドキュメ [ントを参照してくださ](../../sandboxes/home.md)い。

ペイロード(POST、PUT、PATCH)を含むすべてのリクエストには、次のヘッダーが追加で必要です。

- コンテンツタイプ：application/json

## シーケンス図

このチュートリアルでは、次のシーケンス図に示す手順に従って、データアクセスAPIのコア機能を強調表示します。</br>
![](../images/sequence_diagram.png)

カタログAPIを使用すると、バッチやファイルに関する情報を取得できます。 データアクセスAPIを使用すると、ファイルのサイズに応じて、HTTP経由でこれらのファイルにアクセスし、フルダウンロードまたは部分的なダウンロードとしてダウンロードできます。

## データの検索

データアクセスAPIを使用する前に、アクセスするデータの場所を特定する必要があります。 カタログAPIには、組織のメタデータを参照し、アクセスするバッチまたはファイルのIDを取得するために使用できるエンドポイントが2つあります。

- `GET /batches`:組織内のリストのバッチを戻します。
- `GET /dataSetFiles`:組織内のリストのファイルを返します

カタログAPIのエンドポイントの包括的なリストについては、『 [APIリファレンス』を参照してください](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/catalog.yaml)。

## IMS組織でのリストのバッチの取得

カタログAPIを使用して、組織のバッチのリストを返却できます。

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

応答には、IMS組織に関連するすべてのバッチのリストを含むオブジェクトが含まれ、各最上位の値はバッチを表します。 個々のバッチオブジェクトには、その特定のバッチの詳細が含まれます。 以下の応答は最小限に抑えられ、空き容量が確保されました。

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

### バッチのリストのフィルタ

フィルターは、特定の使用事例に関連するデータを取得するために、特定のバッチを検索する必要がある場合が多くあります。 リクエストにパラメーターを追加して、返 `GET /batches` された応答をフィルターすることができます。 以下のリクエストは、特定のデータセット内で、指定した時間が経過した後に作成されたすべてのバッチを、いつ作成されたかで並べ替えて返します。

**API形式**

```http
GET /batches?createdAfter={START_TIMESTAMP}&dataSet={DATASET_ID}&sort={SORT_BY}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{START_TIMESTAMP}` | 開始のタイムスタンプ（ミリ秒）（例：1514836799000）。 |
| `{DATASET_ID}` | データセット識別子。 |
| `{SORT_BY}` | 指定された値で応答を並べ替えます。 例えば、作成日 `desc:created` を降順に並べ替えてオブジェクトを並べ替えます。 |

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

パラメーターとフィルターの完全なリストについては、 [Catalog APIリファレンスを参照してください](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/catalog.yaml)。

## 特定のリストに属するすべてのファイルのバッチを取得する

アクセスするバッチのIDが決まったら、Data Access APIを使用して、そのバッチに属するファイルのリストを取得できます。

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
| `data._links.self.href` | このファイルにアクセスするURL。 |

応答には、指定したバッチ内のすべてのファイルをリストするデータ配列が含まれます。 ファイルは、そのファイルIDによって参照されます。このファイルは、フィールドの下にあ `dataSetFileId` ります。

## ファイルIDを使用したファイルへのアクセス

一意のファイルIDを取得したら、Data Access APIを使用して、ファイルの名前、サイズ（バイト単位）、ダウンロード用のリンクなど、ファイルに関する特定の詳細にアクセスできます。

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

ファイルIDが個々のファイルを指すかディレクトリを指すかに応じて、返されるデータ配列には、そのディレクトリに属するファイルの1つのエントリまたはリストが含まれる場合があります。 各ファイル要素には、ファイル名、サイズ（バイト単位）、ファイルをダウンロードするためのリンクなどの詳細が含まれます。

**ケース1:ファイルIDは単一のファイルを指します**

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
| `_links.self.href` | ファイルをダウンロードするURL。 |

**ケース2:ファイルIDがディレクトリを指している**

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

この応答は、IDとを持つ2つの異なるファイルを含むディレクトリを返 `{FILE_ID_2}` しま `{FILE_ID_3}`す。 このシナリオでは、各ファイルにアクセスするには、各ファイルのURLに従う必要があります。

## ファイルのメタデータの取得

HEAD要求を行うことで、ファイルのメタデータを取得できます。 これは、サイズ（バイト単位）やファイル形式を含む、ファイルのメタデータヘッダを返します。

**API形式**

```http
HEAD /files/{FILE_ID}?path={FILE_NAME}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{FILE_ID}` | ファイルの識別子。 |
| `{FILE_NAME`} | ファイル名(例：プロファイル.parque) |

**リクエスト**

```shell
curl -I 'https://platform.adobe.io/data/foundation/export/files/8dcedb36-1cb2-4496-9a38-7b2041114b56-1?path=profiles.parquet' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

応答ヘッダーには、次のような、クエリーされたファイルのメタデータが含まれます。
- `Content-Length`:ペイロードのサイズをバイト単位で示します
- `Content-Type`:ファイルの種類を示します。

## ファイルのコンテンツへのアクセス

データアクセスAPIを使用して、ファイルのコンテンツにアクセスすることもできます。

**API形式**

```shell
GET /files/{FILE_ID}?path={FILE_NAME}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{FILE_ID}` | ファイルの識別子。 |
| `{FILE_NAME`} | ファイル名(例：プロファイル.パーケ)。 |

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

## ファイルの一部の内容のダウンロード

データアクセスAPIを使用すると、ファイルをチャンク単位でダウンロードできます。 範囲ヘッダーは、ファイルから特定のバイ `GET /files/{FILE_ID}` ト範囲をダウンロードする要求時に指定できます。 範囲を指定しない場合、APIはデフォルトでファイル全体をダウンロードします。

前の節のHEADの例では [](#retrieve-the-metadata-of-a-file) 、特定のファイルのサイズをバイト単位で示しています。

**API形式**

```http
GET /files/{FILE_ID}?path={FILE_NAME}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{FILE_ID} ` | ファイルの識別子。 |
| `{FILE_NAME}` | ファイル名(例：プロファイル.parque) |

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
| `Range: bytes=0-99` | ダウンロードするバイトの範囲を指定します。 指定しなかった場合、APIはファイル全体をダウンロードします。 この例では、最初の100バイトがダウンロードされます。 |

**応答**

応答本体には、（要求の「Range」ヘッダーで指定された）ファイルの最初の100バイトと、HTTPステータス206（部分的な内容）が含まれます。 応答には、次のヘッダーも含まれます。

- Content-Length:100（返されるバイト数）
- コンテンツタイプ：application/parket（パーケファイルが要求されたので、応答内容のタイプはパーケー）
- Content-Range:バイト0 ～ 99/249058(要求された範囲(0 ～ 99)、合計バイト数(249058)から

## API応答のページネーションの設定

データアクセスAPI内の応答はページ分割されます。 デフォルトでは、1ページあたりの最大エントリ数は100です。 ページングパラメーターを使用して、デフォルトの動作を変更できます。

- `limit`:「limit」パラメーターを使用して、必要に応じて1ページあたりのエントリ数を指定できます。
- `start`:オフセットは、「開始」パラメータで設定できます。
- `&`:アンパサンドを使用すると、1回の呼び出しで複数のパラメーターを組み合わせることができます。

**API形式**

```http
GET /batches/{BATCH_ID}/files?start={OFFSET}
GET /batches/{BATCH_ID}/files?limit={LIMIT}
GET /batches/{BATCH_ID}/files?start={OFFSET}&limit={LIMIT}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{BATCH_ID}` | アクセスしようとしているバッチのバッチ識別子。 |
| `{OFFSET}` | 結果配列を開始するインデックス(例：開始= 0) |
| `{LIMIT}` | 結果の配列に返される結果の数を制御します（例：limit=1）。 |

**リクエスト**

```shell
curl -X GET 'https://platform.adobe.io/data/foundation/export/batches/5c102cac7c7ebc14cd6b098e/files?start=0&limit=1' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**:

応答には、リクエストパ `"data"` ラメーターで指定された単一の要素を持つ配列が含まれま `limit=1`す。 この要素は、リクエストのパラメーターで指定された、最初に使用可能なファイルの詳細を含むオブジェクトです（0から始まる番号では、最初の要素は「0」になります）。 `start=0`

この値 `_links.next.href` には、次の回答ページへのリンクが含まれます。このリンクで、パラメーターの詳細が `start` わかります `start=1`。

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
