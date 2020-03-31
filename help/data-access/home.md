---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: データアクセスの概要
topic: overview
translation-type: tm+mt
source-git-commit: 4817162fe2b7cbf4ae4c1ed325db2af31da5b5d3

---


# データアクセスの概要

Data Access APIは、Adobe Experience Platformをサポートしています。Experience Platform内で取り込んだデータセットの検出性とアクセシビリティに重点を置いたRESTfulインターフェイスをユーザーに提供します。

![エクスペリエンスプラットフォームでのデータアクセス](images/Data_Access_Experience_Platform.png)

## API仕様リファレンス

Swagger APIリファレンスドキュメントはこちらを参照して [ください](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/data-access-api.yaml)。

## 用語

このドキュメントでよく使用される用語の説明。

| 用語 | 説明 |
| ----- | ------------ |
| データセット | データとフィールドを含むスキーマのコレクション。 |
| バッチ | 一定期間に収集され、1つの単位として処理される一連のデータ。 |

## バッチ内のリストのファイルの取得

バッチ識別子(batchID)を使用すると、データアクセスAPIは特定のバッチに属するファイルのリストを取得できます。

**API形式**

```http
GET /batches/{BATCH_ID}/files
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{BATCH_ID}` | 指定したバッチのID。 |

**リクエスト**

```shell
curl -X GET https://platform.adobe.io/data/foundation/export/batches/{BATCH_ID}/files \
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
      "dataSetFileId": "{FILE_ID_1}",
      "dataSetViewId": "string",
      "version": "1.0.0",
      "created": "string",
      "updated": "string",
      "isValid": true,
      "_links": {
        "self": {
          "href": "https://platform.adobe.io/data/foundation/export/files/{FILE_ID_1}"
        }
      }
    },
    {
      "dataSetFileId": "{FILE_ID_2}",
      "dataSetViewId": "string",
      "version": "1.0.0",
      "created": "string",
      "updated": "string",
      "isValid": true,
      "_links": {
        "self": {
          "href": "https://platform.adobe.io/data/foundation/export/files/{FILE_ID_2}"
        }
      }
    },
  ],
  "_page": {
    "limit": 100,
    "count": 1
  }
}
```

配列に `"data"` は、指定したバッチ内のすべてのリストが含まれます。 返される各ファイルには、フィールド内に含まれる固有の`{FILE_ID}`ID()が含ま `"dataSetFileId"` れます。 この一意のIDを使用して、ファイルにアクセスしたり、ダウンロードしたりできます。

| プロパティ | 説明 |
| -------- | ----------- |
| `data.dataSetFileId` | 指定したバッチ内の各ファイルのファイルID。 |
| `data._links.self.href` | ファイルにアクセスするURL。 |

## バッチ内のファイルへのアクセスとダウンロード

ファイル識別子(`{FILE_ID}`)を使用すると、Data Access APIを使用して、ファイルの名前、バイト単位のサイズ、ダウンロードするリンクなど、ファイルの特定の詳細にアクセスできます。

応答にはデータ配列が含まれます。 IDで指すファイルが個々のファイルかディレクトリかに応じて、返されるデータ配列には、そのディレクトリに属するファイルの1つのエントリかリストが含まれる場合があります。 各ファイル要素には、ファイルの詳細が含まれます。

**API形式**

```http
GET /files/{FILE_ID}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{FILE_ID}` | に等しい。 `"dataSetFileId"`アクセスするファイルのID。 |

**リクエスト**

```shell
curl -X GET https://platform.adobe.io/data/foundation/export/files/{FILE_ID} \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**単一ファイルの応答**

```JSON
{
  "data": [
    {
      "name": "{FILE_NAME}",
      "length": "{LENGTH}",
      "_links": {
        "self": {
          "href": "https://platform.adobe.io/data/foundation/export/files/{FILE_ID}?path={FILE_NAME}"
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
| `data.name` | ファイルの名前(プロファイル.csvなど)。 |
| `data.length` | ファイルのサイズ（バイト単位）。 |
| `data._links.self.href` | ファイルをダウンロードするURL。 |

**ディレクトリ応答**

```JSON
{
  "data": [
    {
      "dataSetFileId": "{FILE_ID_1}",
      "dataSetViewId": "string",
      "version": "1.0.0",
      "created": "string",
      "updated": "string",
      "isValid": true,
      "_links": {
        "self": {
          "href": "https://platform.adobe.io/data/foundation/export/files/{FILE_ID_1}"
        }
      }
    },
    {
      "dataSetFileId": "{FILE_ID_2}",
      "dataSetViewId": "string",
      "version": "1.0.0",
      "created": "string",
      "updated": "string",
      "isValid": true,
      "_links": {
        "self": {
          "href": "https://platform.adobe.io/data/foundation/export/files/{FILE_ID_2}"
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

ディレクトリが返されると、そのディレクトリ内のすべてのファイルの配列が含まれます。

| プロパティ | 説明 |
| -------- | ----------- |
| `data.name` | ファイルの名前(プロファイル.csvなど)。 |
| `data._links.self.href` | ファイルをダウンロードするURL。 |

## ファイルのコンテンツへのアクセス

データアクセスAPIは、ファイルのコンテンツにアクセスする場合にも使用できます。 これを使用して、コンテンツを外部ソースにダウンロードできます。

**API形式**

```http
GET /files/{dataSetFileId}?path={FILE_NAME}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{FILE_NAME}` | アクセスしようとしているファイルの名前。 |

**リクエスト**

```shell
curl -X GET https://platform.adobe.io/data/foundation/export/files/{FILE_ID}?path={FILE_NAME} \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{FILE_ID}` | データセット内のファイルのID。 |
| `{FILE_NAME}` | ファイルのフルネーム(例：プロファイル.csv)。 |

**応答**

```
Contents of the file
```

## その他のコードサンプル

その他のサンプルについては、データアクセスチュートリ [アルを参照してくださ](tutorials/dataset-data.md)い。


## データ取り込みイベント

プラットフォームは、特定の高価値イベントを [Adobe I/Oコンソールを通じて購読に利用できるようにします](https://console.adobe.io/)。 例えば、データ取り込みイベントをサブスクライブして、遅延や障害の可能性を通知できます。 Adobe I/Oイベントの使用に関する詳細は、入門ガイドを参照し [てください](https://www.adobe.io/apis/experienceplatform/events/docs.html)。