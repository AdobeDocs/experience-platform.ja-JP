---
description: ユーザーを認証し、宛先エンドポイントに対してAdobe Experience Platform Destination SDKをアクティブ化するには、データでサポートされている認証設定を使用します。
title: 認証設定
exl-id: 33eaab24-f867-4744-b424-4ba71727373c
source-git-commit: 92bca3600d854540fd2badd925e453fba41601a7
workflow-type: tm+mt
source-wordcount: '564'
ht-degree: 7%

---

# 認証設定 {#credentials}

## サポートされる認証タイプ {#supported-authentication-types}

選択する認証設定によって、Platform UI での宛先に対するExperience Platformの認証方法が決まります。

Adobe Experience Platform Destination SDKは、次の複数の認証タイプをサポートします。

* Bearer 認証
* （ベータ版）Amazon S3 認証
* （ベータ版）Azure 接続文字列
* （ベータ版）Azure サービスプリンシパル
* （ベータ版）SSH キーを使用した SFTP
* （ベータ版）SFTP とパスワード
* 認証コードを含む OAuth 2
* OAUth 2（パスワード付き）
* クライアント資格情報付きの OAuth 2 が付与されました

宛先の認証情報は、 `customerAuthenticationConfigurations` のパラメーター `/destinations` endpoint.

各タイプの宛先の認証設定の詳細については、次の節を参照してください。

* [ストリーミング宛先の認証設定](destination-configuration.md#customer-authentication-configurations)
* [ファイルベースの宛先の認証設定](file-based-destination-configuration.md#customer-authentication-configurations)

## Bearer 認証 {#bearer}

Bearer 認証は、Bearer でのストリーミング先に対してサポートされます。Experience Platform。

宛先に bearer タイプの認証を設定するには、 `customerAuthenticationConfigurations` パラメーター `/destinations` エンドポイントに次のように表示されます。

```json
   "customerAuthenticationConfigurations":[
      {
         "authType":"BEARER"
      }
   ]
```

## （ベータ版） [!DNL Amazon S3] 認証 {#s3}

[!DNL Amazon S3] 認証は、認証でのファイルベースの宛先に対してサポートされます。Experience Platform

>[!IMPORTANT]
>
>現在、Adobe Experience Platform Destination SDKでのファイルベースの宛先のサポートはベータ版です。 ドキュメントと機能は変更される場合があります。

宛先のAmazon S3 認証を設定するには、 `customerAuthenticationConfigurations` パラメーター `/destinations` エンドポイントに次のように表示されます。

```json
   "customerAuthenticationConfigurations":[
      {
         "authType":"S3"
      }
   ]
```

## (ベータ) [!DNL Azure Blob Storage] {#blob}

[!DNL Azure Blob Storage] 認証は、認証でのファイルベースの宛先に対してサポートされます。Experience Platform

>[!IMPORTANT]
>
>現在、Adobe Experience Platform Destination SDKでのファイルベースの宛先のサポートはベータ版です。 ドキュメントと機能は変更される場合があります。

次の手順でを設定します。 [!DNL Azure Blob] 宛先の認証について、 `customerAuthenticationConfigurations` パラメーター `/destinations` エンドポイントに次のように表示されます。

```json
   "customerAuthenticationConfigurations":[
     {
        "authType":"AZURE_CONNECTION_STRING"
     }
  ]
```

## (ベータ) [!DNL Azure Data Lake Storage] {#adls}

[!DNL Azure Data Lake Storage] 認証は、認証でのファイルベースの宛先に対してサポートされます。Experience Platform

>[!IMPORTANT]
>
>現在、Adobe Experience Platform Destination SDKでのファイルベースの宛先のサポートはベータ版です。 ドキュメントと機能は変更される場合があります。

次の手順でを設定します。 [!DNL Azure Data Lake Storage] (ADLS) 認証を宛先に対しておこない、 `customerAuthenticationConfigurations` パラメーター `/destinations` エンドポイントに次のように表示されます。

```json
   "customerAuthenticationConfigurations":[
     {
        "authType":"AZURE_SERVICE_PRINCIPAL"
     }
  ]
```

## （ベータ版） [!DNL SFTP] 認証 [!DNL SSH] key {#sftp-ssh}

[!DNL SFTP] 認証 [!DNL SSH] キーは、Experience Platformでのファイルベースの宛先でサポートされます。

>[!IMPORTANT]
>
>現在、Adobe Experience Platform Destination SDKでのファイルベースの宛先のサポートはベータ版です。 ドキュメントと機能は変更される場合があります。

宛先の SSH キーを使用した SFTP 認証を設定するには、 `customerAuthenticationConfigurations` パラメーター `/destinations` エンドポイントに次のように表示されます。

```json
   "customerAuthenticationConfigurations":[
      {
         "authType":"SFTP_WITH_SSH_KEY"
      }
   ]
```

## （ベータ版） [!DNL SFTP] パスワードによる認証 {#sftp-password}

[!DNL SFTP] パスワードによる認証は、Experience Platformのファイルベースの宛先でサポートされます。

>[!IMPORTANT]
>
>現在、Adobe Experience Platform Destination SDKでのファイルベースの宛先のサポートはベータ版です。 ドキュメントと機能は変更される場合があります。

宛先のパスワードを使用した SFTP 認証を設定するには、 `customerAuthenticationConfigurations` パラメーター `/destinations` エンドポイントに次のように表示されます。

```json
   "customerAuthenticationConfigurations":[
      {
         "authType":"SFTP_WITH_PASSWORD"
      }
   ]
```

## [!DNL OAuth 2] 認証 {#oauth2}

[!DNL OAuth 2] 認証は、認証でのストリーミング宛先に対してExperience Platformされます。

様々なサポートされる OAuth 2 フローの設定方法とカスタム OAuth 2 サポートの詳細については、次のDestination SDKドキュメントを参照してください。 [OAuth 2 認証](./oauth2-authentication.md).


## 使用するタイミング `/credentials` API エンドポイント {#when-to-use}

>[!IMPORTANT]
>
>ほとんどの場合、 *しない* 使用する必要がある `/credentials` API エンドポイント。 代わりに、 `customerAuthenticationConfigurations` のパラメーター `/destinations` endpoint.

この `/credentials` Adobeと宛先の間にグローバル認証システムがある場合、および [!DNL Platform] のお客様は、宛先に接続するための認証資格情報を提供する必要はありません。

この場合、 `/credentials` API エンドポイント。 また、 `PLATFORM_AUTHENTICATION` 内 [宛先設定](./destination-configuration.md#destination-delivery). 読み取り [資格情報 API エンドポイントの操作](./credentials-configuration-api.md) : `/credentials` endpoint.
