---
title: Adobe Experience Platform Web SDK 用のシングルページアプリケーション実装
description: Adobe Targetを使用してAdobe Experience Platform Web SDK のシングルページアプリケーション (SPA) 実装を作成する方法について説明します。
keywords: target;adobe target;xdm ビュー；ビュー；単一ページアプリケーション；SPA;SPAのライフサイクル；クライアント側；AB テスト；AB；エクスペリエンスのターゲット設定；XT;VEC
exl-id: cc48c375-36b9-433e-b45f-60e6c6ea4883
source-git-commit: 0085306a2f5172eb19590cc12bc9645278bd2b42
workflow-type: tm+mt
source-wordcount: '1665'
ht-degree: 13%

---

# シングルページアプリケーションの実装

Adobe Experience Platform Web SDK は、シングルページアプリケーション (SPA) など、次世代のクライアント側テクノロジーでパーソナライゼーションを実行するための機能を提供します。

従来の Web サイトは、「ページ間」ナビゲーションモデル（別名マルチページアプリケーション）を使用していました。このモデルでは、Web サイトデザインは URL と密接に結合され、ある Web ページから別のページへのトランジションにはページ読み込みが必要になります。

代わりに、シングルページアプリケーションなどの最新の Web アプリケーションでは、ブラウザー UI のレンダリングを迅速に使用するモデルを採用しています。これは、多くの場合、ページのリロードとは無関係です。 これらのエクスペリエンスは、スクロール、クリック、カーソル移動などの顧客のインタラクションによってトリガーできます。 最新の Web の枠組みが進化するにつれて、パーソナライゼーションと実験をデプロイするためのページ読み込みなどの従来の汎用イベントは機能しなくなりました。

![](assets/spa-vs-traditional-lifecycle.png)

## SPA向け Platform Web SDK のメリット

シングルページアプリケーションでAdobe Experience Platform Web SDK を使用するメリットを紹介します。

* ページ読み込み時にすべてのオファーをキャッシュし、複数のサーバー呼び出しを単一のサーバー呼び出しに減らす機能。
* 従来のサーバー呼び出しで発生する遅延時間なしで、キャッシュ経由でオファーが即座に表示されるので、サイトでのユーザーエクスペリエンスが著しく向上します。
* 1 行のコードと開発者による 1 回限りのセットアップで、マーケターはSPA上で Visual Experience Composer(VEC) を使用して A/B およびエクスペリエンスのターゲット設定 (XT) アクティビティを作成し、実行できます。

## XDM ビューとシングルページアプリケーション

Adobe Target VEC for SPAは、ビューと呼ばれる概念を活用します。ビジュアル要素の論理的なグループで、全体でSPAエクスペリエンスを構成します。 したがって、単一ページアプリケーションは、ユーザーのインタラクションに基づいて、URL ではなくビュー間を移行すると考えられます。 通常、ビューはサイト全体またはサイト内のグループ化されたビジュアル要素を表せます。

ビューとは何かをさらに詳しく説明するために、次の例では、React で実装された架空のオンライン e コマースサイトを使用して、サンプルビューを調べます。

ホームサイトに移動した後、ヒーロー画像は、イースターセールと、サイトで利用可能な最新の製品を昇格します。 この場合、ビューはホーム画面全体に対して定義できます。 このビューは、単に「home」と呼ぶことができます。

![](assets/example-views.png)

顧客が販売している製品に興味を持つようになると、顧客は、 **製品** リンク。 ホームサイトと同様、製品サイト全体をビューとして定義できます。このビューには、「products-all」という名前を付けることができます。

![](assets/example-products-all.png)

ビューはサイト全体またはサイト上の視覚要素のグループとして定義できるので、製品サイトに表示される 4 つの製品をグループ化し、ビューと見なすことができます。 このビューには、「products」という名前を付けることができます。

![](assets/example-products.png)

顧客が **さらに読み込み** ボタンをクリックしてサイト上の他の製品を参照する場合、web サイトの URL は変更されませんが、表示される製品の 2 行目のみを表すビューをここで作成できます。 ビュー名は「products-page-2」になります。

![](assets/example-load-more.png)

顧客がサイトでいくつかの製品を購入することを決定し、チェックアウト画面に進みます。 チェックアウトサイトでは、通常配送または速達配送を選択できます。 ビューは、サイト上のビジュアル要素のグループにすることができるので、ビューは、配信設定に対して作成し、「配信設定」と呼ばれることができます。

![](assets/example-check-out.png)

ビューの概念は、これよりもはるかに長く拡張できます。 これらは、サイトで定義できるビューの例の一例です。

## XDM ビューの実装

XDM ビューは、Adobe Targetで活用して、マーケターが Visual Experience Composer を介してSPAで A/B テストや XT テストを実行できるようにすることができます。 これには、1 回限りの開発者向けの設定を完了するために、次の手順を実行する必要があります。

1. インストール [Adobe Experience Platform Web SDK](../../fundamentals/installing-the-sdk.md)
2. パーソナライズするシングルページアプリケーション内のすべての XDM ビューを決定します。
3. XDM ビューを定義した後、AB または XT VEC アクティビティを配信するために、 `sendEvent()` ～と機能する `renderDecisions` に設定 `true` と、対応する XDM ビューをシングルページアプリケーションで表示します。 XDM ビューを渡す必要があります `xdm.web.webPageDetails.viewName`. この手順を使用すると、マーケターは Visual Experience Composer を活用して、これらの XDM の A/B テストや XT テストを開始できます。

   ```javascript
   alloy("sendEvent", { 
     "renderDecisions": true, 
     "xdm": { 
       "web": { 
         "webPageDetails": { 
         "viewName":"home" 
         }
       } 
     } 
   });
   ```

>[!NOTE]
>
>最初の `sendEvent()` を呼び出すと、エンドユーザーにレンダリングされる必要のあるすべての XDM ビューが取得され、キャッシュされます。 後続 `sendEvent()` XDM ビューが渡された呼び出しは、キャッシュから読み取られ、サーバー呼び出しなしでレンダリングされます。

## `sendEvent()` 関数の例

この節では、 `sendEvent()` 関数を React で使用して、仮想的な e コマースSPAに対応させることができます。

### 例 1:A/B テストのホームページ

マーケティングチームは、ホームページ全体で A/B テストを実行したいと考えています。

![](assets/use-case-1.png)

ホームサイト全体で A/B テストを実行するには、 `sendEvent()` は、XDM と共に呼び出す必要があります `viewName` に設定 `home`:

```jsx
function onViewChange() { 
  
  var viewName = window.location.hash; // or use window.location.pathName if router works on path and not hash 

  viewName = viewName || 'home'; // view name cannot be empty 

  // Sanitize viewName to get rid of any trailing symbols derived from URL 

  if (viewName.startsWith('#') || viewName.startsWith('/')) { 
    viewName = viewName.substr(1); 
  }
   
  alloy("sendEvent", { 
    "renderDecisions": true, 
    "xdm": { 
      "web": { 
        "webPageDetails": { 
          "viewName":"home" 
        } 
      } 
    }
  }); 
} 

// react router v4 

const history = syncHistoryWithStore(createBrowserHistory(), store); 

history.listen(onViewChange); 

// react router v3 

<Router history={hashHistory} onUpdate={onViewChange} > 
```

### 例 2:パーソナライズされた製品

マーケティングチームは、ユーザーがクリックした後に価格ラベルの色を赤に変更して、2 行目の製品をパーソナライズしたいと考えています **さらに読み込み**.

![](assets/use-case-2.png)

```jsx
function onViewChange(viewName) { 

  alloy("sendEvent", { 
    "renderDecisions": true, 
    "xdm": { 
      "web": { 
        "webPageDetails": { 
          "viewName": viewName
        }
      } 
    } 
  }); 
} 

class Products extends Component { 
  
  render() { 
    return ( 
      <button type="button" onClick={this.handleLoadMoreClicked}>Load more</button> 
    ); 
  } 

  handleLoadMoreClicked() { 
    var page = this.state.page + 1; // assuming page number is derived from component’s state 
    this.setState({page: page}); 
    onViewChange('PRODUCTS-PAGE-' + page); 
  } 

} 
```

### 例 3:A/B テストの配信環境設定

マーケティングチームは、A/B テストを実行して、次の場合にボタンの色を青から赤に変更するかどうかを確認したいと考えています。 **速達配送** が選択されている場合は、コンバージョンが向上します（両方の配信オプションでボタンの色を青に保つのに対して）。

![](assets/use-case-3.png)

選択した配信設定に応じてサイトのコンテンツをパーソナライズするには、配信設定ごとにビューを作成します。 条件 **通常の配信** が選択されている場合、ビューに「checkout-normal」という名前を付けることができます。 If **速達配送** が選択されている場合、ビューに「checkout-express」という名前を付けることができます。

```jsx
function onViewChange(viewName) { 
  alloy("sendEvent", { 
    "renderDecisions": true, 
    "xdm": { 
      "web": { 
        "webPageDetails": { 
          "viewName": viewName 
        }
      }
    }
  }); 
} 

class Checkout extends Component { 

  render() { 
    return ( 
      <div onChange={this.onDeliveryPreferenceChanged}> 
        <label> 
          <input type="radio" id="normal" name="deliveryPreference" value={"Normal Delivery"} defaultChecked={true}/> 
          <span> Normal Delivery (7-10 business days)</span> 
        </label> 
        <label> 
          <input type="radio" id="express" name="deliveryPreference" value={"Express Delivery"}/> 
          <span> Express Delivery* (2-3 business days)</span> 
        </label> 
      </div> 
    ); 
  } 

  onDeliveryPreferenceChanged(evt) { 
    var selectedPreferenceValue = evt.target.value; 
    onViewChange(selectedPreferenceValue); 
  } 

} 
```

## SPAでの Visual Experience Composer の使用

XDM ビューの定義と実装が完了したら、 `sendEvent()` これらの XDM ビューが渡されると、VEC でこれらのビューを検出し、ユーザーが A/B アクティビティや XT アクティビティのアクションや変更を作成できるようになります。

>[!NOTE]
>
>SPAで VEC を使用するには、 [Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-target-vec-helper/) または [クロム](https://chrome.google.com/webstore/detail/adobe-target-vec-helper/ggjpideecfnbipkacplkhhaflkdjagak) VEC ヘルパー拡張機能。

### 変更パネル

変更パネルでは、特定のビュー用に作成されたアクションが取り込まれます。 ビューのすべてのアクションは、そのビューの下にグループ化されます。

![](assets/modifications-panel.png)

### アクション

アクションをクリックすると、このアクションが適用されるサイトの要素がハイライトされます。ビューの下に作成された各 VEC アクションには、次のアイコンがあります。 **情報**, **編集**, **複製**, **移動**、および **削除**. これらのアイコンの詳細については、次の表で説明します。

![](assets/action-icons.png)

| アイコン | 説明 |
|---|---|
| 情報 | アクションの詳細を表示します。 |
| Edit | アクションのプロパティを直接編集できます。 |
| 複製 | 変更パネルに存在する 1 つ以上のビューまたは VEC で参照および移動した 1 つ以上のビューにアクションを複製します。アクションは、必ずしも変更パネルに存在する必要はありません。<br/><br/>**注意：** 複製操作をおこなった後、参照を使用して VEC のビューに移動し、複製されたアクションが有効な操作かどうかを確認する必要があります。 アクションがビューに適用できない場合、エラーが表示されます。 |
| 移動 | 変更パネルに既に存在するページの読み込みイベントまたはその他のビューにアクションを移動します。<br/><br/>**ページ読み込みイベント：** ページの読み込みイベントに対応するアクションは、Web アプリケーションの最初のページ読み込みに適用されます。 <br/><br/>**注意：** 移動操作をおこなった後、参照を使用して VEC のビューに移動し、移動が有効な操作かどうかを確認する必要があります。 アクションがビューに適用できない場合、エラーが表示されます。 |
| Delete | アクションを削除します。 |

## SPAでの VEC の使用例

この節では、Visual Experience Composer を使用して A/B アクティビティや XT アクティビティのアクションや変更を作成する 3 つの例の概要を説明します。

### 例 1:「ホーム」ビューを更新

このドキュメントでは、ホームサイト全体に対して「home」という名前のビューが定義されていました。 マーケティングチームは、次の方法で「home」ビューを更新します。

* を **買い物かごに追加** および **次に類似** ボタンを青い部分に軽く ページの読み込み時に発生する問題は、ヘッダーのコンポーネントの変更が伴うからです。
* を **2019 年の最新製品** ラベル **2019 年の人気製品** テキストの色を紫に変更します。

VEC でこれらの更新をおこなうには、 **作成** 変更を「ホーム」ビューに適用します。

![](assets/vec-home.png)

### 例 2:製品ラベルの変更

「products-page-2」表示の場合、マーケティングチームは **価格** ラベル **販売価格** ラベルの色を赤に変更します。

VEC でこれらの更新をおこなうには、次の手順が必要です。

1. 選択 **参照** （VEC 内）
2. 選択 **製品** をクリックします。
3. 選択 **さらに読み込み** 1 回実行すると、2 列目の製品が表示されます。
4. 選択 **作成** （VEC 内）
5. アクションを適用してテキストラベルを変更 **販売価格** そして色は赤に変わった

![](assets/vec-products-page-2.png)

### 例 3:配信設定のスタイル設定をパーソナライズ

ビューは、状態やラジオボタンのオプションなど、詳細なレベルで定義できます。 このドキュメントでは、配信設定の「チェックアウト — 通常」と「チェックアウト — 速達配送」に対してビューが定義されていました。 マーケティングチームが「チェックアウト時に速達配送を選択」ビューのボタンの色を赤に変更したいと考えています。

VEC でこれらの更新をおこなうには、次の手順が必要です。

1. 選択 **参照** （VEC 内）
2. サイトの買い物かごに製品を追加します。
3. サイトの右上隅にある買い物かごアイコンを選択します。
4. 選択 **注文のチェックアウト**.
5. を選択します。 **速達配送** 下のラジオボタン **配信環境設定**.
6. 選択 **作成** （VEC 内）
7. を **支払い** ボタンの色を赤に変更

>[!NOTE]
>
>「checkout-express」ビューは、 **速達配送** ラジオボタンが選択されている。 これは、 `sendEvent()` 関数は **速達配送** ラジオボタンが選択されているので、VEC はラジオボタンが選択されるまで「checkout-express」表示を認識しません。

![](assets/vec-delivery-preference.png)
