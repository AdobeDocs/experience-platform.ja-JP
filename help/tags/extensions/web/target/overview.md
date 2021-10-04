---
title: Adobe Target 拡張機能の概要
description: Adobe Experience Platform の Adobe Target のタグ拡張機能について説明します。
exl-id: b1c5e25b-42ea-4835-b2d4-913fa2536e77
source-git-commit: a8b0282004dd57096dfc63a9adb82ad70d37495d
workflow-type: tm+mt
source-wordcount: '1194'
ht-degree: 100%

---

# Adobe Target 拡張機能の概要

>[!NOTE]
>
>Adobe Experience Platform Launch は、Adobe Experience Platform のデータ収集テクノロジースイートとしてリブランドされています。 その結果、製品ドキュメント全体でいくつかの用語の変更がロールアウトされました。 用語の変更点の一覧については、次の[ドキュメント](../../../term-updates.md)を参照してください。

このリファレンスは、この拡張機能を使用してルールを作成するときに使用できるオプションに関する情報です。

## Adobe Target 拡張機能の設定

>[!IMPORTANT]
>
>  Adobe Target 拡張機能には at.js が必要です。mbox.js はサポートしていません。

Adobe Target 拡張機能がまだインストールされていない場合は、プロパティを開いて、**[!UICONTROL 拡張機能／カタログ]**&#x200B;を選択し、Target 拡張機能にカーソルを置いて「**[!UICONTROL インストール]**」を選択します。

拡張機能を設定するには、「[!UICONTROL エクステンション]」タブを開き、拡張機能にカーソルを置いて「**[!UICONTROL 設定]**」を選択します。

![](../../../images/ext-target-config.png)

### at.js の設定

タイムアウトを除くすべての at.js 設定は、Target ユーザーインターフェイスの at.js 設定から自動的に取得されます。拡張機能は、最初に追加されたときに Target ユーザーインターフェイスから設定を取得するだけです。追加の更新が必要な場合は、すべての設定をデータ収集 UI で管理する必要があります。

次の設定オプションを使用できます。

#### Client Code

クライアントコードは Target のアカウント識別子です。このコードはほとんど常にデフォルト値のまま使用されます。

データ要素を使用して変更できます。

#### Organization ID

この ID は、実装を Adobe Experience Cloud アカウントと結び付けます。このコードはほとんど常にデフォルト値のまま使用されます。

データ要素を使用して変更できます。

#### Global Mbox Name

グローバル Target リクエストの名前を示します。デフォルトでは、この名前は「target-global-mbox」です（拡張機能を追加する前に Target ユーザーインターフェイスで名前を変更している場合を除く）。

データ要素を使用して変更できます。

#### Server Domain

Target リクエストが送信されるドメイン。このコードはほとんど常にデフォルト値のまま使用されます。

#### Cross Domain

Target がブラウザー内で Cookie を設定する場所を決定します。

* **Disabled：**&#x200B;ファーストパーティドメインでのみ cookie を設定します。一般的な設定です。
* **Enabled：**&#x200B;ファーストパーティドメインとサードパーティ Target ドメイン（「Server Domain」）の両方で cookie を設定します。

#### Timeout (ms)

定義された期間内に Target からの受信されなかった場合、リクエストはタイムアウトし、デフォルトコンテンツが表示されます。訪問者のセッション中、追加のリクエストが引き続き試行されます。デフォルト値は「3000ms」です（Target ユーザーインターフェイスで設定されているタイムアウトとは異なる可能性があります）。

タイムアウトの設定の仕組みについて詳しくは、 [Adobe Target ヘルプ](https://experienceleague.adobe.com/docs/target/using/implement-target/client-side/deploy-at-js/implementing-target-without-a-tag-manager.html?lang=ja) を参照してください。

#### Target ユーザーインターフェイスで利用可能なその他の at.js 設定

Target UI の [!UICONTROL at.js 設定の編集]ページで利用できる設定には、Target 拡張機能には含まれていないものがあります。次の回避策が推奨されます。

* グローバル mbox の自動作成：この設定は、Target 拡張機能の「Fire Global Mbox」アクションに置き換えられます。
* ライブラリヘッダー：この設定は、Target 拡張機能には含まれていません。Load Target アクションを使用する前に、Core Extension／Custom Code アクションで at.js を読み込む必要があるコードを配置します。
* ライブラリフッター：この設定は、Target 拡張機能には含まれていません。Load Target アクションを使用した後、Core Extension／Custom Code アクションで at.js を読み込む必要があるコードを配置します。

## Target 拡張機能のアクションタイプ

ここでは、Target 拡張機能で使用できるアクションタイプについて説明します。

Target 拡張機能は、ルールの「Then」部分に次のアクションを提供します。

### Load Target

ルールのコンテキストで Target を読み込むと効果的な場合、タグルールにこのアクションを追加します。これにより、at.js ライブラリがページに読み込まれます。ほとんどの実装では、サイトの各ページに Target を読み込む必要があります。

設定は不要です。

### Add Mbox Params

すべての mbox リクエストにパラメーターを追加します。これより前に、Load Target アクションを使用する必要があります。

1. 追加するパラメーターの名前と値を指定します。
1. パラメーターを追加するには、**プラス**&#x200B;アイコンをクリックします。

### Add Global Mbox Params

グローバル mbox リクエストにのみパラメーターを追加します。これより前に、Load Target アクションを使用する必要があります。

1. 追加するパラメーターの名前と値を指定します。
1. パラメーターを追加するには、**プラス**&#x200B;アイコンをクリックします。

### Fire Global Mbox

ページ上でグローバル mbox を実行します。これより前に、Load Target アクションを使用する必要があります。

ちらつきを防ぐために本分の非表示を有効にするかどうか、および body 要素を非表示にするときに使用するスタイルを指定します。

次のオプションがあります。

* **Body Hiding：**&#x200B;この設定を有効または無効にすることができます。デフォルト値は false です。この場合、HTML BODY は非表示にはなります。
* **Body Hidden Style：**&#x200B;デフォルトの値は `body{opacity:0}`です。この値は別の値（`body{display:none}`など）に変更することもできます。

詳しくは、 [Target のオンラインヘルプドキュメント](https://experienceleague.adobe.com/docs/target/using/implement-target/client-side/mbox-implement/advanced-mboxjs-settings.html?lang=ja) を参照してください。

## Adobe Target の基本的なデプロイメント

Target 拡張機能がインストールされたら、1 つ以上のルールを作成して適切にデプロイする必要があります。最初に、Target ライブラリ（at.js）を読み込み、グローバル mbox で使用するパラメーターを指定して、グローバル mbox を実行します。

この基本的な実装を含む Target ルールは次のようになります。

![](../../../images/basic_target_implementation.png)

このルールを保存したら、動作をテストできるよう、ライブラリに追加してビルド／デプロイします。

## 非同期デプロイメントを使用した Adobe Target 拡張機能

タグは非同期でデプロイできます。Target を使用してタグライブラリを非同期で読み込む場合、Target も非同期で読み込まれます。これは完全にサポートされているシナリオですが、処理が必要な追加の検討事項が 1 つあります。

非同期デプロイメントでは、Target ライブラリをの読み込みが完了してコンテンツの入れ替えが実行される前に、ページがデフォルトコンテンツのレンダリングを終了する場合があります。これにより、Target で指定し、パーソナライズされたコンテンツに置き換えられる前に、デフォルトのコンテンツが短時間表示される、「ちらつき」と呼ばれる現象が発生することがあります。コンテンツのちらつきを回避するには、事前非表示のスニペットを使用し、タグバンドルを非同期で読み込むことをお勧めします。

事前非表示のスニペットを使用する際には、次の点に注意してください。

* タグヘッダー埋め込みコードを読み込む前に、スニペットを追加する必要があります。
* このコードはタグでは管理できないので、直接ページに追加する必要があります。
* 次のイベントが発生すると、ページが表示されます。
   * グローバル mbox の応答を受信したとき
   * グローバル mbox リクエストがタイムアウトしたとき
   * スニペット自体がタイムアウトしたとき
* 事前非表示の時間を最小限に抑えるために、「Fire Global Mbox」アクションは、事前非表示スニペットを使用するすべてのページで使用する必要があります。

事前非表示スニペットの例を次に示します。これらは縮小できます。設定可能なオプションは最後にあります。

```javascript
;(function(win, doc, style, timeout) {
  var STYLE_ID = 'at-body-style';

  function getParent() {
    return doc.getElementsByTagName('head')[0];
  }

  function addStyle(parent, id, def) {
    if (!parent) {
      return;
    }

    var style = doc.createElement('style');
    style.id = id;
    style.innerHTML = def;
    parent.appendChild(style);
  }

  function removeStyle(parent, id) {
    if (!parent) {
      return;
    }

    var style = doc.getElementById(id);

    if (!style) {
      return;
    }

    parent.removeChild(style);
  }

  addStyle(getParent(), STYLE_ID, style);
  setTimeout(function() {
    removeStyle(getParent(), STYLE_ID);
  }, timeout);
}(window, document, "body {opacity: 0 !important}", 3000));
```

デフォルトでは、このスニペットによって HTML BODY 全体が事前に非表示になります。ページ全体ではなく、特定の HTML 要素のみを事前に非表示したい場合もあります。その場合はスタイルのパラメーターをカスタマイズします。ページ内の特定の領域のみを事前に非表示にするよう置き換えることができます。

例えば、container-1 と container-2 という ID で識別される 2 つのセクションがある場合は、次のスタイルに置き換えることができます。

```css
#container-1, #container-2 {opacity: 0 !important}
```

デフォルトの代わりに次を使用します。

```css
body {opacity: 0 !important}
```

デフォルトでは、スニペットは 3000 ミリ秒または 3秒でタイムアウトします。この値はカスタマイズできます。
