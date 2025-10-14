---
keywords: Experience Platform；ホーム；人気のトピック；使用可能なサンドボックスのリスト；サンドボックスのリスト
solution: Experience Platform
title: 使用可能なサンドボックス API エンドポイント
description: 使用可能なサンドボックス エンドポイントにGETリクエストを行うことで、現在のユーザーが使用できるサンドボックスをリストできます。
role: Developer
exl-id: 9b0719af-c1ca-439a-9c8b-86c7fa26a3b8
source-git-commit: c16ce1020670065ecc5415bc3e9ca428adbbd50c
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 30%

---

# 使用可能なサンドボックスエンドポイント

>[!NOTE]
>
>Sandbox API で提供される他のエンドポイントとは異なり、このエンドポイントは、サンドボックス管理のアクセス権限を持たないユーザーを含むすべてのユーザーが使用できます。

使用可能なサンドボックス エンドポイントにGETリクエストを行うことで、現在のユーザーが使用できるサンドボックスをリストできます。

**API 形式**

```http
GET /{QUERY_PARAMS}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{QUERY_PARAMS}` | 結果をフィルタリングするオプションのクエリパラメーター。 使用可能なパラメーターのリストについては、[&#x200B; 付録ドキュメント &#x200B;](./appendix.md#query) を参照してください。 |

**リクエスト**

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/sandbox-management/?limit=3&offset=1 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}'
```

**応答**

応答が成功すると、`name`、`title`、`state`、`type` などの詳細を含む、現在のユーザーが使用できるサンドボックスのリストが返されます。

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
    ],
    "_page": {
        "limit": 3,
        "count": 1
    },
    "_links": {
        "page": {
            "href": "https://platform.adobe.io:443/data/foundation/sandbox-management/?limit={limit}&offset={offset}",
            "templated": true
        }
    }
}
```

| プロパティ | 説明 |
| --- | --- |
| `name` | サンドボックスの名前。API 呼び出しで参照目的に使用されます。 |
| `title` | サンドボックスの表示名。 |
| `state` | サンドボックスの現在の処理状態。サンドボックスの状態は、次のいずれかになります。 <ul><li>`creating`: サンドボックスが作成されましたが、まだシステムによるプロビジョニングが行われています。</li><li>`active`：サンドボックスが作成され、アクティブになります。</li><li>`failed`: エラーが発生したので、サンドボックスをシステムでプロビジョニングできなかったため、無効になりました。</li><li>`deleted`：サンドボックスは手動で無効になりました。</li></ul> |
| `type` | サンドボックスタイプ（「開発」または「実稼働」）。 |
| `isDefault` | このサンドボックスが組織のデフォルトの実稼動サンドボックスであるかどうかを示すブール値プロパティ。 |
| `eTag` | サンドボックスの特定のバージョンの識別子。バージョン管理とキャッシュの効率化に使用され、この値はサンドボックスに変更が追加されるたびに更新されます。 |
