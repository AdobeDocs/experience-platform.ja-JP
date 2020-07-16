---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: プライバシー要求のIDデータ
topic: overview
translation-type: tm+mt
source-git-commit: 5b32c1955fac4f137ba44e8189376c81cdbbfc40
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---


# プライバシー要求のIDデータ

Adobe Experience Platformがプライベートデータ [!DNL Privacy Service] に対する顧客の要求（アクセス、削除、オプトアウトオブセールの要求など）を処理するには、Adobe Experience Cloud対応アプリケーションに保存された個人データに特定の顧客をリンクする一意の識別子を提供する必要があります。 [!DNL Privacy Service] 次に、これらの識別子を使用して、顧客のIDの下に保存されているすべてのデータを収集 [!DNL Experience Cloud]し、顧客のリクエストに従って処理します。

このドキュメントでは、データ操作を設定し、アドビのテクノロジーを利用して顧客のプライバシー要求に適したID情報を効果的に取得する方法に関する一般的なガイダンスを提供します。

## IDと名前空間

顧客が複数の異なるチャネルを通じてブランドとやり取りできる場合、多くのインタラクションから記録される異なる識別子を調整するのは困難な場合があります。 そのため、アプリケーション内の特定のユーザーに属するデータを特定するのが困難になる場合があり [!DNL Experience Cloud] ます。

例えば、で顧客データリクエストを処理する場合、IDは、アドビが制御するドメインに設定されたcookie値、サードパーティドメインに設定されてアドビと共有されたcookie値、またはIMS組織内で明示的に定義したカスタム識別子を表す場合があります。 [!DNL Privacy Service]

したがって、に送信される各IDには、アイデンティティ値を接触チャネルのシステムに関連付けること [!DNL Privacy Service] によってコンテキストを提供する **** 名前空間が付随する必要があります。 名前空間は、電子メールアドレス（「電子メール」）などの汎用的な概念を表したり、IDを特定のAdvertising Cloud(AdCloud)やAdobe TargetID(「TNTID」)など)に関連付けたりできます。

Adobe Experience PlatformIDサービスは、グローバルに定義されたID名前空間とユーザー定義のIDユーザーのストアを管理します。 名前空間について詳しくは、「 [ID名前空間の概要](../identity-service/namespaces.md)」を参照してください。 で一般的に使用される標準名前空間と名前空間修飾子のリストにつ [!DNL Privacy Service]いては、『開発者ガイド [』の](api/appendix.md) 付録の節を参照してください。

## ECIDとオプトインサービス

Adobe Experience Cloudは、の共通の識別フレームワーク [!DNL Identity Service] として機能し [!DNL Experience Cloud]、各サイト訪問者に一意の永続的なIDを割り当てます。 ID( [!DNL Experience Cloud] ECID)は、ファーストパーティCookieを使用して顧客のアクティビティを追跡し、複数のアプリケーションでデバイスを一意に識別でき、同じサイトの訪問者とそのデータを異なる [!DNL Experience Cloud] アプリケーションで識別できます。 See the [Experience Cloud Identity Service overview](https://docs.adobe.com/content/help/ja-JP/id-service/using/intro/overview.html) for more information.

オプトインサービスは、の拡張機能で [!DNL Experience Cloud Identity Service]す。オプトインサービスを使用すると、訪問者のデバイスまたはブラウザーにCookieを設定できるかどうかを訪問者が決定できるように、アプリケーションのプロトコルを設定できます。 アプリケーション用のサービスの設定方法など、オプトインサービスに関する詳細な情報については、オプトインサービスのドキュメントを参照して [ください](https://docs.adobe.com/content/help/ja-JP/id-service/using/implementation/opt-in-service/optin-overview.html)。

サイト訪問者にECIDを割り当てると、次のセクションで説明するように、アドビを利用して、プライバシーリクエスト [!DNL Privacy JavaScript Library] で使用するIDを取得できます。

## [!DNL Privacy JS Library]

に [!DNL Adobe Privacy JavaScript Library] は、ブラウザーに保存されている顧客IDを取得および削除できる機能がいくつか用意されています。 ライブラリは、ECIDを含む複数のアドビアプリケーションからID情報を取得するように設定できます。 コールバックまたは約束を使用すると、正常に取得されたIDをプログラムで処理し、 [!DNL Privacy Service] APIに送信できます。

一般的な使用例のコードサンプル [!DNL Privacy JS Library]など、詳細については、 [Privacy JS Libraryの概要を参照してください](js-library.md)。

## 次の手順

このドキュメントでは、プライバシーリクエストで使用する顧客IDデータの取得に関する中心的な概念の概要を簡単に説明しました。 これらの概念およびサービスの詳細については、各節に記載されているドキュメントのリンクを確認することをお勧めします。 取得したIDをアクセス、削除、またはオプトアウトオブセールのリクエスト [!DNL Privacy Service] を作成するためにに送信する手順については、 [Privacy Service開発者ガイドを参照してください](api/getting-started.md)。