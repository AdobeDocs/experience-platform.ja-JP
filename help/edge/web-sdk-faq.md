---
title: Adobe Experience PlatformWeb SDK FAQ
description: Adobe Experience PlatformWeb SDKに関するよくある質問への回答を示します。
exl-id: 6ddb4b4d-c9b8-471a-bd2e-135dc4202876
source-git-commit: 5ead9dc72b8b9fe89e0a1bc8365ceff8affd3c85
workflow-type: tm+mt
source-wordcount: '1847'
ht-degree: 2%

---

# よくある質問

このガイドでは、Adobe Experience PlatformWeb SDKに関してよく尋ねられる質問に対する回答を提供します。

## Adobe Experience PlatformWeb SDKとは

Adobe Experience PlatformWeb SDKは、Adobe Experience Cloudのお客様がExperience Cloudの様々なサービスを操作できるようにする、クライアント側のJavaScriptライブラリです。

データは、ソリューションにとらわれない方法(XDM)でAdobe Experience Platformエッジネットワークに送信され、その後、データをソリューション固有の形式と宛先にマッピングし、リアルタイムで送信します。

**詳し**
[くは、Adobe Summitプレゼンテーション](https://www.adobe.com/summit/2020/with-alloy-js-never-tag-for-an-evar-or-mbox-again.html)

## Adobe Experience PlatformWeb SDKと以前のソリューションとの違い

### Adobe Experience PlatformSDKより前

現在、個々のソリューションに応じて異なるJavaScriptライブラリをデプロイする必要があります。

* 各ソリューションには、独自のJavaScriptライブラリ、スキーマおよびドメインがあります。
* これらのライブラリは互いに連携するように構築されていません。
* ソリューション間およびAdobe Experience Platform間の使用例では、これらの個別のライブラリが相互依存している必要があり、導入が困難になっています。

Adobe Experience Platform Launchではこれらのライブラリの展開と管理を可能な限り簡単に行えますが、次の点に関する問題がまだあります。

* ライブラリサイズ(ページ上のAdobeコードが多すぎる)
* パフォーマンス（サイトの読み込みに時間がかかり過ぎる）
* 単一の使用例に対する複数の呼び出し
* パーソナライゼーション呼び出しの前にECIDが返されるのを待つ（遅延が生じる）
* フラクチャされたデータ収集（eVarとは何ですか）
* ソリューション間のスキーマの混乱(A4T)
* その他多数の最適でないもの

また、現在、Adobe Experience Platformにデータを直接送信するJavaScriptライブラリはありません。

### Adobe Experience PlatformWeb SDKを使用する

新しいWeb SDKは、以下のソリューションのデータを1つの送信先(Adobe Experience Platformエッジネットワーク)に送信し、前述の最も一般的なソリューションの使用例を解決します。

* Adobe Analytics
* Adobe Audience Manager
* Adobe Target
* 訪問者 ID
* Adobe Experience Platform

他の解決策は今年後半に続く予定です。

また、Adobe Experience PlatformWeb SDKは、データを直接Adobe Experience Platformに送信することもできます。 このデータはXDMに格納され、サーバー側のソリューションスキーマにマップされます。

## この新しいWeb SDKの価値

**パフォーマンス：** Web SDKは、現在のAdobeライブラリのすべてを使用するよりも小さく、ページの読み込みが大幅に高速になります。

**シンプル：XDM、Web SDK、** Experience Platform Launch、Experience Edge、Adobe Experience Cloudの各ソリューション、Adobe Experience Platformを組み合わせることで、理解しやすく、追跡しやすいデータ収集ストーリーを構築できます。

* **XDM:Adobe** にデータを送信する際に使用する、ソリューションにとらわれないスキーマです。evarまたはmboxのタグ付けを解除。
* **Adobe Experience PlatformWeb SDK:Adobe Experience Platformエッジネットワーク** に対してデータを簡単に送受信できるようにします。
* **Experience Platform Launch：サイトへのWeb SDK（およびその他のJavaScriptタグ）の導入と設定を** 簡単にします。
* **Experience Edge：データを** Adobe Experience Platformやソリューションに必要な形式で簡単にルーティングできます。
* **Adobe Experience PlatformとAdobeのソリューション：価値提案を** 有効にします。

**制御：す** べてのデータは単一の接続されたデータストリームを使用しているので、ジャーニーのミリ秒ごとに、アプリケーション間で、データがどのように見えるかを論理的に追跡して制御できます。

**最新で将来に備えたもの：Web SDK** とExperience Edge Networkへの接続により、Adobeはデータ収集、パーソナライゼーション、同意およびサードパーティCookieの将来に対するAdobeの取り組みを大幅に最新化できました。(Adobeが管理するファーストパーティドメインを有効にします)。

**Time-to-value:** Adobeは、Experience Platform Launchを介したWeb SDKのデプロイを可能な限り簡単にし、クライアント側のデータをXDMにマッピングするために、懸命に作業を続けていきます。その後、他のすべてのAdobeソリューションおよびAdobe Experience Platformサービスをサーバ側でオンまたはオフにできます。 例えば、Adobe Analyticsに対してこれを使用し、ターゲットやExperience Platformを有効にする場合は、データストリームの設定を切り替えて使用例を明るくします。

## アロイとは？

Alloyは、Adobe Experience PlatformWeb SDKのコード名です。 Adobe Experience PlatformWeb SDKは正式な名前ですが、SDKのソースコードとファイル名の中で使用されます。

## Web SDKを使用するには、Adobe Experience Platformを購入する必要がありますか。

いいえ。任意のAdobeデジタルエクスペリエンスのお客様が使用できます。 完全に自由です。 Web SDKを使用したいお客様には、Adobe Experience PlatformUIでスキーマとデータセットを作成するアクセス権が与えられます。

## Web SDKの使用者

Adobe Experience PlatformウェブSDKは、以下の人々を対象に開発されました。

* Adobe Experience Platformユーザー

   デバイスからAdobe Experience Platformにデータを直接送信する必要がある場合は、これが正式に推奨される方法です。

   お客様が既にAdobe Analyticsを使用している場合、Adobe Analyticsコネクタの使用は高速になることをAdobeは認識していますが、データ収集の長期戦略ではありません。

* Adobe Experience Cloudソリューションのお客様

   新しいAdobe Analytics、Adobe Audience Manager、Adobe Targetのお客様は、レガシーライブラリを使用せずに、新しいWeb SDKと開始する必要があります。

   可能な限り最適化された実装を取得したい既存のお客様は、新しいWeb SDKを使用する必要があります。


## Adobe Experience PlatformWeb SDKを使用して開始にアクセスする方法を教えてください。

Web SDKは、現在一般向けに提供されており、Adobe Experience Cloud製品にデータを送信する際に使用できます。 サードパーティのソリューションにデータを送信する機能は、近い将来、提供される予定です。 SDKは無料で、無料でAdobeがホストし、ダウンロードして独自のサーバーでホストすることもできます。必要に応じて、無料で提供されます。 プラットフォームWeb SDKは、AdobeのサーバーがSDKから受信データを正しく処理できるように、Datastreamの設定とAdobe Experience PlatformXDMスキーマビルダーへのアクセスを必要とします。 アクセスを取得したい場合は、Customer Success Manager(CSM)に問い合わせて、リクエストプロセスを開始してください。

## Web SDKが現在サポートしている使用例を教えてください。

Web SDKは、急速に発展しています。 その他の使用例が検討中です。 現在サポートされている使用例の[リストは、こちらで確認できます。](https://github.com/adobe/alloy/projects/5)

## 現在のお客様は、サイトのタグ付けを変更する必要がありますか。

場合によります。Adobe Experience PlatformWeb SDKは、2つの異なるスタイルでデプロイできます。 今後の移行ドキュメントで詳細が提供されます。

* **別のタグの** み：サイトにソリューションのタグが付いていて、タグを付け直すことができないが、Experience Platformの使用例や今後のExperience Platform Launchのサーバー側機能（以下を参照）のために、データをAdobe Experience Platformエッジネットワークに送信する場合は、 `alloy.js` タグをサイトに追加できます。

* **1つのタグのみ：** Experience Cloudソリューション用のWeb SDKを使用する場合は、そのページのす __ べてのソリューションで使用する必要があります。例えば、サイトが既にAdobe Analyticsのタグ付けされていて、ターゲットに使用したい場合、将来の他のユーザーと同様に、のタグ付けも行う必要があります。

つまり、ソリューション以外の使用例にAdobe Experience PlatformWeb SDKを使用する場合は、サイトに`alloy.js`でタグ付けし、新しいソリューションであるかのように次に進むことができます。 Adobe Analytics、ターゲット、Audience Manager、またはアプリケーションの用途に使用する場合は、ページ上のレガシーコードを削除する必要がある場合があります。

## Allyを使用して開始する際に、Webサイトの訪問者が新しい訪問者として表示されないようにECIDを移行できますか。

はい、Adobe Experience PlatformWeb SDKには、ID移行機能があります。 詳しくは、[プラットフォームWeb SDK IDドキュメント](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/overview.html?lang=en#id-migration)のIDの移行の手順に従ってください。

## Web SDKとAdobe Experience Platform Launchの違いは何ですか。

* **Experience Platform** 起動は、デバイスのコードマネージャです。コードをより簡単に導入できるようにします。 それは自由で力強い。

* **Adobe Experience PlatformWeb** SDKは、Adobeの使用例に対してExperience Platform Launchがデプロイする新しいコードの正式な名前です。自由で力強い。

* **`alloy.js`** は、Adobe Experience PlatformWeb SDKコードのファイル名です。

## Web SDKをデプロイするには、Adobe Experience Platform Launchを使用する必要がありますか。

いいえ。`alloy.js`ファイルは自分でダウンロードできます。

ただし、

* Adobe Experience PlatformWeb SDKでは、エッジネットワークがストリームを識別し、データの処理を決定できるように、データストリームIDと呼ばれる要素が必要です。 このIDはExperience Platform Launch内で作成されます。 これは、プロパティの作成やJavaScriptコードの導入にExperience Platform Launchを使用する必要がないという意味ではありませんが、設定IDの作成にはExperience Platform Launchを使用する必要はありません。

* Adobe Experience Platform Launchは、最高の利用可能なタグとSDK Managerだけでなく、`alloy.js`のデプロイやXDMスキーマへのデータのマッピングを非常に簡単に行えます。 Experience Platform Launchを使用しない場合は、送信する前に、`alloy.js`のデプロイ、イベント、XDMへのデータのマッピングを管理する必要があります。 これは、Experience Platform Launchを使うよりも&#x200B;_ずっと_&#x200B;難しい処理です。

* `alloy.js`を導入するのにExperience Platform Launchを使用することをお勧めします。たとえを使用するタグが唯一のものであってもです。

## 「Adobe Experience Platform Launch・サーバ側」とは

2020年の後半、Experience Platform Launchでは、サーバー側転送機能がリリースされます。 アドビのSDKを使用し、XDMをExperience Edgeに送信する場合、これらの新機能により、新しいサーバー側拡張機能をインストールし、そのデータを任意の場所にマッピングして、アドビのエッジネットワークから任意の場所に送信できます。 これは、「データ収集をサービスとして」と考えてください。  これは費用がかかり、Adobe Experience Platformの一部にまとめられる。

## CNAMEまたはファーストパーティドメインとは何ですか。また、それが重要なのはなぜですか。

CNAMEに関する詳細は、[Adobeドキュメント](https://docs.adobe.com/content/help/ja-JP/id-service/using/reference/analytics-reference/cname.html)を参照してください

## Adobe Experience PlatformWeb SDKはcookieを使用しますか。 その場合、どのcookieが使用されますか。

はい。現在、Web SDKは、実装に応じて1 ～ 4のcookieを使用します。 以下は、Web SDKで表示される4つのcookieのリストと、その使用方法です。

**kndct_orgid_identity:ECID** と、ECIDに関連するその他の情報の保存には、ID cookieが使用されます。

**kndctr_orgid_consent:** このcookieは、Webサイトに対するユーザーの同意の優先度を保存します。

**kndctr_orgid_personalization:** このcookieには、Adobe Targetがwebページのパーソナライズに使用するセッション情報が含まれます。

**kndctr_orgid_consentcheck:** このセッションベースのcookieは、サーバーに対して、同意の環境設定サーバー側を調べるように通知します。

## Adobe Experience PlatformWeb SDKはどのブラウザーをサポートしていますか。

Adobe Experience PlatformWeb SDKは、Google Chrome、Safari、Firefox、Internet Explorer 11、Microsoft Edge Chromの最新バージョンで最適に動作するように設計されています。 古いバージョンのブラウザーでは、特定の機能の使用に問題が生じる場合があります。

## Adobe Experience PlatformWeb SDKに関する詳細情報はどこで入手できますか？

* [ドキュメント](https://docs.adobe.com/content/help/ja-JP/experience-platform/edge/home.html)
* [ソースコード](https://github.com/adobe/alloy)
