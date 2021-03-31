---
keywords: Experience Platform；ホーム；人気のあるトピック；Sandbox;Sandbox
solution: Experience Platform
title: APIでサンドボックスを作成する
topic: 開発ガイド
description: '''/sandboxs''エンドポイントにPOSTリクエストを作成することで、新しいサンドボックスを作成できます。'
translation-type: tm+mt
source-git-commit: ee2fb54ba59f22a1ace56a6afd78277baba5271e
workflow-type: tm+mt
source-wordcount: '306'
ht-degree: 49%

---


# APIでサンドボックスを作成する

`/sandboxes`エンドポイントにPOSTリクエストを行うことで、開発用サンドボックスまたは実稼働用サンドボックスを作成できます。

## 開発用サンドボックスの作成

開発用サンドボックスを作成するには、`/sandboxes`エンドポイントにPOSTリクエストを行い、プロパティ`type`の値`development`を指定します。

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
  -H 'Content-Type: application/json' \
  -d '{
    "name": "dev-3",
    "title": "Development 3",
    "type": "development"
  }'
```

| プロパティ | 説明 |
| --- | --- |
| `name` | 今後のリクエストでサンドボックスにアクセスするために使用される識別子。この値は一意である必要があります。ベストプラクティスはできる限りわかりやすい値にすることです。この値には、スペースや特殊文字を含めることはできません。 |
| `title` | Platform ユーザーインターフェイスでの表示用に使用される、わかりやすい名前。 |
| `type` | 作成するサンドボックスのタイプ。`type`プロパティの値は、開発版と実稼働版のどちらでもかまいません。 |

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

## 実稼働用サンドボックスの作成

>[!NOTE]
>
>複数の実稼働サンドボックス機能はベータ版です。

実稼動用サンドボックスを作成するには、`/sandboxes`エンドポイントにPOSTリクエストを行い、プロパティ`type`の値`production`を指定します。

**API 形式**

```http
POST /sandboxes
```

**リクエスト**

次のリクエストは、「test-prod-sandbox」という名前の新しい実稼働用サンドボックスを作成します。

```shell
curl -X POST \
  https://platform.adobe.io/data/foundation/sandbox-management/sandboxes \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'Content-Type: application/json' \
  -d '{
    "name": "test-prod-sandbox",
    "title": "Test Production Sandbox",
    "type": "production"
}'
```

| プロパティ | 説明 |
| --- | --- |
| `name` | 今後のリクエストでサンドボックスにアクセスするために使用される識別子。この値は一意である必要があります。ベストプラクティスはできる限りわかりやすい値にすることです。この値には、スペースや特殊文字を含めることはできません。 |
| `title` | Platform ユーザーインターフェイスでの表示用に使用される、わかりやすい名前。 |
| `type` | 作成するサンドボックスのタイプ。`type`プロパティの値は、開発版と実稼働版のどちらでもかまいません。 |

**応答**

正常な応答は、新しく作成されたサンドボックスの詳細を返し、`state` が「作成中」であることを示します。

```json
{
    "name": "test-production-sandbox",
    "title": "Test Production Sandbox",
    "state": "creating",
    "type": "production",
    "region": "VA7"
}
```
