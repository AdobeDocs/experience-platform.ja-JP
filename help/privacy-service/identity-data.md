---
keywords: エクスペリエンス Platform、home、人気のある話題。その他 d; その他 d
solution: Experience Platform
title: プライバシー要求に使用する id データ
topic-legacy: overview
description: このドキュメントでは、データ操作を設定し、アドビのテクノロジーを活用して、顧客のプライバシー要求に適した ID 情報を効果的に取得する方法について、一般的なガイダンスを提供しています。
exl-id: 43b0292a-ea4d-4858-b584-ba71029724f6
source-git-commit: 82dea48c732b3ddea957511c22f90bbd032ed9b7
workflow-type: tm+mt
source-wordcount: '646'
ht-degree: 43%

---

# プライバシーリクエストの ID データ

Adobe エクスペリエンスプラットフォームで、 [!DNL Privacy Service] お客様からの個人データに対する要求 (アクセス、削除、または販売中止) を処理するには、Adobe エクスペリエンスクラウド対応アプリケーションで、そのお客様によって保存された個人データへのリンクを作成するための一意の識別子を指定する必要があります。 [!DNL Privacy Service] はこれらの識別子を使用して、顧客の ID の下に保存されているすべてのデータを 内で収集し、顧客のリクエストに従って処理します。[!DNL Experience Cloud]

このドキュメントでは、データ操作を設定し、アドビのテクノロジーを活用して、顧客のプライバシー要求に適した ID 情報を効果的に取得する方法について、一般的なガイダンスを提供しています。

## ID と名前空間

顧客が複数の異なるチャネルを通じてブランドとやり取りできる場合、多数のインタラクションから記録される様々な識別子を調整するのは困難なことがあります。これにより、アプリケーションの特定の人物に属するデータを決定することが困難になり [!DNL Experience Cloud] ます。

例えば、での顧客データ要求を処理する場合、アイデンティティは、Adobe 管理下のドメインにある cookie 値、 [!DNL Privacy Service] サードパーティのドメインに属する cookie の値、および、IMS 組織内に明示的に定義されたカスタム識別子を表すことができます。

そのため、に送られる各 id に [!DNL Privacy Service] は、id 値をそのシステムのシステムに関連付けることによって、コンテキストを提供する名前空間が付属している必要があります。 名前空間では、電子メールアドレス（「電子メール」）などの汎用概念を表したり、ID を特定のアプリケーションに関連付けたりすることができます（Adobe Advertising Cloud ID（「AdCloud」）、Adobe Target ID（「TNTID」）など）。

Adobe Experience Platform ID サービスは、グローバルに定義された、またユーザー定義の ID 名前空間を管理します。名前空間について詳しくは、[ID 名前空間の概要](../identity-service/namespaces.md)を参照してください。に一般的に使用されている標準的な名前空間と名前空間修飾子の一覧については、 [!DNL Privacy Service] 「API ガイド」の「付録」を参照してください [ ](api/appendix.md) 。

## ECID とオプトインサービス

Adobe エクスペリエンスクラウドは [!DNL Identity Service] 、の一般的な識別フレームワークとして機能 [!DNL Experience Cloud] し、一意の永続的な ID が各サイトビジターに割り当てられます。 この [!DNL Experience Cloud] ID (その結果、d) は、ファーストパーティの cookie を使用してお客様のアクティビティを追跡することで、複数のアプリケーションで1つのデバイスを一意に識別し、異なるアプリケーションで同じサイトビジターとそのデータを識別することができ [!DNL Experience Cloud] ます。 詳しくは、[Experience Cloud ID サービスの概要](https://experienceleague.adobe.com/docs/id-service/using/intro/overview.html?lang=ja)を参照してください。

の拡張機能であるオプトインサービスを [!DNL Experience Cloud Identity Service] 使用すると、ビジターのデバイスやブラウザーで cookie を設定できるかどうかをユーザーが決定できるように、アプリケーションのプロトコルを設定することができます。 アプリケーションでのサービスの設定方法など、オプトインサービスについて詳しくは、[オプトインサービスに関するドキュメント](https://experienceleague.adobe.com/docs/id-service/using/implementation/opt-in-service/optin-overview.html?lang=ja)を参照してください。

サイトのビジターに設定された Ds に移動したら、次の節で説明するように、アドビシステムズ社を使用し [!DNL Privacy JavaScript Library] てこれらの id を取得し、プライバシー要求に使用することができます。

## [!DNL Privacy JS Library]

に [!DNL Adobe Privacy JavaScript Library] 用意されているいくつかの関数を使用すると、ブラウザーに保存されている顧客のアイデンティティ情報を取得したり削除したりすることができます。 複数のアドビアプリケーションから ECID を含む ID 情報を取得するように、ライブラリを設定できます。コールバックまたは約束を使用して、取得した Id をプログラムによって処理し、API に送信することができ [!DNL Privacy Service] ます。

一般的な使用例について詳しくは、「 [!DNL Privacy JS Library] プライバシー保護機能の概要」を参照してください [ ](js-library.md) 。

## 次の手順

このドキュメントでは、プライバシーリクエストで使用する顧客 ID データの取得に関する主要概念を簡単に説明しました。これらの概念とサービスについて詳しくは、各節に記載されているドキュメントへのリンクを確認することをお勧めします。取得した Id をに送信して [!DNL Privacy Service] 、access、delete、またはその他の不在要請を作成する手順については、プライバシーに関するサービスの API ガイドを参照してください [ ](api/overview.md) 。
