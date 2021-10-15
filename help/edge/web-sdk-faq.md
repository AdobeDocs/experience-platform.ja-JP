---
title: Adobe Experience Platform Web SDK FAQ
description: Adobe Experience Platform Web SDK に関するよくある質問への回答を示します。
exl-id: 6ddb4b4d-c9b8-471a-bd2e-135dc4202876
source-git-commit: 7e27735697882065566ebdeccc36998ec368e404
workflow-type: tm+mt
source-wordcount: '1802'
ht-degree: 2%

---

# よくある質問

このガイドでは、Adobe Experience Platform Web SDK に関するよくある質問に対する回答を示します。

## Adobe Experience Platform Web SDK とは

Adobe Experience Platform Web SDK は、Adobe Experience Cloudのお客様がExperience Cloud内の様々なサービスを操作できるようにする、クライアントサイド JavaScript ライブラリです。

データは、ソリューションに依存しない方法 (XDM) でAdobe Experience Platform Edge Network に送信され、データをソリューション固有の形式および宛先にマッピングして、リアルタイムで送信します。

**詳細情報 Adobe**
[Summit プレゼンテーション](https://www.adobe.com/summit/2020/with-alloy-js-never-tag-for-an-evar-or-mbox-again.html)

## Adobe Experience Platform Web SDK は以前のソリューションとどのように異なりますか。

### Adobe Experience Platform SDK より前

現在、個々のソリューションに応じて異なる JavaScript ライブラリをデプロイする必要があります。

* 各ソリューションには、独自の JavaScript ライブラリ、スキーマ、ドメインがあります。
* これらのライブラリは互いに機能するように構築されていません。
* クロスソリューションおよびAdobe Experience Platformの使用例では、これらの異なるライブラリが相互に依存している必要があり、デプロイメントの摩擦が生じます。

Platform のタグを使用すると、これらのライブラリをできるだけ簡単にデプロイおよび管理できますが、次の点に関する問題が解決しません。

* ライブラリのサイズ ( ページ上のAdobeコードが多すぎる )
* パフォーマンス（サイトの読み込みに時間がかかりすぎる）
* 単一の使用例に対する複数の呼び出し
* パーソナライゼーション呼び出しの前に ECID が返されるのを待つ（遅延の原因）
* フラクチャされたデータ収集（eVar とは何か）
* ソリューション間のスキーマの混同 (A4T)
* 他の多くの最適でないもの

また、現在、データをAdobe Experience Platformに直接送信する JavaScript ライブラリはありません。

### Adobe Experience Platform Web SDK を使用

新しい Web SDK は、次のソリューションのデータを単一の宛先 (Adobe Experience Platform Edge Network) に送信し、前述の最も一般的なソリューションの使用例を解決します。

* Adobe Analytics
* Adobe Audience Manager
* Adobe Target
* 訪問者 ID
* Adobe Experience Platform

今年中に他の解決策が導入される予定です。

Adobe Experience Platform Web SDK は、データを直接Adobe Experience Platformに送信することもできます。 このデータは XDM にあり、サーバー側のソリューションスキーマにマッピングされます。

## この新しい Web SDK の価値は何ですか？

**パフォーマンス：** Web SDK は、現在のすべてのAdobeライブラリを使用するよりも小さく、ページ読み込みが大幅に高速です。

**シンプル：** XDM、Web SDK、タグ、Experience Edge、Adobe Experience Cloudソリューション、Adobe Experience Platformを組み合わせることで、わかりやすく、追跡しやすいデータ収集ストーリーを作り出します。

* **XDM:** データをAdobeに送信する際に使用する、ソリューションに依存しないスキーマ。eVar や mbox のタグ付けは不要になりました。
* **Adobe Experience Platform Web SDK:** Adobe Experience Platform Edge Network へのデータの送受信を簡単にします。
* **タグ：** Web SDK（およびその他の JavaScript タグ）をサイトに簡単にデプロイして設定できます。
* **Experience Edge:** データをAdobe Experience Platformおよびソリューションに必要な形式で簡単にルーティングできます。
* **Adobe Experience PlatformおよびAdobeソリューション：** 価値の提案を有効にします。

**制御：** すべてのデータは単一の接続されたデータストリームを使用しているので、ジャーニーのミリ秒ごとに、アプリケーションとの間で、データがどのように表示されるかを論理的に追跡および制御できます。

**最新で将来に備えている：** Web SDK と Experience Edge Network への接続により、Adobeがデータ収集、パーソナライゼーション、同意およびサードパーティ Cookie を扱う方法を大幅に最新化するAdobeが可能になりました。( ファーストパーティドメインが有効になり、Adobeが管理します )。

**価値までの時間：** Adobeは、タグを使用した Web SDK のデプロイと、クライアント側のデータの XDM へのマッピングを可能な限り簡単におこなえるよう、十分に作業を進めています（今後も続けます）。その後、他のすべてのAdobeソリューションおよびAdobe Experience Platformサービスのオン/オフをサーバー側で切り替えることができます。 例えば、Adobe Analyticsでこれを使用し、Target またはExperience Platformをオンにする場合は、Datastream の設定を切り替えて、これらの使用例を明確にできます。

## 合金とは？

Alloy は、Adobe Experience Platform Web SDK のコード名です。 Adobe Experience Platform Web SDK は正式な名前ですが、SDK のソースコードおよびファイル名内で使用されます。

## Web SDK を使用するには、Adobe Experience Platformを購入する必要がありますか。

いいえ。任意のAdobeデジタルエクスペリエンスの顧客が使用できます。 完全に自由です。 Web SDK を使用するお客様には、Adobe Experience Platform UI でスキーマとデータセットを作成するためのアクセス権が付与されます。

## Web SDK の使用者

Adobe Experience Platform Web SDK は、次のユーザー向けに開発されました。

* Adobe Experience Platformユーザー

   デバイスからAdobe Experience Platformに直接データを送信する必要がある場合は、この方法をお勧めします。

   Adobeは、お客様が既にAdobe Analyticsを持っている場合、Adobe Analyticsコネクタの使用がより高速になることを認識していますが、これはデータ収集の長期的な戦略ではありません。

* Adobe Experience Cloudソリューションのお客様

   新しいAdobe Analytics、Adobe Audience ManagerおよびAdobe Targetのお客様は、レガシーライブラリを使用せず、新しい Web SDK から始める必要があります。

   可能な限り最適化された実装を取得したい既存のお客様は、新しい Web SDK を使用する必要があります。


## Adobe Experience Platform Web SDK を使用し始めるためのアクセス方法を教えてください。

Web SDK は、現在、一般公開されており、Adobe Experience Cloud製品へのデータ送信に使用できます。 サードパーティソリューションにデータを送信する機能は近い将来に提供される予定です。 この SDK は無料で、無料でAdobeでホストされています。また、必要に応じて独自のサーバーで無料でホストできるように、ダウンロードすることもできます。 Platform Web SDK は、Adobeのサーバーが SDK からの受信データを適切に処理できるように、データストリーム設定とAdobe Experience Platform XDM スキーマビルダーにアクセスする必要があります。 アクセス権を取得する場合は、カスタマーサクセスマネージャー (CSM) に問い合わせて、リクエストプロセスを開始してください。

## Web SDK で現在サポートされているユースケースを教えてください。

Web SDK は急速に進化しています。 現在、さらに多くの使用例が取り組まれています。 現在サポートされている使用例の [ リストは、こちらをご覧ください。](https://github.com/adobe/alloy/projects/5)

## 現在のお客様は、サイトの再タグ付けが必要ですか。

場合によります。Adobe Experience Platform Web SDK は、2 つの異なるスタイルでデプロイできます。 今後の移行ドキュメントでは、追加の詳細情報が提供されます。

* **別のタグ：** サイトが既にソリューション用にタグ付けされていて、再タグ付けできない場合でも、Experience Platformの使用例や今後のイベント転送機能（以下を参照）のためにAdobe Experience Platform Edge Network にデータを送信したい場合、サイトにタグを追加しま `alloy.js` す。

* **1 つの唯一のタグ：** Experience Cloudソリューションに Web SDK を使用する場合は、そのページのすべてのソリューションに Web SDK を使用する必要がありま __ す。例えば、サイトが既にAdobe Analytics用にタグ付けされていて、Target に使用する場合は、今後そのサイトを他のユーザーと共に使用する必要があります。

つまり、ソリューション以外の使用例にAdobe Experience Platform Web SDK を使用する場合は、サイトに `alloy.js` でタグ付けし、新しいソリューションであるかのように次に進むことができます。 Adobe Analytics、Target、Audience Manager、またはアプリケーションの使用例で使用する場合は、ページ上の従来のコードを削除する必要が生じる場合があります。

## Alloy を使用し始めると、Web サイトの訪問者が新規訪問者として表示されなくなるように、ECID を移行できますか。

はい、Adobe Experience Platform Web SDK には、ID 移行機能が用意されています。 詳しくは、[Platform Web SDK ID ドキュメント ](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/overview.html?lang=en#id-migration) の ID 移行の手順に従ってください。

## Web SDK とタグの違いを教えてください。

* **Experience Platform のタグはデ** バイスコードを管理します。これらを使用して、コードをより簡単にデプロイできます。 彼らは自由で強力です。

* **Adobe Experience Platform Web** SDK は、Adobeの使用例に対してタグでデプロイされる新しいコードの正式な名前です。また、自由で強力です。

* **`alloy.js`** は、Adobe Experience Platform Web SDK コードのファイル名です。

## Web SDK のデプロイにはタグを使用する必要がありますか？

いいえ。`alloy.js` ファイルは自分でダウンロードできます。

ただし、

* Adobe Experience Platform Web SDK には、エッジネットワークがストリームを識別し、データの処理方法を決定できるように、Datastream ID と呼ばれるものが必要です。 この ID はExperience Platform内で作成されます。 これは、データ収集 UI を使用してプロパティを作成したり、JavaScript コードをデプロイしたりする必要はないのですが、設定 ID を作成するにはタグを使用する必要があります。

* タグは、最も利用可能なタグおよび SDK Manager であるだけでなく、 `alloy.js` のデプロイと XDM スキーマへのデータのマッピングが非常に簡単におこなえます。 タグを使用しない場合は、デプロイ `alloy.js`、イベンティング、XDM へのデータのマッピングを管理してから、データを送信する必要があります。 これは、タグを使用するよりも _多く_ 難しい処理です。

* `alloy.js` を導入するのにタグを使用することをお勧めします。たとえ唯一のタグを使用する場合でもです。

## イベント転送とは

アドビの SDK を使用し、XDM を Experience Edge に送信する場合、これらの新機能のイベント転送を使用すると、新しいサーバー側拡張機能をインストールして、そのデータを任意のものにマッピングし、アドビのエッジネットワークから任意の場所に送信できます。 これは、「データ収集はサービスとして」と考えます。  これはコストで利用でき、Adobe Experience Platformの一部としてバンドルされます。

## CNAME またはファーストパーティドメインとは何ですか。なぜ重要なのですか。

CNAME の詳細については、[Adobeのドキュメント ](https://experienceleague.adobe.com/docs/id-service/using/reference/analytics-reference/cname.html?lang=ja) を参照してください。

## Adobe Experience Platform Web SDK は Cookie を使用しますか。 その場合、どのような Cookie が使用されますか。

はい。現在、Web SDK は、実装に応じて 1 ～ 4 個の Cookie を使用します。 Web SDK で表示される 4 つの Cookie と、その使用方法の一覧を以下に示します。

**kndct_orgid_identity:** ID Cookie は、ECID および ECID に関連するその他の情報を保存するために使用されます。

**kndctr_orgid_consent:** この cookie は、Web サイトに対するユーザーの同意設定を保存します。

**kndctr_orgid_personalization:** この cookie には、Adobe Targetが web ページのパーソナライズに使用するセッション情報が含まれます。

**kndctr_orgid_consentcheck:** このセッションベースの cookie は、サーバー側で同意設定を検索するようにサーバーに通知します。

## Adobe Experience Platform Web SDK はどのブラウザーをサポートしていますか？

Adobe Experience Platform Web SDK は、Google Chrome、Safari、Firefox、Internet Explorer 11 およびMicrosoft Edge Chromium の最新バージョンで最適に動作するように設計されています。 古いバージョンのブラウザーでは、特定の機能の使用に問題が生じる場合があります。

## Adobe Experience Platform Web SDK の詳細はどこで入手できますか？

* [ドキュメント](https://experienceleague.adobe.com/docs/experience-platform/edge/home.html?lang=ja)
* [ソースコード](https://github.com/adobe/alloy)
