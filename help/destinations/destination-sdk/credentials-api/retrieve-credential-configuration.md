---
description: このページでは、Adobe Experience Platform Destination SDK を通じて、資格情報設定を取得するために使用される API 呼び出しの例を示します。
title: 資格情報設定の取得
exl-id: cec55073-6e2f-4412-a9dd-1aeb445279c0
source-git-commit: 560200a6553a1aae66c608eef7901b3248c886b4
workflow-type: tm+mt
source-wordcount: '499'
ht-degree: 88%

---

# 資格情報設定の取得

>[!IMPORTANT]
>
>**API エンドポイント**：`platform.adobe.io/data/core/activation/authoring/credentials`

このページでは、`/authoring/credentials` API エンドポイントを使用して、資格情報設定を取得するために使用できる API リクエストおよびペイロードの例を示します。

## `/credentials` API エンドポイントを使用するタイミング {#when-to-use}

>[!IMPORTANT]
>
>ほとんどの場合、`/credentials` API エンドポイントを使用する必要は&#x200B;***ありません***。代わりに、`/destinations` エンドポイントの `customerAuthenticationConfigurations` パラメーターを介して宛先の認証情報を設定できます。
> 
>サポートされる認証タイプについて詳しくは、[顧客認証設定](../functionality/destination-configuration/customer-authentication.md)を参照してください。

アドビと宛先プラットフォームとの間にグローバル認証システムがあり、[!DNL Experience Platform] の顧客が宛先への接続に認証資格情報を提供する必要がない場合にのみ、この API エンドポイントを使用して資格情報設定を作成します。この場合、`/credentials` API エンドポイントを使用して、資格情報設定を作成する必要があります。

グローバル認証システムを使用する場合、（新しい宛先設定の作成 `"authenticationRule":"PLATFORM_AUTHENTICATION"` 時に、[&#x200B; 宛先配信 &#x200B;](../functionality/destination-configuration/destination-delivery.md) 設定で [&#x200B; を設定する必要が &#x200B;](../authoring-api/destination-configuration/create-destination-configuration.md) ります。 次に、[&#x200B; 資格情報設定 &#x200B;](../credentials-api/create-credential-configuration.md) を作成し、`authenticationId` 宛先配信 [&#x200B; 設定の &#x200B;](/help/destinations/destination-sdk/functionality/destination-configuration/destination-delivery.md#platform-authentication) パラメーターで資格情報オブジェクトの ID を渡す必要があります。

>[!IMPORTANT]
>
>Destination SDK でサポートされているすべてのパラメーター名および値は、**大文字と小文字が区別**&#x200B;されます。大文字と小文字を区別することに関するエラーを避けるために、ドキュメントに示すように、パラメーター名および値を正確に使用してください。

## 資格情報 API 操作の概要 {#get-started}

続行する前に、「[はじめる前に](../getting-started.md)」を参照し、API の呼び出しを正常に行うために必要となる重要な情報（必要な宛先オーサリング権限および必要なヘッダーの取得方法など）を確認してください。

## 資格情報設定の取得 {#retrieve}

`/authoring/credentials` エンドポイントに `GET` リクエストを行うことで、[既存の](create-credential-configuration.md)資格情報設定を取得できます。

**API 形式**

以下の API 形式を使用して、お使いのアカウントに関するすべての資格情報設定を取得します。

```http
GET /authoring/credentials
```

以下の API 形式を使用して、`{INSTANCE_ID}` パラメーターで定義された、特定の資格情報設定を取得します。

```http
GET /authoring/credentials/{INSTANCE_ID}
```

以下の 2 つのリクエストは、リクエストで `INSTANCE_ID` パラメーターを渡すかどうかに応じて、お客様の IMS 組織に対するすべての資格情報設定か、特定の資格情報設定を取得します。

以下の各タブを選択して、対応するペイロードを表示します。

>[!BEGINTABS]

>[!TAB すべての資格情報設定の取得]

+++リクエスト

```shell
curl -X GET https://platform.adobe.io/data/core/activation/authoring/credentials \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
```

+++

+++応答

応答が成功すると、HTTP ステータス 200 が、使用した [!DNL IMS Org ID] およびサンドボックス名に基づいた、アクセス権のある資格情報設定のリストと共に返されます。1 つの `instanceId` は、1 つの資格情報設定に対応します。

```json
{
   "instanceId":"n55affa0-3747-4030-895d-1d1236bb3680",
   "createdDate":"2021-06-07T06:41:48.641943Z",
   "lastModifiedDate":"2021-06-07T06:41:48.641943Z",
   "type":"s3Authentication",
   "name":"yourdestination",
   "s3Authentication":{
      "accessId":"string",
      "secretKey":"string"
   }
},
{
   "instanceId":"a25bffa0-3127-4030-895d-1d1236bb3680",
   "createdDate":"2022-06-07T06:41:48.641943Z",
   "lastModifiedDate":"2022-08-07T06:41:48.641943Z",
   "type":"basic",
   "name":"yourdestination",
   "s3Authentication":{
      "url":"string",
      "username":"string",
      "password":"string"
   }
}
```

+++

>[!TAB 特定の資格情報設定の取得]

+++リクエスト

```shell
curl -X GET https://platform.adobe.io/data/core/activation/authoring/credentials/{INSTANCE_ID} \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

| パラメーター | 説明 |
| -------- | ----------- |
| `{INSTANCE_ID}` | 取得する資格情報設定の ID。 |

+++

+++応答

応答が成功すると、HTTP ステータス 200 が、リクエストで提供された `instanceId` に対応する資格情報設定の詳細と共に返されます。

```json
{
   "instanceId":"n55affa0-3747-4030-895d-1d1236bb3680",
   "createdDate":"2021-06-07T06:41:48.641943Z",
   "lastModifiedDate":"2021-06-07T06:41:48.641943Z",
   "type":"s3Authentication",
   "name":"yourdestination",
   "s3Authentication":{
      "accessId":"string",
      "secretKey":"string"
   }
}
```

+++

>[!ENDTABS]

## API エラー処理 {#error-handling}

Destination SDK API エンドポイントは、一般的な Experience Platform API エラーメッセージの原則に従います。Experience Platform トラブルシューティングガイドの [API ステータスコード &#x200B;](../../../landing/troubleshooting.md#api-status-codes) および [&#x200B; リクエストヘッダーエラー &#x200B;](../../../landing/troubleshooting.md#request-header-errors) を参照してください。

## 次の手順 {#next-steps}

このドキュメントでは、`/authoring/credentials` API エンドポイントを使用した、資格情報設定に関する詳細の取得方法を確認しました。この手順が宛先設定プロセスのどこに当てはまるかを把握するには、[Destination SDK を使用した宛先の設定方法](../guides/configure-destination-instructions.md)を参照してください。
