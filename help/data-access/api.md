---
keywords: Experience Platform；ホーム；人気のトピック；データアクセス；python sdk;spark sdk；データアクセス api;export;Export
solution: Experience Platform
title: データアクセス API ガイド
description: データアクセス API は、Adobe Experience Platformをサポートし、Experience Platform内に取り込んだデータセットの検出性とアクセシビリティに重点を置いた RESTful インターフェイスを開発者に提供します。
exl-id: 278ec322-dafa-4e3f-ae45-2d20459c5653
source-git-commit: d8694c094ae4a7284e4a3ed0ae5bc3dc198e501a
workflow-type: tm+mt
source-wordcount: '524'
ht-degree: 49%

---

# データアクセス API ガイド

データアクセス API は、Adobe Experience Platformをサポートし、内に取り込んだデータセットの検出性とアクセシビリティに重点を置いた RESTful インターフェイスをユーザーに提供します [!DNL Experience Platform].

![データアクセスを使用すると、取り込んだデータセットをExperience Platform内で検出し、アクセシビリティを容易にする方法を示す図です。](images/Data_Access_Experience_Platform.png)

## API 仕様リファレンス

Swagger API リファレンスドキュメントは、[こちら](https://developer.adobe.com/experience-platform-apis/references/data-access/)を参照してください。

## 用語 {#terminology}

次の表に、このドキュメントでよく使用される用語の説明を示します。

| 用語 | 説明 |
| ----- | ------------ |
| データセット | スキーマとフィールドを含むデータのコレクションです。 |
| バッチ | 一定期間に収集され、1 つの単位として処理される一連のデータ。 |

## バッチ内のファイルのリストの取得 {#retrieve-list-of-files-in-a-batch}

特定のバッチに属するファイルのリストを取得するには、データアクセス API でバッチ識別子 (batchID) を使用します。

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
  -H 'x-gw-ims-org-id: {ORG_ID}' \
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

`"data"` 配列には、指定したバッチ内のすべてのファイルのリストが含まれています。返されるファイルごとに、`"dataSetFileId"` フィールド内に固有の ID（`{FILE_ID}`）が含まれています。この一意の ID を使用して、ファイルにアクセスしたり、ファイルをダウンロードしたりできます。

| プロパティ | 説明 |
| -------- | ----------- |
| `data.dataSetFileId` | 指定したバッチ内の各ファイルのファイル ID。 |
| `data._links.self.href` | ファイルにアクセスするための URL。 |

## バッチ内のファイルへのアクセスとダウンロード

ファイルの特定の詳細にアクセスするには、ファイル識別子 (`{FILE_ID}`) を Data Access API に置き換えます。

応答にはデータ配列が含まれています。 ID で指すファイルが個々のファイルかディレクトリかに応じて、返されるデータ配列には、1 つのエントリか、そのディレクトリに属するファイルのリストが含まれます。各ファイル要素には、ファイルの詳細が含まれます。

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
  -H 'x-gw-ims-org-id: {ORG_ID}' \
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
| `data.name` | ファイルの名前 ( 例： `profiles.csv`) をクリックします。 |
| `data.length` | ファイルのサイズ（バイト単位）。 |
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
| `data.name` | ファイルの名前 ( 例： `profiles.csv`) をクリックします。 |
| `data._links.self.href` | ファイルをダウンロードするための URL です。 |

## ファイルのコンテンツへのアクセス {#access-file-contents}

また、 [!DNL Data Access] ファイルのコンテンツにアクセスする API。 その後、コンテンツを外部ソースにダウンロードできます。

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
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{FILE_ID}` | データセット内のファイルの ID です。 |
| `{FILE_NAME}` | ファイルの完全名 ( 例： `profiles.csv`) をクリックします。 |

**応答**

`Contents of the file`

## その他のコードサンプル

その他のサンプルについては、 [データアクセスのチュートリアル](tutorials/dataset-data.md).

## データ取得イベントへのサブスクライブ {#subscribe-to-data-ingestion-events}

を通じて、価値の高い特定のイベントに登録できます。 [Adobe Developer Console](https://developer.adobe.com/console/). 例えば、データ取り込みイベントに登録して、遅延や障害の可能性についての通知を受け取ることができます。詳しくは、[データ取得通知のサブスクライブ](../ingestion/quality/subscribe-events.md)のチュートリアルを参照してください。
