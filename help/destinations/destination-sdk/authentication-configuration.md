---
description: Adobe Experience Platform Destination SDK でサポートされている認証設定を使用してユーザーを認証し、宛先エンドポイントに対してデータを有効化します。
title: 認証設定
exl-id: 33eaab24-f867-4744-b424-4ba71727373c
source-git-commit: 59ac7749d788d8527da3578ec140248f7acf8e98
workflow-type: tm+mt
source-wordcount: '498'
ht-degree: 83%

---

# 認証設定 {#credentials}

## サポートしている認証タイプ {#supported-authentication-types}

選択する認証設定によって、Platform の UI での宛先に対する Experience Platform の認証方法が決まります。

Adobe Experience Platform Destination SDK は、次の複数の認証タイプをサポートしています。

* [ベアラー認証](#bearer)
* [基本認証](#basic)
* [[!DNL Amazon S3] 認証](#s3)
* [[!DNL Azure Blob] ストレージ](#blob)
* [[!DNL Azure Data Lake Storage]](#adls)
* [[!DNL Google Cloud Storage]](#gcs)
* [SSH キーを使用した SFTP](#sftp-ssh)
* [パスワード付き SFTP](#sftp-password)
* [認証コードを使用した OAuth 2](#oauth2)
* [パスワード付与を使用した OAuth 2](#oauth2)
* [クライアント資格情報の付与を使用した OAuth 2](#oauth2)

宛先の認証情報は、`/destinations` エンドポイントの `customerAuthenticationConfigurations` パラメーターを介して設定できます。

各タイプの宛先の認証設定の詳細については、次の節を参照してください。

* [ストリーミング宛先の認証設定](destination-configuration.md#customer-authentication-configurations)
* [ファイルベースの宛先の認証設定](file-based-destination-configuration.md#customer-authentication-configurations)

## 基本認証 {#basic}

基本認証は、Experience Platformのストリーミング先でサポートされます。

基本認証タイプを設定する場合、ユーザーは宛先に接続するためのユーザー名とパスワードを入力する必要があります。

宛先の基本認証を設定するには、 `customerAuthenticationConfigurations` セクション ( `/destinations` エンドポイントに次のように表示されます。

```json
"customerAuthenticationConfigurations":[
   {
      "authType":"BASIC"
   }
]
```

## ベアラー認証 {#bearer}

Experience Platform では、ストリーミング宛先に対してベアラー認証をサポートしています。

宛先にベアラータイプの認証を設定するには、`/destinations` エンドポイントの `customerAuthenticationConfigurations` パラメーターを次のように設定します。

```json
"customerAuthenticationConfigurations":[
   {
      "authType":"BEARER"
   }
]
```

## [!DNL Amazon S3] 認証 {#s3}

Experience Platform では、[!DNL Amazon S3] 認証がファイルベースの宛先に対してサポートされています。

[!DNL Amazon S3] 認証を宛先に設定するには、エンドポイント `/destinations` の `customerAuthenticationConfigurations` パラメーターを次のように設定します。

```json
"customerAuthenticationConfigurations":[
   {
      "authType":"S3"
   }
]
```

## [!DNL Azure Blob Storage] {#blob}

Experience Platform では、[!DNL Azure Blob Storage] 認証がファイルベースの宛先に対してサポートされています。

[!DNL Azure Blob] 認証を宛先に設定するには、エンドポイント `/destinations` の `customerAuthenticationConfigurations` パラメーターを次のように設定します。

```json
"customerAuthenticationConfigurations":[
   {
      "authType":"AZURE_CONNECTION_STRING"
   }
]
```

## [!DNL Azure Data Lake Storage] {#adls}

Experience Platform では、[!DNL Azure Data Lake Storage] 認証がファイルベースの宛先に対してサポートされています。

[!DNL Azure Data Lake Storage]（ADLS）認証を宛先に設定するには、エンドポイント `/destinations` の `customerAuthenticationConfigurations` パラメーターを次のように設定します。

```json
"customerAuthenticationConfigurations":[
   {
      "authType":"AZURE_SERVICE_PRINCIPAL"
   }
]
```

## [!DNL Google Cloud Storage] {#gcs}

Experience Platform では、[!DNL Google Cloud Storage] 認証がファイルベースの宛先に対してサポートされています。

```json
"customerAuthenticationConfigurations":[
   {
      "authType":"GOOGLE_CLOUD_STORAGE"
   }
]
```


## [!DNL SFTP] 認証 [!DNL SSH] key {#sftp-ssh}

Experience Platform では、[!DNL SSH] キーを使用した [!DNL SFTP] 認証がファイルベースの宛先に対してサポートされています。

SSH キーを使用した SFTP 認証を宛先に設定するには、エンドポイント `/destinations` の `customerAuthenticationConfigurations` パラメーターを次のように設定します。

```json
"customerAuthenticationConfigurations":[
   {
      "authType":"SFTP_WITH_SSH_KEY"
   }
]
```

## [!DNL SFTP] パスワードによる認証 {#sftp-password}

Experience Platform では、パスワードを使用した [!DNL SFTP] 認証がファイルベースの宛先に対してサポートされています。

パスワードを使用した SFTP 認証を宛先に設定するには、エンドポイント `/destinations` の `customerAuthenticationConfigurations` パラメーターを次のように設定します。

```json
"customerAuthenticationConfigurations":[
   {
      "authType":"SFTP_WITH_PASSWORD"
   }
]
```

## [!DNL OAuth 2] 認証 {#oauth2}

Experience Platform では、[!DNL OAuth 2] 認証がストリーミング宛先に対してサポートされています。

サポートされる各種の [!DNL OAuth 2] フロー、およびカスタム [!DNL OAuth 2] サポート、次のDestination SDKドキュメントを読む [[!DNL OAuth 2] 認証](./oauth2-authentication.md).


## `/credentials` API エンドポイントを使用するタイミング {#when-to-use}

>[!IMPORTANT]
>
>ほとんどの場合、`/credentials` API エンドポイントを使用する必要は&#x200B;*ありません*。代わりに、エンドポイント `/destinations` の `customerAuthenticationConfigurations` パラメーターを介して宛先の認証情報を設定できます。

アドビと宛先の間にグローバル認証システムがあり、宛先に接続するための認証資格情報を [!DNL Platform] の顧客が提供する必要がない場合は、`/credentials` API エンドポイントが宛先デベロッパーに対して提供されています。

この場合、`/credentials` API エンドポイントを使用して、資格情報オブジェクトを作成する必要があります。また、[宛先設定](./destination-configuration.md#destination-delivery)で `PLATFORM_AUTHENTICATION` を選択する必要があります。`/credentials` エンドポイントで実行できる操作の完全な一覧については、[資格情報 API エンドポイントの操作](./credentials-configuration-api.md)を参照してください。
