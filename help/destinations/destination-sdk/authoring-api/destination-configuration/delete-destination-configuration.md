---
description: このページでは、Adobe Experience Platform Destination SDKを通じた既存の宛先設定の削除に使用する API 呼び出しの例を示します。
title: 宛先設定の削除
source-git-commit: acb7075f49b4194c31371d2de63709eea7821329
workflow-type: tm+mt
source-wordcount: '278'
ht-degree: 35%

---


# 宛先設定の削除

このページの例では、 `/authoring/destinations` API エンドポイント。

>[!IMPORTANT]
>
>Destination SDKでサポートされるすべてのパラメーター名と値は **大文字と小文字を区別**. 大文字と小文字の区別に関するエラーを避けるには、ドキュメントに示すように、パラメーターの名前と値を正確に使用してください。

## 宛先設定 API 操作の概要 {#get-started}

続ける前に「[はじめる前に](../../getting-started.md)」を参照し、必要な宛先オーサリング権限および必要なヘッダーの取得方法など、API の呼び出しを正常に行うために必要となる重要な情報を確認してください。

## 宛先設定の削除 {#delete}

次の項目を削除できます。 [既存](create-destination-configuration.md) 宛先サーバーの設定を行う `DELETE` にリクエスト `/authoring/destinations` endpoint に `{INSTANCE_ID}`削除する宛先設定の情報です。

>[!TIP]
>
>**API エンドポイント**：`platform.adobe.io/data/core/activation/authoring/destinations`

既存の宛先設定とそれに対応する設定を取得するには `{INSTANCE_ID}`に関する記事を参照してください。 [宛先設定の取得](retrieve-destination-configuration.md).

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

リクエストが成功した場合は、空の HTTP 応答とともに HTTP ステータス 200 が返されます。


## API エラー処理 {#error-handling}

Destination SDK API エンドポイントは、一般的な Experience Platform API エラーメッセージの原則に従います。Platform トラブルシューティングガイドの [API ステータスコード](../../../../landing/troubleshooting.md#api-status-codes)および[リクエストヘッダーエラー](../../../../landing/troubleshooting.md#request-header-errors)を参照してください。

## 次の手順

このドキュメントを読んだ後、Destination SDKを使用して既存の宛先設定を削除する方法を理解できました。 `/authoring/destinations` API エンドポイント。

このエンドポイントで実行できる操作について詳しくは、次の記事を参照してください。

* [宛先設定の作成](create-destination-configuration.md)
* [宛先設定の取得](retrieve-destination-configuration.md)
* [宛先設定の更新](update-destination-configuration.md)

