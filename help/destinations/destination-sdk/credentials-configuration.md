---
description: この設定は、データをアクティブ化するためにAdobe Experience Platformユーザーが宛先エンドポイントに対して認証する方法を決定します。
title: 宛先SDKの資格情報の設定オプション
source-git-commit: 11f6421665acc2041aa9483b1e0efb6fe48b6dfb
workflow-type: tm+mt
source-wordcount: '200'
ht-degree: 0%

---

# 認証の設定 {#credentials}

## サポートされる認証の種類 {#supported-authentication-types}

Adobe Experience Platformは、複数の認証タイプをサポートします。

* ベアラ認証
* 認証コードを含むOAuth 2
* OAUth 2（パスワード付与）
* クライアント資格情報付きOAuth 2付与

様々なサポートされるOAuth 2のフローと、カスタムOAuth 2のサポートについては、 [OAuth 2認証](./oauth2-authentication.md)の宛先SDKのドキュメントをお読みください。

`/destinations`エンドポイントの`customerAuthenticationConfigurations`パラメーターを使用して、宛先の認証情報を設定できます。 宛先の設定に関する記事の[顧客認証の設定の節](./destination-configuration.md#customer-authentication-configurations)を参照してください。

## `/credentials` APIエンドポイントを使用するタイミング {#when-to-use}

>[!IMPORTANT]
>
>ほとんどの場合、*APIエンドポイントを`/credentials`使用する必要は*&#x200B;ありません。 代わりに、`/destinations`エンドポイントの`customerAuthenticationConfigurations`パラメーターを使用して、宛先の認証情報を設定できます。

Adobeと宛先の間にグローバル認証システムがあり、[!DNL Platform]顧客が宛先に接続するための認証資格情報を提供する必要がない場合は、`activation/authoring/credentials` APIエンドポイントを使用し、[宛先設定](./destination-configuration.md#destination-delivery)で`PLATFORM_AUTHENTICATION`を選択します。 この場合、`/credentials` APIエンドポイントを使用してcredentialsオブジェクトを作成する必要があります。 [Credentials APIエンドポイントの操作](./credentials-configuration-api.md)を読み、`/credentials`エンドポイントで実行できる操作の完全なリストを確認します。