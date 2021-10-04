---
keywords: Experience Platform：ホーム;人気のトピック;データアクセス;python sdk;spark sdk;data access api；書き出し；書き出し
solution: Experience Platform
title: データアクセス API ガイド
topic-legacy: developer guide
description: データアクセス API は、Experience Platform内で取り込んだデータセットの検出性とアクセシビリティに重点を置いた RESTful インターフェイスを開発者に提供することで、Adobe Experience Platformをサポートします。
exl-id: 278ec322-dafa-4e3f-ae45-2d20459c5653
source-git-commit: 5160bc8057a7f71e6b0f7f2d594ba414bae9d8f6
workflow-type: tm+mt
source-wordcount: '524'
ht-degree: 85%

---

# データアクセス API ガイド

データアクセス API は、[!DNL Experience Platform] 内で取り込んだデータセットの検出性とアクセシビリティに重点を置いた RESTful インターフェイスをユーザーに提供することで、Adobe Experience Platformをサポートします。

![Experience Platform でのデータアクセス](images/Data_Access_Experience_Platform.png)

## API 仕様リファレンス

Swagger API リファレンスドキュメントは、[こちら](https://www.adobe.io/experience-platform-apis/references/data-access/)を参照してください。

## 用語

このドキュメントでよく使用される用語の説明。

| 用語 | 説明 |
| ----- | ------------ |
| データセット | スキーマとフィールドを含むデータのコレクション。 |
| バッチ | 一定期間に収集され、1 つの単位として処理される一連のデータ。 |

## バッチ内のファイルのリストの取得

バッチ識別子（batchID）を使用すると、データアクセス API は特定のバッチに属するファイルのリストを取得できます。

**API 形式**

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

`"data"` 配列には、指定したバッチ内のすべてのファイルのリストが含まれています。返されるファイルごとに、`"dataSetFileId"` フィールド内に固有の ID（`{FILE_ID}`）が含まれています。この一意のID を使用して、ファイルにアクセスしたり、ダウンロードしたりできます。

| プロパティ | 説明 |
| -------- | ----------- |
| `data.dataSetFileId` | 指定したバッチ内の各ファイルのファイル ID。 |
| `data._links.self.href` | ファイルにアクセスするための URL。 |

## バッチ内のファイルへのアクセスとダウンロード

ファイル識別子（`{FILE_ID}`）を使用すると、データアクセス API で、ファイルの名前、バイト単位のサイズ、ダウンロードするためのリンクなど、ファイルの詳細にアクセスできます。

応答にはデータ配列が含まれています。ID で指すファイルが個々のファイルかディレクトリかに応じて、返されるデータ配列には、1 つのエントリか、そのディレクトリに属するファイルのリストが含まれます。各ファイル要素には、ファイルの詳細が含まれています。

**API 形式**

```http
GET /files/{FILE_ID}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{FILE_ID}` | アクセスするファイルの ID です（`"dataSetFileId"` に等しい）。 |

**リクエスト**

```shell
curl -X GET https://platform.adobe.io/data/foundation/export/files/{FILE_ID} \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**単一ファイル応答**

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
| `data.name` | ファイルの名前（profiles.csv など）です。 |
| `data.length` | ファイルのサイズ（バイト単位）です。 |
| `data._links.self.href` | ファイルをダウンロードするための URL です。 |

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

ディレクトリが返される場合、そのディレクトリ内のすべてのファイルの配列が含まれています。

| プロパティ | 説明 |
| -------- | ----------- |
| `data.name` | ファイルの名前（profiles.csv など）です。 |
| `data._links.self.href` | ファイルをダウンロードするための URL です。 |

## ファイルのコンテンツへのアクセス

[!DNL Data Access] API を使用して、ファイルのコンテンツにアクセスすることもできます。 これを使用して、コンテンツを外部ソースにダウンロードできます。

**API 形式**

```http
GET /files/{dataSetFileId}?path={FILE_NAME}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{FILE_NAME}` | アクセスしようとしているファイルの名前です。 |

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
| `{FILE_ID}` | データセット内のファイルの ID です。 |
| `{FILE_NAME}` | ファイルの完全名（例：profiles.csv）。 |

**応答**

`Contents of the file`

## その他のコードサンプル

その他のサンプルについては、[データアクセスのチュートリアル](tutorials/dataset-data.md)を参照してください。

## データ取得イベントへのサブスクライブ

[!DNL Platform] は、 [Adobe Developer Console](https://www.adobe.com/go/devs_console_ui) を介して特定の価値の高いイベントをサブスクリプションで利用できるようにします。例えば、データ取り込みイベントに登録して、遅延や障害の可能性についての通知を受け取ることができます。詳しくは、[データ取得通知のサブスクライブ](../ingestion/quality/subscribe-events.md)のチュートリアルを参照してください。
