---
description: Adobe Experience Platform Destination SDK でサポートされている認証設定を使用してユーザーを認証し、宛先エンドポイントに対してデータを有効化します。
title: 認証設定
exl-id: 33eaab24-f867-4744-b424-4ba71727373c
source-git-commit: 92bca3600d854540fd2badd925e453fba41601a7
workflow-type: ht
source-wordcount: '564'
ht-degree: 100%

---

# 認証設定 {#credentials}

## サポートしている認証タイプ {#supported-authentication-types}

選択する認証設定によって、Platform の UI での宛先に対する Experience Platform の認証方法が決まります。

Adobe Experience Platform Destination SDK は、次の複数の認証タイプをサポートしています。

* ベアラー認証
* （ベータ版）Amazon S3 認証
* （ベータ版）Azure 接続文字列
* （ベータ版）Azure サービスプリンシパル
* （ベータ版）SSH キーを使用した SFTP
* （ベータ版）パスワードを使用した SFTP
* 認証コードを使用した OAuth 2
* パスワード付与を使用した OAuth 2
* クライアント資格情報の付与を使用した OAuth 2

宛先の認証情報は、`/destinations` エンドポイントの `customerAuthenticationConfigurations` パラメーターを介して設定できます。

各タイプの宛先の認証設定の詳細については、次の節を参照してください。

* [ストリーミング宛先の認証設定](destination-configuration.md#customer-authentication-configurations)
* [ファイルベースの宛先の認証設定](file-based-destination-configuration.md#customer-authentication-configurations)

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

## （ベータ版）[!DNL Amazon S3] 認証 {#s3}

Experience Platform では、[!DNL Amazon S3] 認証がファイルベースの宛先に対してサポートされています。

>[!IMPORTANT]
>
>現在、Adobe Experience Platform Destination SDK でのファイルベースの宛先のサポートはベータ版です。ドキュメントと機能は変更される場合があります。

宛先に Amazon S3 認証を設定するには、エンドポイント `/destinations` の `customerAuthenticationConfigurations` パラメーターを次のように設定します。

```json
   "customerAuthenticationConfigurations":[
      {
         "authType":"S3"
      }
   ]
```

## （ベータ版）[!DNL Azure Blob Storage] {#blob}

Experience Platform では、[!DNL Azure Blob Storage] 認証がファイルベースの宛先に対してサポートされています。

>[!IMPORTANT]
>
>現在、Adobe Experience Platform Destination SDK でのファイルベースの宛先のサポートはベータ版です。ドキュメントと機能は変更される場合があります。

[!DNL Azure Blob] 認証を宛先に設定するには、エンドポイント `/destinations` の `customerAuthenticationConfigurations` パラメーターを次のように設定します。

```json
   "customerAuthenticationConfigurations":[
     {
        "authType":"AZURE_CONNECTION_STRING"
     }
  ]
```

## （ベータ版）[!DNL Azure Data Lake Storage] {#adls}

Experience Platform では、[!DNL Azure Data Lake Storage] 認証がファイルベースの宛先に対してサポートされています。

>[!IMPORTANT]
>
>現在、Adobe Experience Platform Destination SDK でのファイルベースの宛先のサポートはベータ版です。ドキュメントと機能は変更される場合があります。

[!DNL Azure Data Lake Storage]（ADLS）認証を宛先に設定するには、エンドポイント `/destinations` の `customerAuthenticationConfigurations` パラメーターを次のように設定します。

```json
   "customerAuthenticationConfigurations":[
     {
        "authType":"AZURE_SERVICE_PRINCIPAL"
     }
  ]
```

## （ベータ版）[!DNL SSH] キーを使用した [!DNL SFTP] 認証 {#sftp-ssh}

Experience Platform では、[!DNL SSH] キーを使用した [!DNL SFTP] 認証がファイルベースの宛先に対してサポートされています。

>[!IMPORTANT]
>
>現在、Adobe Experience Platform Destination SDK でのファイルベースの宛先のサポートはベータ版です。ドキュメントと機能は変更される場合があります。

SSH キーを使用した SFTP 認証を宛先に設定するには、エンドポイント `/destinations` の `customerAuthenticationConfigurations` パラメーターを次のように設定します。

```json
   "customerAuthenticationConfigurations":[
      {
         "authType":"SFTP_WITH_SSH_KEY"
      }
   ]
```

## （ベータ版）パスワードを使用した [!DNL SFTP] 認証 {#sftp-password}

Experience Platform では、パスワードを使用した [!DNL SFTP] 認証がファイルベースの宛先に対してサポートされています。

>[!IMPORTANT]
>
>現在、Adobe Experience Platform Destination SDK でのファイルベースの宛先のサポートはベータ版です。ドキュメントと機能は変更される場合があります。

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

サポートしている様々な OAuth 2 フローの設定方法とカスタム OAuth 2 のサポートについて詳しくは、[OAuth 2 認証](./oauth2-authentication.md)の Destination SDK ドキュメントを参照してください。


## `/credentials` API エンドポイントを使用するタイミング {#when-to-use}

>[!IMPORTANT]
>
>ほとんどの場合、`/credentials` API エンドポイントを使用する必要は&#x200B;*ありません*。代わりに、エンドポイント `/destinations` の `customerAuthenticationConfigurations` パラメーターを介して宛先の認証情報を設定できます。

アドビと宛先の間にグローバル認証システムがあり、宛先に接続するための認証資格情報を [!DNL Platform] の顧客が提供する必要がない場合は、`/credentials` API エンドポイントが宛先デベロッパーに対して提供されています。

この場合、`/credentials` API エンドポイントを使用して、資格情報オブジェクトを作成する必要があります。また、[宛先設定](./destination-configuration.md#destination-delivery)で `PLATFORM_AUTHENTICATION` を選択する必要があります。`/credentials` エンドポイントで実行できる操作の完全な一覧については、[資格情報 API エンドポイントの操作](./credentials-configuration-api.md)を参照してください。
