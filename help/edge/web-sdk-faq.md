---
title: Web SDK FAQ
seo-title: Adobe Experience PlatformWeb SDK FAQ
description: Adobe Experience PlatformWeb SDKに関するよくある質問(FAQ)
seo-description: Adobe Experience PlatformWeb SDKに関するよくある質問(FAQ)
translation-type: tm+mt
source-git-commit: 1b5ee9b1f9bdc7835fa8de59020b3eebb4f59505
workflow-type: tm+mt
source-wordcount: '1629'
ht-degree: 2%

---


# よくある質問

このFAQには、AdobeWeb SDKに関するよくある質問が含まれています。

## Adobe Experience PlatformWeb SDKとは

Adobe Experience PlatformWeb SDKは、Adobe Experience Cloudのお客様がExperience Cloudの様々なサービスを操作できるようにする、クライアント側のJavaScriptライブラリです。

データは、ソリューションにとらわれない方法(XDM)でAdobe Experience Platformエッジネットワークに送信され、その後、データをソリューション固有の形式と宛先にマッピングし、リアルタイムで送信します。

**詳細**[Adobeサミットプレゼンテーション](https://www.adobe.com/summit/2020/with-alloy-js-never-tag-for-an-evar-or-mbox-again.html)

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

**パフォーマンス：** Web SDKは、現在のAdobeライブラリをすべて使用するよりも小さく、ページ読み込みを大幅に高速化します。

**シンプルさ：** XDM、Web SDK、Experience Platform Launch、Experience Edge、Adobe Experience Cloudの各ソリューション、Adobe Experience Platformを組み合わせることで、理解しやすく、追跡しやすいデータ収集ストーリーを構築できます。

* **XDM:** Adobeにデータを送信する際に使用する、ソリューションにとらわれないスキーマです。 evarまたはmboxのタグ付けを解除。
* **Adobe Experience PlatformウェブSDK:** データをAdobe Experience Platformエッジネットワークに簡単に送受信できるようにします。
* **Experience Platform Launch:** サイトへのWeb SDK（およびその他のJavaScriptタグ）の導入と設定を簡単にします。
* **エクスペリエンスエッジ：** データをAdobe Experience Platformやソリューションに必要な形式で簡単にルーティングできます。
* **Adobe Experience PlatformとAdobeのソリューション：** 価値提案を有効にします。

**コントロール：** すべてのデータは単一の接続されたデータストリームを使用しているので、データがアプリケーション間やアプリケーション間を1ミリ秒ごとに論理的に追跡し、制御できます。

**現代的で未来に備えている：** Web SDKとExperience Edge Networkへの接続により、Adobeは、データ収集、パーソナライゼーション、同意およびサードパーティcookieの将来に関するAdobeの扱い方を大幅に最新化できます。 (Adobeが管理するファーストパーティドメインを有効にします)。

**Time-to-value:** Adobeは、Experience Platform Launchを介したWeb SDKのデプロイを可能な限り簡単にし、クライアント側のデータをXDMにマップできるように努めてきました（今後も続けます）。  その後、他のすべてのAdobeソリューションおよびAdobe Experience Platformサービスをサーバ側でオンまたはオフにできます。 例えば、Adobe Analyticsに対してこれを使用し、ターゲットやExperience Platformを有効にする場合は、Experience Edge設定を切り替えて使用例を明るくします。

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

Web SDKは、現在一般向けに提供されており、Adobe Experience Cloud製品にデータを送信する際に使用できます。 サードパーティのソリューションにデータを送信する機能は、近い将来に提供される予定です。 Web SDKにアクセスする場合は、Certified Software Manager(CSM)に連絡して、要求プロセスを開始します。

## Web SDKが現在サポートしている使用例を教えてください。

Web SDKは、急速に発展しています。 その他の使用例が検討中です。 現在サポートされている使用例の [リストは、こちらを参照してください。](https://github.com/adobe/alloy/projects/5)

## 現在のお客様は、サイトのタグ付けを変更する必要がありますか。

場合によります。Adobe Experience PlatformWeb SDKは、2つの異なるスタイルでデプロイできます。 今後の移行ドキュメントで詳細が提供されます。

* **別のタグのみ：** サイトにソリューションのタグが付いていて、再タグ付けができないが、Experience Platformの使用例や今後のExperience Platform Launchサーバー側の機能（以下を参照）のために、データをAdobe Experience Platformエッジネットワークに送信する場合は、タグをサイトに追加し、「別のタグのみ」として機能します。 `alloy.js`

* **1つのみのタグ：** Experience CloudソリューションにWeb SDKを使用する場合は、そのページのす _べてのソリューションで使用する必要があります_ 。 例えば、サイトが既にAdobe Analyticsのタグ付けされていて、ターゲットに使用したい場合、将来の他のユーザーと同様に、のタグ付けも行う必要があります。

つまり、ソリューション以外の使用例にAdobe Experience PlatformWeb SDKを使用する場合は、サイトにタグを付け、新しいソリューションであるかのようにタグを付け `alloy.js` て、先に進むことができます。 Adobe Analytics、ターゲット、Audience Manager、またはアプリケーションの用途に使用する場合は、ページ上のレガシーコードを削除する必要がある場合があります。

## Allyを使用して開始する際に、Webサイトの訪問者が新しい訪問者として表示されないようにECIDを移行できますか。

はい、Adobe Experience PlatformWeb SDKには、ID移行機能があります。 詳しくは、 [このドキュメントの手順に従ってください](https://docs.adobe.com/content/help/en/experience-platform/edge/fundamentals/identity.html#id-migration) 。

## Web SDKとAdobe Experience Platform Launchの違いは何ですか。

* **Experience Platform Launch** は、デバイスのコードマネージャーです。 コードをより簡単に導入できるようにします。 それは自由で力強い。

* **Adobe Experience PlatformWeb SDK** (Web SDK)は、Adobeの使用例のためにExperience Platform Launchがデプロイする新しいコードの正式な名前です。 自由で力強い。

* **`alloy.js`** は、Adobe Experience PlatformWeb SDKコードのファイル名です。

## Web SDKをデプロイするには、Adobe Experience Platform Launchを使用する必要がありますか。

いいえ。この `alloy.js` ファイルは自分でダウンロードできます。

ただし、次の点に注意してください。

* Adobe Experience PlatformWeb SDKでは、エッジネットワークがストリームを識別し、データの処理を決定できるように、エクスペリエンスエッジ設定IDと呼ばれる要素が必要です。 このIDはExperience Platform Launch内で作成されます。 これは、プロパティの作成やJavaScriptコードの導入にExperience Platform Launchを使用する必要がないという意味ではありませんが、設定IDの作成にはExperience Platform Launchを使用する必要はありません。

* Adobe Experience Platform Launchは、最も使用可能なタグとSDK Managerだけでなく、XDMスキーマにデータを簡単にデプロイ `alloy.js` およびマッピングできます。 Experience Platform Launchを使用しない場合は、データを送信する前に、データのデプロイ、イベント、XDMへのマッピングを管理する必要があり `alloy.js`ます。 これは、Experience Platform Launchを使うより _はるかに難しいプロセスです_ 。

* デプロイに使用するタグがExperience Platform Launchのみで `alloy.js`ある場合でも、デプロイにはを使用することをお勧めします。

## 「Adobe Experience Platform Launch・サーバ側」とは

2020年の後半、Experience Platform Launchでは、サーバー側転送機能がリリースされます。 アドビのSDKを使用し、XDMをExperience Edgeに送信する場合、これらの新機能により、新しいサーバー側拡張機能をインストールし、そのデータを任意の場所にマッピングして、アドビのエッジネットワークから任意の場所に送信できます。 これは、「データ収集をサービスとして」と考えてください。  これは費用がかかり、Adobe Experience Platformの一部にまとめられる。

## CNAMEまたはファーストパーティドメインとは何ですか。また、それが重要なのはなぜですか。

CNAMEの詳細については、 [Adobeマニュアルを参照してください](https://docs.adobe.com/content/help/ja-JP/id-service/using/reference/analytics-reference/cname.html)

## Adobe Experience PlatformWeb SDKに関する詳細情報はどこで入手できますか？

* [ドキュメント](https://docs.adobe.com/content/help/ja-JP/experience-platform/edge/home.html)
* [ソースコード](https://github.com/adobe/alloy)
