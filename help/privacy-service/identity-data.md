---
keywords: Experience Platform;ホーム;人気のトピック;ECID;ecid
solution: Experience Platform
title: プライバシーリクエストの ID データ
description: このドキュメントでは、データ操作を設定し、アドビのテクノロジーを活用して、顧客のプライバシー要求に適した ID 情報を効果的に取得する方法について、一般的なガイダンスを提供しています。
exl-id: 43b0292a-ea4d-4858-b584-ba71029724f6
source-git-commit: ba39f62cd77acedb7bfc0081dbb5f59906c9b287
workflow-type: tm+mt
source-wordcount: '632'
ht-degree: 40%

---

# プライバシーリクエストの ID データ

Adobe Experience Platform [!DNL Privacy Service] がプライベートデータ（アクセス、削除、またはセールのオプトアウトリクエストを含む）に対する顧客リクエストを処理するには、Adobe Experience Cloud対応アプリケーションに保存されているプライベートデータに特定の顧客をリンクする一意の ID を提供する必要があります。 次に、[!DNL Privacy Service] はこれらの識別子を使用して、[!DNL Experience Cloud] 内で顧客の id に保存されているすべてのデータを収集し、顧客の要求に応じて処理します。

このドキュメントでは、データ操作を設定し、アドビのテクノロジーを活用して、顧客のプライバシー要求に適した ID 情報を効果的に取得する方法について、一般的なガイダンスを提供しています。

## ID と名前空間

顧客が複数の異なるチャネルを通じてブランドとやり取りできる場合、多数のインタラクションから記録される様々な識別子を紐付けるのは困難なことがあります。これにより、[!DNL Experience Cloud] アプリケーションの特定のユーザーに属するデータを判断するのが難しくなる可能性があります。

例えば、[!DNL Privacy Service] で顧客データリクエストを処理する場合、ID は、Adobeが制御するドメイン下で設定された Cookie 値、サードパーティドメイン下でAdobeと共有される Cookie 値、または組織内で明示的に定義するカスタム ID を表す場合があります。

したがって、[!DNL Privacy Service] に送信される各 ID は、ID 値を元のシステムに関連付けてコンテキストを提供する名前空間を伴う必要があります。 名前空間では、電子メールアドレス（「電子メール」）などの汎用概念を表したり、ID を特定のアプリケーションに関連付けたりすることができます（Adobe Advertising Cloud ID（「AdCloud」）、Adobe Target ID（「TNTID」）など）。

Adobe Experience Platform ID サービスは、グローバルに定義された、またユーザー定義の ID 名前空間を管理します。名前空間について詳しくは、[ID 名前空間の概要](../identity-service/features/namespaces.md)を参照してください。[!DNL Privacy Service] で一般的に使用される標準名前空間と名前空間修飾子のリストについては、API ガイドの [&#x200B; 付録の節 &#x200B;](api/appendix.md) を参照してください。

## ECID とオプトインサービス

Adobe Experience Cloud [!DNL Identity Service] は、[!DNL Experience Cloud] の共通の ID フレームワークとして機能し、各サイト訪問者に一意の永続的な ID を割り当てます。 [!DNL Experience Cloud] ID （ECID）は、ファーストパーティ cookie を使用して顧客のアクティビティを追跡し、複数のアプリケーションをまたいでデバイスを一意に識別できます。また、同じサイト訪問者と、異なる [!DNL Experience Cloud] アプリケーションにあるそのデータを識別できます。 詳しくは、[Experience Cloud ID サービスの概要](https://experienceleague.adobe.com/docs/id-service/using/intro/overview.html?lang=ja)を参照してください。

[!DNL Experience Cloud Identity Service] の拡張機能であるオプトインサービスを使用すると、アプリケーション上でプロトコルを設定し、訪問者のデバイスまたはブラウザーで cookie を設定できるかどうかを訪問者が決定できます。 アプリケーションでのサービスの設定方法など、オプトインサービスについて詳しくは、[オプトインサービスに関するドキュメント](https://experienceleague.adobe.com/docs/id-service/using/implementation/opt-in-service/optin-overview.html?lang=ja)を参照してください。

サイト訪問者に ECID が割り当てられたら、次の節で説明するように、Adobe [!DNL Privacy JavaScript Library] を使用してこれらの ID を取得し、プライバシーリクエストで使用できます。

## [!DNL Privacy JS Library]

[!DNL Adobe Privacy JavaScript Library] には、ブラウザーに保存されている顧客 ID を取得および削除できる機能がいくつか用意されています。 複数のアドビアプリケーションから ECID を含む ID 情報を取得するように、ライブラリを設定できます。コールバックまたは Promises を使用すると、正常に取得された ID をプログラムで処理して [!DNL Privacy Service] API に送信できます。

いくつかの一般的なユースケースのコードサンプルなど、[!DNL Privacy JS Library] について詳しくは、[&#x200B; プライバシー JS ライブラリの概要 &#x200B;](js-library.md) を参照してください。

## 次の手順

このドキュメントでは、プライバシーリクエストで使用する顧客 ID データの取得に関する主要概念を簡単に説明しました。これらの概念とサービスについて詳しくは、各節に記載されているドキュメントへのリンクを確認することをお勧めします。取得した ID を [!DNL Privacy Service] に送信して、アクセス、削除、販売のオプトアウトリクエストを作成する手順については、[Privacy ServiceAPI ガイド &#x200B;](api/overview.md) を参照してください。
