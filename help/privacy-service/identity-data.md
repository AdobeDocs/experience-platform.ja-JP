---
keywords: Experience Platform;home;popular topics;ECID;ecid
solution: Experience Platform
title: プライバシーリクエストの ID データ
topic: overview
description: このドキュメントでは、データ操作を設定し、アドビのテクノロジーを活用して、顧客のプライバシー要求に適した ID 情報を効果的に取得する方法について、一般的なガイダンスを提供しています。
translation-type: tm+mt
source-git-commit: 28b733a16b067f951a885c299d59e079f0074df8
workflow-type: tm+mt
source-wordcount: '643'
ht-degree: 45%

---


# プライバシーリクエストの ID データ

In order for Adobe Experience Platform [!DNL Privacy Service] to process customer requests for their private data (including access, delete, or opt-out-of-sale requests), it must be provided with unique identifiers that link a specific customer to their stored private data in your Adobe Experience Cloud enabled applications. [!DNL Privacy Service] はこれらの識別子を使用して、顧客の ID の下に保存されているすべてのデータを 内で収集し、顧客のリクエストに従って処理します。[!DNL Experience Cloud]

このドキュメントでは、データ操作を設定し、アドビのテクノロジーを活用して、顧客のプライバシー要求に適した ID 情報を効果的に取得する方法について、一般的なガイダンスを提供しています。

## ID と名前空間

顧客が複数の異なるチャネルを通じてブランドとやり取りできる場合、多数のインタラクションから記録される様々な識別子を調整するのは困難なことがあります。This in turn can make it difficult to determine which data belongs to a particular person in your [!DNL Experience Cloud] applications.

For example, when handling customer data requests in [!DNL Privacy Service], an identity may represent a cookie value set under an Adobe-controlled domain, a cookie value under a third-party domain and shared with Adobe, or a custom identifier that you explicitly define within your IMS Organization.

It is therefore required that each identity sent to [!DNL Privacy Service] is accompanied with a namespace that provides context by relating the identity value to its system of origin. 名前空間では、電子メールアドレス（「電子メール」）などの汎用概念を表したり、ID を特定のアプリケーションに関連付けたりすることができます（Adobe Advertising Cloud ID（「AdCloud」）、Adobe Target ID（「TNTID」）など）。

Adobe Experience Platform ID サービスは、グローバルに定義された、またユーザー定義の ID 名前空間を管理します。名前空間について詳しくは、[ID 名前空間の概要](../identity-service/namespaces.md)を参照してください。For a list of standard namespaces and namespace qualifiers that are commonly used in [!DNL Privacy Service], see the [appendix section](api/appendix.md) in the developer guide.

## ECID とオプトインサービス

Adobe Experience Cloud [!DNL Identity Service] serves as a common identification framework for [!DNL Experience Cloud], and assigns a unique, persistent ID to each site visitor. The [!DNL Experience Cloud] ID (ECID) tracks a customer&#39;s activity through the use of a first-party cookie, can uniquely identify a device across multiple applications, and allows you to identify the same site visitor and their data in different [!DNL Experience Cloud] applications. 詳しくは、[Experience Cloud ID サービスの概要](https://docs.adobe.com/content/help/ja-JP/id-service/using/intro/overview.html)を参照してください。

Opt-in Service, an extension of [!DNL Experience Cloud Identity Service], allows you set up protocols on your application to let visitors determine whether you can set a cookie on the visitor&#39;s device or browser. アプリケーションでのサービスの設定方法など、オプトインサービスについて詳しくは、[オプトインサービスに関するドキュメント](https://docs.adobe.com/content/help/ja-JP/id-service/using/implementation/opt-in-service/optin-overview.html)を参照してください。

Once your site visitors have been assigned ECIDs, you can utilize the Adobe [!DNL Privacy JavaScript Library] to retrieve those IDs for use in privacy requests, as described in the next section.

## [!DNL Privacy JS Library]

The [!DNL Adobe Privacy JavaScript Library] provides several functions that allow you to retrieve and remove customer identities that are stored in the browser. 複数のアドビアプリケーションから ECID を含む ID 情報を取得するように、ライブラリを設定できます。Through the use of callbacks or promises, you can programmatically handle successfully retrieved IDs and send them to the [!DNL Privacy Service] API.

For more information about [!DNL Privacy JS Library], including code samples for several common use cases, please refer to the [Privacy JS Library overview](js-library.md).

## 次の手順

このドキュメントでは、プライバシーリクエストで使用する顧客 ID データの取得に関する主要概念を簡単に説明しました。これらの概念とサービスについて詳しくは、各節に記載されているドキュメントへのリンクを確認することをお勧めします。For steps on how to send retrieved IDs to [!DNL Privacy Service] for creating access, delete, or opt-out-of-sale requests, see the [Privacy Service developer guide](api/getting-started.md).