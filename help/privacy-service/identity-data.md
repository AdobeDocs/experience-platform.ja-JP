---
keywords: Experience Platform；ホーム；人気のあるトピック；ECID;ecid
solution: Experience Platform
title: プライバシー要求のIDデータ
topic-legacy: overview
description: このドキュメントでは、データ操作を設定し、アドビのテクノロジーを活用して、顧客のプライバシー要求に適した ID 情報を効果的に取得する方法について、一般的なガイダンスを提供しています。
exl-id: 43b0292a-ea4d-4858-b584-ba71029724f6
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '650'
ht-degree: 44%

---

# プライバシーリクエストの ID データ

Adobe Experience Platform[!DNL Privacy Service]が自分のプライベートデータに対する顧客の要求（アクセス、削除、オプトアウトオブセールの要求を含む）を処理するには、特定の顧客をAdobe Experience Cloud対応のアプリケーションに保存されたプライベートデータにリンクする一意の識別子を提供する必要があります。 [!DNL Privacy Service] はこれらの識別子を使用して、顧客の ID の下に保存されているすべてのデータを 内で収集し、顧客のリクエストに従って処理します。[!DNL Experience Cloud]

このドキュメントでは、データ操作を設定し、アドビのテクノロジーを活用して、顧客のプライバシー要求に適した ID 情報を効果的に取得する方法について、一般的なガイダンスを提供しています。

## ID と名前空間

顧客が複数の異なるチャネルを通じてブランドとやり取りできる場合、多数のインタラクションから記録される様々な識別子を調整するのは困難なことがあります。その結果、[!DNL Experience Cloud]アプリケーションの特定のユーザーに属するデータを特定するのが困難になる場合があります。

例えば、[!DNL Privacy Service]で顧客データリクエストを処理する場合、IDは、Adobeが制御するドメインに設定されたcookie値、サードパーティのドメインに設定されてAdobeと共有されたcookie値、またはIMS組織内で明示的に定義したカスタム識別子を表します。

したがって、[!DNL Privacy Service]に送信される各IDには、ID値を接触チャネルのシステムに関連付けることでコンテキストを提供する名前空間が付随する必要があります。 名前空間では、電子メールアドレス（「電子メール」）などの汎用概念を表したり、ID を特定のアプリケーションに関連付けたりすることができます（Adobe Advertising Cloud ID（「AdCloud」）、Adobe Target ID（「TNTID」）など）。

Adobe Experience Platform ID サービスは、グローバルに定義された、またユーザー定義の ID 名前空間を管理します。名前空間について詳しくは、[ID 名前空間の概要](../identity-service/namespaces.md)を参照してください。[!DNL Privacy Service]で一般的に使用される標準名前空間と名前空間修飾子のリストについては、『開発者ガイド』の[付録](api/appendix.md)を参照してください。

## ECID とオプトインサービス

Adobe Experience Cloud[!DNL Identity Service]は[!DNL Experience Cloud]の共通の識別フレームワークとして機能し、一意の永続的なIDを各サイト訪問者に割り当てます。 [!DNL Experience Cloud] ID(ECID)は、ファーストパーティcookieを使用して顧客のアクティビティを追跡し、複数のアプリケーションでデバイスを一意に識別でき、同じサイト訪問者とそのデータを異なる[!DNL Experience Cloud]アプリケーションで識別できます。 詳しくは、[Experience Cloud ID サービスの概要](https://docs.adobe.com/content/help/ja-JP/id-service/using/intro/overview.html)を参照してください。

オプトインサービス（[!DNL Experience Cloud Identity Service]の拡張機能）を使用すると、訪問者のデバイスまたはブラウザーにcookieを設定できるかどうかを訪問者が判断できるように、アプリケーションのプロトコルを設定できます。 アプリケーションでのサービスの設定方法など、オプトインサービスについて詳しくは、[オプトインサービスに関するドキュメント](https://docs.adobe.com/content/help/ja-JP/id-service/using/implementation/opt-in-service/optin-overview.html)を参照してください。

サイトの訪問者にECIDを割り当てると、次のセクションで説明するように、Adobe[!DNL Privacy JavaScript Library]を利用して、プライバシーリクエストで使用するIDを取得できます。

## [!DNL Privacy JS Library]

[!DNL Adobe Privacy JavaScript Library]には、ブラウザーに保存されている顧客IDを取得および削除するための機能がいくつか用意されています。 複数のアドビアプリケーションから ECID を含む ID 情報を取得するように、ライブラリを設定できます。コールバックまたはプロミスを使用すると、正常に取得されたIDをプログラムで処理し、[!DNL Privacy Service] APIに送信できます。

[!DNL Privacy JS Library]について詳しくは、一般的な使用例のコードサンプルを含めて、[プライバシーJSライブラリの概要](js-library.md)を参照してください。

## 次の手順

このドキュメントでは、プライバシーリクエストで使用する顧客 ID データの取得に関する主要概念を簡単に説明しました。これらの概念とサービスについて詳しくは、各節に記載されているドキュメントへのリンクを確認することをお勧めします。取得したIDを[!DNL Privacy Service]に送信して、アクセス、削除、またはオプトアウトオブセールのリクエストを作成する手順については、[Privacy Service開発ガイド](api/getting-started.md)を参照してください。
