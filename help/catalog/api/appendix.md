---
keywords: Experience Platform；ホーム；人気のあるトピック；カタログサービス；カタログ api；付録
solution: Experience Platform
title: カタログサービス API ガイドの付録
topic-legacy: developer guide
description: このドキュメントには、Adobe Experience Platformでカタログ API を使用する際に役立つ追加情報が含まれています。
exl-id: fafc8187-a95b-4592-9736-cfd9d32fd135
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '920'
ht-degree: 75%

---

# [!DNL Catalog Service] API ガイドの付録

このドキュメントには、[!DNL Catalog] API を使用する際に役立つ追加情報が含まれています。

## 相互に関連するオブジェクトの表示 {#view-interrelated-objects}

一部の [!DNL Catalog] オブジェクトは、他の [!DNL Catalog] オブジェクトと相互に関連付けることができます。 応答ペイロードの先頭に `@` プリフィックスが付いたフィールドは、関連オブジェクトを表します。これらのフィールドの値は URI の形式をとります。それらが表す関連オブジェクトは、個別の GET リクエストで取得できます。

[特定のデータセットの検索](look-up-object.md)時にドキュメントに返されるデータセットの例には、URI 値 `"@/dataSets/5ba9452f7de80400007fc52a/views/5ba9452f7de80400007fc52b/files"` を持つ `files` フィールドが含まれています。この URI を新しい GET リクエストのパスとして使用すると、`files` フィールドの内容を表示できます。

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
  'https://platform.adobe.io/data/foundation/catalog/dataSets/5ba9452f7de80400007fc52a/views/5ba9452f7de80400007fc52b/files' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
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
        "imsOrg": "{IMS_ORG}",
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
        "imsOrg": "{IMS_ORG}",
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
        "imsOrg": "{IMS_ORG}",
        "createdUser": "{USER_ID}",
        "createdClient": "{CLIENT_ID}",
        "updatedUser": "{USER_ID}",
        "version": "1.0.0",
        "created": 1569499425037,
        "updated": 1569499425037
    }
}
```

## 1 回の呼び出しでの複数リクエスト

[!DNL Catalog] API のルートエンドポイントを使用すると、1 回の呼び出しで複数のリクエストを実行できます。 リクエストのペイロードには、通常は個々のリクエストを表すオブジェクトの配列が含まれており、順番に実行されます。

これらのリクエストが [!DNL Catalog] に対する変更または追加で、いずれかの変更が失敗した場合、すべての変更が元に戻ります。

**API 形式**

```http
POST /
```

**リクエスト**

次のリクエストでは、新しいデータセットを作成し、そのデータセットに関連する表示を作成します。この例は、テンプレート言語を使用して、以前の呼び出しで返された値にアクセスし、後続の呼び出しで使用する方法を示しています。

例えば、前のサブリクエストから返された値を参照する場合、`<<{REQUEST_ID}.{ATTRIBUTE_NAME}>>` の形式で参照を作成できます。ここで `{REQUEST_ID}` は、以下に示すように、サブリクエストのユーザー指定 ID です。これらのテンプレートを使用して、前のサブリクエストの応答オブジェクトの本文で使用可能な任意の属性を参照できます。

>[!NOTE]
>
> 実行されたサブリクエストがオブジェクトへの参照のみを返す場合（カタログ API のほとんどの POST リクエストと PUT リクエストのデフォルト）、この参照は `id` 値にエイリアスされ、`<<{OBJECT_ID}.id>>` として使用できます 。

```shell
curl -X POST \
  https://platform.adobe.io/data/foundation/catalog \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '[
    {
      "id": "firstObjectId",
      "resource": "/dataSets",
      "method": "post",
      "body": {
        "type": "raw",
        "name": "First Dataset"
      }
    }, 
    {
      "id": "secondObjectId",
      "resource": "/datasetViews",
      "method": "post",
      "body": {
        "status": "enabled",
        "dataSetId": "<<firstObjectId.id>>"
      }
    }
  ]'
```

| プロパティ | 説明 |
| --- | --- |
| `id` | 応答にリクエストを照合できるように、応答オブジェクトに添付されるユーザー指定の ID。[!DNL Catalog] はこの値を格納せず、単に参照用に応答で返します。 |
| `resource` | [!DNL Catalog] API のルートに対する相対リソースパス。 プロトコルとドメインはこの値の一部ではなく、「/」というプリフィックスを付ける必要があります。<br/><br/> サブリクエストの `method` として PATCH または DELETE を使用する場合は、リソースパスにオブジェクト ID を含めます。ユーザーが指定した `id` と混同しないように、リソースパスは [!DNL Catalog] オブジェクト自体の ID（例えば `resource: "/dataSets/1234567890"`）を使用します。 |
| `method` | リクエストでとられるアクションに関連するメソッド（GET、PUT、POST、PATCH、DELETE）の名前。 |
| `body` | 通常、POST、PUT、PATCH リクエストのペイロードとして渡される JSON ドキュメント。このプロパティは、GET または DELETE リクエストには必要ありません。 |

**応答**

成功した応答は、各リクエストに割り当てた `id`、個々のリクエストの HTTP ステータスコード、および応答 `body` を含むオブジェクトの配列を返します。3 つのリクエスト例はすべて新しいオブジェクトを作成するものなので、各オブジェクトの `body` は、新しく作成されたオブジェクトの ID のみを含む配列であり、[!DNL Catalog] でのPOST応答で最も成功した標準です。

```json
[
    {
        "id": "firstObjectId",
        "code": 200,
        "body": [
            "@/dataSets/5be230aef5b02914cd52dbfa"
        ]
    },
    {
        "id": "secondObjectId",
        "code": 200,
        "body": [
            "@/dataSetViews/5be230aef5b02914cd52dbfb"
        ]
    }
]
```

複数のリクエストに対する応答を検査する際は、各サブリクエストのコードを検証し、親の POST リクエストの HTTP ステータスコードだけに依存しないように注意してください。  単一のサブリクエストが 404（無効なリソースに対する GET リクエストなど）を返す一方、リクエスト全体が 200 を返す可能性があります。

## 追加のリクエストヘッダー

[!DNL Catalog] には、更新時にデータの整合性を維持するのに役立つ、いくつかのヘッダー規則が用意されています。

### If-Match

複数のユーザーがほぼ同時にオブジェクトを保存する場合に発生するデータ破損の種類を防ぐには、オブジェクトのバージョン管理を使用することをお勧めします。

オブジェクトを更新する際のベストプラクティスは、まず、更新するオブジェクトを表示するための API 呼び出し（GET）を実行することです。応答（および応答に単一のオブジェクトが含まれる任意の呼び出し）内に含まれるのは、オブジェクトのバージョンを含む `E-Tag` ヘッダーです。更新（PUT または PATCH）呼び出しでオブジェクトバージョンを `If-Match` という名前のリクエストヘッダーとして追加すると、バージョンがまだ同じ場合にのみ更新が成功し、データの衝突を防ぐのに役立ちます。

バージョンが一致しない場合（オブジェクトを取得してからオブジェクトが別のプロセスによって変更されている場合）、ターゲットリソースへのアクセスが拒否されたことを示す HTTP ステータス 412（前提条件の失敗）を受け取ります。

### Pragma

場合によっては、情報を保存せずにオブジェクトを検証する必要があります。`Pragma` ヘッダーを `validate-only` の値で使用すると、検証目的でのみ POST または PUT リクエストを送信でき、データに対する変更が保持されません。

## データの圧縮

圧縮は、データを変更せずに小さなファイルのデータを大きなファイルに結合する [!DNL Experience Platform] サービスです。 パフォーマンス上の理由から、小さなファイルのセットを大きなファイルに組み合わせて、クエリを実行する際にデータへのアクセスを高速化する方が有益な場合があります。

取り込まれたバッチ内のファイルが圧縮されると、関連する [!DNL Catalog] オブジェクトが監視目的で更新されます。
