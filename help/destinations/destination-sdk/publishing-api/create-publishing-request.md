---
description: API 呼び出しを形式設定して、Adobe Experience Platform Destination SDKを通じて宛先公開リクエストを送信する方法を説明します。
title: 宛先の公開リクエストの作成
source-git-commit: acb7075f49b4194c31371d2de63709eea7821329
workflow-type: tm+mt
source-wordcount: '447'
ht-degree: 56%

---


# 宛先の公開リクエストの作成

>[!IMPORTANT]
>
>この API エンドポイントを使用する必要があるのは、製品化された（公開）宛先を送信し、他のExperience Platformのお客様が使用できるようにする場合のみです。 独自の用途で非公開の宛先を作成する場合は、公開 API を使用して正式に宛先を送信する必要はありません。

>[!IMPORTANT]
>
>**API エンドポイント**：`platform.adobe.io/data/core/activation/authoring/destinations/publish`

宛先を設定およびテストしたら、レビューおよび公開用にアドビへと送信できます。読み取り [送信してレビュー用に、Destination SDKで作成した宛先を送信](../guides/submit-destination.md) 宛先の送信プロセスの一環として実行する必要があるその他のすべての手順については、を参照してください。

公開リクエストを送信するには、次の場合に Publish Destinations API エンドポイントを使用します。

* Destination SDK パートナーとして、すべての Experience Platform 組織をまたいですべての Experience Platform の顧客がすべての Experience Platform の顧客が製品化された宛先を利用できるようにする場合。
* あなたは *更新の有無* を設定に追加します。 設定の更新は、新しい公開リクエストを送信した後にのみ、宛先に反映されます。このリクエストはExperience Platformチームが承認します。

>[!IMPORTANT]
>
>Destination SDKでサポートされるすべてのパラメーター名と値は **大文字と小文字を区別**. 大文字と小文字の区別に関するエラーを避けるには、ドキュメントに示すように、パラメーターの名前と値を正確に使用してください。

## 宛先公開 API 操作の概要 {#get-started}

続行する前に、[はじめる前に](../getting-started.md)の重要情報を参照してください。これは、必要な宛先オーサリング権限および必要なヘッダーを取得する方法を含む、API への呼び出しに成功するために確認する必要があります。

## 公開用の宛先設定を送信 {#create}

公開用の宛先設定を送信するには、`/authoring/destinations/publish` エンドポイントへの POST リクエストを作成します。

**API 形式**

```http
POST /authoring/destinations/publish
```

+++リクエスト

次のリクエストでは、ペイロードで指定されたパラメーターで設定された組織全体に対して、公開用の宛先を送信します。以下のペイロードには、`/authoring/destinations/publish` エンドポイントで使用可能なすべてのパラメーターが含まれます。

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
| `destinationId` | 文字列 | 公開用に送信する宛先設定の宛先 ID。を使用して、宛先設定の宛先 ID を取得する [宛先設定の取得](../authoring-api/destination-configuration/retrieve-destination-configuration.md) API 呼び出し。 |
| `destinationAccess` | 文字列 | 用途 `ALL` を設定します。 |

{style="table-layout:auto"}

+++応答

応答が成功すると、HTTP ステータス 201 と共に、宛先公開リクエストの詳細が返されます。

## API エラー処理

Destination SDK API エンドポイントは、一般的な Experience Platform API エラーメッセージの原則に従います。Platform トラブルシューティングガイドの [API ステータスコード](../../../landing/troubleshooting.md#api-status-codes)および[リクエストヘッダーエラー](../../../landing/troubleshooting.md#request-header-errors)を参照してください。

## 次の手順

このドキュメントでは、宛先へ公開リクエストを送信する方法を確認しました。Adobe Experience Platform チームが公開リクエストを確認し、5 営業日以内に連絡します。