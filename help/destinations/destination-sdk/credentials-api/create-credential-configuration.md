---
description: このページでは、Adobe Experience Platform Destination SDK で、資格情報設定を作成するために使用される API 呼び出しの例を示します。
title: 資格情報設定の作成
exl-id: 9844c9c5-d2dc-4d4b-ae93-759bf23b87fa
source-git-commit: b4334b4f73428f94f5a7e5088f98e2459afcaf3c
workflow-type: tm+mt
source-wordcount: '559'
ht-degree: 100%

---

# 資格情報設定の作成

>[!IMPORTANT]
>
>**API エンドポイント**：`platform.adobe.io/data/core/activation/authoring/credentials`

このページでは、`/authoring/credentials` API エンドポイントを使用して、資格情報設定を作成するために使用できる API リクエストおよびペイロードの例を示します。

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

## 資格情報設定の作成 {#create}

`/authoring/credentials` エンドポイントに `POST` リクエストを行うことで、新しい資格情報設定を作成できます。

**API 形式**

```http
POST /authoring/credentials
```

以下のリクエストは、ペイロードで提供されるパラメーターの定義に基づいて、新しい資格情報設定を作成します。

以下の各タブを選択して、対応するペイロードを表示します。

>[!BEGINTABS]

>[!TAB 基本]

**基本資格情報設定の作成**

+++リクエスト

```shell
curl -X POST https://platform.adobe.io/data/core/activation/authoring/credentials \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d '
{
    "basicAuthentication":{
      "url":"string",
      "username":"string",
      "password":"string"
   }
}
```

| パラメーター | タイプ | 説明 |
| -------- | ----------- | ----------- |
| `url` | 文字列 | 認証プロバイダーの URL |
| `username` | 文字列 | 資格情報設定のログインユーザー名 |
| `password` | 文字列 | 資格情報設定のログインパスワード |

{style="table-layout:auto"}

+++

+++応答

応答が成功すると、HTTP ステータス 200 が、新しく作成された資格情報設定の詳細と共に返されます。

+++

>[!TAB Amazon S3]

**[!DNL Amazon S3] 資格情報設定の作成**

+++**リクエスト**

```shell
curl -X POST https://platform.adobe.io/data/core/activation/authoring/credentials \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d '
{
   "s3Authentication":{
      "accessId":"string",
      "secretKey":"string"
   }
}
```

| パラメーター | タイプ | 説明 |
| -------- | ----------- | ----------- |
| `accessId` | 文字列 | [!DNL Amazon S3] アクセス ID |
| `secretKey` | 文字列 | [!DNL Amazon S3] 秘密鍵 |

{style="table-layout:auto"}

+++

+++応答

応答が成功すると、HTTP ステータス 200 が、新しく作成された資格情報設定の詳細と共に返されます。

+++

>[!TAB SSH]

**SSH 資格情報設定の作成**

+++リクエスト

```shell
curl -X POST https://platform.adobe.io/data/core/activation/authoring/credentials \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d '
{
    "sshAuthentication":{
      "username":"string",
      "sshKey":"string"
   }
}
```

| パラメーター | タイプ | 説明 |
| -------- | ----------- | ----------- |
| `username` | 文字列 | 資格情報設定のログインユーザー名 |
| `sshKey` | 文字列 | SSH 認証を使用した SFTP 用の SSH キー |

{style="table-layout:auto"}

+++

+++応答

応答が成功すると、HTTP ステータス 200 が、新しく作成された資格情報設定の詳細と共に返されます。

+++

>[!TAB Azure Data Lake Storage]

**[!DNL Azure Data Lake Storage] 資格情報設定の作成**

+++リクエスト

```shell
curl -X POST https://platform.adobe.io/data/core/activation/authoring/credentials \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d '
{
    "azureAuthentication":{
      "url":"string",
      "tenant":"string",
      "servicePrincipalId":"string",
      "servicePrincipalKey":"string"
   }
}
```

| パラメーター | タイプ | 説明 |
| -------- | ----------- | ----------- |
| `url` | 文字列 | 認証プロバイダーの URL |
| `tenant` | 文字列 | Azure Data Lake Storage のテナント |
| `servicePrincipalId` | 文字列 | Azure Data Lake Storage の Azure サービスプリンシパル ID |
| `servicePrincipalKey` | 文字列 | Azure Data Lake Storage の Azure サービスプリンシパルキー |

{style="table-layout:auto"}

+++

+++応答

応答が成功すると、HTTP ステータス 200 が、新しく作成された資格情報設定の詳細と共に返されます。

+++

>[!TAB Azure Blob Storage]

**[!DNL Azure Blob Storage] 資格情報設定の作成**

+++リクエスト

```shell
curl -X POST https://platform.adobe.io/data/core/activation/authoring/credentials \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d '
{
    "azureConnectionStringAuthentication":{
      "connectionString":"string"
   }
}
```

| パラメーター | タイプ | 説明 |
| -------- | ----------- | ----------- |
| `connectionString` | 文字列 | [!DNL Azure Blob Storage] 接続文字列 |

{style="table-layout:auto"}

+++

+++応答

応答が成功すると、HTTP ステータス 200 が、新しく作成された資格情報設定の詳細と共に返されます。

+++

>[!ENDTABS]

## API エラー処理 {#error-handling}

Destination SDK API エンドポイントは、一般的な Experience Platform API エラーメッセージの原則に従います。Platform トラブルシューティングガイドの [API ステータスコード](../../../landing/troubleshooting.md#api-status-codes)および[リクエストヘッダーエラー](../../../landing/troubleshooting.md#request-header-errors)を参照してください。

## 次の手順 {#next-steps}

このドキュメントでは、資格情報エンドポイントを使用するタイミングと、`/authoring/credentials` API エンドポイントを使用した資格情報設定の設定方法を確認しました。この手順が宛先設定プロセスのどこに当てはまるかを把握するには、[Destination SDK を使用した宛先の設定方法](../guides/configure-destination-instructions.md)を参照してください。
