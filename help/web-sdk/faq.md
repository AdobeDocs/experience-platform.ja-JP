---
title: Adobe Experience Platform Web SDK に関するよくある質問
description: Adobe Experience Platform Web SDK に関するよくある質問への回答を示します。
exl-id: 6ddb4b4d-c9b8-471a-bd2e-135dc4202876
source-git-commit: 002a57d1d5cfb2e7bdbd9b587e77ca4487a28f65
workflow-type: tm+mt
source-wordcount: '2268'
ht-degree: 5%

---


# よくある質問

このガイドでは、Adobe Experience Platform Web SDK に関するよくある質問に対する回答を示します。

## Adobe Experience Platform Web SDK とは

Adobe Experience Platform Web SDK は、Adobe Experience Cloudの様々なサービスとのやり取りを可能にする、クライアントサイド JavaScript ライブラリです。

Web SDK は、ソリューションに依存しない方法（XDM）でデータをExperience PlatformEdge Networkに送信し、その後、データをソリューション固有の形式と宛先にマッピングしてリアルタイムで送信します。

Web SDK について詳しくは、次のビデオを参照してください。[Alloy.js を満たし、eVarまたは Mbox を二度とタグ付けしない ](https://www.adobe.com/summit/2020/with-alloy-js-never-tag-for-an-evar-or-mbox-again.html)。

## Adobe Experience Platform Web SDK の以前のソリューションとの相違点

### Web SDKExperience Platform以前

現在、個々のソリューションに基づいて異なるJavaScript ライブラリをデプロイする必要があります。

* ソリューションごとに独自のJavaScript ライブラリ、スキーマ、ドメインがあります。
* これらのライブラリは、互いに動作するように構築されたものではありません。
* クロスソリューションとAdobe Experience Platformのユースケースでは、これらの異なるライブラリが相互に依存する必要があるので、デプロイメントに問題が生じます。

Platform のタグを使用すると、これらのライブラリをできるだけ簡単にデプロイおよび管理できますが、次の点に問題があります。

* ライブラリサイズ （ページ上のAdobeコードが多すぎる）
* パフォーマンス（サイトの読み込みに時間がかかる）
* 1 つのユースケースに対する複数の呼び出し
* パーソナライゼーションの呼び出し前に ECID の返しを待つ（遅延の原因）
* フラクチャされたデータ収集（evar とは）
* ソリューション間のスキーマの混乱（A4T）
* その他の非最適なものの多く

また、現在、Adobe Experience Platformにデータを直接送信するJavaScript ライブラリはありません。

### Experience PlatformWeb SDK を使用

新しい Web SDK は、次のソリューションのデータを 1 つの宛先（Experience PlatformEdge Network）に送信し、前述のソリューションの最も一般的なユースケースに対応します。

* Adobe Analytics
* Adobe Audience Manager
* Adobe Target
* 訪問者 ID
* Adobe Experience Platform

その他のソリューションも同様です。

Adobe Experience Platform Web SDK は、Adobe Experience Platformに直接データを送信することもできます。 このデータは XDM 形式で、サーバー側のソリューションスキーマにマッピングされます。

## この新しい Web SDK の価値は何ですか。

**パフォーマンス：** Web SDK は、現在のすべてのAdobeライブラリを使用するよりも小さく、ページの読み込みを大幅に高速化します。

**シンプル：** XDM、Web SDK、タグ、Edge Network、Adobe Experience Cloud ソリューションおよびAdobe Experience Platformを組み合わせると、理解しやすく、シンプルにデータ収集ストーリーを作成できます。

* **XDM:** Adobeにデータを送信するために使用する、ソリューションに依存しないスキーマ。 eVar または mbox 用のタグ付けはこれ以上ありません。
* **Web SDK:** Adobe Experience Platform Edge Networkとのデータの送受信を容易にします。
* **Tags:** サイトへの Web SDK （およびその他のJavaScript タグ）のデプロイメントと設定を簡素化します。
* **Edge Network:** Adobe Experience Platformおよびソリューションに、必要な形式で簡単にデータをルーティングします。
* **Adobe Experience PlatformおよびAdobeソリューション：** 価値提案を実現します。

**コントロール：** すべてのデータは、接続された単一のデータストリームを使用しているので、アプリケーションとの間で、ジャーニーのミリ秒ごとにデータがどのように表示されるかを論理的に追跡および制御できます。

**最新で未来に対応：** Web SDK とそのEdge NetworkAdobeへの接続により、Adobeでは、データ収集、パーソナライゼーション、同意、サードパーティ cookie の将来に関する扱いを大幅に最新化できるようになりました。 （Adobeが管理するファーストパーティドメインが有効になります）。

**価値実現までの時間：** Adobeは、タグを使用して Web SDK をデプロイし、クライアントサイドのデータを XDM にマッピングをできるだけ簡単にできるように鋭意取り組んでいます（そして、今後も継続します）。 その作業が完了したら、他のすべてのAdobeソリューションとAdobe Experience Platform サービスを、サーバーサイドでオンまたはオフにできます。 例えば、Adobe Analyticsでこの機能を使用していて、ターゲットまたはExperience Platformをオンにする場合、データストリーム設定の切り替えをオンにして、これらのユースケースを照らし出すだけです。

## [!DNL alloy.js] とは？

[!DNL alloy.js] は、Web SDK JavaScript ライブラリの名前です。 SDK のソースコードとファイル名で参照されます。

## [!DNL Web SDK] を使用するには、Adobe Experience Platformを購入する必要がありますか？

いいえ。Adobeのデジタルエクスペリエンスのお客様は誰でも、Adobe Experience Platform Web SDK を無料で使用できます。 [!DNL Web SDK] を使用する場合は、データ収集 UI またはExperience PlatformUI で、スキーマ、データセット、ID 名前空間およびデータストリームを作成するための適切な権限を設定する必要があります。

これらの権限の設定について詳しくは、[ データ収集の権限管理 ](https://experienceleague.adobe.com/docs/experience-platform/collection/permissions.html) に関するドキュメントを参照してください。

## Web SDK を使用するのは誰ですか？

Adobe Experience Platform Web SDK は、次のお客様向けに開発されました。

* Adobe Experience Platform ユーザー
   * デバイスからAdobe Experience Platformに直接データを送信する必要がある場合は、これが公式に推奨されている方法です。
   * Adobeは、既にAdobe Analyticsがある場合はAdobe Analytics コネクタを使用する方が速いと認識していますが、データ収集に関する長期的な戦略ではありません。

* Adobe Experience Cloud ソリューションのお客様
   * Adobe Analytics、Adobe Audience Manager、Adobe Targetの新規のお客様は、従来のライブラリを使用せず、新しい Web SDK から開始する必要があります。
   * 可能な限り最適化された実装を取得したい既存のお客様は、新しい Web SDK を使用する必要があります。

## Web SDK へのアクセス方法を教えてください。

Web SDK は現在一般に利用可能で、Adobe Experience Cloud製品へのデータ送信に使用できます。 サードパーティのソリューションにデータを送信する機能は、近い将来に提供される予定です。

SDK は無料で、Adobeがホストします。 必要に応じて、無料でダウンロードして独自のサーバーにホストできます。

Web SDK は、Adobeのサーバーが SDK からの受信データを適切に処理するために、[ データストリーム設定 ](../datastreams/overview.md) およびExperience Platform[XDM スキーマビルダー ](../xdm/tutorials/create-schema-ui.md) へのアクセスを必要とします。 アクセス権を希望する場合は、Adobeアカウントチームに連絡してリクエストプロセスを開始してください。

## Web SDK で現在サポートされているユースケースは何ですか？

Web SDK は急速に進化しています。 現在取り組んでいるユースケースは増えています。 [ 現在サポートされているユースケースのリストは、こちらで確認できます ](https://github.com/orgs/adobe/projects/18/views/1?filterQuery=)。

## 現在の顧客は、サイトを再タグ付けする必要がありますか？

場合によります。 Adobe Experience Platform Web SDK は、2 つの異なるスタイルでデプロイできます。 今後の移行ドキュメントには、追加の詳細が記載されます。

* **別のタグのみ：** サイトに既にソリューションのタグが付けられており、再タグ付けできない場合でも、Experience Platformのユースケースや今後のイベント転送機能に関するデータをAdobe Experience Platform Edge Networkに送信したい場合（以下を参照）、`alloy.js` タグをサイトに追加すると、「別のタグのみ」として機能します。

* **唯一のタグ：** Web SDK をExperience Cloudソリューションに使用する場合は、そのページのソリューションの _すべて_ に使用する必要があります。 例えば、サイトが既にAdobe Analytics用にタグ付けされていて、Target に使用する場合は、将来、他のユーザーと同様に、両方に使用する必要があります。

つまり、ソリューション以外のユースケースにAdobe Experience Platform Web SDK を使用する場合は、`alloy.js` でサイトにタグを付けて、新しいソリューションであるかのようにサイトを進めることができます。 Adobe Analytics、Target、Audience Manager、またはアプリケーションのユースケースに使用する場合は、ページ上の従来のコードを削除する必要が生じる場合があります。

## Web SDK の使用を開始する際に ECID を移行して、web サイトの訪問者が新しい訪問者として表示されないようにできますか？

はい。Adobe Experience Platform Web SDK には、ID 移行機能が用意されています。 詳しくは、[Platform Web SDK ID ドキュメント ](/help/web-sdk/identity/overview.md#id-migration) の ID 移行の手順に従います。

## Web SDK とタグの違いは何ですか？

* **Experience Platform内のタグ** がデバイスコードを管理します。 これらを使用すると、コードをより簡単にデプロイできます。 彼らは自由で強力です。

* **Adobe Experience Platform Web SDK** は、Adobeのユースケースのためにタグによってデプロイされる新しいコードの正式名前です。 また、無料で強力です。

* **`alloy.js`** は、Adobe Experience Platform Web SDK コードのファイル名です。

## Web SDK をデプロイするには、タグを使用する必要がありますか？

いいえ。`alloy.js` ファイルは自分でダウンロードできます。

ただし、

* Adobe Experience Platform Web SDK では、Edge Network がストリームを識別してデータの処理方法を決定できるように、データストリーム ID と呼ばれるものが必要です。 この ID は、Experience Platform内で作成されます。 UI を使用してプロパティを作成したりJavaScript コードをデプロイしたりする必要があるわけではありませんが、タグを使用して設定 ID を作成する必要があります。

* タグは、利用可能な最高のタグおよび SDK マネージャーであるだけでなく、`alloy.js` のデプロイやデータの XDM スキーマへのマッピングを非常に簡単にします。 タグを使用しない場合は、データを送信する前に、`alloy.js` のデプロイ、イベントおよび XDM へのマッピングを管理する必要があります。 これは、タグを使用する場合よりも _はるかに_ 難しいプロセスです。

* タグのみを使用する場合でも、タグを使用してデプロイすることを `alloy.js` 勧めします。

## イベント転送とは

SDK を使用して XDM をEdge Networkに送信すると、これらの新機能イベント転送により、新しいサーバーサイド拡張機能をインストールして、エッジネットワークから任意の場所にデータをマッピングして、任意の場所に送信できます。 「サービスとしてのデータ収集」と考えてください。 これは、費用が必要で、Adobe Experience Platformの一部としてバンドルされています。

## CNAME またはファーストパーティドメインとは何ですか？また、それが重要な理由は何ですか？

CNAME について詳しくは、[Adobeドキュメントを参照してください ](https://experienceleague.adobe.com/docs/id-service/using/reference/analytics-reference/cname.html?lang=ja)

## Adobe Experience Platform Web SDK は Cookie を使用しますか？ その場合、どのような cookie を使用しますか？

はい。現在、Web SDK では、実装に応じて 1～7 個の Cookie を使用します。 Web SDK で表示される可能性のある Cookie とその使用方法のリストを以下に示します。

| **名前** | **maxAge** | **友好時代** | **説明** |
|---|---|---|---|
| **kndct_orgid_identity** | 34128000 | 395 日 | ID cookie は、ECID と、ECID に関連するその他の情報を保存します。 |
| **kndctr_orgid_consent_check** | 7200 | 2 時間 | このセッションベースの Cookie は、同意設定サーバー側を検索するようにサーバーにシグナルで通知します。 |
| **kndctr_orgid_consent** | 15552000 | 180 日 | この cookie は、web サイトに対するユーザーの同意設定を保存します。 |
| **kndctr_orgid_cluster** | 1800 | 30 分 | この cookie は、現在のユーザーのリクエストを処理するEdge Networkリージョンを格納します。 Edge Networkがリクエストを正しいリージョンにルーティングできるように、リージョンは URL パスで使用されます。 この cookie の有効期間は 30 分です。これにより、ユーザーが別の IP アドレスで接続した場合、リクエストは最も近い地域にルーティングできます。 |
| **mbox** | 63072000 | 2 年。 | この cookie は、Target 移行設定が true に設定されている場合に表示されます。 これにより、Web SDK で Target [mbox cookie](https://developer.adobe.com/target/implement/client-side/atjs/atjs-cookies/) を設定できます。 |
| **mboxEdgeCluster** | 1800 | 30 分 | この cookie は、Target 移行設定が true に設定されている場合に表示されます。 この cookie を使用すると、Web SDK は正しいエッジクラスターを at.js に通信できるため、ユーザーがサイト全体を移動しても、Target プロファイルの同期が維持されます。 |
| **AMCV_###@AdobeOrg** | 34128000 | 395 日 | この cookie は、Adobe Experience Platform Web SDK で ID 移行が有効な場合にのみ表示されます。 この cookie は、サイトの一部がまだ visitor.js を使用している場合に Web SDK に移行するのに役立ちます。 詳細は、[`idMigrationEnabled`](/help/web-sdk/commands/configure/idmigrationenabled.md) を参照してください。 |

Web SDK を使用する場合、Edge Networkは上記の Cookie を 1 つ以上設定します。 Edge Networkは、すべての Cookie を `secure` 属性と `sameSite="none"` 属性に設定します。

現在、web サイトにセキュリティで保護されたセクションと保護されていないセクションの両方がある場合は、ユーザーの識別が妨げられることがあります。 ユーザーがサイトの保護されたセクションから保護されていないセクションに移動すると、Edge Networkはリクエストで新しい `ECID` を生成します。

## Adobe Experience Platform Web SDK はどのブラウザーをサポートしていますか？

Adobe Experience Platform Web SDK は、最新バージョンのGoogle Chrome、Safari、Firefox、Internet Explorer 11、Microsoft Edge Chromium で最適に動作するように設計されています。 古いバージョンのブラウザーでは、特定の機能を使用する際に問題が発生する場合があります。

## Adobe Experience Platform Web SDK に関する詳細情報はどこで入手できますか？

* [ドキュメント](/help/web-sdk/home.md)
* [Source コード ](https://github.com/adobe/alloy)

### Internet Explorer のサポート {#support-internet-explore}

この SDK では、非同期タスクの完了を通信する方法である promises を使用します。 SDK で使用される [Promise](https://developer.mozilla.org/ja-JP/docs/Web/JavaScript/Reference/Global_Objects/Promise) 実装は、[!DNL Internet Explorer] 以外のすべてのターゲットブラウザーでネイティブにサポートされています。 [!DNL Internet Explorer] で SDK を使用するには、`window.Promise` [polyfilled](https://remysharp.com/2010/10/08/what-is-a-polyfill) が必要です。

既に `window.Promise` がポリフィルされているかどうかを判断するには、次の手順を実行します。

1. Web サイトを [!DNL Internet Explorer] で開きます。
1. ブラウザーのデバッグコンソールを開きます。
1. コンソールに「`window.Promise`」とに入力し、Enter キーを押します。

`undefined` 以外が表示された場合は、既に `window.Promise`　がポリフィルされている可能性があります。上記のインストール手順を完了した後に Web サイトを読み込むことで、`window.Promise` がポリフィルされているかどうかを判断する方法もあります。SDK が promise に関するエラーをスローした場合、`window.Promise` がポリフィルされていない可能性が高くなります。

`window.Promise` をポリフィルする必要があると判断した場合は、以前に指定したベースコードの上に次のスクリプトタグを含めます。

```html
<script src="https://cdn.jsdelivr.net/npm/promise-polyfill@8/dist/polyfill.min.js"></script>
```

このタグは、`window.Promise` が有効な Promise 実装であることを確認するスクリプトを読み込みます。

>[!NOTE]
>
>別の Promise 実装を読み込む場合は、`Promise.prototype.finally` をサポートしていることを確認してください。

### Internet Explorer のサポート

Adobe Experience Platform SDK は、プロミスを使用します。プロミスは、非同期タスクの完了を伝える手段です。 SDK で使用される [Promise](https://developer.mozilla.org/ja-JP/docs/Web/JavaScript/Reference/Global_Objects/Promise) 実装は、[!DNL Internet Explorer] 以外のすべてのターゲットブラウザーでネイティブにサポートされています。 [!DNL Internet Explorer] で SDK を使用するには、`window.Promise` [polyfilled](https://remysharp.com/2010/10/08/what-is-a-polyfill) が必要です。

promise をポリフィルするために使用できるライブラリの 1 つに、promise-polyfill があります。 NPM を使用したインストール方法について詳しくは、[promise-polyfill のドキュメント ](https://www.npmjs.com/package/promise-polyfill) を参照してください。

>[!NOTE]
>
>別の Promise 実装を読み込む場合は、`Promise.prototype.finally` をサポートしていることを確認してください。
