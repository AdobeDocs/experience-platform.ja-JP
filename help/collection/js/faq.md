---
title: Adobe Experience Platform Web SDKに関する FAQ
description: Adobe Experience Platform web SDKに関するよくある質問への回答を示します。
exl-id: 6ddb4b4d-c9b8-471a-bd2e-135dc4202876
source-git-commit: 66105ca19ff1c75f1185b08b70634b7d4a6fd639
workflow-type: tm+mt
source-wordcount: '1665'
ht-degree: 1%

---

# よくある質問

このガイドでは、Adobe Experience Platform web SDKに関するよくある質問に対する回答を示します。

## Adobe Experience Platform Web SDKの以前のソリューションとの違いは何ですか？

### Experience Platform Web SDK以前

現在、個々のソリューションに基づいて異なるJavaScript ライブラリをデプロイする必要があります。

* ソリューションごとに独自のJavaScript ライブラリ、スキーマ、ドメインがあります。
* これらのライブラリは、互いに動作するように構築されたものではありません。
* クロスソリューションとAdobe Experience Platformのユースケースでは、これらの異なるライブラリが相互に依存する必要があるので、デプロイメントに問題が生じます。

Experience Platformのタグを使用すると、これらのライブラリをできる限り簡単にデプロイおよび管理できますが、次の点に問題があります。

* ライブラリサイズ （ページ上のAdobe コードが多すぎる）
* パフォーマンス（サイトの読み込みに時間がかかる）
* 1 つのユースケースに対する複数の呼び出し
* パーソナライゼーションの呼び出し前に ECID の返しを待つ（遅延の原因）
* フラクチャされたデータ収集（evar とは）
* ソリューション間のスキーマの混乱（A4T）
* その他の非最適なものの多く

また、現在、Adobe Experience Platformにデータを直接送信するJavaScript ライブラリはありません。

### （Experience Platform Web SDKを使用）

新しい web SDKは、次のソリューションのデータを単一の宛先（Experience Platform Edge Network）に送信し、前述のソリューションの最も一般的なユースケースに対応します。

* Adobe Analytics
* Adobe Audience Manager
* Adobe Target
* 訪問者 ID
* Adobe Experience Platform

その他のソリューションも同様です。

Adobe Experience Platform Web SDKは、Adobe Experience Platformに直接データを送信することもできます。 このデータは XDM 形式で、サーバー側のソリューションスキーマにマッピングされます。

## この新しい web SDKの価値は何ですか。

**パフォーマンス：** web SDKは、現在のすべてのAdobe ライブラリを使用するよりも小さく、ページの読み込みを大幅に高速化します。

**シンプル：** XDM、Web SDK、タグ、Edge Network、Adobe Experience Cloud ソリューションとAdobe Experience Platformを組み合わせると、理解しやすく、シンプルにデータ収集ストーリーを作成できます。

* **XDM:** Adobeにデータを送信するために使用する、ソリューションに依存しないスキーマ。 eVar または mbox 用のタグ付けはこれ以上ありません。
* **Web SDK:** Adobe Experience Platform Edge Networkとの間でデータを簡単に送受信できるようにします。
* **タグ：** サイトへの web SDK（およびその他のJavaScript タグ）のデプロイメントと設定を簡素化します。
* **Edge Network:** データを、必要な形式でAdobe Experience Platformおよびソリューションに簡単にルーティングします。
* **Adobe Experience PlatformおよびAdobeのソリューション：** 価値提案を可能にします。

**コントロール：** すべてのデータは、接続された単一のデータストリームを使用しているので、アプリケーションとの間で、ジャーニーのミリ秒ごとにデータがどのように表示されるかを論理的に追跡および制御できます。

**最新で未来に対応：** Web SDKとAdobeへの接続により、Adobeでは、データ収集、パーソナライゼーション、同意、サードパーティ cookie の将来に関するEdge Networkの取り扱いを大幅に最新化できます。 （Adobeで管理されるファーストパーティドメインが有効になります）。

**価値実現までの時間：** Adobeは、タグを使用して web SDKを可能な限り簡単にデプロイし、クライアントサイドのデータを XDM にマッピングできるように鋭意取り組んでいます（そして、今後も継続します）。 その作業が完了したら、他のすべてのAdobe ソリューションおよびAdobe Experience Platform サービスを、サーバーサイドでオンまたはオフにできます。 例えば、Adobe Analyticsでこの機能を使用していて、Target またはExperience Platformをオンにする場合、データストリーム設定のトグルを切り替えるだけで、これらのユースケースを実現できます。

## `alloy.js` とは？

`alloy.js` は、Web SDK JavaScript ライブラリの名前です。 SDKのソースコードとファイル名で参照されます。

## [!DNL Web SDK] を使用するには、Adobe Experience Platformを購入する必要がありますか？

いいえ。Adobe Digital Experience のお客様は誰でも、Adobe Experience Platform Web SDKを無料で使用できます。

* Experience Platformまたは Real-time CDP へのアクセス権を持 *ていない* お客様が [!DNL Web SDK] を使用するには、データ収集 UI またはExperience Platform UI でスキーマとデータストリームを作成するための適切な権限を設定する必要があります。
* Experience Platformまたは Real-time CDP へのアクセス権を持ち、[!DNL Web SDK] を使用する場合は、データ収集 UI またはExperience Platform UI でスキーマ、データセット、ID 名前空間およびデータストリームを作成するための適切な権限を設定する必要があります。

これらの権限の設定について詳しくは、[ データ収集の権限管理 ](https://experienceleague.adobe.com/docs/experience-platform/collection/permissions.html) に関するドキュメントを参照してください。

## Web SDKを使用するのは誰ですか？

Adobe Experience Platform Web SDKは、次のお客様向けに開発されました。

* Adobe Experience Platform ユーザー
   * デバイスからAdobe Experience Platformに直接データを送信する必要がある場合は、これが公式に推奨されている方法です。
   * Adobeでは、既にAdobe Analyticsがある場合はAdobe Analytics コネクタの使用がより速いと認識していますが、データ収集に関する長期的な戦略ではありません。

* Adobe Experience Cloud ソリューションのお客様
   * Adobe Analytics、Adobe Audience Manager、Adobe Targetの新規のお客様は、従来のライブラリを使用せず、新しい web SDKから開始する必要があります。
   * 最大限に最適化された実装を得たい既存のお客様は、新しい web SDKを使用する必要があります。

## Web SDKにアクセスするにはどうすればよいですか？

Web SDKは現在、一般公開されており、Adobe Experience Cloud製品へのデータ送信に使用できます。 サードパーティのソリューションにデータを送信する機能は、近い将来に提供される予定です。

SDKはAdobeが無償でホストしています。 必要に応じて、無料でダウンロードして独自のサーバーにホストできます。

Web SDKサーバーがSDKからの受信データを適切に処理するために、Web Adobeは [ データストリーム設定 ](/help/datastreams/overview.md) およびExperience Platform [XDM スキーマビルダー ](/help/xdm/tutorials/create-schema-ui.md) にアクセスする必要があります。 アクセス権を希望する場合は、Adobe アカウントチームに連絡してリクエストプロセスを開始してください。

## Web SDKで現在サポートされているユースケースは何ですか？

Web SDKは急速に進化しています。 現在取り組んでいるユースケースは増えています。 [ 現在サポートされているユースケースのリストは、こちらで確認できます ](https://github.com/orgs/adobe/projects/18/views/1?filterQuery=)。

## 現在の顧客は、サイトを再タグ付けする必要がありますか？

場合によります。 Adobe Experience Platform Web SDKは、2 つの異なるスタイルでデプロイできます。 今後の移行ドキュメントには、追加の詳細が記載されます。

* **別のタグのみ：** サイトが既にソリューション用にタグ付けされており、再タグ付けできない場合で、Experience Platformのユースケースや今後のイベント転送機能に関するデータをAdobe Experience Platform Edge Networkに送信したい場合（以下を参照）、`alloy.js` タグをサイトに追加できます。この場合、サイトは「別のタグのみ」として機能します。

* **唯一のタグ：** Web SDKをExperience Cloud ソリューションに使用する場合は、そのページのソリューションの _すべて_ に使用する必要があります。 例えば、サイトが既にAdobe Analytics用にタグ付けされていて、Target に使用する場合は、将来、他のユーザーと同様に、両方に使用する必要があります。

つまり、Adobe Experience Platform Web SDKをソリューション以外のユースケースに使用する場合は、`alloy.js` でサイトにタグを付けて、新しいソリューションであるかのように作業を進めることができます。 Adobe Analytics、Target、Audience Manager、またはアプリケーションのユースケースに使用する場合は、ページ上の従来のコードを削除する必要が生じる場合があります。

## Web SDKを使い始める際に ECID を移行して、web サイトの訪問者が新しい訪問者として表示されないようにできますか？

はい。Adobe Experience Platform Web SDKには、ID 移行機能が用意されています。 詳しくは、[Experience Platform Web SDK ID ドキュメント ](/help/collection/use-cases/identity/id-overview.md#migrating-visitor-api-ecid) の ID 移行の手順に従います。

## Web SDKとタグの違いは何ですか？

* **Experience Platformのタグ** がデバイスコードを管理します。 これらを使用すると、コードをより簡単にデプロイできます。 彼らは自由で強力です。

* **Adobe Experience Platform Web SDK** は、Adobeのユースケース用にタグによってデプロイされる新しいコードの正式名前です。 また、無料で強力です。

* **`alloy.js`** は、Adobe Experience Platform Web SDK コードのファイル名です。

## タグを使用して web SDKをデプロイする必要がありますか？

いいえ。`alloy.js` ファイルは自分でダウンロードできます。

ただし、

* Adobe Experience Platform Web SDKでは、Edge Network でストリームを識別し、データの処理方法を決定できるように、データストリーム ID と呼ばれるものが必要です。 この ID は、Experience Platform内で作成されます。 UI を使用してプロパティを作成したりJavaScript コードをデプロイしたりする必要があるわけではありませんが、タグを使用して設定 ID を作成する必要があります。

* タグは、利用できる最高のタグであり、SDK Manager であるだけでなく、`alloy.js` のデプロイやデータの XDM スキーマへのマッピングを非常に簡単にします。 タグを使用しない場合は、データを送信する前に、`alloy.js` のデプロイ、イベントおよび XDM へのマッピングを管理する必要があります。 これは、タグを使用する場合よりも _はるかに_ 難しいプロセスです。

* タグのみを使用する場合でも、タグを使用してデプロイすることを `alloy.js` 勧めします。

## イベント転送とは

SDK を使用して XDM をEdge Networkに送信すると、これらの新機能イベント転送により、新しいサーバーサイド拡張機能をインストールして、そのデータをエッジネットワークから任意の場所にマッピングして、任意の場所に送信できます。 「サービスとしてのデータ収集」と考えてください。 これは、費用が必要で、Adobe Experience Platformの一部としてバンドルされています。

## CNAME またはファーストパーティドメインとは何ですか？また、それが重要な理由は何ですか？

コアサービスガイドの [Adobe管理の証明書プログラム ](https://experienceleague.adobe.com/en/docs/core-services/interface/data-collection/adobe-managed-cert) を参照してください。

## Adobe Experience Platform Web SDKでは Cookie を使用しますか？ その場合、どのような cookie を使用しますか？

コアサービスガイドの [Adobe Experience Platform Web SDK Cookie](https://experienceleague.adobe.com/en/docs/core-services/interface/data-collection/cookies/web-sdk) を参照してください。

## Adobe Experience Platform Web SDKはどのブラウザーをサポートしていますか？

Adobe Experience Platform Web SDKは、最新バージョンのGoogle Chrome、Safari、Firefox およびMicrosoft Edge Chromium で最適に動作するように設計されています。 古いバージョンのブラウザーや、Internet Explorer などの非推奨（廃止予定）のブラウザーでは、特定の機能を使用する際に問題が発生する場合があります。

## ソースコードはどこで web SDKに入手できますか？

詳しくは、GitHub の [Alloy](https://github.com/adobe/alloy) リポジトリを参照してください。
