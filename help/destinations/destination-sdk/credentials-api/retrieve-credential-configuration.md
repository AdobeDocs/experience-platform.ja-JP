---
description: このページでは、Adobe Experience Platform Destination SDKを使用した秘密鍵証明書設定の取得に使用する API 呼び出しの例を示します。
title: 資格情報設定の取得
source-git-commit: 9e1ae44f83b886f0b5dd5a9fc9cd9b7db6154ff0
workflow-type: tm+mt
source-wordcount: '475'
ht-degree: 28%

---


# 資格情報設定の取得

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

## 資格情報設定の取得 {#retrieve}

次の [既存](create-credential-configuration.md) 資格情報の設定 `GET` にリクエスト `/authoring/credentials` endpoint.

**API 形式**

次の API 形式を使用して、アカウントのすべての資格情報設定を取得します。

```http
GET /authoring/credentials
```

次の API 形式を使用して、 `{INSTANCE_ID}` パラメーター。

```http
GET /authoring/credentials/{INSTANCE_ID}
```

次の 2 つのリクエストでは、IMS 組織のすべての資格情報設定を取得するか、 `INSTANCE_ID` パラメーターを指定します。

下の各タブを選択し、対応するペイロードを表示します。

>[!BEGINTABS]

>[!TAB すべての資格情報設定を取得]

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

正常な応答は、HTTP ステータス 200 と、アクセス権のある資格情報設定のリストを、 [!DNL IMS Org ID] および使用したサンドボックス名。 1 `instanceId` は、1 つの資格情報設定に対応します。

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

>[!TAB 特定の秘密鍵証明書設定の取得]

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
| `{INSTANCE_ID}` | 取得する秘密鍵証明書設定の ID。 |

+++

+++応答

正常な応答は、HTTP ステータス 200 と、 `instanceId` リクエストに基づいて提供されます。

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

Destination SDK API エンドポイントは、一般的な Experience Platform API エラーメッセージの原則に従います。Platform トラブルシューティングガイドの [API ステータスコード](../../../landing/troubleshooting.md#api-status-codes)および[リクエストヘッダーエラー](../../../landing/troubleshooting.md#request-header-errors)を参照してください。

## 次の手順 {#next-steps}

このドキュメントを読んだ後、 `/authoring/credentials` API エンドポイント。 [Destination SDK を使用して宛先を設定する方法](../guides/configure-destination-instructions.md)を参照して、この手順が宛先を設定するプロセスの中でどのように位置づけられるかを把握します。
