---
keywords: Experience Platform；ホーム；人気のあるトピック；リストのアクティブなサンドボックス；リストのサンドボックス
solution: Experience Platform
title: APIの現在のリスト用のユーザーのアクティブなサンドボックス
topic: 開発ガイド
description: ルートエンドポイントにGETリクエストを行うことで、現在のユーザーに対してアクティブなサンドボックスをリストできます。
translation-type: tm+mt
source-git-commit: ca3de18c093d7b692b582045afea4401d7133b9b
workflow-type: tm+mt
source-wordcount: '363'
ht-degree: 63%

---


# APIの現在のリスト用のアクティブなサンドボックス

>[!NOTE]
>
> Sandbox API で提供される他のエンドポイントとは異なり、このエンドポイントは、サンドボックス管理のアクセス権限を持たないユーザーも含め、すべてのユーザーが使用できます。

現在のユーザーのアクティブなサンドボックスのリストを作成するには、ルート（`/`）エンドポイントに GET リクエストを送信します。

**API 形式**

```http
GET /{QUERY_PARAMS}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{QUERY_PARAMS}` | 結果をフィルターするオプションのクエリパラメーター。詳しくは、[クエリパラメーター](#query)の節を参照してください。 |

**リクエスト**

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/sandbox-management/?&limit=3&offset=1 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
```

**応答**

正常な応答は、現在のユーザーのアクティブなサンドボックスのリスト（`name`、`title`、`state`、`type` などの詳細を含む）を返します。

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
| `state` | サンドボックスの現在の処理状態。サンドボックスの状態は、次のいずれかになります。 <ul><li>**作成**：サンドボックスは作成されましたが、システムによって引き続きプロビジョニングされています。</li><li>**アクティブ**：サンドボックスが作成され、アクティブです。</li><li>**失敗**：エラーが原因で、サンドボックスはシステムでプロビジョニングできず、無効になっています。</li><li>**削除**：サンドボックスは手動で無効にされています。</li></ul> |
| `type` | サンドボックスタイプ（「開発」または「実稼働」）。 |
| `isDefault` | このサンドボックスが組織のデフォルトのサンドボックスであるかどうかを示すブール型のプロパティです。通常、これは実稼働用サンドボックスです。 |
| `eTag` | サンドボックスの特定のバージョンの識別子。バージョン管理とキャッシュの効率化に使用され、この値はサンドボックスに変更が追加されるたびに更新されます。 |

## クエリパラメーターの使用 {#query}

[[!DNL Sandbox]](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/sandbox-api.yaml) APIは、サンドボックスのリスト表示時に、ページへのクエリパラメーターの使用と結果のフィルターをサポートしています。

>[!NOTE]
>
>`limit`と`offset`のクエリパラメーターは一緒に指定する必要があります。 1つのみ指定した場合、APIはエラーを返します。 noneを指定した場合、デフォルトの制限は50で、オフセットは0です。

| パラメーター | 説明 |
| --------- | ----------- |
| `limit` | 応答で返されるレコードの最大数です。 |
| `offset` | 最初のレコードから応答リストの開始（オフセット）までのエンティティ数。 |
