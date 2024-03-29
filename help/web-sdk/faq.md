---
title: Adobe Experience Platform Web SDK FAQ
description: Adobe Experience Platform Web SDK に関するよくある質問への回答を示します。
exl-id: 6ddb4b4d-c9b8-471a-bd2e-135dc4202876
source-git-commit: 58cd6300307881c3de7c52e07c401bf2ed908517
workflow-type: tm+mt
source-wordcount: '2268'
ht-degree: 5%

---


# よくある質問

このガイドでは、Adobe Experience Platform Web SDK に関するよくある質問に対する回答を示します。

## Adobe Experience Platform Web SDK とは

Adobe Experience Platform Web SDK は、Adobe Experience Cloudの様々なサービスを操作できる、クライアントサイド JavaScript ライブラリです。

Web SDK は、ソリューションに依存しない方法 (XDM) でExperience Platformを Edge ネットワークに送信し、その後、データをソリューション固有の形式および宛先にマッピングして、リアルタイムで送信します。

Web SDK について詳しくは、次のビデオを参照してください。 [Alloy.js を満たし、eVarまたは mbox のタグを再び設定しない](https://www.adobe.com/summit/2020/with-alloy-js-never-tag-for-an-evar-or-mbox-again.html).

## Adobe Experience Platform Web SDK は以前のソリューションとどのように異なりますか。

### Experience PlatformWeb SDK より前

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

### Experience PlatformWeb SDK を使用

新しい Web SDK は、次のソリューションのデータを単一の宛先 (Experience PlatformEdge ネットワーク ) に送信し、前述のソリューションの最も一般的な使用例を解決します。

* Adobe Analytics
* Adobe Audience Manager
* Adobe Target
* 訪問者 ID
* Adobe Experience Platform

その他のソリューションもそれに従います。

Adobe Experience Platform Web SDK は、データをAdobe Experience Platformに直接送信することもできます。 このデータは XDM 形式で、サーバー側のソリューションスキーマにマッピングされます。

## この新しい Web SDK にはどのような価値がありますか？

**パフォーマンス：** Web SDK は、現在のすべてのライブラリを使用するよりも小さく、Adobe読み込みが大幅に高速です。

**シンプル：** XDM、Web SDK、タグ、Edge Network、Adobe Experience Cloudソリューション、Adobe Experience Platformを組み合わせることで、わかりやすく、簡単に追跡できるデータ収集の仕組みを作成できます。

* **XDM:** データをAdobeに送信する際に使用する、ソリューションに依存しないスキーマ。 eVar や mbox のタグ付けは不要になりました。
* **Web SDK:** Adobe Experience Platform Edge Network へのデータの送受信が簡単になります。
* **タグ：** サイト上での Web SDK（およびその他の JavaScript タグ）のデプロイと設定を簡素化します。
* **Edge ネットワーク：** データを必要な形式でAdobe Experience Platformおよびソリューションに簡単にルーティングできます。
* **Adobe Experience PlatformとAdobeのソリューション：** 価値提案を有効にします。

**コントロール：** すべてのデータは 1 つの接続されたデータストリームを使用しているので、ジャーニーのミリ秒ごとに、アプリケーションとの間で、データがどのように表示されるかを論理的に追跡し、制御できます。

**現代的で将来に備えたもの：** Web SDK と Edge Network への接続により、Adobeでのデータ収集、パーソナライゼーション、同意およびサードパーティ Cookie の将来に対する処理方法を大幅に最新化するAdobeが可能になりました。 ( ファーストパーティドメインが有効になり、Adobeが管理します )。

**値への時間：** Adobeは、タグを介して Web SDK を簡単にデプロイし、クライアント側のデータを XDM にマッピングできるよう、十分に作業を進めてきました（今後も続けます）。 その後、他のすべてのAdobeソリューションおよびAdobe Experience Platformサービスをサーバー側でオンまたはオフにできます。 例えば、Adobe Analyticsでこれを使用し、Target またはExperience Platformをオンにする場合は、Datastream の設定を切り替えて、これらの使用例を明確にできます。

## [!DNL alloy.js] とは？

[!DNL alloy.js] は、Web SDK JavaScript ライブラリの名前です。 SDK のソースコード内およびファイル名で参照されます。

## お客様が、 [!DNL Web SDK]?

いいえ。Adobeの Digital Experience のお客様は、Adobe Experience Platform Web SDK を無料で使用できます。 を使用するお客様 [!DNL Web SDK] は、データ収集 UI またはExperience PlatformUI でスキーマ、データセット、ID 名前空間、データストリームを作成するために、適切な権限を設定する必要があります。

これらの権限の設定について詳しくは、 [データ収集権限の管理](https://experienceleague.adobe.com/docs/experience-platform/collection/permissions.html?lang=ja).

## Web SDK の使用者

Adobe Experience Platform Web SDK は、次のお客様向けに開発されました。

* Adobe Experience Platformユーザー
   * デバイスからAdobe Experience Platformに直接データを送信する必要がある場合は、この方法を正式にお勧めします。
   * Adobeは、既にAdobe Analyticsがある場合、Adobe Analyticsコネクタの使用はより高速になることを認識していますが、これはデータ収集の長期的な戦略ではありません。

* Adobe Experience Cloudソリューションのお客様
   * 新しいAdobe Analytics、Adobe Audience ManagerおよびAdobe Targetのお客様は、レガシーライブラリを使用せず、新しい Web SDK から始める必要があります。
   * 可能な限り最適化された実装を取得したい既存のお客様は、新しい Web SDK を使用する必要があります。

## Web SDK にアクセスするにはどうすればよいですか？

Web SDK は、現在、一般ユーザーが利用でき、Adobe Experience Cloud製品へのデータ送信に使用できます。 サードパーティソリューションにデータを送信する機能は、近い将来に提供される予定です。

SDK は無料で、Adobeで無料でホストされます。 必要に応じて、ダウンロードして独自のサーバーで無料でホストできます。

Web SDK は、 [データストリーム設定](../datastreams/overview.md) そしてExperience Platform [XDM スキーマビルダー](../xdm/tutorials/create-schema-ui.md):Adobeのサーバーが SDK からの受信データを適切に処理できるようにするために使用します。 アクセス権を取得する場合は、担当のAdobeアカウントチームに連絡して、リクエストプロセスを開始してください。

## Web SDK で現在サポートされている使用例を教えてください。

Web SDK は急速に進化しています。 現在、さらに多くの使用例が対象となっています。 次の項目が見つかります。 [現在サポートされている使用例のリストはこちらです。](https://github.com/orgs/adobe/projects/18/views/1?filterQuery=)

## 現在のお客様はサイトを再タグ付けする必要がありますか。

場合によります。Adobe Experience Platform Web SDK は、2 つの異なるスタイルでデプロイできます。 今後の移行ドキュメントでは、追加の詳細情報が提供されます。

* **別のタグのみ：** サイトが既にソリューション用にタグ付けされていて、再タグ付けできず、Experience Platformの使用例や今後のイベント転送機能（以下を参照）を目的としてAdobe Experience Platform Edge Network にデータを送信する場合、 `alloy.js` タグをサイトに追加します。このタグは、「単なる別のタグ」として機能します。

* **1 つの唯一のタグは、次のようになります。** Experience Cloudソリューションで Web SDK を使用する場合は、Web SDK を _すべて_ 」という名前に変更します。 例えば、サイトが既にAdobe Analytics用にタグ付けされていて、それを Target で使用する場合は、今後の他のユーザーも同様に使用する必要があります。

つまり、ソリューション以外の使用例でAdobe Experience Platform Web SDK を使用する場合は、 `alloy.js` 新しい解決策のように進みます。 Adobe Analytics、Target、Audience Manager、またはアプリケーションの使用例で使用する場合は、ページ上の従来のコードを削除する必要が生じる場合があります。

## Web サイトの訪問者が新規訪問者として表示されないように、Web SDK の使用を開始する際に ECID を移行することはできますか？

はい、Adobe Experience Platform Web SDK には、ID 移行機能が用意されています。 ID の移行手順に従います。 [Platform Web SDK ID ドキュメント](/help/web-sdk/identity/overview.md#id-migration) を参照してください。

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

アドビの SDK を使用し、Edge ネットワークに XDM を送信する場合、これらの新機能のイベント転送を使用すると、新しいサーバー側拡張機能をインストールして、そのデータを任意の場所にマッピングし、Edge ネットワークから任意の場所に送信できます。 これは、「データ収集はサービスとして」と考えてください。 これは、コストで利用できるほか、Adobe Experience Platformの一部としてバンドルされます。

## CNAME またはファーストパーティドメインとは何ですか？また、なぜ重要なのですか？

CNAME について詳しくは、 [Adobe文書](https://experienceleague.adobe.com/docs/id-service/using/reference/analytics-reference/cname.html?lang=ja)

## Adobe Experience Platform Web SDK は Cookie を使用しますか？ その場合、どのような cookie が使用されますか。

はい。現在、Web SDK は、実装に応じて、1 ～ 7 個の Cookie を使用します。 Web SDK で使用される Cookie と、その使用方法の一覧を以下に示します。

| **名前** | **maxAge** | **親しみやすい年齢** | **説明** |
|---|---|---|---|
| **kndct_orgid_identity** | 34128000 | 395 日 | ID Cookie には、ECID と、ECID に関連するその他の情報が格納されます。 |
| **kndctr_orgid_consent_check** | 7200 | 2 時間 | この cookie は、Web サイトに対するユーザーの同意設定を保存します。 |
| **kndctr_orgid_consent** | 15552000 | 180 日 | このセッションベースの Cookie は、サーバーが同意設定サーバー側を検索するように通知します。 |
| **kndctr_orgid_cluster** | 1800 | 30 分 | この cookie は、現在のユーザーのリクエストを処理する Edge ネットワーク地域を保存します。 Edge Network がリクエストを正しい地域にルーティングできるように、地域は URL パスで使用されます。 この cookie の有効期間は 30 分なので、ユーザーが別の IP アドレスで接続した場合、リクエストは最も近い地域にルーティングできます。 |
| **mbox** | 63072000 | 2 年。 | この cookie は、Target の移行設定が true に設定されている場合に表示されます。 これにより、Target が許可されます。 [mbox の Cookie](https://developer.adobe.com/target/implement/client-side/atjs/atjs-cookies/) を Web SDK で設定します。 |
| **mboxEdgeCluster** | 1800 | 30 分 | この cookie は、Target の移行設定が true に設定されている場合に表示されます。 Web SDK はこの cookie を使用して正しいエッジクラスターを at.js に通信し、ユーザーがサイト内を移動する際に Target プロファイルの同期を維持できます。 |
| **AMCV_###@AdobeOrg** | 34128000 | 395 日 | この Cookie は、Adobe Experience Platform Web SDK での ID の移行が有効になっている場合にのみ表示されます。 この cookie は、サイトの一部で visitor.js をまだ使用している間に Web SDK に移行する際に役立ちます。 詳しくは、 [`idMigrationEnabled`](/help/web-sdk/commands/configure/idmigrationenabled.md) を参照してください。 |

Web SDK を使用する場合、Edge ネットワークは上記の 1 つ以上の cookie を設定します。 Edge ネットワークは、 `secure` および `sameSite="none"` 属性。

現在、Web サイトにセキュリティで保護されたセクションとセキュリティで保護されていないセクションの両方がある場合、これがユーザーの識別を妨げる可能性があります。 ユーザーがサイトのセキュリティで保護されたセクションからセキュリティで保護されていないセクションに移動すると、Edge ネットワークは新しい `ECID` リクエストに置き換えます。

## Adobe Experience Platform Web SDK はどのブラウザーをサポートしていますか？

Adobe Experience Platform Web SDK は、Google Chrome、Safari、Firefox、Internet Explorer 11、Microsoft Edge Chromium の最新バージョンで最適に動作するように設計されています。 古いバージョンのブラウザーでは、特定の機能の使用に問題が生じる場合があります。

## Adobe Experience Platform Web SDK に関する詳細はどこで入手できますか？

* [ドキュメント](/help/web-sdk/home.md)
* [ソースコード](https://github.com/adobe/alloy)

### Internet Explorer のサポート {#support-internet-explore}

この SDK は、非同期タスクの完了を伝える方法として promise を使用します。 The [プロミス](https://developer.mozilla.org/ja-JP/docs/Web/JavaScript/Reference/Global_Objects/Promise) SDK で使用される実装は、次を除くすべての target ブラウザーでネイティブにサポートされます。 [!DNL Internet Explorer]. で SDK を使用するには、以下を実行します。 [!DNL Internet Explorer]に設定するには、 `window.Promise` [多充填の](https://remysharp.com/2010/10/08/what-is-a-polyfill).

既に `window.Promise` がポリフィルされているかどうかを判断するには、次の手順を実行します。

1. で Web サイトを開きます。 [!DNL Internet Explorer].
1. ブラウザーのデバッグコンソールを開きます。
1. コンソールに「`window.Promise`」とに入力し、Enter キーを押します。

`undefined` 以外が表示された場合は、既に `window.Promise`　がポリフィルされている可能性があります。上記のインストール手順を完了した後に Web サイトを読み込むことで、`window.Promise` がポリフィルされているかどうかを判断する方法もあります。SDK が promise に関するエラーをスローした場合、`window.Promise` がポリフィルされていない可能性が高くなります。

既に決定している場合は、ポリフィルする必要があります `window.Promise`の上に、前に提供したベースコードの上に次のスクリプトタグを含めます。

```html
<script src="https://cdn.jsdelivr.net/npm/promise-polyfill@8/dist/polyfill.min.js"></script>
```

このタグは、 `window.Promise` は、有効な Promise 実装です。

>[!NOTE]
>
>別の Promise 実装を読み込む場合は、がをサポートしていることを確認してください。 `Promise.prototype.finally`.

### Internet Explorer のサポート

Adobe Experience Platform SDK は、非同期タスクの完了を伝える方法として promise を使用します。 The [プロミス](https://developer.mozilla.org/ja-JP/docs/Web/JavaScript/Reference/Global_Objects/Promise) SDK で使用される実装は、次を除くすべての target ブラウザーでネイティブにサポートされます。 [!DNL Internet Explorer]. で SDK を使用するには、以下を実行します。 [!DNL Internet Explorer]に設定するには、 `window.Promise` [多充填の](https://remysharp.com/2010/10/08/what-is-a-polyfill).

promise のポリフィルに使用できるライブラリの 1 つに、promise-polyfill があります。 詳しくは、 [promise-polyfill ドキュメント](https://www.npmjs.com/package/promise-polyfill) を参照してください。

>[!NOTE]
>
>別の Promise 実装を読み込む場合は、がをサポートしていることを確認してください。 `Promise.prototype.finally`.