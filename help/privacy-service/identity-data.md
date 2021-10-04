---
keywords: Experience Platform；ホーム；人気のあるトピック；ECID;ecid
solution: Experience Platform
title: プライバシーリクエストの ID データ
topic-legacy: overview
description: このドキュメントでは、データ操作を設定し、アドビのテクノロジーを活用して、顧客のプライバシー要求に適した ID 情報を効果的に取得する方法について、一般的なガイダンスを提供しています。
exl-id: 43b0292a-ea4d-4858-b584-ba71029724f6
source-git-commit: d316c199c7e2d87d175015c1828af6fd0d57f32a
workflow-type: tm+mt
source-wordcount: '646'
ht-degree: 43%

---

# プライバシーリクエストの ID データ

Adobe Experience Platform [!DNL Privacy Service] で個人データに対する顧客の要求（アクセス、削除、販売オプトアウトの要求を含む）を処理するには、Adobe Experience Cloud対応アプリケーションで、保存された個人データに特定の顧客をリンクする一意の識別子を提供する必要があります。 [!DNL Privacy Service] はこれらの識別子を使用して、顧客の ID の下に保存されているすべてのデータを 内で収集し、顧客のリクエストに従って処理します。[!DNL Experience Cloud]

このドキュメントでは、データ操作を設定し、アドビのテクノロジーを活用して、顧客のプライバシー要求に適した ID 情報を効果的に取得する方法について、一般的なガイダンスを提供しています。

## ID と名前空間

顧客が複数の異なるチャネルを通じてブランドとやり取りできる場合、多数のインタラクションから記録される様々な識別子を調整するのは困難なことがあります。その結果、[!DNL Experience Cloud] アプリケーション内の特定のユーザーに属するデータを特定するのが困難になる場合があります。

例えば、[!DNL Privacy Service] で顧客データリクエストを処理する場合、ID は、Adobe制御ドメインの下に設定された Cookie 値、サードパーティドメインの下に設定され、Adobeと共有される Cookie 値、IMS 組織内で明示的に定義したカスタム識別子を表します。

したがって、[!DNL Privacy Service] に送信される各 ID には、ID 値を元のシステムに関連付けることでコンテキストを提供する名前空間を付ける必要があります。 名前空間では、電子メールアドレス（「電子メール」）などの汎用概念を表したり、ID を特定のアプリケーションに関連付けたりすることができます（Adobe Advertising Cloud ID（「AdCloud」）、Adobe Target ID（「TNTID」）など）。

Adobe Experience Platform ID サービスは、グローバルに定義された、またユーザー定義の ID 名前空間を管理します。名前空間について詳しくは、[ID 名前空間の概要](../identity-service/namespaces.md)を参照してください。[!DNL Privacy Service] で一般的に使用される標準名前空間と名前空間修飾子のリストについては、開発者ガイドの [ 付録の節 ](api/appendix.md) を参照してください。

## ECID とオプトインサービス

Adobe Experience Cloud [!DNL Identity Service] は、[!DNL Experience Cloud] の共通の識別フレームワークとして機能し、各サイト訪問者に一意の永続的 ID を割り当てます。 [!DNL Experience Cloud] ID(ECID) は、ファーストパーティ Cookie を使用して顧客のアクティビティを追跡し、複数のアプリケーションでデバイスを一意に識別でき、同じサイト訪問者とそのデータを異なる [!DNL Experience Cloud] アプリケーションで識別できます。 詳しくは、[Experience Cloud ID サービスの概要](https://experienceleague.adobe.com/docs/id-service/using/intro/overview.html?lang=ja)を参照してください。

オプトインサービス（[!DNL Experience Cloud Identity Service] の拡張機能）を使用すると、アプリケーション上でプロトコルを設定し、訪問者が訪問者のデバイスまたはブラウザーに Cookie を設定できるかどうかを判断できるようにします。 アプリケーションでのサービスの設定方法など、オプトインサービスについて詳しくは、[オプトインサービスに関するドキュメント](https://experienceleague.adobe.com/docs/id-service/using/implementation/opt-in-service/optin-overview.html?lang=ja)を参照してください。

サイト訪問者に ECID が割り当てられると、Adobe[!DNL Privacy JavaScript Library] を利用して、プライバシーリクエストで使用する ID を取得できます（次の節を参照）。

## [!DNL Privacy JS Library]

[!DNL Adobe Privacy JavaScript Library] には、ブラウザーに保存されている顧客 ID を取得および削除できる関数がいくつか用意されています。 複数のアドビアプリケーションから ECID を含む ID 情報を取得するように、ライブラリを設定できます。コールバックや promise を使用することで、正常に取得された ID をプログラムで処理し、[!DNL Privacy Service] API に送信できます。

[!DNL Privacy JS Library] について詳しくは、[ プライバシー JS ライブラリの概要 ](js-library.md) を参照してください。一般的な使用例のコードサンプルも含まれています。

## 次の手順

このドキュメントでは、プライバシーリクエストで使用する顧客 ID データの取得に関する主要概念を簡単に説明しました。これらの概念とサービスについて詳しくは、各節に記載されているドキュメントへのリンクを確認することをお勧めします。取得した ID を [!DNL Privacy Service] に送信して、アクセス、削除または販売オプトアウトの各リクエストを作成する手順については、[Privacy Service開発者ガイド ](api/getting-started.md) を参照してください。
