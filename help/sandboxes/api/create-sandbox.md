---
keywords: Experience Platform；ホーム；人気のあるトピック；Sandbox;Sandbox
solution: Experience Platform
title: APIでサンドボックスを作成する
topic-legacy: developer guide
description: '''/sandboxs''エンドポイントにPOSTリクエストを作成することで、新しいサンドボックスを作成できます。'
exl-id: 676c5de8-2c3a-4612-9dd8-93e01cafe90e
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '164'
ht-degree: 79%

---

# APIでサンドボックスを作成する

`/sandboxes` エンドポイントに POST リクエストを実行することで、新しいサンドボックスを作成できます。

**API 形式**

```http
POST /sandboxes
```

**リクエスト**

次のリクエストは、「dev-3」という名前の新しい開発用サンドボックスを作成します。

```shell
curl -X POST \
  https://platform.adobe.io/data/foundation/sandbox-management/sandboxes \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
    "name": "dev-3",
    "title": "Development 3",
    "type": "development"
  }'
```

| プロパティ | 説明 |
| --- | --- |
| `name` | 今後のリクエストでサンドボックスにアクセスするために使用される識別子。この値は一意である必要があります。ベストプラクティスはできる限りわかりやすい値にすることです。スペースや大文字は使用できません。 |
| `title` | Platform ユーザーインターフェイスでの表示用に使用される、わかりやすい名前。 |
| `type` | 作成するサンドボックスのタイプ。現在、組織で作成できるのは、「開発」タイプのサンドボックスのみです。 |

**応答**

正常な応答は、新しく作成されたサンドボックスの詳細を返し、`state` が「作成中」であることを示します。

```json
{
    "name": "dev-3",
    "title": "Development 3",
    "state": "creating",
    "type": "development",
    "region": "VA7"
}
```

>[!NOTE]
>
> サンドボックスのプロビジョニングは、システムによって約 15 分かかります。その後、サンドボックスの `state` が「アクティブ」または「失敗」になります。
