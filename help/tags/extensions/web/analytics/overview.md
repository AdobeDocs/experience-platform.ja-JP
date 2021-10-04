---
title: Adobe Analytics 拡張機能の概要
description: Adobe Experience Platform の Adobe Analytics タグ拡張機能について説明します。
exl-id: 33ebdcb6-9bf0-44e6-b016-e93fe78af578
source-git-commit: a8b0282004dd57096dfc63a9adb82ad70d37495d
workflow-type: tm+mt
source-wordcount: '2202'
ht-degree: 100%

---

# Adobe Analytics 拡張機能の概要

>[!NOTE]
>
>Adobe Experience Platform Launch は、Adobe Experience Platform のデータ収集テクノロジースイートとしてリブランドされています。 その結果、製品ドキュメント全体でいくつかの用語の変更がロールアウトされました。 用語の変更点の一覧については、次の[ドキュメント](../../../term-updates.md)を参照してください。

このリファレンスは、Adobe Analytics 拡張機能の設定に関する情報、およびこの拡張機能を使用してルールを作成するときに使用できるオプションに関する情報です。

## Adobe Analytics 拡張機能の設定

この節では、Adobe Analytics 拡張機能を設定する際に使用できるオプションについて説明します。

Adobe Analytics 拡張機能がまだインストールされていない場合は、プロパティを開いて&#x200B;**[!UICONTROL エクステンション／カタログ]**&#x200B;を選択し、Adobe Analytics 拡張機能にカーソルを置いて「**[!UICONTROL インストール]**」を選択します。

拡張機能を設定するには、「拡張」タブを開き、拡張機能にカーソルを置いて「**[!UICONTROL 設定]**」を選択します。

![](../../../images/ext-analytics-config.png)

## ライブラリ管理

設定ページの「Library Management」セクションからオプションを選択します。次の設定オプションを使用できます。

### Manage the library for me

#### レポートスイート

次の各環境に対して、1 つ以上のレポートスイートを指定します。

* 開発
* ステージング
* 実稼動

### ページに既にインストールされているライブラリを使用する

#### トラッカーで次のレポートスイートを設定する

このオプションを選択する場合は、次の各環境に対して 1 つ以上のレポートスイートを指定します。

* 開発
* ステージング
* 実稼動

#### Activity Map モジュールの使用

Activity Map は、AAM モジュールと同様、個別のモジュールとして読み込まれます。デフォルトでは Activity Map はオンになっていますが、オフにしたい場合は、設定でチェックボックスをオフにすることができます。

#### 次の名前のグローバル変数でトラッカーにアクセスできる

このチェックボックスをオンにすると、トラッカーオブジェクトをグローバルに使用することができます。例えば、サイト上の任意の場所で変数 `window.s.pageName` を定義できます。

### カスタム URL からのライブラリの読み込み

#### HTTP URL

ライブラリの URL を指定します。

#### HTTPS URL

ライブラリの URL を指定します。

#### トラッカーで次のレポートスイートを設定する

このオプションを選択する場合は、次の各環境に対して 1 つ以上のレポートスイートを指定します。

* 開発
* ステージング
* 実稼動

#### 次の名前のグローバル変数でトラッカーにアクセスできる

グローバルに使用するトラッカーオブジェクトを指定します。

### カスタムライブラリコードを自分で提供する

#### 編集画面を開く

コアの [AppMeasurement.js](https://experienceleague.adobe.com/docs/analytics/implementation/js/overview.html?lang=ja) コードを挿入できます。このコードは、自動生成メソッドを使用すると自動的に生成されます。

>[!NOTE]
>
>タグコードエディターで使用されるバリデーターは、開発者が作成したコードの問題を識別するように作られています。縮小処理をおこなったコード（コードマネージャーからダウンロードした AppMeasurement.js コードなど）は、タグバリデーターによって、誤って「問題あり」とフラグ付けされることがありますが、通常は無視できます。

#### トラッカーで次のレポートスイートを設定する

このオプションを選択する場合は、次の各環境に対して 1 つ以上のレポートスイートを指定します。

* 開発
* ステージング
* 実稼動

#### 次の名前のグローバル変数でトラッカーにアクセスできる

グローバルに使用するトラッカーオブジェクトを指定します。

## 全般

設定ページの「General」セクションからオプションを選択します。次の設定オプションを使用できます。

### Enable EU compliance for Adobe Analytics で EU 

EU プライバシー Cookie に基づいたトラッキングを有効または無効にします。

EU コンプライアンスのチェックボックスをオンにすると、「[!UICONTROL Cookie 名をトラッキング]」フィールドが表示されます。トラッキング cookie により、デフォルトのトラッキング cookie 名が上書きされます。タグが、別の cookie の受信に対するオプトアウトステータスを追跡するために使用する名前をカスタマイズできます。

ページが読み込まれる際に、システムは、sat\_track という名前（またはプロパティ編集ページで変更したカスタム名）の Cookie が設定されているかどうかを確認します。次の情報を考慮してください。

* Cookie が存在しない、または cookie が存在するが true 以外に設定されている場合、この設定を有効にするとツールの読み込みはスキップされます。つまり、ツールを使用するルールはどの部分も適用されません。ルールで分析の EU コンプライアンスがオンになっていて、ルールにサードパーティコードが含まれ、その Cookie が false に設定されている場合、そのサードパーティコードは実行されます。ただし、Analytics 変数は設定されません。
* Cookie が存在するが true に設定されている場合、ツールは通常どおり読み込まれます。

訪問者がオプトアウトしている場合、sat\_track（またはカスタム名）cookie を false に設定する必要があります。次のカスタムコードを実行することでこれを達成できます：

```javascript
_satellite.cookie.set("sat_track", "false");
```

また、後で訪問者によるオプトイン操作ができるようにするには、この Cookie を次のようにしてtrue に設定するための仕組みを用意する必要があります。

```javascript
_satellite.cookie.set("sat_track", "true");
```

### 文字セット

イメージリクエストのエンコード方法を決定します。実装またはサイトで非 ASCII 文字を使用している場合は、ここで文字セットを定義することが重要です。プリセット文字セットを選択するか、カスタム文字セットを指定できます。サイトと同じ文字コードを使用することをお勧めします。通常、この値は UTF-8 です。

文字セットは、`s.charSet` 変数を使用して Analytics カスタムコードで設定できます。文字セットについて詳しくは、 [charSet のドキュメント](https://experienceleague.adobe.com/docs/analytics/implementation/vars/config-vars/charset.html?lang=ja) を参照してください。

### 通貨コード

売上高および通貨イベントに適用するコンバージョン率を決定します。サイトが訪問者に対し、複数の通貨での購入を許可している場合、通貨コードを設定すると、金額が正しく変換されて保存されます。

サポートされる通貨コードについて詳しくは、 [currencyCode](https://experienceleague.adobe.com/docs/analytics/implementation/vars/config-vars/currencycode.html?lang=ja) を参照してください。

### トラッキングサーバー

ファーストパーティ cookie の保存場所を示すために、ファーストパーティ cookie の実装で使用されます。Experience Cloud ID サービスを使用する場合、アドビでは、このフィールドの入力についてアドバイスします。

トラッキングサーバーは、`s.trackingServer` 変数を使用して Analytics カスタムコードで設定できます。

Adobe Analytics 実装ガイドの「[trackingServer](https://experienceleague.adobe.com/docs/analytics/implementation/vars/config-vars/trackingserver.html?lang=ja)」を参照してください。

### SSL トラッキングサーバー

ファーストパーティ cookie の保存場所を示すために、SSL ファーストパーティ cookie の実装で使用されます。Experience Cloud ID サービスを使用する場合、アドビでは、このフィールドの入力についてアドバイスします。定義されていない場合、SSL データはトラッキングサーバーを使用します。

SSL トラッキングサーバーは、`s.trackingServerSecure` 変数を使用して Analytics カスタムコードで設定できます。

「[trackingServerSecure](https://experienceleague.adobe.com/docs/analytics/implementation/vars/config-vars/trackingserversecure.html?lang=ja)」を参照してください。

## グローバル変数

このセクションを使用して、[eVar と Prop](https://experienceleague.adobe.com/docs/analytics/implementation/vars/page-vars/evar.html?lang=ja) を設定し、階層を作成します。

グローバル変数は、Analytics トラッキングオブジェクトがページ上で初期化されたときに、そのオブジェクトで設定される変数です。ここで設定した変数は、各ページでトラッキングオブジェクトが作成されるときに設定されます。これらの変数を設定すると、他のすべての変数と同様に、これらの変数が設定されます。つまり、ルールはこれらの変数を修正、変更またはクリアできます。

Web アプリケーションが通常 1 ページに 1 つのビーコンを送信する場合、このセクションを使用すれば、変数を 1 か所で簡単に設定できます。アプリケーションがページごとに 1 つ以上のビーコンを送信し（シングルページアプリケーションなど）、変数をクリアして同じトラッキングオブジェクトを使用してリセットする必要がある場合は、変数を設定およびクリアするルールに依存する方が簡単です。

## リンクトラッキング

設定ページの「Link Trackng」セクションからオプションを選択します。次の設定オプションを使用できます。

### ClickMap を有効にする

[ClickMap](https://experienceleague.adobe.com/docs/analytics/analyze/activity-map/activity-map.html?lang=ja) は、Internet Explorer および Firefox 用のプラグインであり、Reports &amp; Analytics を構成する 1 つのモジュールです。

### ダウンロードリンクを追跡

サイト上のダウンロード可能なファイルへのリンクを追跡します。

「[s.trackDownLoadLinks](https://experienceleague.adobe.com/docs/analytics/implementation/vars/config-vars/trackdownloadlinks.html?lang=ja)」を参照してください。

### ダウンロード拡張機能

「Track Download」オプションを有効にすると、ダウンロードレポートに含まれるファイルリンクの拡張機能を選択できます。リンクされた拡張機能を使用しているファイルへのリンクがサイトに含まれている場合、これらのリンクの URL がレポートに表示されます。

「[s.linkDownloadFileTypes](https://experienceleague.adobe.com/docs/analytics/implementation/vars/config-vars/linkdownloadfiletypes.html?lang=ja)」を参照してください。

### 離脱リンクを追跡

選択されたリンクのいずれかが他のサイトへのリンクであるかどうかが判断されます。

「[s.trackExternalLinks](https://experienceleague.adobe.com/docs/analytics/implementation/vars/config-vars/trackexternallinks.html?lang=ja)」を参照してください。

**単一ページアプリに関する考慮事項：**&#x200B;一部の SPA（単一ページアプリ）Web サイトで採用されているコーディング方法の場合、その SPA サイト上にあるページへの内部リンクが、外部リンクの形式になっていることがあります。

SPA サイトから外部へのリンクは、次のいずれかの方法でトラッキングできます。

* SPA からの外部リンクをトラッキングする必要がない場合は、「Never Track」セクションにエントリを追加します。例：`http://testsite.com/spa/\#`。このホストへのすべての \# リンクは無視されます。その他のホストに対するすべての外部リンク（例：[https://www.google.com](https://www.google.com)）は追跡されます。
* SPA 上にある一部のリンクをトラッキングする必要がある場合は、「Always Track」セクションを使用します。

例えば、spa/\#/about にあるページをトラッキングするには、「about」を「Always Track」セクションに登録します。

そうすると、外部リンクのうち「about」ページだけがトラッキングされます。ページ上にあるその他のリンク（例：[https://www.google.com](https://www.google.com)）は追跡されません。

>[!NOTE]
>
>これらの 2 つのオプションは、相互に排他的です。

### URL パラメーターを保持

クエリー文字列を維持します。

「[s.linkLeaveQueryString](https://experienceleague.adobe.com/docs/analytics/implementation/vars/config-vars/linkleavequerystring.html?lang=ja)」を参照してください。

## Cookie

Adobe Analytics 拡張機能のデプロイに使用する Cookie のグローバル設定について、フィールドの説明を設定します。次の設定オプションを使用できます。

### 訪問者 ID

オンラインとオフラインの両方のシステムに存在する顧客を表す一意の値。

[visitorID](https://experienceleague.adobe.com/docs/analytics/import/data-sources/data-types-and-categories/datasrc-visitorid.html?lang=ja) を参照してください。

### 訪問者名前空間

Cookie の設定場所であるドメインを識別する変数。

[visitorNamespace](https://experienceleague.adobe.com/docs/analytics/implementation/vars/config-vars/visitornamespace.html?lang=ja) を参照してください。

### ドメインピリオド

ページ URL のドメイン内のピリオド数を指定することで、Analytics の Cookie である `s_cc` と `s_sq` を設定するドメインが決定されます。この変数は、一部のプラグインで、プラグインの cookie を設定するための適切なドメインを決定する際にも使用されます。

「[s.cookieDomainPeriods](https://experienceleague.adobe.com/docs/analytics/implementation/vars/config-vars/cookiedomainperiods.html?lang=ja)」を参照してください。

### ファーストパーティドメインピリオド

`fpCookieDomainPeriods` 変数は、導入でサードパーティ 2o7.net または omtrdc.net ドメインが使用されている場合でも、JavaScript（`s_sq`、`s_cc`、プラグイン）によって設定された本質的にファーストパーティの Cookie 用に使用されます。

「[s.fpCookieDomainPeriods](https://experienceleague.adobe.com/docs/analytics/implementation/vars/config-vars/fpcookiedomainperiods.html?lang=ja)」を参照してください。

### Cookie の有効期限

Cookie の存続期間を決定します。

「[s.cookieLifetime](https://experienceleague.adobe.com/docs/analytics/implementation/vars/config-vars/cookielifetime.html?lang=ja)」を参照してください。

### cookie の保護

この変数を使用すると、AppMeasurement に対して安全な cookie の書き込みを許可します。

[writeSecureCookies](https://experienceleague.adobe.com/docs/analytics/implementation/vars/config-vars/writesecurecookies.html?lang=ja) を参照してください


## ページコードのカスタマイズ

エディターを使用してページコードをカスタマイズします。

## Adobe Audience Manager

拡張機能設定のこのセクションを使用して、Audience Manager が Analytics でどのように操作するかを指定します。

「**Automatically share Analytics data with Audience Manager**」を有効にします。

次のオプションが表示されます。

![](../../../images/an-ext-aam.png)

Audience Manager サブドメインは、Adobe Audience Manager によって割り当てられます。「パートナー ID」や「パートナーサブドメイン」と呼ばれることもあります。パートナー名が不明な場合は、アドビのコンサルタントまたはカスタマーケアにお問い合わせください。

「**詳細設定を表示**」を選択し、環境設定を入力することで、詳細設定をおこなうことができます。

![](../../../images/an-ext-aam-adv.png)

各設定について詳しくは、情報アイコンを選択するか、 [Adobe Audience Manager のドキュメント](https://experienceleague.adobe.com/docs/audience-manager/user-guide/aam-home.html?lang=ja) を参照してください。

## Analytics 拡張機能のアクションタイプ

この節では、Analytics 拡張機能で使用できるアクションタイプについて説明します。

Analytics 拡張機能は、次のアクションを提供します。

* [変数を設定](#set-variables)
* [ビーコンを送信](#send-beacon)
* [変数をクリア](#clear-variables)

### 変数を設定 {#set-variables}

重要: 「Set Variables」アクションを使用しても、ビーコンは送信されません。「Send Beacon」アクションを使用する必要があります。

#### eVar

1 つ以上の [eVar](https://experienceleague.adobe.com/docs/analytics/implementation/vars/page-vars/evar.html) を設定します。

1. ドロップダウンから eVar を選択します。
1. eVar を値として設定（Set As）するか、別の eVar をコピー（Duplicate From）するかを指定します。
1. 「Set As」に値を指定するか、複製する eVar を選択します。
1. （オプション）「Add eVar」を選択して、さらに eVar を設定します。
1. 「**[!UICONTROL 変更を保持]**」を選択します。

#### prop

1 つ以上の [prop](https://experienceleague.adobe.com/docs/analytics/implementation/vars/page-vars/prop.html?lang=ja) を設定します。

1. ドロップダウンから prop を選択します。
1. prop を値（設定形式）として設定するか、別の eVar をコピー（複製）するかを指定します。
1. 「Set As」値を指定するか、prop を複製する eVar を選択します。
1. （オプション）「**[!UICONTROL プロパティを追加]**」を選択してさらにプロパティを設定します。
1. 「**[!UICONTROL 変更を保持]**」を選択します。

#### イベント

1 つ以上の[イベント](https://experienceleague.adobe.com/docs/analytics/implementation/vars/page-vars/events/events-overview.html?lang=ja)を設定します。

1. ドロップダウンからイベントを選択します。
1. （オプション）[イベントのシリアル化](https://experienceleague.adobe.com/docs/analytics/implementation/vars/page-vars/events/event-serialization.html?lang=ja)に使用するデータ要素を選択または指定します。
1. （オプション）「**[!UICONTROL イベントを追加]**」を選択してさらにイベントを設定します。
1. 「**[!UICONTROL 変更を保持]**」を選択します。

#### 階層

[Analytics 階層変数](https://experienceleague.adobe.com/docs/analytics/implementation/vars/page-vars/hier.html?lang=ja)を設定します。

階層内の各レベルを指定します。

必要に応じて、追加の階層を設定します。

#### その他の情報

ページで使用するその他の情報を指定します。

この設定には、次のものが含まれています。

* ページ名
* ページ URL
* サーバー
* チャネル
* リファラー
* キャンペーン
* 購入 ID

   値またはクエリパラメータのいずれかを指定します

* 都道府県
* 郵便番号
* トランザクション ID

これらの設定は、「追加設定」チェックボックスを選択すると、「グローバル変数」メニューに表示されます。

#### カスタムのページコード

**説明**

エディターを使用してカスタムのページコードを指定します。

**設定**

1. 「**[!UICONTROL エディターを開く]**」を選択します。
1. カスタムコードを入力します。
1. 「**[!UICONTROL 保存]**」を選択します。

### ビーコンを送信 {#send-beacon}

#### ページプレビューを増分 - s.t()

ページビューを増分する場合に選択します。

#### ページビューをインクリメントしないでください - s.tl()

ページビューを増分しない場合に選択します。

**設定**

1. リンクタイプを選択します。

   次のいずれかを選択できます。

   * カスタムリンク。
   * ダウンロードリンク。
   * 離脱リンク。

1. 選択したリンクのパラメーターを設定します。
   * カスタムリンク：リンク名を指定します。
   * ダウンロードリンク：ファイル名を指定します。
   * 離脱リンク：宛先 URL を指定します。
1. 「**[!UICONTROL 変更を保持]**」を選択します。

### 変数をクリア {#clear-variables}

「Clear Variables」アクションタイプが選択されている場合、設定オプションはありません。
