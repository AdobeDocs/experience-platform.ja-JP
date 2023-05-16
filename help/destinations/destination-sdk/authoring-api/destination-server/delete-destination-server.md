---
description: このページでは、Adobe Experience Platform Destination SDKを介した既存の宛先サーバー設定の削除に使用する API 呼び出しの例を示します。
title: 宛先サーバー設定の削除
source-git-commit: acb7075f49b4194c31371d2de63709eea7821329
workflow-type: tm+mt
source-wordcount: '329'
ht-degree: 33%

---


# 宛先サーバー設定の削除

このページの例では、 `/authoring/destination-servers` API エンドポイント。

このエンドポイントを通じて削除できる機能について詳しくは、次の記事を参照してください。

* [Destination SDK](../../../destination-sdk/functionality/destination-server/server-specs.md)
* [Destination SDK](../../../destination-sdk/functionality/destination-server/templating-specs.md)
* [メッセージの形式](../../../destination-sdk/functionality/destination-server/message-format.md)
* [ファイル形式設定](../../../destination-sdk/functionality/destination-server/file-formatting.md)

>[!IMPORTANT]
>
>Destination SDKでサポートされるすべてのパラメーター名と値は **大文字と小文字を区別**. 大文字と小文字の区別に関するエラーを避けるには、ドキュメントに示すように、パラメーターの名前と値を正確に使用してください。

## 宛先サーバー API 操作の概要 {#get-started}

続行する前に「[はじめる前に](../../getting-started.md)」を参照し、必要な宛先オーサリング権限および必要なヘッダーの取得方法など、API の呼び出しを正常に行うために必要となる重要な情報を確認してください。

## 宛先サーバー設定の削除 {#delete}

次の項目を削除できます。 [既存](create-destination-server.md) 宛先サーバーの設定を行う `DELETE` にリクエスト `/authoring/destination-servers` endpoint に `{INSTANCE_ID}`削除する宛先サーバー設定の情報です。

>[!TIP]
>
>**API エンドポイント**：`platform.adobe.io/data/core/activation/authoring/destination-servers`

既存の宛先サーバー設定と、それに対応する設定を取得するには `{INSTANCE_ID}`に関する記事を参照してください。 [宛先サーバー設定の取得](retrieve-destination-server.md).

**API 形式**

```http
DELETE /authoring/destination-servers/{INSTANCE_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{INSTANCE_ID}` | 削除する宛先サーバー設定の `ID` です。 |

+++リクエスト

```shell
curl -X DELETE https://platform.adobe.io/data/core/activation/authoring/destination-servers/{INSTANCE_ID} \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
```

+++応答

リクエストが成功した場合は、空の HTTP 応答とともに HTTP ステータス 200 が返されます。

## API エラー処理 {#error-handling}

Destination SDK API エンドポイントは、一般的な Experience Platform API エラーメッセージの原則に従います。Platform トラブルシューティングガイドの [API ステータスコード](../../../../landing/troubleshooting.md#api-status-codes)および[リクエストヘッダーエラー](../../../../landing/troubleshooting.md#request-header-errors)を参照してください。

## 次の手順 {#next-steps}

このドキュメントを読んだ後、Destination SDKを使用して既存の宛先サーバーを削除する方法がわかりました。 `/authoring/destination-servers` API エンドポイント。

このエンドポイントで実行できる操作について詳しくは、次の記事を参照してください。

* [宛先サーバー設定の作成](create-destination-server.md)
* [宛先サーバー設定の取得](retrieve-destination-server.md)
* [宛先サーバー設定の更新](update-destination-server.md)

