---
description: この設定は、データをアクティブ化するためにAdobe Experience Platformユーザーが宛先エンドポイントに対して認証する方法を決定します。
title: 宛先 SDK での資格情報の設定オプション
exl-id: 33eaab24-f867-4744-b424-4ba71727373c
source-git-commit: 6ff5fd0e80f7ca1015969e91cc23c88251509b61
workflow-type: tm+mt
source-wordcount: '200'
ht-degree: 0%

---

# 認証の設定 {#credentials}

## サポートされる認証タイプ {#supported-authentication-types}

Adobe Experience Platformは、複数の認証タイプをサポートしています。

* ベアラ認証
* 認証コードを持つ OAuth 2
* OAUth 2（パスワード付き）
* クライアント資格情報付き OAuth 2 付与

サポートされる様々な OAuth 2 のフローとカスタム OAuth 2 のサポートについては、 [OAuth 2 認証 ](./oauth2-authentication.md) の宛先 SDK ドキュメントをお読みください。

`/destinations` エンドポイントの `customerAuthenticationConfigurations` パラメーターを使用して、宛先の認証情報を設定できます。 宛先の設定に関する記事の [ 顧客認証の設定の節 ](./destination-configuration.md#customer-authentication-configurations) を参照してください。

## `/credentials` API エンドポイントを使用するタイミング {#when-to-use}

>[!IMPORTANT]
>
>ほとんどの場合、*API エンドポイントを使用する必要は* ありません。`/credentials` 代わりに、`/destinations` エンドポイントの `customerAuthenticationConfigurations` パラメーターを使用して、宛先の認証情報を設定できます。

Adobeと宛先の間にグローバル認証システムがあり、[!DNL Platform] 顧客が宛先に接続するための認証資格情報を提供する必要がない場合は、`activation/authoring/credentials` API エンドポイントを使用し、[ 宛先設定 ](./destination-configuration.md#destination-delivery) で `PLATFORM_AUTHENTICATION` を選択します。 この場合、`/credentials` API エンドポイントを使用して資格情報オブジェクトを作成する必要があります。 [Credentials API エンドポイントの操作 ](./credentials-configuration-api.md) を読んで、`/credentials` エンドポイントで実行できる操作の完全なリストを確認してください。
