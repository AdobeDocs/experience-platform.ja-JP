---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: 現在のユーザー用のリストアクティブサンドボックス
topic: developer guide
translation-type: tm+mt
source-git-commit: bd9884a24c5301121f30090946ab24d9c394db1b
workflow-type: tm+mt
source-wordcount: '220'
ht-degree: 1%

---


# 現在のユーザー用のリストアクティブサンドボックス

>[!NOTE]
>
>Sandbox APIで提供される他のエンドポイントとは異なり、このエンドポイントは、Sandbox管理アクセス権限のないユーザーも含め、すべてのユーザーが使用できます。

ルート(`/`)エンドポイントにGETリクエストを行うことで、現在のユーザーに対してアクティブなサンドボックスをリストできます。

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

正常に完了すると、 `name`、 `title`、 `state`、およびなどの詳細を含む、現在のユーザーに対してアクティブなサンドボックスのリストが返され `type`ます。

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
| `name` | サンドボックスの名前です。 API呼び出しで参照用に使用されます。 |
| `title` | サンドボックスの表示名です。 |
| `state` | サンドボックスの現在の処理状態です。 サンドボックスの状態は、次のいずれかになります。 <ul><li>**作成**: サンドボックスは作成されましたが、システムによってプロビジョニングされています。</li><li>**active**: サンドボックスが作成され、アクティブになります。</li><li>**失敗**: エラーが発生したため、サンドボックスはシステムでプロビジョニングできず、無効になっています。</li><li>**削除済み**: サンドボックスは手動で無効になっています。</li></ul> |
| `type` | サンドボックスタイプ（「開発版」または「実稼働版」）。 |
| `isDefault` | このサンドボックスが組織のデフォルトのサンドボックスであるかどうかを示すブールプロパティです。 通常、これは実稼働用サンドボックスです。 |
| `eTag` | サンドボックスの特定のバージョンの識別子。 バージョン管理とキャッシュの効率性を考慮して、この値はサンドボックスに対して変更が行われるたびに更新されます。 |
