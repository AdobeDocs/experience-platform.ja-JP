---
title: Adobe Experience Platform Web SDK FAQ
description: Adobe Experience Platform Web SDKに関するよくある質問への回答を得ます。
exl-id: 6ddb4b4d-c9b8-471a-bd2e-135dc4202876
source-git-commit: c3d66e50f647c2203fcdd5ad36ad86ed223733e3
workflow-type: tm+mt
source-wordcount: '1843'
ht-degree: 1%

---

# よくある質問

このガイドでは、Adobe Experience Platform Web SDKに関するよくある質問に対する回答を示します。

## Adobe Experience Platform Web SDKとは

Adobe Experience Platform Web SDKは、Adobe Experience Cloudのお客様がExperience Cloud内の様々なサービスを操作できる、クライアントサイドJavaScriptライブラリです。

データは、ソリューションに依存しない方法(XDM)でAdobe Experience Platform Edge Networkに送信され、データをソリューション固有の形式および宛先にマッピングして、リアルタイムで送信します。

**詳細情報Adobe**
[Summitプレゼンテーション](https://www.adobe.com/summit/2020/with-alloy-js-never-tag-for-an-evar-or-mbox-again.html)

## Adobe Experience Platform Web SDKは以前のソリューションとどのように異なりますか。

### Adobe Experience Platform SDKより前

現在、個々のソリューションに応じて異なるJavaScriptライブラリをデプロイする必要があります。

* 各ソリューションには、独自のJavaScriptライブラリ、スキーマ、ドメインがあります。
* これらのライブラリは互いに連携するようには構築されていません。
* クロスソリューションおよびAdobe Experience Platformの使用例では、これらの異なるライブラリを相互に依存させる必要があり、デプロイメント上の問題が生じます。

Adobe Experience Platform Launchでは、これらのライブラリのデプロイと管理を可能な限り簡単におこなえますが、次の点に関する問題は引き続き発生します。

* ライブラリのサイズ(ページ上のAdobeコードが多すぎる)
* パフォーマンス（サイトの読み込みに時間がかかりすぎる）
* 1つの使用例に対する複数の呼び出し
* パーソナライゼーション呼び出しの前にECIDが返されるのを待機する（遅延の原因）
* フラクチャされたデータ収集（eVarとは何か）
* ソリューション間のスキーマの混同(A4T)
* 他にも、最適でないものの多く

また、現在、Adobe Experience Platformに直接データを送信するJavaScriptライブラリはありません。

### Adobe Experience Platform Web SDKを使用

新しいWeb SDKは、次のソリューションのデータを単一の宛先(Adobe Experience Platform Edge Network)に送信し、前述のソリューションの最も一般的な使用例を解決します。

* Adobe Analytics
* Adobe Audience Manager
* Adobe Target
* 訪問者 ID
* Adobe Experience Platform

今年中に他のソリューションが導入される予定です。

Adobe Experience Platform Web SDKは、Adobe Experience Platformに直接データを送信することもできます。 このデータはXDMにあり、サーバー側のソリューションスキーマにマッピングされます。

## この新しいWeb SDKの価値は何ですか。

**パフォーマンス：** Web SDKは、現在のすべてのAdobeライブラリを使用するよりも小さく、ページ読み込みが大幅に高速になります。

**シンプル：** XDM、Web SDK、Experience Platform Launch、Experience Edge、Adobe Experience Cloudソリューション、Adobe Experience Platformを組み合わせることで、わかりやすく追跡しやすいデータ収集ストーリーを作り出します。

* **XDM:** データをAdobeに送信する際に使用する、ソリューションに依存しないスキーマ。eVarやmboxのタグ付けは不要になりました。
* **Adobe Experience Platform Web SDK:** Adobe Experience Platform Edge Networkに対して簡単にデータを送受信できるようにします。
* **Experience Platform Launch:** Web SDK（およびその他のJavaScriptタグ）をサイトに簡単にデプロイして設定できます。
* **Experience Edge:** 必要な形式でAdobe Experience Platformとソリューションに簡単にデータをルーティングできます。
* **Adobe Experience PlatformおよびAdobeソリューション：** 価値の提案を有効にします。

**制御：** すべてのデータは単一の接続されたデータストリームを使用しているので、ジャーニーのミリ秒ごとに、アプリケーション間で、データがどのように表示されるかを論理的に追跡および制御できます。

**最新で将来に備えている：** Web SDKとExperience Edge Networkへの接続により、Adobeがデータ収集、パーソナライゼーション、同意およびサードパーティCookieを扱う方法を大幅に最新化するAdobeが可能になりました。(ファーストパーティドメインの管理をAdobeが有効にします)。

**価値創出までの時間：** Adobeは、Experience Platform Launchを通じてWeb SDKを簡単にデプロイし、クライアント側のデータをXDMにマッピングできるよう、十分に取り組んできました（今後も続けます）。その作業が完了したら、他のすべてのAdobeソリューションとAdobe Experience Platformサービスをサーバー側でオンまたはオフにできます。 例えば、Adobe Analyticsでこれを使用し、TargetまたはExperience Platformをオンにする場合は、Datastreamの設定を切り替えて、これらの使用例を明確にできます。

## 合金とは？

Alloyは、Adobe Experience Platform Web SDKのコード名です。 Adobe Experience Platform Web SDKは正式な名前ですが、SDKのソースコードおよびファイル名内で使用されます。

## Web SDKを使用するには、Adobe Experience Platformを購入する必要がありますか。

いいえ。任意のAdobeデジタルエクスペリエンスの顧客が使用できます。 完全に自由です。 Web SDKを使用したいお客様には、Adobe Experience Platform UIでスキーマとデータセットを作成するアクセス権が付与されます。

## Web SDKの使用者

Adobe Experience Platform Web SDKは、次のユーザー向けに開発されました。

* Adobe Experience Platformユーザー

   デバイスからAdobe Experience Platformに直接データを送信する必要がある場合は、この方法をお勧めします。

   Adobeは、お客様が既にAdobe Analyticsを所有している場合、Adobe Analyticsコネクタの使用はより高速になることを認識していますが、データ収集の長期的戦略ではありません。

* Adobe Experience Cloudソリューションのお客様

   新しいAdobe Analytics、Adobe Audience ManagerおよびAdobe Targetのお客様は、レガシーライブラリを使用せず、新しいWeb SDKを使用する必要があります。

   可能な限り最適化された実装を取得したい既存のお客様は、新しいWeb SDKを使用する必要があります。


## Adobe Experience Platform Web SDKの使用を開始するためのアクセス方法を教えてください。

Web SDKは、現在、一般公開されており、Adobe Experience Cloud製品へのデータ送信に使用できます。 サードパーティソリューションにデータを送信する機能は近い将来に提供される予定です。 このSDKは無料で、無料でAdobeがホストします。また、必要に応じて独自のサーバーで無料でホストできるように、ダウンロードすることもできます。 Platform Web SDKは、AdobeのサーバーがSDKからの受信データを適切に処理できるように、データストリーム設定とAdobe Experience Platform XDMスキーマビルダーにアクセスする必要があります。 アクセス権を取得する場合は、カスタマーサクセスマネージャー(CSM)に連絡して、リクエストプロセスを開始してください。

## Web SDKで現在サポートされている使用例を教えてください。

Web SDKは急速に進化しています。 その他の使用例が開発中です。 現在サポートされている使用例の[リストは、こちらを参照してください。](https://github.com/adobe/alloy/projects/5)

## 現在のお客様はサイトの再タグ付けが必要ですか。

場合によります。Adobe Experience Platform Web SDKは、2つの異なるスタイルでデプロイできます。 今後の移行ドキュメントでは、追加の詳細情報が提供されます。

* **別のタグ：** サイトが既にソリューション用にタグ付けされていて、再タグ付けできない場合でも、Experience Platformの使用例や今後のExperience Platform Launchサーバー側機能（以下を参照）用にAdobe Experience Platform Edge Networkにデータを送信したい場合 `alloy.js` は、「別のタグ」として機能するサイトにタグを追加できます。

* **1つの唯一のタグ：** Experience CloudソリューションにWeb SDKを使用する場合は、そのページのすべてのソリューションにWeb SDKを使用する必要がありま __ す。例えば、サイトが既にAdobe Analytics用にタグ付けされていて、Targetで使用する場合は、将来の他のユーザーにもそのサイトを使用する必要があります。

つまり、ソリューション以外の使用例にAdobe Experience Platform Web SDKを使用する場合は、`alloy.js`でサイトにタグ付けし、新しいソリューションであるかのように次に進むことができます。 Adobe Analytics、Target、Audience Manager、またはアプリケーションの使用例で使用する場合は、ページ上のレガシーコードのいずれかを削除する必要が生じる場合があります。

## Alloyを使用し始めたときに、Webサイトの訪問者が新規訪問者として表示されないように、ECIDを移行できますか。

はい、Adobe Experience Platform Web SDKには、ID移行機能が用意されています。 詳しくは、[Platform Web SDK IDドキュメント](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/overview.html?lang=en#id-migration)のID移行の手順に従ってください。

## Web SDKとAdobe Experience Platform Launchの違いは何ですか。

* **Experience Platform** Launchは、デバイスコードマネージャーです。これを使用すると、コードをより簡単にデプロイできます。 それは自由で強力です。

* **Adobe Experience Platform Web** SDKは、Adobeの使用例のためにExperience Platform Launchでデプロイされる新しいコードの正式な名前です。また、自由で強力です。

* **`alloy.js`** は、Adobe Experience Platform Web SDKコードのファイル名です。

## Web SDKをデプロイするには、Adobe Experience Platform Launchを使用する必要がありますか。

いいえ。`alloy.js`ファイルは自分でダウンロードできます。

ただし、

* Adobe Experience Platform Web SDKを使用するには、エッジネットワークでストリームを識別し、データの処理方法を決定できるように、Datastream IDと呼ばれるものが必要です。 このIDは、Experience Platform Launch内で作成されます。 これは、Experience Platform Launchを使用してプロパティを作成したり、JavaScriptコードをデプロイしたりする必要はないのですが、Experience Platform Launchを使用して設定IDを作成する必要があるということです。

* Adobe Experience Platform Launchは、利用可能な最高のタグとSDK Managerであるだけでなく、`alloy.js`のデプロイとXDMスキーマへのデータのマッピングが非常に簡単に行えます。 Experience Platform Launchを使用しない場合は、送信する前に、データのデプロイ`alloy.js`、イベントおよびXDMへのマッピングを管理する必要があります。 これは、Experience Platform Launchを使用するよりも&#x200B;_多く_&#x200B;困難なプロセスです。

* `alloy.js`をデプロイする際には、Experience Platform Launchを使用することをお勧めします。たとえ唯一のタグであってもです。

## 「Adobe Experience Platform Launch Server Side」とは

2020年後半に、Experience Platform Launchはサーバー側転送機能をリリースします。 アドビのSDKを使用し、XDMをExperience Edgeに送信する場合、これらの新機能により、新しいサーバー側拡張機能をインストールして、そのデータをあらゆるものにマッピングし、アドビのエッジネットワークから任意の場所に送信できます。 これは、「データ収集はサービスとして」と考えます。  これは、コストで利用でき、Adobe Experience Platformの一部としてバンドルされます。

## CNAMEまたはファーストパーティドメインとは何ですか？また、なぜ重要なのですか？

CNAMEの詳細については、[Adobeのドキュメント](https://experienceleague.adobe.com/docs/id-service/using/reference/analytics-reference/cname.html)を参照してください。

## Adobe Experience Platform Web SDKはCookieを使用しますか。 その場合、どのようなCookieを使用しますか。

はい。現在、Web SDKは、実装に応じて、1 ～ 4のCookieを使用します。 以下に、Web SDKで表示される4つのCookieとその使用方法の一覧を示します。

**kndct_orgid_identity:** ECIDと、ECIDに関連するその他の情報を保存するためにID Cookieが使用されます。

**kndctr_orgid_consent:** このcookieは、Webサイトに対するユーザーの同意設定を保存します。

**kndctr_orgid_personalization:** このcookieには、Adobe Targetがwebページをパーソナライズするために使用するセッション情報が含まれます。

**kndctr_orgid_consentcheck:** このセッションベースのcookieは、サーバー側で同意設定を検索するようにサーバーに通知します。

## Adobe Experience Platform Web SDKはどのブラウザーをサポートしていますか？

Adobe Experience Platform Web SDKは、最新バージョンのGoogle Chrome、Safari、Firefox、Internet Explorer 11およびMicrosoft Edge Chromiumで最適に動作するように設計されています。 古いバージョンのブラウザーでは、特定の機能の使用に問題が生じる場合があります。

## Adobe Experience Platform Web SDKの詳細はどこで入手できますか？

* [ドキュメント](https://experienceleague.adobe.com/docs/experience-platform/edge/home.html)
* [ソースコード](https://github.com/adobe/alloy)
