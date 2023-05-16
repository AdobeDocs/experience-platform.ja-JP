---
description: このページでは、Adobe Experience Platform Destination SDKを通じて既存の資格情報設定を更新するための API 呼び出しの例を示します。
title: 資格情報設定の更新
source-git-commit: 9e1ae44f83b886f0b5dd5a9fc9cd9b7db6154ff0
workflow-type: tm+mt
source-wordcount: '583'
ht-degree: 34%

---


# 資格情報設定の更新

>[!IMPORTANT]
>
>**API エンドポイント**：`platform.adobe.io/data/core/activation/authoring/credentials`

このページでは、 `/authoring/credentials` API エンドポイント。

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

## 資格情報設定の更新 {#update}

以下の項目を更新し、 [既存](create-credential-configuration.md) 資格情報の設定 `PUT` にリクエスト `/authoring/credentials` エンドポイントと、更新されたペイロード。

既存の秘密鍵証明書の設定と、それに対応する証明書の設定を取得するには `{INSTANCE_ID}`に関する記事を参照してください。 [資格情報設定の取得](retrieve-credential-configuration.md).

**API 形式**

```http
PUT /authoring/credentials/{INSTANCE_ID}
```

| パラメーター | 説明 |
| -------- | ----------- |
| `{INSTANCE_ID}` | 更新する秘密鍵証明書設定の ID。 既存の秘密鍵証明書の設定と、それに対応する証明書の設定を取得するには `{INSTANCE_ID}`を参照してください。 [資格情報設定の取得](retrieve-credential-configuration.md). |

次のリクエストは、ペイロードで指定されたパラメーターで定義された、既存の資格情報設定を更新します。

下の各タブを選択し、対応するペイロードを表示します。

>[!BEGINTABS]

>[!TAB 基本]

**基本的な秘密鍵証明書設定の更新**

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
| `username` | 文字列 | 認証情報設定ログインユーザー名 |
| `password` | 文字列 | 認証情報構成のログインパスワード |

{style="table-layout:auto"}

+++

+++応答

正常な応答は、更新された資格情報設定の詳細と共に HTTP ステータス 200 を返します。

+++

>[!TAB Amazon S3]

**の更新 [!DNL Amazon S3] 資格情報設定**

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

正常な応答は、更新された資格情報設定の詳細と共に HTTP ステータス 200 を返します。

+++

>[!TAB SSH]

**の更新 [!DNL SSH] 資格情報設定**

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
| `username` | 文字列 | 認証情報設定ログインユーザー名 |
| `sshKey` | 文字列 | [!DNL SSH] ～の鍵 [!DNL SFTP] と [!DNL SSH] 認証 |

{style="table-layout:auto"}

+++

+++応答

正常な応答は、更新された資格情報設定の詳細と共に HTTP ステータス 200 を返します。

+++

>[!TAB Azure Data Lake Storage]

**の更新 [!DNL Azure Data Lake Storage] 資格情報設定**

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
| `servicePrincipalId` | 文字列 | [!DNL Azure Service Principal] の ID [!DNL Azure Data Lake Storage] |
| `servicePrincipalKey` | 文字列 | [!DNL Azure Service Principal Key] for [!DNL Azure Data Lake Storage] |

{style="table-layout:auto"}

+++

+++応答

正常な応答は、更新された資格情報設定の詳細と共に HTTP ステータス 200 を返します。

+++

>[!TAB Azure Blob ストレージ]

**の更新 [!DNL Azure Blob] 資格情報設定**

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

正常な応答は、更新された資格情報設定の詳細と共に HTTP ステータス 200 を返します。

+++

>[!ENDTABS]

## API エラー処理 {#error-handling}

Destination SDK API エンドポイントは、一般的な Experience Platform API エラーメッセージの原則に従います。Platform トラブルシューティングガイドの [API ステータスコード](../../../landing/troubleshooting.md#api-status-codes)および[リクエストヘッダーエラー](../../../landing/troubleshooting.md#request-header-errors)を参照してください。

## 次の手順 {#next-steps}

このドキュメントを読んだ後、 `/authoring/credentials` API エンドポイント。 [Destination SDK を使用して宛先を設定する方法](../guides/configure-destination-instructions.md)を参照して、この手順が宛先を設定するプロセスの中でどのように位置づけられるかを把握します。
