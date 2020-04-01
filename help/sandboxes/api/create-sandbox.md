---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: サンドボックスの作成
topic: developer guide
translation-type: tm+mt
source-git-commit: ef423a8c1b412315d03cddf7d8c351a232eb509b

---


# サンドボックスの作成

エンドポイントにPOSTリクエストを作成することで、新しいサンドボックスを作成で `/sandboxes` きます。

**API形式**

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
| `name` | 今後のリクエストでサンドボックスにアクセスするために使用される識別子。 この値は一意である必要があり、できる限りわかりやすい値にすることをお勧めします。 スペースや大文字は使用できません。 |
| `title` | プラットフォームユーザーインターフェイスでの表示用に使用される、わかりやすい名前。 |
| `type` | 作成するサンドボックスのタイプ。 現在、組織で作成できるのは、「開発」タイプのサンドボックスのみです。 |

**応答**

成功した応答は、新しく作成されたサンドボックスの詳細を返し、「作成中」であ `state` ることを示します。

```json
{
    "name": "dev-3",
    "title": "Development 3",
    "state": "creating",
    "type": "development",
    "region": "VA7"
}
```

>[!NOTE] サンドボックスのプロビジョニングは、システムによって約15分かかります。その後、サンドボックスの「ア `state` クティブ」または「失敗」になります。
