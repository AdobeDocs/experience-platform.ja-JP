---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: カタログサービス開発者ガイドの付録
topic: developer guide
translation-type: tm+mt
source-git-commit: 4b2524fcfda578d0debb89623bec6edb537db852

---


# カタログサービス開発者ガイドの付録

このドキュメントには、カタログAPIの操作に役立つ追加情報が含まれています。

## 表示相互関連オブジェクト {#view-interrelated-objects}

一部のCatalogオブジェクトは、他のCatalogオブジェクトと相互に関連付けることができます。 応答ペイロードの先頭にプリフィックスが付い `@` たフィールドは、関連するオブジェクトを表します。 これらのフィールドの値はURIの形式をとります。URIは、それらが表す関連オブジェクトを取得するために別のGETリクエストで使用できます。

特定のデータセットの検索時にドキュメントに返さ [れるデータセットの例には](look-up-object.md) 、次のURI値を `files` 持つフィールドが含まれています。 `"@/dataSets/5ba9452f7de80400007fc52a/views/5ba9452f7de80400007fc52b/files"`. このURIを新しいGET `files` リクエストのパスとして使用すると、フィールドの内容を表示できます。

**API形式**

```http
GET {OBJECT_URI}
```

| パラメーター | 説明 |
| --- | --- |
| `{OBJECT_URI}` | 相互に関連するオブジェクトフィールド（シンボルを除く）によって提供 `@` されるURI。 |

**リクエスト**

次のリクエストでは、データセットの例のプロパティが提供され `files` たURIを使用して、データセットに関連付けられたファイルのリストを取得します。

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/catalog/dataSets/5ba9452f7de80400007fc52a/views/5ba9452f7de80400007fc52b/files' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、関連オブジェクトのリストを返します。 この例では、データセットファイルのリストが返されます。

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

## 1回の呼び出しで複数のリクエストを行う

カタログAPIのルートエンドポイントを使用すると、1回の呼び出しで複数のリクエストを作成できます。 要求ペイロードには、通常は個々の要求であるものを表すオブジェクトの配列が含まれ、その後順に実行されます。

これらのリクエストが変更またはカタログへの追加で、いずれかの変更が失敗した場合、すべての変更が元に戻されます。

**API形式**

```http
POST /
```

**リクエスト**

次のリクエストでは、新しいデータセットを作成し、そのデータセットに関連する表示を作成します。 この例は、テンプレート言語を使用して、以前の呼び出しで返された値にアクセスし、後続の呼び出しで使用する方法を示しています。

例えば、前のサブリクエストから返された値を参照する場合、次の形式で参照を作成できます。(ここ `<<{REQUEST_ID}.{ATTRIBUTE_NAME}>>``{REQUEST_ID}` では、以下に示すように、サブリクエストのユーザー指定IDです)。 これらのテンプレートを使用して、前のサブリクエストの応答オブジェクトの本文で使用可能な任意の属性を参照できます。

>[!NOTE] 実行されたサブリクエストがオブジェクトへの参照のみを返す場合（カタログAPIのほとんどのPOSTリクエストとPUTリクエストのデフォルト）、この参照は値にエイリアスされ、として `id` 使用できます `<<{OBJECT_ID}.id>>`。

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
        "aspect": "production",
        "dataSetId": "<<firstObjectId.id>>"
      }
    }
  ]'
```

| プロパティ | 説明 |
| --- | --- |
| `id` | 応答にリクエストを照合できるように、応答オブジェクトに添付されるユーザー指定のID。 カタログにはこの値が格納されず、参照用に応答に返されます。 |
| `resource` | カタログAPIのルートを基準とするリソースパス。 プロトコルとドメインはこの値の一部ではなく、「/」というプリフィックスを付ける必要があります。 <br/><br/> サブリクエストとしてPATCHまたはDELETEを使用する場合は、リ `method`ソースパスにオブジェクトIDを含めます。 ユーザーが指定したIDと混同しないように、リソ `id`ースパスはCatalogオブジェクト自体のID(例えば、 `resource: "/dataSets/1234567890"`)を使用します。 |
| `method` | リクエストで行われるアクションに関連するメソッド（GET、PUT、POST、PATCH、またはDELETE）の名前。 |
| `body` | 通常、POST、PUTまたはPATCHリクエストのペイロードとして渡されるJSONドキュメント。 このプロパティは、GETまたはDELETE要求には必要ありません。 |

**応答**

成功した応答は、各要求に割り当てたオブジェク `id` ト、個々の要求のHTTPステータスコード、および応答を含むオブジェクトの配列を返します `body`。 3つのサンプルリクエストはすべて新しいオブジェクトを作成するものなので、各オブジェクトの配列は、新しく作成されたオブジェクトの `body` IDのみを含む配列で、カタログで最も成功したPOST応答の標準と同様です。

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

複数のリクエストに対する応答を検査する際は、各サブリクエストのコードを検証し、親のPOSTリクエストのHTTPステータスコードだけに依存しないように注意してください。  単一のサブリクエストが404（無効なリソースに対するGETリクエストなど）を返す一方、リクエスト全体が200を返す可能性があります。

## 追加のリクエストヘッダー

カタログには、更新時にデータの整合性を維持するためのヘッダ規則がいくつか用意されています。

### If-Match

複数のユーザーが同時にオブジェクトを保存する場合に発生するデータ破損の種類を防ぐには、オブジェクトのバージョン管理を使用することをお勧めします。

オブジェクトを更新する際のベストプラクティスは、最初に表示(GET)へのAPI呼び出しを更新することです。 応答（および応答に単一のオブジェクトが含まれる任意の呼び出し）内に含まれるのは、オブジェ `E-Tag` クトのバージョンを含むヘッダーです。 更新（PUTまたはPATCH）の呼び出しで名前が付いたリクエストヘッダーとしてオブジェクトのバージョンを追加すると、そのバージョンが同じ場合にのみ更新が成功し、データの衝突を防ぐのに役立ちます。 `If-Match`

バージョンが一致しない場合（オブジェクトを取得した後に別のプロセスによって変更された場合）、ターゲットリソースへのアクセスが拒否されたことを示すHTTPステータス412（前提条件が失敗）を受け取ります。

### Pragma

場合によっては、情報を保存せずにオブジェクトを検証する必要があります。 の値でヘ `Pragma` ッダーを使用す `validate-only` ると、検証目的でのみPOSTまたはPUTリクエストを送信でき、データに対する変更が保持されません。

## データの圧縮

圧縮は、データを変更せずに小さいファイルのデータを大きいファイルに結合するExperience Platformサービスです。 パフォーマンス上の理由から、小さなファイルのセットを大きなファイルに組み合わせて、クエリーを行う際のデータへのアクセスを高速化する方が有益な場合があります。

取り込んだバッチ内のファイルが圧縮されると、関連するCatalogオブジェクトが更新され、監視のために使用されます。