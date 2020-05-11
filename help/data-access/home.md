---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: データアクセスの概要
topic: overview
translation-type: tm+mt
source-git-commit: d9aa21a7439a6c40f6f51dfbdf5c7b3690c4593a
workflow-type: tm+mt
source-wordcount: '488'
ht-degree: 4%

---


# データアクセスの概要

Data Access APIは、Experience Platform内で取り込まれたデータセットの検出性とアクセシビリティに重点を置いたRESTfulインターフェイスをユーザーに提供することで、Adobe Experience Platformをサポートします。

![エクスペリエンスプラットフォームでのデータアクセス](images/Data_Access_Experience_Platform.png)

## API仕様リファレンス

Swagger APIリファレンスドキュメントは [こちら](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/data-access-api.yaml)。

## 用語

このドキュメントでよく使用される用語の一部を紹介します。

| 用語 | 説明 |
| ----- | ------------ |
| データセット | スキーマとフィールドを含むデータのコレクションです。 |
| バッチ | 一定期間に収集され、1つのユニットとして一緒に処理される一連のデータ。 |

## バッチ内のファイルのリストを取得

バッチ識別子(batchID)を使用すると、Data Access APIは特定のバッチに属するファイルのリストを取得できます。

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

この `"data"` 配列には、指定したバッチ内のすべてのファイルのリストが含まれます。 返される各ファイルには、フィールド内に含まれる固有のID(`{FILE_ID}`)があり `"dataSetFileId"` ます。 その後、この一意のIDを使用して、ファイルにアクセスしたり、ダウンロードしたりできます。

| プロパティ | 説明 |
| -------- | ----------- |
| `data.dataSetFileId` | 指定したバッチ内の各ファイルのファイルID。 |
| `data._links.self.href` | ファイルにアクセスするURLです。 |

## バッチ内のファイルへのアクセスとダウンロード

ファイル識別子(`{FILE_ID}`)を使用すると、Data Access APIを使用して、ファイル名、バイト単位のサイズ、ダウンロードするリンクなど、ファイルの特定の詳細にアクセスできます。

応答にはデータ配列が含まれます。 IDで指すファイルが個別のファイルかディレクトリかに応じて、返されるデータ配列には、そのディレクトリに属するファイルのリストが1つだけ含まれる場合もあります。 各ファイル要素には、ファイルの詳細が含まれます。

**API形式**

```http
GET /files/{FILE_ID}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{FILE_ID}` | に等し `"dataSetFileId"`い。アクセスするファイルのID。 |

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
| `data.name` | ファイルの名前(例：プロファイル.csv)。 |
| `data.length` | ファイルのサイズ（バイト単位）。 |
| `data._links.self.href` | ファイルをダウンロードするURLです。 |

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
| `data.name` | ファイルの名前(例：プロファイル.csv)。 |
| `data._links.self.href` | ファイルをダウンロードするURLです。 |

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

その他のサンプルについては、 [データアクセスのチュートリアルを参照してください](tutorials/dataset-data.md)。

## データ取り込みイベントのサブスクライブ

プラットフォームは、 [Adobe Developer Consoleを通じて特定の高価値イベントを購読に使用できるようにします](https://www.adobe.com/go/devs_console_ui)。 例えば、データ取り込みイベントを登録して、遅延や失敗の可能性を通知することができます。 詳しくは、データインジェスト通知の [サブスクライブに関するチュートリアル](../ingestion/quality/subscribe-events.md) を参照してください。