---
description: このページでは、秘密鍵証明書の設定Adobe Experience Platform Destination SDKの作成に使用する API 呼び出しの例を示します。
title: 資格情報設定の作成
source-git-commit: 9e1ae44f83b886f0b5dd5a9fc9cd9b7db6154ff0
workflow-type: tm+mt
source-wordcount: '559'
ht-degree: 49%

---


# 資格情報設定の作成

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

続行する前に、[入門ガイド](../getting-started.md)で、必要な宛先作成許可やヘッダーの取得方法など、API に対する呼び出しを正常に行うためにに知っておく必要がある、重要な情報を確認しておいてください。

## 認証情報設定の作成 {#create}

新しい資格情報設定を作成するには、 `POST` にリクエスト `/authoring/credentials` endpoint.

**API 形式**

```http
POST /authoring/credentials
```

次のリクエストでは、ペイロードで指定されたパラメーターで定義された新しい資格情報設定を作成します。

下の各タブを選択し、対応するペイロードを表示します。

>[!BEGINTABS]

>[!TAB 基本]

**基本的な秘密鍵証明書設定の作成**

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
| `username` | 文字列 | 認証情報設定ログインユーザー名 |
| `password` | 文字列 | 認証情報構成のログインパスワード |

{style="table-layout:auto"}

+++

+++応答

リクエストが成功した場合は、新しく作成した資格情報の構成の詳細とともに、HTTP ステータス 200 が返されます。

+++

>[!TAB Amazon S3]

**の作成 [!DNL Amazon S3] 資格情報設定**

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

リクエストが成功した場合は、新しく作成した資格情報の構成の詳細とともに、HTTP ステータス 200 が返されます。

+++

>[!TAB SSH]

**SSH 秘密鍵証明書設定の作成**

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
| `username` | 文字列 | 認証情報設定ログインユーザー名 |
| `sshKey` | 文字列 | SSH 認証を使用した SFTP 用の SSH キー |

{style="table-layout:auto"}

+++

+++応答

リクエストが成功した場合は、新しく作成した資格情報の構成の詳細とともに、HTTP ステータス 200 が返されます。

+++

>[!TAB Azure Data Lake Storage]

**の作成 [!DNL Azure Data Lake Storage] 資格情報設定**

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

リクエストが成功した場合は、新しく作成した資格情報の構成の詳細とともに、HTTP ステータス 200 が返されます。

+++

>[!TAB Azure Blob ストレージ]

**の作成 [!DNL Azure Blob Storage] 資格情報設定**

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

リクエストが成功した場合は、新しく作成した資格情報の構成の詳細とともに、HTTP ステータス 200 が返されます。

+++

>[!ENDTABS]

## API エラー処理 {#error-handling}

Destination SDK API エンドポイントは、一般的な Experience Platform API エラーメッセージの原則に従います。Platform トラブルシューティングガイドの [API ステータスコード](../../../landing/troubleshooting.md#api-status-codes)および[リクエストヘッダーエラー](../../../landing/troubleshooting.md#request-header-errors)を参照してください。

## 次の手順 {#next-steps}

このドキュメントを読むと、資格情報エンドポイントを使用するタイミングと、 `/authoring/credentials` API エンドポイント読み取り [宛先の設定にDestination SDKを使用する方法](../guides/configure-destination-instructions.md) を参照して、この手順が宛先を設定するプロセスに適した場所を把握します。
