---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: サンドボックスの作成
topic: developer guide
translation-type: tm+mt
source-git-commit: bd9884a24c5301121f30090946ab24d9c394db1b
workflow-type: tm+mt
source-wordcount: '136'
ht-degree: 100%

---


# サンドボックスの作成

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
