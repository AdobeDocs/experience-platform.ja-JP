---
description: Adobe Experience Platform Destination SDK でサポートされている認証設定を使用して、ユーザーを認証し、宛先エンドポイントに対してデータをアクティブ化します。
title: 認証設定
exl-id: 33eaab24-f867-4744-b424-4ba71727373c
source-git-commit: e6d922800c17312df8529061c56d8a2deac46662
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 0%

---

# 認証設定 {#credentials}

## サポートされる認証タイプ {#supported-authentication-types}

Adobe Experience Platform Destination SDK は、次の複数の認証タイプをサポートしています。

* Bearer 認証
* 認証コードを含む OAuth 2
* OAUth 2（パスワード付き）
* クライアント資格情報付きの OAuth 2 が付与されました

宛先の認証情報は、 `customerAuthenticationConfigurations` のパラメーター `/destinations` endpoint. 詳しくは、 [顧客認証設定セクション](./destination-configuration.md#customer-authentication-configurations) 各認証タイプの設定に関する詳細は、「宛先の設定」の記事と以下の節を参照してください。

## Bearer 認証 {#bearer}

宛先に対して bearer タイプの認証を設定するには、 `customerAuthenticationConfigurations` パラメーター `/destinations` エンドポイントに次のように表示されます。

```json
   "customerAuthenticationConfigurations":[
      {
         "authType":"BEARER"
      }
   ]
```

## OAuth 2 認証 {#oauth2}

サポートされる様々な OAuth 2 フローの設定方法とカスタム OAuth 2 のサポートについては、 [OAuth 2 認証](./oauth2-authentication.md).


## 使用するタイミング `/credentials` API エンドポイント {#when-to-use}

>[!IMPORTANT]
>
>ほとんどの場合、 *しない* 使用する必要がある `/credentials` API エンドポイント。 代わりに、 `customerAuthenticationConfigurations` のパラメーター `/destinations` endpoint.

この `/credentials` Adobeと宛先の間にグローバル認証システムがある場合、API エンドポイントは、宛先の開発者に対して提供されます。 [!DNL Platform] のお客様は、宛先に接続するための認証資格情報を提供する必要はありません。

この場合、 `/credentials` API エンドポイント。 また、 `PLATFORM_AUTHENTICATION` 内 [宛先設定](./destination-configuration.md#destination-delivery). 読み取り [資格情報 API エンドポイントの操作](./credentials-configuration-api.md) : `/credentials` endpoint.
