---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: プライバシー要求のIDデータ
topic: overview
translation-type: tm+mt
source-git-commit: a1161630c8edae107b784f32ee20af225f9f8c46
workflow-type: tm+mt
source-wordcount: '659'
ht-degree: 3%

---


# プライバシー要求のIDデータ

Adobe Experience Platformプライバシーサービスで、個人データに対する顧客の要求（アクセス、削除、オプトアウトオブセールの要求を含む）を処理するには、Adobe Experience Cloud対応アプリケーションに保存された個人データに特定の顧客をリンクする一意の識別子を提供する必要があります。 次に、プライバシーサービスは、これらのIDを使用して、顧客のIDの下にExperience Cloud内に保存されたすべてのデータを収集し、顧客の要求に従って処理します。

このドキュメントでは、データ操作を設定し、アドビのテクノロジーを利用して顧客のプライバシー要求に適したID情報を効果的に取得する方法に関する一般的なガイダンスを提供します。

## IDと名前空間

顧客が複数の異なるチャネルを通じてブランドとやり取りできる場合、多くのインタラクションから記録される異なる識別子を調整するのは困難な場合があります。 そのため、Experience Cloudアプリケーションの特定のユーザーに属するデータを特定するのが困難になる場合があります。

例えば、プライバシーサービスで顧客データリクエストを処理する場合、IDは、アドビが制御するドメインに設定されたcookie値、サードパーティドメインに属してアドビと共有されたcookie値、またはIMS組織内で明示的に定義したカスタム識別子を表します。

したがって、プライバシーサービスに送信される各IDには、ID値を接触チャネルのシステムに関連付けることでコンテキストを提供する **** 名前空間が付随する必要があります。 名前空間は、電子メールアドレス（「電子メール」）などの汎用的な概念を表したり、IDを特定のアプリケーション(Adobe Advertising Cloud ID(「AdCloud」)やAdobeターゲットID(「TNTID」)など)に関連付けたりできます。

Adobe Experience Platform Identity Serviceは、グローバルに定義されたID名前空間とユーザー定義のIDプラットフォームのストアを管理します。 名前空間について詳しくは、「 [ID名前空間の概要](../identity-service/namespaces.md)」を参照してください。 プライバシーサービスで一般的に使用される標準名前空間と名前空間修飾子のリストについては、『開発者ガイド [』の](api/appendix.md) 付録の節を参照してください。

## ECIDとオプトインサービス

Adobe Experience Cloud Identity Serviceは、Experience Cloudの共通の識別フレームワークとして機能し、各サイト訪問者に一意の永続的なIDを割り当てます。 Experience Cloud ID(ECID)は、ファーストパーティcookieを使用して顧客のアクティビティを追跡し、複数のアプリでデバイスを一意に識別できます。また、同じサイトの訪問者とそのデータを様々なExperience Cloudアプリケーションで識別できます。 See the [Experience Cloud Identity Service overview](https://docs.adobe.com/content/help/ja-JP/id-service/using/intro/overview.html) for more information.

Experience Cloud IDサービスの拡張機能であるオプトインサービスでは、訪問者に対してプロトコルを設定し、訪問者がCookieを設定できるかどうかを判断できるようにします。 アプリケーション用のサービスの設定方法など、オプトインサービスに関する詳細な情報については、オプトインサービスのドキュメントを参照して [ください](https://docs.adobe.com/content/help/ja-JP/id-service/using/implementation/opt-in-service/optin-overview.html)。

サイト訪問者にECIDを割り当てると、次のセクションで説明するように、アドビのプライバシーJavaScriptライブラリを利用して、プライバシーリクエストで使用するIDを取得できます。

## プライバシーJSライブラリ

アドビのプライバシーJavaScriptライブラリには、ブラウザーに保存されている顧客IDを取得および削除できる機能がいくつか用意されています。 ライブラリは、ECIDを含む複数のアドビアプリケーションからID情報を取得するように設定できます。 コールバックまたは約束を使用すると、正常に取得されたIDをプログラムで処理し、それらをPrivacy Service APIに送信できます。

一般的な使用例のコードサンプルなど、プライバシーJSライブラリについて詳しくは、 [プライバシーJSライブラリの概要を参照してください](js-library.md)。

## 次の手順

このドキュメントでは、プライバシーリクエストで使用する顧客IDデータの取得に関する中心的な概念の概要を簡単に説明しました。 これらの概念およびサービスの詳細については、各節に記載されているドキュメントのリンクを確認することをお勧めします。 取得したIDをプライバシーサービスに送信して、アクセス、削除、またはオプトアウト（販売外）の要求を作成する方法については、 [プライバシーサービス開発者ガイドを参照してください](api/getting-started.md)。