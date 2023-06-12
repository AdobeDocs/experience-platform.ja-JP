---
description: Adobe Experience Platform Destination SDK を通じて、宛先公開リクエストを送信するための API 呼び出しを書式設定する方法を説明します。
title: 宛先公開リクエストの作成
source-git-commit: acb7075f49b4194c31371d2de63709eea7821329
workflow-type: tm+mt
source-wordcount: '447'
ht-degree: 100%

---


# 宛先公開リクエストの作成

>[!IMPORTANT]
>
>他の Experience Platform 顧客が使用できるように、製品化（公開）された宛先を送信している場合にのみ、この API エンドポイントを使用する必要があります。自分で使用するためにプライベート宛先を作成している場合は、公開 API を使用して正式に宛先を送信する必要はありません。

>[!IMPORTANT]
>
>**API エンドポイント**：`platform.adobe.io/data/core/activation/authoring/destinations/publish`

宛先を設定してテストしたら、レビューおよび公開用にアドビに送信できます。宛先送信プロセスの一環として実行する必要があるその他のすべての手順については、[Destination SDK で作成した宛先をレビュー用に送信](../guides/submit-destination.md)を参照してください。

以下の場合に、公開宛先 API エンドポイントを使用して公開リクエストを送信します。

* Destination SDK パートナーとして、すべての Experience Platform 顧客のすべての Experience Platform 組織にわたって、製品化された宛先を利用できるようにする場合。
* 設定に対して&#x200B;*任意の更新*&#x200B;を行います。設定の更新は、新しい公開リクエストを送信し、Experience Platform チームによって承認された後にのみ、宛先に反映されます。

>[!IMPORTANT]
>
>Destination SDK でサポートされているすべてのパラメーター名および値は、**大文字と小文字が区別**&#x200B;されます。大文字と小文字を区別することに関するエラーを避けるために、ドキュメントに示すように、パラメーター名および値を正確に使用してください。

## 宛先公開 API 操作の概要 {#get-started}

続行する前に、「[はじめる前に](../getting-started.md)」を参照し、API の呼び出しを正常に行うために必要となる重要な情報（必要な宛先オーサリング権限および必要なヘッダーの取得方法など）を確認してください。

## 公開用の宛先設定を送信 {#create}

公開用の宛先設定を送信するには、`/authoring/destinations/publish` エンドポイントへの POST リクエストを作成します。

**API 形式**

```http
POST /authoring/destinations/publish
```

+++リクエスト

以下のリクエストは、ペイロードで提供されるパラメーターで設定された組織全体にわたって、公開用の宛先を送信します。以下のペイロードには、`/authoring/destinations/publish` エンドポイントで使用可能なすべてのパラメーターが含まれます。

```shell
curl -X POST https://platform.adobe.io/data/core/activation/authoring/destinations/publish \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d '
{
   "destinationId":"1230e5e4-4ab8-4655-ae1e-a6296b30f2ec",
   "destinationAccess":"ALL"
}
```

| パラメーター | タイプ | 説明 |
|---------|----------|------|
| `destinationId` | 文字列 | 公開用に送信する宛先設定の宛先 ID。[宛先設定 API 呼び出しの取得](../authoring-api/destination-configuration/retrieve-destination-configuration.md)を使用して、宛先設定の宛先 ID を取得します。 |
| `destinationAccess` | 文字列 | 宛先がすべての Experience Platform 顧客のカタログに表示されるようにするには、`ALL` を使用します。 |

{style="table-layout:auto"}

+++応答

応答が成功すると、HTTP ステータス 201 が、宛先公開リクエストの詳細と共に返されます。

## API エラー処理

Destination SDK API エンドポイントは、一般的な Experience Platform API エラーメッセージの原則に従います。Platform トラブルシューティングガイドの [API ステータスコード](../../../landing/troubleshooting.md#api-status-codes)および[リクエストヘッダーエラー](../../../landing/troubleshooting.md#request-header-errors)を参照してください。

## 次の手順

このドキュメントでは、宛先に対する公開リクエストを送信する方法を確認しました。Adobe Experience Platform チームが公開リクエストを確認し、5 営業日以内に連絡します。