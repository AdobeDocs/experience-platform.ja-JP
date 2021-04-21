---
keywords: Experience Platform；ホーム；人気のあるトピック；サンドボックスを検索；サンドボックスを検索
solution: Experience Platform
title: APIでのサンドボックスの検索
topic-legacy: developer guide
description: 個々のサンドボックスを検索するには、要求パスにサンドボックスのnameプロパティを含むGET要求を作成します。
exl-id: de8d965c-ca58-436e-b5b1-05a492de70f5
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '243'
ht-degree: 80%

---

# APIでサンドボックスを検索する

個々のサンドボックスを検索するには、リクエストパスにサンドボックスの `name` プロパティを含む GET リクエストを作成する必要があります。

**API 形式**

```http
GET /sandboxes/{SANDBOX_NAME}
```

| パラメーター | 説明 |
| --- | --- |
| `{SANDBOX_NAME}` | 検索するサンドボックスの `name` プロパティです。 |

**リクエスト**

次のリクエストは、「dev-2」という名前のサンドボックスを取得します。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/sandbox-management/sandboxes/dev-2 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、サンドボックスの詳細（`name`、`title`、`state`、`type`）を返します。

```json
{
    "name": "dev-2",
    "title": "Development 2",
    "state": "creating",
    "type": "development",
    "region": "VA7",
    "isDefault": false,
    "eTag": 1,
    "createdDate": "2019-09-07 10:16:02",
    "lastModifiedDate": "2019-09-07 10:16:02",
    "createdBy": "{USER_ID}",
    "modifiedBy": "{USER_ID}"
}
```

| プロパティ | 説明 |
| --- | --- |
| `name` | サンドボックスの名前。API 呼び出しで参照目的に使用されます。 |
| `title` | サンドボックスの表示名。 |
| `state` | サンドボックスの現在の処理状態。サンドボックスの状態は、次のいずれかになります。 <ul><li>**作成**：サンドボックスは作成されましたが、システムによって引き続きプロビジョニングされています。</li><li>**アクティブ**：サンドボックスが作成され、アクティブです。</li><li>**失敗**：エラーが原因で、サンドボックスはシステムでプロビジョニングできず、無効になっています。</li><li>**削除**：サンドボックスは手動で無効にされています。</li></ul> |
| `type` | サンドボックスタイプ（「開発」または「実稼働」）。 |
| `isDefault` | このサンドボックスが組織のデフォルトのサンドボックスであるかどうかを示すブール型のプロパティです。通常、これは実稼働用サンドボックスです。 |
| `eTag` | サンドボックスの特定のバージョンの識別子。バージョン管理とキャッシュの効率化に使用され、この値はサンドボックスに変更が追加されるたびに更新されます。 |
