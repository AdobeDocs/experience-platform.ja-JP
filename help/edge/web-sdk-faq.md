---
title: Adobe Experience Platform Web SDK FAQ
description: Adobe Experience Platform Web SDK に関するよくある質問への回答を示します。
exl-id: 6ddb4b4d-c9b8-471a-bd2e-135dc4202876
source-git-commit: 77313baabee10e21845fa79763c7ade4e479e080
workflow-type: tm+mt
source-wordcount: '1934'
ht-degree: 2%

---

# よくある質問

このガイドでは、Adobe Experience Platform Web SDK に関するよくある質問に対する回答を示します。

## Adobe Experience Platform Web SDK とは

Adobe Experience Platform Web SDK は、Adobe Experience Cloudのお客様がExperience Cloud内の様々なサービスを操作できるようにする、クライアントサイド JavaScript ライブラリです。

データは、ソリューションに依存しない方法 (XDM) でAdobe Experience Platform Edge Network に送信され、データをソリューション固有の形式および宛先にマッピングして、リアルタイムで送信します。

**詳細情報**
[Adobe Summit表示](https://www.adobe.com/summit/2020/with-alloy-js-never-tag-for-an-evar-or-mbox-again.html)

## Adobe Experience Platform Web SDK は以前のソリューションとどのように異なりますか。

### Adobe Experience Platform SDK より前

現在、個々のソリューションに応じて異なる JavaScript ライブラリをデプロイする必要があります。

* 各ソリューションには、独自の JavaScript ライブラリ、スキーマおよびドメインがあります。
* これらのライブラリは互いに連携するように設計されていません。
* クロスソリューションおよびAdobe Experience Platformの使用例では、これらの異なるライブラリが相互に依存している必要があり、デプロイメント上の摩擦が生じます。

Platform のタグを使用すると、これらのライブラリをできるだけ簡単にデプロイおよび管理できますが、次の点で問題が生じます。

* ライブラリのサイズ ( ページ上のAdobeコードが多すぎます )
* パフォーマンス（サイトの読み込みに時間がかかりすぎる）
* 1 つのユースケースに対する複数の呼び出し
* パーソナライゼーション呼び出しの前に ECID が返されるのを待機する（ラグの原因）
* フラクチャされたデータ収集（eVar とは何か）
* ソリューション間のスキーマの混同 (A4T)
* その他の多くの最適でないもの

また、現在、Adobe Experience Platformに直接データを送信する JavaScript ライブラリはありません。

### Adobe Experience Platform Web SDK を使用

新しい Web SDK は、次のソリューションのデータを単一の宛先 (Adobe Experience Platform Edge Network) に送信し、前述のソリューションの最も一般的な使用例を解決します。

* Adobe Analytics
* Adobe Audience Manager
* Adobe Target
* 訪問者 ID
* Adobe Experience Platform

今年中に他のソリューションが導入される予定です。

Adobe Experience Platform Web SDK は、データをAdobe Experience Platformに直接送信することもできます。 このデータは XDM に存在し、サーバー側のソリューションスキーマにマッピングされます。

## この新しい Web SDK にはどのような価値がありますか？

**パフォーマンス：** Web SDK は、現在のすべてのライブラリを使用するよりも小さく、Adobe読み込みが大幅に高速です。

**シンプル：** XDM、Web SDK、タグ、Experience Edge、Adobe Experience Cloudソリューション、Adobe Experience Platformを組み合わせることで、わかりやすく、簡単に追跡できるデータ収集の仕組みを作成できます。

* **XDM:** データをAdobeに送信する際に使用する、ソリューションに依存しないスキーマ。 eVar や mbox のタグ付けは不要になりました。
* **Adobe Experience Platform Web SDK:** Adobe Experience Platform Edge Network へのデータの送受信が簡単になります。
* **タグ：** サイト上での Web SDK（およびその他の JavaScript タグ）のデプロイと設定を簡素化します。
* **エクスペリエンスエッジ：** データを必要な形式でAdobe Experience Platformおよびソリューションに簡単にルーティングできます。
* **Adobe Experience PlatformとAdobeのソリューション：** 価値提案を有効にします。

**コントロール：** すべてのデータは 1 つの接続されたデータストリームを使用しているので、ジャーニーのミリ秒ごとに、アプリケーションとの間で、データがどのように表示されるかを論理的に追跡し、制御できます。

**現代的で将来に備えたもの：** Web SDK と Experience Edge Network への接続により、Adobeでのデータ収集、パーソナライゼーション、同意およびサードパーティ Cookie の将来に対する処理方法を大幅に最新化するAdobeが可能になりました。 ( ファーストパーティドメインが有効になり、Adobeが管理します )。

**値への時間：** Adobeは、タグを介して Web SDK を簡単にデプロイし、クライアント側のデータを XDM にマッピングできるよう、十分に作業を進めてきました（今後も続けます）。 その後、他のすべてのAdobeソリューションおよびAdobe Experience Platformサービスをサーバー側でオンまたはオフにできます。 例えば、Adobe Analyticsでこれを使用し、Target またはExperience Platformをオンにする場合は、Datastream の設定を切り替えて、これらの使用例を明確にできます。

## Alloy とは

Alloy は、Adobe Experience Platform Web SDK のコード名です。 Adobe Experience Platform Web SDK は正式な名前ですが、SDK のソースコードおよびファイル名内で使用されます。

## 顧客が [!DNL Web SDK]?

いいえ。Adobeの Digital Experience のお客様は、Adobe Experience Platform Web SDK を無料で使用できます。 を使用するお客様 [!DNL Web SDK] は、データ収集 UI またはExperience PlatformUI でスキーマ、データセット、ID 名前空間、データストリームを作成するために、適切な権限を設定する必要があります。

これらの権限の設定について詳しくは、 [データ収集権限の管理](https://experienceleague.adobe.com/docs/experience-platform/collection/permissions.html?lang=en).

## Web SDK の使用者

Adobe Experience Platform Web SDK は、以下のユーザー向けに開発されました。

* Adobe Experience Platformユーザー
   * デバイスからAdobe Experience Platformに直接データを送信する必要がある場合は、この方法を正式にお勧めします。
   * Adobeは、お客様が既にAdobe Analyticsを使用している場合、Adobe Analyticsコネクタの使用はより高速になることを認識していますが、これはデータ収集の長期的な戦略ではありません。

* Adobe Experience Cloudソリューションのお客様
   * 新しいAdobe Analytics、Adobe Audience ManagerおよびAdobe Targetのお客様は、レガシーライブラリを使用せず、新しい Web SDK から始める必要があります。
   * 可能な限り最適化された実装を取得したい既存のお客様は、新しい Web SDK を使用する必要があります。


## Adobe Experience Platform Web SDK にアクセスして使用を開始するには、どうすればよいですか？

Web SDK は、現在、一般ユーザーが利用でき、Adobe Experience Cloud製品へのデータ送信に使用できます。 サードパーティソリューションにデータを送信する機能は、近い将来に提供される予定です。 SDK は無料で、無料でAdobeによってホストされます。また、必要に応じて独自のサーバーで無料でホストできるように、ダウンロードすることもできます。 Adobeのサーバーが SDK からの受信データを適切に処理するには、Platform Web SDK が Datastream 設定とAdobe Experience Platform XDM スキーマビルダーにアクセスする必要があります。 アクセス権を取得する場合は、カスタマーサクセスマネージャー (CSM) に問い合わせて、リクエストプロセスを開始してください。

## Web SDK で現在サポートされている使用例を教えてください。

Web SDK は急速に進化しています。 現在、さらに多くの使用例が対象となっています。 次の [現在サポートされている使用例のリストはこちらです。](https://github.com/adobe/alloy/projects/5)

## 現在のお客様はサイトを再タグ付けする必要がありますか。

場合によります。Adobe Experience Platform Web SDK は、2 つの異なるスタイルでデプロイできます。 今後の移行ドキュメントでは、追加の詳細情報が提供されます。

* **別のタグ：** サイトが既にソリューション用にタグ付けされていて、再タグ付けできず、Experience Platformの使用例や今後のイベント転送機能（以下を参照）を目的としてAdobe Experience Platform Edge Network にデータを送信する場合、 `alloy.js` タグをサイトに追加します。このタグは、「単なる別のタグ」として機能します。

* **1 つの唯一のタグは、次のようになります。** Experience Cloudソリューションで Web SDK を使用する場合は、 _すべて_ 」という名前に設定します。 例えば、サイトが既にAdobe Analytics用にタグ付けされていて、それを Target で使用する場合は、今後の他のユーザーも同様に使用する必要があります。

つまり、ソリューション以外の使用例でAdobe Experience Platform Web SDK を使用する場合は、 `alloy.js` 新しい解決策のように進みます。 Adobe Analytics、Target、Audience Manager、またはアプリケーションの使用例で使用する場合は、ページ上の従来のコードを削除する必要が生じる場合があります。

## Alloy を使用し始めたときに、Web サイトの訪問者が新規訪問者として表示されないように、ECID を移行することはできますか？

はい、Adobe Experience Platform Web SDK には、ID 移行機能が用意されています。 ID の移行手順に従います。 [Platform Web SDK ID ドキュメント](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/overview.html?lang=en#id-migration) を参照してください。

## Web SDK とタグの違いは何ですか？

* **タグのExperience Platform** デバイスコードを管理します。 これらを使用すると、コードをより簡単にデプロイできます。 彼らは自由で強力です。

* **Adobe Experience Platform Web SDK** は、Adobeの使用例のためにタグでデプロイされる新しいコードの正式な名前です。 それはまた、自由で強力です。

* **`alloy.js`** は、Adobe Experience Platform Web SDK コードのファイル名です。

## Web SDK をデプロイするにはタグを使用する必要がありますか？

いいえ。次をダウンロード： `alloy.js` ファイルを自分で作成します。

ただし、

* Adobe Experience Platform Web SDK には、エッジネットワークがストリームを識別し、データの処理方法を決定できるように、Datastream ID と呼ばれるものが必要です。 この ID はExperience Platform内で作成されます。 これは、UI を使用してプロパティを作成したり JavaScript コードをデプロイしたりする必要があるわけではありませんが、タグを使用して設定 ID を作成する必要はありません。

* タグは、最適な利用可能なタグと SDK Manager であるだけでなく、簡単にデプロイできます `alloy.js` XDM スキーマにデータをマッピングします。 タグを使用しない場合は、デプロイの管理が必要になります `alloy.js`、イベンティングをおこない、データを XDM にマッピングしてから、送信します。 これは、 _多い_ タグを使用するよりプロセスが難しい。

* タグを使用してをデプロイすることをお勧めします `alloy.js`使用するのが唯一のタグであっても、 。

## イベント転送とは

アドビの SDK を使用し、Experience Edge に XDM を送信する場合、これらの新機能のイベント転送を使用すると、新しいサーバー側拡張機能をインストールして、そのデータを任意の場所にマッピングし、アドビのエッジネットワークから任意の場所に送信できます。 これは、「データ収集はサービスとして」と考えてください。 これは、コストで利用できるほか、Adobe Experience Platformの一部としてバンドルされます。

## CNAME またはファーストパーティドメインとは何ですか？また、なぜ重要なのですか？

CNAME について詳しくは、 [Adobe文書](https://experienceleague.adobe.com/docs/id-service/using/reference/analytics-reference/cname.html?lang=ja)

## Adobe Experience Platform Web SDK は Cookie を使用しますか？ その場合、どのような cookie が使用されますか。

はい。現在、Web SDK は、実装に応じて、1 ～ 4 個の Cookie を使用します。 以下に、Web SDK で表示される 4 つの Cookie と、その使用方法の一覧を示します。

**kndct_orgid_identity:** ID Cookie は、ECID および ECID に関連するその他の情報を保存するために使用されます。

**kndctr_orgid_consent:** この cookie は、Web サイトに対するユーザーの同意設定を保存します。

**kndctr_orgid_cluster:** この Cookie は、現在のユーザーのリクエストを処理する Experience Edge 地域を保存します。 Experience Edge がリクエストを正しい地域にルーティングできるように、地域は、URL パスで使用されます。 この cookie の有効期間は 30 分なので、ユーザーが別の IP アドレスで接続した場合、リクエストは最も近い地域にルーティングできます。

Web SDK を使用する場合、Edge ネットワークは上記の cookie を 1 つ以上設定します。 Edge ネットワークは、 `secure` および `sameSite="none"` 属性。

現在、Web サイトにセキュリティで保護されたセクションとセキュリティで保護されていないセクションの両方がある場合、これがユーザーの識別を妨げる可能性があります。 ユーザーがサイトのセキュリティで保護されたセクションからセキュリティで保護されていないセクションに移動すると、Edge ネットワークは新しい `ECID` リクエストに置き換えます。

## Adobe Experience Platform Web SDK はどのブラウザーをサポートしていますか？

Adobe Experience Platform Web SDK は、Google Chrome、Safari、Firefox、Internet Explorer 11、Microsoft Edge Chromium の最新バージョンで最適に動作するように設計されています。 古いバージョンのブラウザーでは、特定の機能の使用に問題が生じる場合があります。

## Adobe Experience Platform Web SDK に関する詳細はどこで入手できますか？

* [ドキュメント](https://experienceleague.adobe.com/docs/experience-platform/edge/home.html?lang=ja)
* [ソースコード](https://github.com/adobe/alloy)
