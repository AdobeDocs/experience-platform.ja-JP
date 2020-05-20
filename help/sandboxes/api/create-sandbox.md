---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: サンドボックスの作成
topic: developer guide
translation-type: tm+mt
source-git-commit: ef423a8c1b412315d03cddf7d8c351a232eb509b
workflow-type: tm+mt
source-wordcount: '136'
ht-degree: 2%

---


# サンドボックスの作成

エンドポイントにPOSTリクエストを行うことで、新しいサンドボックスを作成でき `/sandboxes` ます。

**API形式**

```http
POST /sandboxes
```

**リクエスト**

次の要求は、「dev-3」という名前の新しい開発用サンドボックスを作成します。

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
| `name` | 今後の要求でサンドボックスにアクセスするために使用される識別子です。 この値は一意にする必要があります。ベストプラクティスは、値をできるだけ説明的にすることです。 スペースや大文字を含めることはできません。 |
| `title` | プラットフォームユーザーインターフェイスに表示する目的で使用される、解読可能な名前です。 |
| `type` | 作成するサンドボックスの種類です。 現在、組織で作成できるのは「開発」タイプのサンドボックスのみです。 |

**応答**

正常に応答すると、新たに作成されたサンドボックスの詳細が返され、そのサンドボックスが「作成中」であ `state` ることが示されます。

```json
{
    "name": "dev-3",
    "title": "Development 3",
    "state": "creating",
    "type": "development",
    "region": "VA7"
}
```

>[!NOTE] サンドボックスのプロビジョニングは、システムによって約15分かかります。その後、サンドボックスのプロビジョニングが「アクティブ」または「失敗」 `state` になります。
