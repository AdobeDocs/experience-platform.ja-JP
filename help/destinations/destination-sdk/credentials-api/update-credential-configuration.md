---
description: このページでは、Adobe Experience Platform Destination SDK を通じて、既存の資格情報設定を更新するために使用される API 呼び出しの例を示します。
title: 資格情報設定の更新
exl-id: ebff370c-9189-48df-871f-ed0e1cd535c8
source-git-commit: b4334b4f73428f94f5a7e5088f98e2459afcaf3c
workflow-type: tm+mt
source-wordcount: '583'
ht-degree: 100%

---

# 資格情報設定の更新

>[!IMPORTANT]
>
>**API エンドポイント**：`platform.adobe.io/data/core/activation/authoring/credentials`

このページでは、`/authoring/credentials` API エンドポイントを使用して、既存の資格情報設定を更新するために使用できる API リクエストおよびペイロードの例を示します。

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

## 資格情報設定の更新 {#update}

更新されたペイロードで `/authoring/credentials` エンドポイントに `PUT` リクエストを行うことで、[既存の](create-credential-configuration.md)資格情報設定を更新できます。

既存の資格情報設定およびその関連する `{INSTANCE_ID}` を取得するには、[資格情報設定の取得](retrieve-credential-configuration.md)に関する記事を参照してください。

**API 形式**

```http
PUT /authoring/credentials/{INSTANCE_ID}
```

| パラメーター | 説明 |
| -------- | ----------- |
| `{INSTANCE_ID}` | 更新する資格情報設定の ID。既存の資格情報設定およびその関連する `{INSTANCE_ID}` を取得するには、[資格情報設定の取得](retrieve-credential-configuration.md)を参照してください。 |

以下のリクエストは、ペイロードで提供されるパラメーターの定義に基づいて、既存の資格情報設定を更新します。

以下の各タブを選択して、対応するペイロードを表示します。

>[!BEGINTABS]

>[!TAB 基本]

**基本資格情報設定の更新**

+++リクエスト

```shell
curl -X PUT https://platform.adobe.io/data/core/activation/authoring/credentials/{INSTANCE_ID} \
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

応答が成功すると、HTTP ステータス 200 が、更新された資格情報設定の詳細と共に返されます。

+++

>[!TAB Amazon S3]

**[!DNL Amazon S3] 資格情報設定の更新**

+++リクエスト

```shell
curl -X PUT https://platform.adobe.io/data/core/activation/authoring/credentials/{INSTANCE_ID} \
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

応答が成功すると、HTTP ステータス 200 が、更新された資格情報設定の詳細と共に返されます。

+++

>[!TAB SSH]

**[!DNL SSH] 資格情報設定の更新**

+++リクエスト

```shell
curl -X PUT https://platform.adobe.io/data/core/activation/authoring/credentials/{INSTANCE_ID} \
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
| `sshKey` | 文字列 | [!DNL SSH] 認証を使用した [!DNL SFTP] の [!DNL SSH] キー |

{style="table-layout:auto"}

+++

+++応答

応答が成功すると、HTTP ステータス 200 が、更新された資格情報設定の詳細と共に返されます。

+++

>[!TAB Azure Data Lake Storage]

**[!DNL Azure Data Lake Storage] 資格情報設定の更新**

+++リクエスト

```shell
curl -X PUT https://platform.adobe.io/data/core/activation/authoring/credentials/{INSTANCE_ID} \
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
| `servicePrincipalId` | 文字列 | [!DNL Azure Data Lake Storage] の [!DNL Azure Service Principal] ID |
| `servicePrincipalKey` | 文字列 | [!DNL Azure Service Principal Key] for [!DNL Azure Data Lake Storage] |

{style="table-layout:auto"}

+++

+++応答

応答が成功すると、HTTP ステータス 200 が、更新された資格情報設定の詳細と共に返されます。

+++

>[!TAB Azure Blob Storage]

**[!DNL Azure Blob] 資格情報設定の更新**

+++リクエスト

```shell
curl -X PUT https://platform.adobe.io/data/core/activation/authoring/credentials/{INSTANCE_ID} \
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

応答が成功すると、HTTP ステータス 200 が、更新された資格情報設定の詳細と共に返されます。

+++

>[!ENDTABS]

## API エラー処理 {#error-handling}

Destination SDK API エンドポイントは、一般的な Experience Platform API エラーメッセージの原則に従います。Platform トラブルシューティングガイドの [API ステータスコード](../../../landing/troubleshooting.md#api-status-codes)および[リクエストヘッダーエラー](../../../landing/troubleshooting.md#request-header-errors)を参照してください。

## 次の手順 {#next-steps}

このドキュメントでは、`/authoring/credentials` API エンドポイントを使用した、資格情報設定の更新方法を確認しました。この手順が宛先設定プロセスのどこに当てはまるかを把握するには、[Destination SDK を使用した宛先の設定方法](../guides/configure-destination-instructions.md)を参照してください。
