---
description: このページでは、Adobe Experience Platform Destination SDK で、資格情報設定を削除するために使用される API 呼び出しの例を示します。
title: 資格情報設定の削除
exl-id: a540e349-043c-4f04-8ca8-f650b9943492
source-git-commit: b4334b4f73428f94f5a7e5088f98e2459afcaf3c
workflow-type: tm+mt
source-wordcount: '395'
ht-degree: 100%

---

# 資格情報設定の削除

>[!IMPORTANT]
>
>**API エンドポイント**：`platform.adobe.io/data/core/activation/authoring/credentials`

このページでは、`/authoring/credentials` API エンドポイントを使用して、資格情報設定を削除するために使用できる API リクエストおよびペイロードの例を示します。

## `/credentials` API エンドポイントを使用するタイミング {#when-to-use}

>[!IMPORTANT]
>
>ほとんどの場合、`/credentials` API エンドポイントを使用する必要は&#x200B;***ありません***。代わりに、`/destinations` エンドポイントの `customerAuthenticationConfigurations` パラメーターを介して宛先の認証情報を設定できます。
> 
>サポートされる認証タイプについて詳しくは、[顧客認証設定](../functionality/destination-configuration/customer-authentication.md)を参照してください。

アドビと宛先プラットフォームとの間にグローバル認証システムがあり、[!DNL Platform] の顧客が宛先への接続に認証資格情報を提供する必要がない場合にのみ、この API エンドポイントを使用して資格情報設定を作成します。この場合、`/credentials` API エンドポイントを使用して、資格情報設定を作成する必要があります。

グローバル認証システムを使用する場合、[新しい宛先設定を作成する](../authoring-api/destination-configuration/create-destination-configuration.md)際に、[宛先配信](../functionality/destination-configuration/destination-delivery.md)設定で `"authenticationRule":"PLATFORM_AUTHENTICATION"` を設定する必要があります。

>[!IMPORTANT]
>
>Destination SDK でサポートされているすべてのパラメーター名および値は、**大文字と小文字が区別**&#x200B;されます。大文字と小文字を区別することに関するエラーを避けるために、ドキュメントに示すように、パラメーター名および値を正確に使用してください。

## 資格情報 API 操作の概要 {#get-started}

続行する前に、「[はじめる前に](../getting-started.md)」を参照し、API の呼び出しを正常に行うために必要となる重要な情報（必要な宛先オーサリング権限および必要なヘッダーの取得方法など）を確認してください。

## 資格情報設定の削除 {#delete}

削除したい資格情報設定の `{INSTANCE_ID}` で `/authoring/credentials` エンドポイントに `DELETE` リクエストを行うことで、[既存の](create-credential-configuration.md)資格情報設定を削除できます。

既存の宛先設定およびその関連する `{INSTANCE_ID}` を取得するには、[資格情報設定の取得](retrieve-credential-configuration.md)に関する記事を参照してください。

**API 形式**

```http
DELETE /authoring/credentials/{INSTANCE_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{INSTANCE_ID}` | 削除する資格情報設定の `ID`。 |

以下のリクエストは、`{INSTANCE_ID}` パラメーターで定義された資格情報設定を削除します。

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

応答が成功すると、HTTP ステータス 200 が、空の HTTP 応答と共に返されます。

+++

## API エラー処理 {#error-handling}

Destination SDK API エンドポイントは、一般的な Experience Platform API エラーメッセージの原則に従います。Platform トラブルシューティングガイドの [API ステータスコード](../../../landing/troubleshooting.md#api-status-codes)および[リクエストヘッダーエラー](../../../landing/troubleshooting.md#request-header-errors)を参照してください。

## 次の手順 {#next-steps}

このドキュメントでは、`/authoring/credentials` API エンドポイントを使用した、資格情報設定の削除方法を確認しました。この手順が宛先設定プロセスのどこに当てはまるかを把握するには、[Destination SDK を使用した宛先の設定方法](../guides/configure-destination-instructions.md)を参照してください。
