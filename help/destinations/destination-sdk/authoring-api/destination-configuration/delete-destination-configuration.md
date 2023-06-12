---
description: このページでは、Adobe Experience Platform Destination SDK を通じて、既存の宛先設定を削除するために使用される API 呼び出しの例を示します。
title: 宛先設定の削除
source-git-commit: acb7075f49b4194c31371d2de63709eea7821329
workflow-type: tm+mt
source-wordcount: '278'
ht-degree: 100%

---


# 宛先設定の削除

このページでは、`/authoring/destinations` API エンドポイントを使用して、既存の宛先設定を削除するために使用できる API リクエストおよびペイロードの例を示します。

>[!IMPORTANT]
>
>Destination SDK でサポートされているすべてのパラメーター名および値は、**大文字と小文字が区別**&#x200B;されます。大文字と小文字を区別することに関するエラーを避けるために、ドキュメントに示すように、パラメーター名および値を正確に使用してください。

## 宛先設定 API 操作の概要 {#get-started}

続行する前に、「[はじめる前に](../../getting-started.md)」を参照し、API の呼び出しを正常に行うために必要となる重要な情報（必要な宛先オーサリング権限および必要なヘッダーの取得方法など）を確認してください。

## 宛先設定の削除 {#delete}

削除したい宛先設定の `{INSTANCE_ID}` で `/authoring/destinations` エンドポイントに `DELETE` リクエストを行うことで、[既存の](create-destination-configuration.md)宛先サーバー設定を削除できます。

>[!TIP]
>
>**API エンドポイント**：`platform.adobe.io/data/core/activation/authoring/destinations`

既存の宛先設定およびその関連する `{INSTANCE_ID}` を取得するには、[宛先設定の取得](retrieve-destination-configuration.md)に関する記事を参照してください。

**API 形式**

```http
DELETE /authoring/destinations/{INSTANCE_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{INSTANCE_ID}` | 削除する宛先設定の `ID`。 |

+++リクエスト

```shell
curl -X DELETE https://platform.adobe.io/data/core/activation/authoring/destinations/{INSTANCE_ID} \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
```

+++応答

応答が成功すると、HTTP ステータス 200 が、空の HTTP 応答と共に返されます。


## API エラー処理 {#error-handling}

Destination SDK API エンドポイントは、一般的な Experience Platform API エラーメッセージの原則に従います。Platform トラブルシューティングガイドの [API ステータスコード](../../../../landing/troubleshooting.md#api-status-codes)および[リクエストヘッダーエラー](../../../../landing/troubleshooting.md#request-header-errors)を参照してください。

## 次の手順

このドキュメントでは、Destination SDK `/authoring/destinations` API エンドポイントを使用した、既存の宛先設定の削除方法を確認しました。

このエンドポイントでできることについて詳しくは、以下の記事を参照してください。

* [宛先設定の作成](create-destination-configuration.md)
* [宛先設定の取得](retrieve-destination-configuration.md)
* [宛先設定の更新](update-destination-configuration.md)

