---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Catalog Service開発者ガイドの付録
topic: developer guide
translation-type: tm+mt
source-git-commit: bd9884a24c5301121f30090946ab24d9c394db1b
workflow-type: tm+mt
source-wordcount: '908'
ht-degree: 0%

---


# Catalog Service開発者ガイドの付録

このドキュメントには、カタログAPIの操作に役立つ追加情報が含まれています。

## 表示相互関連オブジェクト {#view-interrelated-objects}

一部のCatalogオブジェクトは、他のCatalogオブジェクトと相互に関連付けることができます。 応答ペイロードで先頭に付くフィールド `@` は、関連オブジェクトを示します。 これらのフィールドの値はURIの形式をとります。URIは、それらが表す関連オブジェクトを取得するために別のGET要求で使用できます。

特定のデータセットの検索時にドキュメントに返さ [れるデータセットの例には](look-up-object.md) 、 `files` 次のURI値を持つフィールドが含まれています。 `"@/dataSets/5ba9452f7de80400007fc52a/views/5ba9452f7de80400007fc52b/files"`. このURIを新しいGET要求のパスとして使用すると、 `files` フィールドの内容を表示できます。

**API形式**

```http
GET {OBJECT_URI}
```

| パラメーター | 説明 |
| --- | --- |
| `{OBJECT_URI}` | 相互に関連するオブジェクトフィールド( `@` 記号を除く)で指定されるURI。 |

**リクエスト**

次のリクエストでは、データセットの例のプロパティに指定されたURIを使用して、データセットに関連付けられたファイルのリストを取得します。 `files`

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/catalog/dataSets/5ba9452f7de80400007fc52a/views/5ba9452f7de80400007fc52b/files' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常に応答すると、関連オブジェクトのリストが返されます。 この例では、データセットファイルのリストが返されます。

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

カタログAPIのルートエンドポイントを使用すると、1回の呼び出しで複数のリクエストを実行できます。 要求ペイロードには、通常は個々の要求となるものを表すオブジェクトの配列が含まれ、その配列が順に実行されます。

これらのリクエストがカタログへの変更または追加で、いずれかの変更が失敗した場合、すべての変更が元に戻されます。

**API形式**

```http
POST /
```

**リクエスト**

次のリクエストでは、新しいデータセットを作成し、そのデータセットに関連する表示を作成します。 この例は、テンプレート言語を使用して、以前の呼び出しで返された値にアクセスし、以降の呼び出しで使用する方法を示しています。

例えば、前のサブリクエストから返された値を参照する場合、次の形式で参照を作成できます。 `<<{REQUEST_ID}.{ATTRIBUTE_NAME}>>` (ここ `{REQUEST_ID}` では、以下に示すように、サブリクエストのユーザー指定のIDを示します)。 これらのテンプレートを使用して、前のサブリクエストの応答オブジェクトの本文で使用可能な任意の属性を参照できます。

>[!NOTE]
>
>実行されたサブリクエストがオブジェクトへの参照のみを返す場合（カタログAPIのほとんどのPOSTおよびPUTリクエストのデフォルト）、この参照は値にエイリアスされ、として使用 `id` でき `<<{OBJECT_ID}.id>>`ます。

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
| `id` | リクエストを応答に一致させるために、応答オブジェクトに添付されるユーザー指定ID。 カタログはこの値を保存せず、単に参照用に応答に返します。 |
| `resource` | カタログAPIのルートを基準とするリソースパス。 プロトコルとドメインは、この値の一部にすることはできません。また、「/」というプリフィックスを付ける必要があります。 <br/><br/> PATCHまたはDELETEをサブリクエストとして使用する場合は、オブジェクトIDをリソースパス `method`に含めます。 ユーザーが提供するIDと混同しないで `id`ください。リソースパスには、Catalogオブジェクト自体のIDが使用されます(例 `resource: "/dataSets/1234567890"`)。 |
| `method` | リクエストで行われるアクションに関連するメソッド(GET、PUT、POST、PATCH、またはDELETE)の名前。 |
| `body` | 通常はPOST、PUT、またはPATCHリクエストのペイロードとして渡されるJSONドキュメント。 このプロパティは、GET要求やDELETE要求には必要ありません。 |

**応答**

成功した応答は、各要求に割り当てたオブジェクト `id` の配列、個々の要求のHTTPステータスコード、および応答を返し `body`ます。 3つのサンプルリクエストはすべて新しいオブジェクトを作成するものなので、各オブジェクト `body` の配列は、カタログ内で最も成功したPOST応答の標準と同様に、新しく作成されたオブジェクトのIDのみを含みます。

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

マルチリクエストへの応答を検査する際は注意が必要です。各サブリクエストのコードを検証し、親のPOSTリクエストのHTTPステータスコードのみに依存しないようにする必要があります。  単一のサブリクエストは404 （無効なリソースに対するGET要求など）を返すことができますが、リクエスト全体は200を返します。

## 追加のリクエストヘッダー

カタログには、更新時にデータの整合性を維持するのに役立つ、いくつかのヘッダー規則が用意されています。

### 一致する場合

オブジェクトのバージョン管理を使用して、複数のユーザーが同時にオブジェクトを保存した場合に発生するデータ破損の種類を防ぐことをお勧めします。

オブジェクトを更新する際のベストプラクティスは、最初に更新対象のオブジェクトに対するAPI呼び出し(GET)を行うことです。 応答（および応答に単一のオブジェクトが含まれるすべての呼び出し）内に含まれるのは、オブジェクトのバージョンを含む `E-Tag` ヘッダーです。 オブジェクトバージョンをアップデート（PUTまたはPATCH）の呼び出しで名前付け `If-Match` されたリクエストヘッダーとして追加すると、バージョンが同じ場合にのみ更新が成功し、データの衝突を防ぐのに役立ちます。

バージョンが一致しない場合（オブジェクトを取得した後に別のプロセスによって変更された場合）、ターゲットリソースへのアクセスが拒否されたことを示すHTTPステータス412（「前提条件が失敗しました」）を受け取ります。

### Pragma

場合によっては、情報を保存せずにオブジェクトを検証する必要があります。 の値を指定して `Pragma``validate-only` ヘッダーを使用すると、POSTまたはPUT要求を検証目的でのみ送信でき、データに対する変更が保持されるのを防ぐことができます。

## データの圧縮

圧縮は、小さいファイルのデータを、データを変更せずに大きいファイルにマージするExperience Platformサービスです。 パフォーマンス上の理由から、小さなファイルのセットを大きなファイルに組み合わせて、クエリーを実行する際のデータへのアクセスを高速化すると便利な場合があります。

取り込まれたバッチ内のファイルが圧縮されると、関連付けられたカタログオブジェクトが更新され、監視のために使用されます。