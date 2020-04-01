---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: リスト
topic: developer guide
translation-type: tm+mt
source-git-commit: b4741cdfd065bbaed7f2feeafe8619191e4b8f6c

---


# リスト

IMS組織（アクティブまたはその他）に属するすべてのサンドボックスをリストするには、エンドポイントにGETリクエストを作成 `/sandboxes` します。

**API形式**

```http
GET /sandboxes
```

**リクエスト**

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/sandbox-management/sandboxes \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した場合は、組織に属するリスト（、、、などの詳細を含む）の `name`サンドボックス `title`が返されま `state``type`す。

```json
{
    "sandboxes": [
        {
            "name": "prod",
            "title": "Production",
            "state": "active",
            "type": "production",
            "region": "VA7",
            "isDefault": true,
            "eTag": 2,
            "createdDate": "2019-09-04 04:57:24",
            "lastModifiedDate": "2019-09-04 04:57:24",
            "createdBy": "{USER_ID}",
            "modifiedBy": "{USER_ID}"
        },
        {
            "name": "dev",
            "title": "Development",
            "state": "active",
            "type": "development",
            "region": "VA7",
            "isDefault": false,
            "eTag": 1,
            "createdDate": "2019-09-03 22:27:48",
            "lastModifiedDate": "2019-09-03 22:27:48",
            "createdBy": "{USER_ID}",
            "modifiedBy": "{USER_ID}"
        },
        {
            "name": "stage",
            "title": "Staging",
            "state": "active",
            "type": "development",
            "region": "VA7",
            "isDefault": false,
            "eTag": 1,
            "createdDate": "2019-09-03 22:27:48",
            "lastModifiedDate": "2019-09-03 22:27:48",
            "createdBy": "{USER_ID}",
            "modifiedBy": "{USER_ID}"
        },
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
    ]
}
```

| プロパティ | 説明 |
| --- | --- |
| `name` | サンドボックスの名前。 API呼び出しで参照目的に使用されます。 |
| `title` | サンドボックスの表示名です。 |
| `state` | サンドボックスの現在の処理状態です。 サンドボックスの状態は、次のいずれかになります。 <br/><ul><li>**作成**:サンドボックスは作成されましたが、システムによって引き続きプロビジョニングされています。</li><li>**アクティブ**:サンドボックスが作成され、アクティブになります。</li><li>**失敗**:エラーが原因で、サンドボックスをシステムでプロビジョニングできず、無効になっています。</li><li>**削除**:サンドボックスは手動で無効になっています。</li></ul> |
| `type` | サンドボックスタイプ（「開発」または「実稼働」）。 |
| `isDefault` | このサンドボックスが組織のデフォルトのサンドボックスであるかどうかを示すブールプロパティです。 通常、これは実稼働用サンドボックスです。 |
| `eTag` | サンドボックスの特定のバージョンの識別子。 バージョン管理とキャッシュの効率化に使用され、この値はサンドボックスに対して変更が行われるたびに更新されます。 |
