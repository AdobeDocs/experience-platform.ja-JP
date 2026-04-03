---
title: Adobe Experience Platform Web SDKに関するFAQ
description: Adobe Experience Platform Web SDKに関するよくある質問への回答を表示します。
exl-id: 6ddb4b4d-c9b8-471a-bd2e-135dc4202876
source-git-commit: b292b9243816b1eed7fd3939096ddc30d6be0606
workflow-type: tm+mt
source-wordcount: '1655'
ht-degree: 1%

---

# よくある質問

このガイドでは、Adobe Experience Platform Web SDKに関してよく寄せられる質問に対する回答を提供します。

## Adobe Experience Platform Web SDKと以前のソリューションとの違いは何ですか？

### Experience Platform Web SDK以前

現在は、個別のソリューションごとに異なるJavaScript ライブラリをデプロイする必要があります。

* 各ソリューションには、独自のJavaScriptライブラリ、スキーマ、ドメインがあります。
* これらのライブラリはどれも、互いに連携するように構築されていませんでした。
* クロスソリューションとAdobe Experience Platformのユースケースでは、これらの異なるライブラリが相互依存する必要があり、デプロイメントの摩擦が生じます。

Experience Platformのタグを使用すると、これらのライブラリをできるだけ簡単にデプロイおよび管理できますが、次の問題が残っています。

* ライブラリサイズ （ページ上のAdobe コードが多すぎる）
* パフォーマンス （サイトの読み込みに時間がかかりすぎる）
* 単一のユースケースに対する複数の呼び出し
* パーソナライゼーション呼び出しの前にECID リターンを待機中（遅延の原因）
* フラクチャ化されたデータ収集（evarとは）
* ソリューション間のスキーマの混乱（A4T）
* 他にも最適でない機能がたくさんあります

また、現在、Adobe Experience Platformに直接データを送信するJavaScript ライブラリはありません。

### Experience Platform Web SDKと連携します

新しいWeb SDKは、次のソリューションのデータを1つの宛先（Experience Platform Edge Network）に送信し、前述の最も一般的なソリューションのユースケースを解決します。

* Adobe Analytics
* Adobe Audience Manager
* Adobe Target
* 訪問者 ID
* Adobe Experience Platform

その他の解決策も続きます。

Adobe Experience Platform Web SDKからAdobe Experience Platformに直接データを送信することもできます。 このデータはXDM形式で、サーバーサイドのソリューションスキーマにマッピングされます。

## 新しいWeb SDKの価値は何ですか？

**パフォーマンス：** Web SDKは、現在のすべてのAdobe ライブラリを使用するよりも小さく、ページの読み込みを大幅に高速化します。

**簡素化：** XDM、Web SDK、タグ、Edge Network、Adobe Experience Cloud ソリューション、およびAdobe Experience Platformを組み合わせることで、わかりやすく、わかりやすいデータ収集ストーリーを作成できます。

* **XDM:** Adobeへのデータ送信に使用するソリューションに依存しないスキーマ。 eVarやmboxへのタグ付けは不要になります。
* **Web SDK:** Adobe Experience Platform Edge Networkへのデータの送受信を容易にします。
* **タグ：** サイト上のWeb SDK（およびその他のJavaScript タグ）のデプロイメントと設定を簡略化します。
* **Edge Network:** データをAdobe Experience Platformおよびソリューションに必要な形式で簡単に転送できます。
* **Adobe Experience PlatformおよびAdobe ソリューション：**&#x200B;その価値提案を有効にします。

**制御：** データはすべて単一の接続されたデータ ストリームを使用しているため、ジャーニーのミリ秒単位で、アプリケーションとの間でデータがどのように表示されるかを論理的に追跡および制御できます。

**モダンで将来に向けた準備が整いました：** Web SDKとEdge Networkとの連携により、Adobeは、データ収集、パーソナライゼーション、同意、サードパーティ Cookieの将来に関するAdobeの取り組みを大幅に近代化できました。 （Adobeが管理する1st パーティドメインを有効にします）。

**価値創出までの時間：** Adobeは、タグを使用してWeb SDKをデプロイし、クライアントサイドのデータをXDMにマッピングすることを可能な限り容易にするために、懸命に取り組んでいます（今後も継続します）。 その作業が完了したら、他のすべてのAdobe ソリューションとAdobe Experience Platform サービスをサーバーサイドでオンまたはオフにすることができます。 例えば、これをAdobe Analyticsに使用していて、TargetまたはExperience Platformをオンにする場合は、データストリーム設定のトグルを反転して、これらのユースケースを明るくすることができます。

## `alloy.js` とは？

`alloy.js`は、Web SDK JavaScript ライブラリの名前です。 SDKのソースコードとファイル名で参照されます。

## [!DNL Web SDK]を使用するには、Adobe Experience Platformを購入する必要がありますか？

いいえ。Adobe デジタルエクスペリエンスをご利用のお客様は、Adobe Experience Platform Web SDKを無料で利用できます。

* *not*&#x200B;のお客様は、Experience PlatformまたはReal-time CDPにアクセスでき、[!DNL Web SDK]を使用するには、データ収集UIまたはExperience Platform UIでスキーマとデータストリームを作成するための適切な権限を設定する必要があります。
* Experience PlatformまたはReal-time CDPにアクセスでき、[!DNL Web SDK]を使用したいお客様は、データ収集UIまたはExperience Platform UIでスキーマ、データセット、ID名前空間およびデータストリームを作成するための適切な権限を設定する必要があります。

これらの権限の設定について詳しくは、[&#x200B; データ収集の権限管理](https://experienceleague.adobe.com/docs/experience-platform/collection/permissions.html?lang=ja)に関するドキュメントを参照してください。

## Web SDKを使用すべきユーザー？

Adobe Experience Platform Web SDKは、次のお客様向けに開発されました。

* Adobe Experience Platform ユーザー
   * デバイスからAdobe Experience Platformに直接データを送信する必要がある場合は、これが正式にお勧めの方法です。
   * Adobeは、既にAdobe Analyticsをお持ちの場合はAdobe Analytics コネクタの使用が速くなることを認識していますが、データ収集の長期的な戦略ではありません。

* Adobe Experience Cloud ソリューションのお客様
   * 新しいAdobe Analytics、Adobe Audience Manager、およびAdobe Targetのお客様は、新しいWeb SDKから開始し、従来のライブラリを使用しないでください。
   * 既存のお客様は、可能な限り最適化された実装を行うために、新しいWeb SDKを使用する必要があります。

## Web SDKにアクセスするにはどうすればよいですか？

Web SDKは現在一般に公開されており、Adobe Experience Cloud製品にデータを送信するために使用できます。 データをサードパーティソリューションに送信する機能は、近日中に登場します。

SDKは無料で、Adobeが無料でホストしています。 必要に応じて、無料でダウンロードし、独自のサーバーでホスティングできます。

AdobeのサーバーがSDKからのインバウンドデータを適切に処理するには、Web SDKで[&#x200B; データストリーム設定](/help/datastreams/overview.md)およびExperience Platform [XDM スキーマビルダー](/help/xdm/tutorials/create-schema-ui.md)へのアクセスが必要です。 アクセス権を取得する場合は、Adobe アカウントチームに連絡してリクエストプロセスを開始してください。

## 現在、Web SDKではどのようなユースケースがサポートされていますか？

Web SDKは、急速に進化しています。 より多くのユースケースに取り組んでいます。 現在サポートされているユースケースの[&#x200B; リストについては、こちらを参照してください。](https://github.com/orgs/adobe/projects/18/views/1?filterQuery=)

## 既存の顧客は、サイトへのリタグ付けを余儀なくされていますか？

それは企業によって異なります。 Adobe Experience Platform Web SDKは、2つの異なるスタイルでデプロイできます。 将来の移行ドキュメントでは、追加の詳細が提供されます。

* **別のタグのみ：** サイトが既にソリューション用にタグ付けされていて、再タグ付けができない場合に、Adobe Experience Platform Edge Networkにデータを送信してExperience Platformのユースケースまたは今後のイベント転送機能（以下を参照）を使用する場合は、`alloy.js` タグをサイトに追加し、「別のタグのみ」として機能させることができます。

* **唯一のタグ：** Web SDKをExperience Cloud ソリューションに使用する場合は、そのページの&#x200B;_すべての_ ソリューションに使用する必要があります。 例えば、既にAdobe Analytics用にタグ付けされているサイトをTargetに使用する場合は、そのサイトを今後の両方に使用する必要があります。

言い換えれば、ソリューション以外のユースケースにAdobe Experience Platform Web SDKを使用する場合は、サイトに`alloy.js`というタグを付けて、新しいソリューションであるかのように先に進むことができます。 Adobe Analytics、Target、Audience Managerに使用する場合、またはアプリケーションのユースケースに使用する場合は、ページ上のレガシーコードのいずれかを削除する必要がある場合があります。

## Web SDKを使用し始めると、web サイトの訪問者が新規訪問者として表示されないように、ECIDを移行できますか？

はい。Adobe Experience Platform Web SDKにはID移行機能が用意されています。 詳細については、[`idMigrationEnabled`](/help/collection/js/commands/configure/idmigrationenabled.md)の指示に従ってください。

## Web SDKとタグの違い？

* Experience Platform **の** タグは、デバイスコードを管理します。 コードをより簡単にデプロイするために使用します。 彼らは自由で強力です。

* **Adobe Experience Platform Web SDK**&#x200B;は、Adobe ユースケース用のタグによってデプロイされる新しいコードの正式名です。 無料で強力です。

* **`alloy.js`**&#x200B;は、Adobe Experience Platform Web SDK コードのファイル名です。

## Web SDKのデプロイにタグを使用する必要がありますか？

いいえ。`alloy.js` ファイルは自分でダウンロードできます。

ただし：

* Adobe Experience Platform Web SDKでは、エッジネットワークがストリームを識別し、データの処理方法を決定できるように、データストリーム IDと呼ばれるIDが必要です。 このIDはExperience Platform内で作成されます。 これは、UIを使用してプロパティを作成したり、JavaScript コードをデプロイしたりする必要はありませんが、タグを使用して設定IDを作成する必要があります。

* タグは、利用可能な最良のタグおよびSDK Managerであるだけでなく、`alloy.js`のデプロイとデータのXDM スキーマへのマッピングを非常に簡単にします。 タグを使用しない場合は、データを送信する前に、`alloy.js`のデプロイ、イベント、XDMへのデータのマッピングを管理する必要があります。 これは、タグを使用するよりも&#x200B;_はるかに_&#x200B;困難なプロセスです。

* タグを使用して`alloy.js`をデプロイすることをお勧めします。このタグは、使用するタグが1つしかない場合でも使用できます。

## イベント転送とは

アドビのSDKを使用してXDMをEdge Networkに送信する場合、これらの新機能であるイベント転送により、新しいサーバーサイド拡張機能をインストールして、そのデータをアドビのエッジネットワークからあらゆる場所にマッピングし、どこからでも送信できます。 これを「サービスとしてのデータ収集」と考えてください。 この機能は有料で利用でき、Adobe Experience Platformの一部としてバンドルされます。

## CNAMEまたはファーストパーティドメインとは何ですか？なぜそれが重要なのですか？

Core Services ガイドの[Adobeで管理されている証明書プログラム &#x200B;](https://experienceleague.adobe.com/ja/docs/core-services/interface/data-collection/adobe-managed-cert)を参照してください。

## Adobe Experience Platform Web SDKはCookieを使用しますか？ その場合、どのようなCookieを使用しますか？

コアサービスガイドの[Adobe Experience Platform Web SDK Cookie](https://experienceleague.adobe.com/ja/docs/core-services/interface/data-collection/cookies/web-sdk)を参照してください。

## Adobe Experience Platform Web SDKはどのブラウザーをサポートしていますか？

Adobe Experience Platform Web SDKは、最新バージョンのGoogle Chrome、Safari、Firefox、Microsoft Edge Chromiumで最適に動作するように設計されています。 古いバージョンのブラウザーやInternet Explorerなどの非推奨のブラウザーで特定の機能を使用すると、問題が発生する場合があります。

## Web SDKのソースコードはどこで入手できますか？

GitHubの[Alloy](https://github.com/adobe/alloy) リポジトリを参照してください。
