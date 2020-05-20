---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: サンドボックスの検索
topic: developer guide
translation-type: tm+mt
source-git-commit: ef423a8c1b412315d03cddf7d8c351a232eb509b
workflow-type: tm+mt
source-wordcount: '204'
ht-degree: 2%

---


# サンドボックスの検索

個々のサンドボックスを検索するには、要求パスにサンドボックスの `name` プロパティを含むGET要求を作成します。

**API形式**

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

成功した応答は、サンドボックスの `name`、、、、およびなどの詳細を返し `title``state``type`ます。

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
| `name` | サンドボックスの名前です。 API呼び出しで参照用に使用されます。 |
| `title` | サンドボックスの表示名です。 |
| `state` | サンドボックスの現在の処理状態です。 サンドボックスの状態は、次のいずれかになります。 <ul><li>**作成**: サンドボックスは作成されましたが、システムによってプロビジョニングされています。</li><li>**active**: サンドボックスが作成され、アクティブになります。</li><li>**失敗**: エラーが発生したため、サンドボックスはシステムでプロビジョニングできず、無効になっています。</li><li>**削除済み**: サンドボックスは手動で無効になっています。</li></ul> |
| `type` | サンドボックスタイプ（「開発版」または「実稼働版」）。 |
| `isDefault` | このサンドボックスが組織のデフォルトのサンドボックスであるかどうかを示すブールプロパティです。 通常、これは実稼働用サンドボックスです。 |
| `eTag` | サンドボックスの特定のバージョンの識別子。 バージョン管理とキャッシュの効率性を考慮して、この値はサンドボックスに対して変更が行われるたびに更新されます。 |