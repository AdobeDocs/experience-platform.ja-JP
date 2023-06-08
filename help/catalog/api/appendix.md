---
keywords: Experience Platform；ホーム；人気の高いトピック；カタログサービス；カタログ api；付録
solution: Experience Platform
title: カタログサービス API ガイドの付録
description: このドキュメントには、Adobe Experience Platformでカタログ API を使用する際に役立つ追加情報が含まれています。
exl-id: fafc8187-a95b-4592-9736-cfd9d32fd135
source-git-commit: 24db94b959d1bad925af1e8e9cbd49f20d9a46dc
workflow-type: tm+mt
source-wordcount: '458'
ht-degree: 77%

---

# [!DNL Catalog Service] API ガイドの付録

このドキュメントでは、 [!DNL Catalog] API

## 相互に関連するオブジェクトの表示 {#view-interrelated-objects}

一部 [!DNL Catalog] オブジェクトは他のオブジェクトと相互に関連付けることができます [!DNL Catalog] オブジェクト。 応答ペイロードの先頭に `@` プリフィックスが付いたフィールドは、関連オブジェクトを表します。これらのフィールドの値は URI の形式をとります。それらが表す関連オブジェクトは、個別の GET リクエストで取得できます。

[特定のデータセットの検索](look-up-object.md)時にドキュメントに返されるデータセットの例には、URI 値 `"@/datasetFiles?datasetId={DATASET_ID}"` を持つ `files` フィールドが含まれています。この URI を新しい GET リクエストのパスとして使用すると、`files` フィールドの内容を表示できます。

**API 形式**

```http
GET {OBJECT_URI}
```

| パラメーター | 説明 |
| --- | --- |
| `{OBJECT_URI}` | 相互に関連するオブジェクトフィールド（`@` シンボルを除く）によって提供される URI。 |

**リクエスト**

次のリクエストでは、例にあるデータセットの `files` プロパティが提供する URI を使用して、データセットに関連付けられたファイルのリストを取得します。

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/catalog/dataSets/datasetFiles?datasetId={DATASET_ID}' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、関連オブジェクトのリストを返します。この例では、データセットファイルのリストが返されます。

```json
{
    "7d501090-0280-11ea-a6bb-f18323b7005c-1": {
        "id": "7d501090-0280-11ea-a6bb-f18323b7005c-1",
        "batchId": "7d501090-0280-11ea-a6bb-f18323b7005c",
        "dataSetViewId": "5ba9452f7de80400007fc52b",
        "imsOrg": "{ORG_ID}",
        "createdUser": "{USER_ID}",
        "createdClient": "{CLIENT_ID}",
        "updatedUser": "{USER_ID}",
        "version": "1.0.0",
        "created": 1573256315368,
        "updated": 1573256315368
    },
    "148ac690-0280-11ea-8d23-8571a35dce49-1": {
        "id": "148ac690-0280-11ea-8d23-8571a35dce49-1",
        "batchId": "148ac690-0280-11ea-8d23-8571a35dce49",
        "dataSetViewId": "5ba9452f7de80400007fc52b",
        "imsOrg": "{ORG_ID}",
        "createdUser": "{USER_ID}",
        "createdClient": "{CLIENT_ID}",
        "updatedUser": "{USER_ID}",
        "version": "1.0.0",
        "created": 1573255982433,
        "updated": 1573255982433
    },
    "64dd5e19-8ea4-4ddd-acd1-f43cccd8eddb-1": {
        "id": "64dd5e19-8ea4-4ddd-acd1-f43cccd8eddb-1",
        "batchId": "64dd5e19-8ea4-4ddd-acd1-f43cccd8eddb",
        "dataSetViewId": "5ba9452f7de80400007fc52b",
        "imsOrg": "{ORG_ID}",
        "createdUser": "{USER_ID}",
        "createdClient": "{CLIENT_ID}",
        "updatedUser": "{USER_ID}",
        "version": "1.0.0",
        "created": 1569499425037,
        "updated": 1569499425037
    }
}
```

## 追加のリクエストヘッダー

[!DNL Catalog] には、更新時にデータの整合性を維持するのに役立つ、いくつかのヘッダー規則が用意されています。

### If-Match

複数のユーザーがほぼ同時にオブジェクトを保存する場合に発生するデータ破損の種類を防ぐには、オブジェクトのバージョン管理を使用することをお勧めします。

オブジェクトを更新する際のベストプラクティスは、まず、更新するオブジェクトを表示するための API 呼び出し（GET）を実行することです。応答（および応答に単一のオブジェクトが含まれる任意の呼び出し）内に含まれるのは、オブジェクトのバージョンを含む `E-Tag` ヘッダーです。更新（PUT または PATCH）呼び出しでオブジェクトバージョンを `If-Match` という名前のリクエストヘッダーとして追加すると、バージョンがまだ同じ場合にのみ更新が成功し、データの衝突を防ぐのに役立ちます。

バージョンが一致しない場合（オブジェクトを取得してからオブジェクトが別のプロセスによって変更されている場合）、ターゲットリソースへのアクセスが拒否されたことを示す HTTP ステータス 412（前提条件の失敗）を受け取ります。

### Pragma

場合によっては、情報を保存せずにオブジェクトを検証する必要があります。`Pragma` ヘッダーを `validate-only` の値で使用すると、検証目的でのみ POST または PUT リクエストを送信でき、データに対する変更が保持されません。

## データの圧縮

圧縮は [!DNL Experience Platform] データを変更せずに小さいファイルのデータを大きいファイルに結合するサービス。 パフォーマンス上の理由から、小さなファイルのセットを大きなファイルに組み合わせて、クエリを実行する際にデータへのアクセスを高速化する方が有益な場合があります。

取り込まれたバッチ内のファイルが圧縮されると、関連付けられた [!DNL Catalog] オブジェクトが、監視目的で更新されます。
