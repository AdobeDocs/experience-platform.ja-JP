---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: リストのアクティブなサンドボックス（現在のユーザー用）
topic: developer guide
translation-type: tm+mt
source-git-commit: 982764ae7807e40cbca5ca60c70bf363a271e3c2

---


# リストのアクティブなサンドボックス（現在のユーザー用）

>[!NOTE] Sandbox APIで提供される他のエンドポイントとは異なり、このエンドポイントは、Sandbox管理のアクセス権限を持たないユーザーも含め、すべてのユーザーで使用できます。

現在のリストに対してアクティブなサンドボックスを作成するには、ルート(`/`)エンドポイントにGETリクエストを送信します。

**API形式**

```http
GET /
```

**リクエスト**

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/sandbox-management \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、現在のユーザーに対してアクティブなサンドボックスのリスト(、、、などの `name`詳細な `title`ど)を `state`返しま `type`す。

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
        }
    ]
}
```

| プロパティ | 説明 |
| --- | --- |
| `name` | サンドボックスの名前。 API呼び出しで参照目的に使用されます。 |
| `title` | サンドボックスの表示名です。 |
| `state` | サンドボックスの現在の処理状態です。 サンドボックスの状態は、次のいずれかになります。 <ul><li>**作成**:サンドボックスは作成されましたが、システムによって引き続きプロビジョニングされています。</li><li>**アクティブ**:サンドボックスが作成され、アクティブになります。</li><li>**失敗**:エラーが原因で、サンドボックスをシステムでプロビジョニングできず、無効になっています。</li><li>**削除**:サンドボックスは手動で無効になっています。</li></ul> |
| `type` | サンドボックスタイプ（「開発」または「実稼働」）。 |
| `isDefault` | このサンドボックスが組織のデフォルトのサンドボックスであるかどうかを示すブールプロパティです。 通常、これは実稼働用サンドボックスです。 |
| `eTag` | サンドボックスの特定のバージョンの識別子。 バージョン管理とキャッシュの効率化に使用され、この値はサンドボックスに対して変更が行われるたびに更新されます。 |
