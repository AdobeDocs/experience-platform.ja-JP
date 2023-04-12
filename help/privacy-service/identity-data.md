---
keywords: Experience Platform;ホーム;人気のトピック;ECID;ecid
solution: Experience Platform
title: プライバシーリクエストの ID データ
description: このドキュメントでは、データ操作を設定し、アドビのテクノロジーを活用して、顧客のプライバシー要求に適した ID 情報を効果的に取得する方法について、一般的なガイダンスを提供しています。
exl-id: 43b0292a-ea4d-4858-b584-ba71029724f6
source-git-commit: fcd44aef026c1049ccdfe5896e6199d32b4d1114
workflow-type: tm+mt
source-wordcount: '645'
ht-degree: 45%

---

# プライバシーリクエストの ID データ

Adobe Experience Platform [!DNL Privacy Service] 非公開データに対する顧客要求（アクセス、削除、販売のオプトアウトの要求など）を処理するには、Adobe Experience Cloud対応アプリケーションで、保存されている非公開データに特定の顧客をリンクする一意の ID を提供する必要があります。 [!DNL Privacy Service] はこれらの識別子を使用して、顧客の ID の下に保存されているすべてのデータを 内で収集し、顧客のリクエストに従って処理します。[!DNL Experience Cloud]

このドキュメントでは、データ操作を設定し、アドビのテクノロジーを活用して、顧客のプライバシー要求に適した ID 情報を効果的に取得する方法について、一般的なガイダンスを提供しています。

## ID と名前空間

顧客が複数の異なるチャネルを通じてブランドとやり取りできる場合、多数のインタラクションから記録される様々な識別子を調整するのは困難なことがあります。その結果、特定の個人に属するデータを特定するのが困難になる場合があります [!DNL Experience Cloud] アプリケーション。

例えば、 [!DNL Privacy Service]の場合、id は、Adobeが管理するドメインの下に設定された cookie 値、サードパーティドメインの下に設定され、Adobeと共有される cookie 値、または組織内で明示的に定義したカスタム識別子を表す場合があります。

したがって、各 ID はに送信する必要があります。 [!DNL Privacy Service] は、id 値を元のシステムに関連付けることでコンテキストを提供する名前空間を伴います。 名前空間では、電子メールアドレス（「電子メール」）などの汎用概念を表したり、ID を特定のアプリケーションに関連付けたりすることができます（Adobe Advertising Cloud ID（「AdCloud」）、Adobe Target ID（「TNTID」）など）。

Adobe Experience Platform ID サービスは、グローバルに定義された、またユーザー定義の ID 名前空間を管理します。名前空間について詳しくは、[ID 名前空間の概要](../identity-service/namespaces.md)を参照してください。標準名前空間と、 [!DNL Privacy Service]を参照し、 [付録の節](api/appendix.md) （ API ガイド）を参照してください。

## ECID とオプトインサービス

Adobe Experience Cloud [!DNL Identity Service] ～の共通の識別枠組みとして機能する [!DNL Experience Cloud]をクリックし、各サイト訪問者に一意の永続的 ID を割り当てます。 この [!DNL Experience Cloud] ID(ECID) は、ファーストパーティ Cookie を使用して顧客のアクティビティを追跡し、複数のアプリケーションでデバイスを一意に識別でき、同じサイト訪問者とそのデータを異なる場所で識別できます [!DNL Experience Cloud] アプリケーション。 詳しくは、[Experience Cloud ID サービスの概要](https://experienceleague.adobe.com/docs/id-service/using/intro/overview.html?lang=ja)を参照してください。

オプトインサービス、の拡張 [!DNL Experience Cloud Identity Service]を使用すると、アプリケーションでプロトコルを設定し、訪問者が訪問者のデバイスまたはブラウザーに cookie を設定できるかどうかを訪問者が決定できるようにします。 アプリケーションでのサービスの設定方法など、オプトインサービスについて詳しくは、[オプトインサービスに関するドキュメント](https://experienceleague.adobe.com/docs/id-service/using/implementation/opt-in-service/optin-overview.html?lang=ja)を参照してください。

サイト訪問者に ECID が割り当てられると、Adobe [!DNL Privacy JavaScript Library] を使用してプライバシーリクエストで使用する ID を取得することもできます（次の節で説明）。

## [!DNL Privacy JS Library]

この [!DNL Adobe Privacy JavaScript Library] には、ブラウザーに保存されている顧客 ID を取得および削除できる機能がいくつか用意されています。 複数のアドビアプリケーションから ECID を含む ID 情報を取得するように、ライブラリを設定できます。コールバックや promise を使用することで、正常に取得された ID をプログラムで処理し、それらをに送信できます。 [!DNL Privacy Service] API

詳しくは、 [!DNL Privacy JS Library]（一般的な使用例のコードサンプルを含む） [プライバシー JS ライブラリの概要](js-library.md).

## 次の手順

このドキュメントでは、プライバシーリクエストで使用する顧客 ID データの取得に関する主要概念を簡単に説明しました。これらの概念とサービスについて詳しくは、各節に記載されているドキュメントへのリンクを確認することをお勧めします。取得した ID をに送信する手順については、を参照してください。 [!DNL Privacy Service] アクセス、削除、販売オプトアウトの各リクエストの作成については、 [Privacy ServiceAPI ガイド](api/overview.md).
