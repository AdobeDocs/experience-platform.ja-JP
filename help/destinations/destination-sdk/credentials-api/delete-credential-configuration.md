---
description: このページでは、秘密鍵証明書の設定Adobe Experience Platform Destination SDKの削除に使用する API 呼び出しの例を示します。
title: 秘密鍵証明書の設定の削除
source-git-commit: 9e1ae44f83b886f0b5dd5a9fc9cd9b7db6154ff0
workflow-type: tm+mt
source-wordcount: '395'
ht-degree: 36%

---


# 秘密鍵証明書の設定の削除

>[!IMPORTANT]
>
>**API エンドポイント**：`platform.adobe.io/data/core/activation/authoring/credentials`

このページでは、API リクエストとペイロードの例を示します。この例を使用して、 `/authoring/credentials` API エンドポイント。

## `/credentials` API エンドポイントを使用するタイミング {#when-to-use}

>[!IMPORTANT]
>
>ほとんどの場合、`/credentials` API エンドポイントを使用する必要は&#x200B;***ありません***。代わりに、エンドポイント `/destinations` の `customerAuthenticationConfigurations` パラメーターを介して宛先の認証情報を設定できます。
> 
>読み取り [顧客認証設定](../functionality/destination-configuration/customer-authentication.md) を参照してください。

この API エンドポイントを使用して、Adobeと宛先プラットフォームの間にグローバル認証システムがあり、 [!DNL Platform] のお客様は、宛先に接続するための認証資格情報を提供する必要はありません。 この場合、 `/credentials` API エンドポイント。

グローバル認証システムを使用する場合、 `"authenticationRule":"PLATFORM_AUTHENTICATION"` 内 [宛先の配信](../functionality/destination-configuration/destination-delivery.md) 設定、時 [新しい宛先設定の作成](../authoring-api/destination-configuration/create-destination-configuration.md).

>[!IMPORTANT]
>
>Destination SDKでサポートされるすべてのパラメーター名と値は **大文字と小文字を区別**. 大文字と小文字の区別に関するエラーを避けるには、ドキュメントに示すように、パラメーターの名前と値を正確に使用してください。

## 資格情報 API 操作の概要 {#get-started}

続ける前に「[はじめる前に](../getting-started.md)」を参照し、必要な宛先オーサリング権限および必要なヘッダーの取得方法など、API の呼び出しを正常に行うために必要となる重要な情報を確認してください。

## 秘密鍵証明書の設定の削除 {#delete}

次の項目を削除できます。 [既存](create-credential-configuration.md) 資格情報の設定 `DELETE` にリクエスト `/authoring/credentials` endpoint に `{INSTANCE_ID}`を設定します。

既存の宛先設定とそれに対応する設定を取得するには `{INSTANCE_ID}`に関する記事を参照してください。 [資格情報設定の取得](retrieve-credential-configuration.md).

**API 形式**

```http
DELETE /authoring/credentials/{INSTANCE_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{INSTANCE_ID}` | この `ID` を設定します。 |

次のリクエストでは、 `{INSTANCE_ID}` パラメーター。

+++リクエスト

```shell
curl -X DELETE https://platform.adobe.io/data/core/activation/authoring/credentials/{INSTANCE_ID} \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
```

+++

+++応答

リクエストが成功した場合は、空の HTTP 応答とともに HTTP ステータス 200 が返されます。

+++

## API エラー処理 {#error-handling}

Destination SDK API エンドポイントは、一般的な Experience Platform API エラーメッセージの原則に従います。Platform トラブルシューティングガイドの [API ステータスコード](../../../landing/troubleshooting.md#api-status-codes)および[リクエストヘッダーエラー](../../../landing/troubleshooting.md#request-header-errors)を参照してください。

## 次の手順 {#next-steps}

このドキュメントを読んだ後、 `/authoring/credentials` API エンドポイント。 [Destination SDK を使用して宛先を設定する方法](../guides/configure-destination-instructions.md)を参照して、この手順が宛先を設定するプロセスの中でどのように位置づけられるかを把握します。
