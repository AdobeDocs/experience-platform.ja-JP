---
title: Adobe Experience Platform Web SDK 用のシングルページアプリケーション実装
description: Adobe Targetを使用してAdobe Experience Platform Web SDK のシングルページアプリケーション (SPA) 実装を作成する方法について説明します。
keywords: target;adobe target;xdm ビュー；表示；単一ページアプリケーション；SPA;SPAライフサイクル；クライアント側；AB テスト；AB；エクスペリエンスターゲット設定；XT;VEC
exl-id: cc48c375-36b9-433e-b45f-60e6c6ea4883
source-git-commit: 0085306a2f5172eb19590cc12bc9645278bd2b42
workflow-type: tm+mt
source-wordcount: '1665'
ht-degree: 12%

---

# シングルページアプリケーションの実装

Adobe Experience Platform Web SDK は、シングルページアプリケーション (SPA) など、次世代のクライアント側テクノロジーでパーソナライゼーションを実行するための機能を提供します。

従来の Web サイトは、「ページ間」ナビゲーションモデル（別名マルチページアプリケーション）を使用していました。このモデルでは、Web サイトデザインは URL と密接に結合され、ある Web ページから別のページへのトランジションにはページ読み込みが必要になります。

代わりに、シングルページアプリケーションなどの最新の Web アプリケーションでは、ブラウザー UI のレンダリングを迅速に使用するモデルを採用しています。これは多くの場合、ページのリロードとは無関係です。 これらのエクスペリエンスは、スクロール、クリック、カーソル移動などの顧客のインタラクションによってトリガーできます。 最新の Web の枠組みが進化するにつれ、パーソナライゼーションと実験をデプロイするための、ページ読み込みなどの従来の汎用イベントの関連性は機能しなくなりました。

![](assets/spa-vs-traditional-lifecycle.png)

## SPA向け Platform Web SDK のメリット

シングルページアプリケーションでAdobe Experience Platform Web SDK を使用するメリットを紹介します。

* ページ読み込み時にすべてのオファーをキャッシュし、複数のサーバー呼び出しを単一のサーバー呼び出しに減らす機能。
* 従来のサーバー呼び出しで発生する遅延時間なしで、キャッシュ経由でオファーが即座に表示されるので、サイトでのユーザーエクスペリエンスが著しく向上します。
* 1 行のコードと開発者による 1 回限りのセットアップで、マーケターはSPA上で Visual Experience Composer(VEC) を使用して A/B およびエクスペリエンスのターゲット設定 (XT) アクティビティを作成し、実行できます。

## XDM ビューとシングルページアプリケーション

Adobe Target VEC for SPAは、ビューと呼ばれる概念を活用します。ビジュアル要素の論理的なグループで、全体でSPAエクスペリエンスを構成します。 したがって、単一ページアプリケーションは、URL ではなく、ユーザーのインタラクションに基づいてビュー間を移行すると考えることができます。 通常、ビューはサイト全体またはサイト内のグループ化されたビジュアル要素を表せます。

ビューの内容をさらに詳しく説明するために、次の例では、React で実装された架空のオンライン e コマースサイトを使用して、サンプルビューを調べます。

ホームサイトに移動した後、ヒーロー画像はイースターセールと、サイトで利用可能な最新の製品をプロモーションします。 この場合、ビューはホーム画面全体に対して定義できます。 このビューは、単に「home」と呼ぶことができます。

![](assets/example-views.png)

お客様が販売している製品に興味を持つようになると、**Products** リンクをクリックすることになります。 ホームサイトと同様、製品サイト全体をビューとして定義できます。このビューの名前は「products-all」にできます。

![](assets/example-products-all.png)

ビューはサイト全体またはサイト上の視覚要素のグループとして定義できるので、製品サイトに表示される 4 つの製品をグループ化して、ビューと見なすことができます。 このビューには、「products」という名前を付けることができます。

![](assets/example-products.png)

顧客が「 **さらに読み込む** 」ボタンをクリックしてサイト上の他の製品を調査すると、Web サイトの URL は変わりませんが、表示される 2 行目の製品のみを表すビューをここで作成できます。 ビュー名は「products-page-2」になります。

![](assets/example-load-more.png)

顧客がサイトからいくつかの製品を購入することを決定し、チェックアウト画面に進みます。 チェックアウトサイトでは、通常の配送または速達配送を選択できます。 ビューは、サイト上のビジュアル要素のグループにすることができるので、配信設定用にビューを作成して、「配信設定」と呼ぶことができます。

![](assets/example-check-out.png)

ビューの概念は、これよりもはるかに長く拡張できます。 これらは、サイトで定義できるビューの例の一例です。

## XDM ビューの実装

XDM ビューは、Adobe Targetで活用して、Visual Experience Composer を介してSPAで A/B テストや XT テストを実行できるようにします。 これには、開発者による 1 回限りの設定を完了するために、次の手順を実行する必要があります。

1. [Adobe Experience Platform Web SDK](../../fundamentals/installing-the-sdk.md) をインストールします
2. パーソナライズするシングルページアプリケーション内のすべての XDM ビューを決定します。
3. XDM ビューを定義した後、AB または XT VEC アクティビティを配信するために、`renderDecisions` を `true` に設定し、対応する XDM ビューをシングルページアプリケーションに実装します。 `sendEvent()`XDM ビューは `xdm.web.webPageDetails.viewName` で渡す必要があります。 この手順を使用すると、マーケターは Visual Experience Composer を活用して、これらの XDM の A/B テストと XT テストを開始できます。

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
>最初の `sendEvent()` 呼び出し時に、エンドユーザーにレンダリングする必要のあるすべての XDM ビューが取得され、キャッシュされます。 XDM ビューを渡した後続の `sendEvent()` 呼び出しは、キャッシュから読み取られ、サーバー呼び出しなしでレンダリングされます。

## `sendEvent()` 関数の例

この節では、仮想的な e コマースSPA用に React で `sendEvent()` 関数を呼び出す方法を示す 3 つの例を説明します。

### 例 1:A/B テストのホームページ

マーケティングチームは、ホームページ全体で A/B テストを実行したいと考えています。

![](assets/use-case-1.png)

ホームサイト全体で A/B テストを実行するには、 `sendEvent()` を呼び出し、XDM `viewName` を `home` に設定する必要があります。

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

### 例 2:パーソナライズした製品

マーケティングチームは、ユーザーが「**Load More**」をクリックした後、価格ラベルの色を赤に変更して、2 行目の製品をパーソナライズしたいと考えています。

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

### 例 3:A/B テストの配信設定

マーケティングチームは A/B テストを実行し、**速達配送** を選択したときにボタンの色を青から赤に変更すると、（両方の配信オプションのボタンの色を青に保つのではなく）コンバージョンが促進されるかどうかを確認します。

![](assets/use-case-3.png)

選択した配信設定に応じてサイトのコンテンツをパーソナライズするには、配信設定ごとにビューを作成します。 **通常の配信** を選択した場合、ビューに「checkout-normal」という名前を付けることができます。 「**速達配送**」を選択した場合、ビューに「checkout-express」という名前を付けることができます。

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

## SPA用 Visual Experience Composer の使用

XDM ビューの定義を完了し、渡された XDM ビューで `sendEvent()` を実装すると、VEC でこれらのビューを検出でき、ユーザーが A/B または XT アクティビティのアクションや変更を作成できるようになります。

>[!NOTE]
>
>SPAに VEC を使用するには、[Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-target-vec-helper/) または [Chrome](https://chrome.google.com/webstore/detail/adobe-target-vec-helper/ggjpideecfnbipkacplkhhaflkdjagak) VEC ヘルパー拡張機能をインストールしてアクティブ化する必要があります。

### 変更パネル

変更パネルは、特定のビュー用に作成されたアクションを取り込みます。 ビューのすべてのアクションは、そのビューの下にグループ化されます。

![](assets/modifications-panel.png)

### アクション

アクションをクリックすると、このアクションが適用されるサイトの要素がハイライトされます。「表示」の下に作成される各 VEC アクションには、次のアイコンがあります。**情報**、**編集**、**クローン**、**移動**、**削除**。 これらのアイコンについては、次の表で詳しく説明します。

![](assets/action-icons.png)

| アイコン | 説明 |
|---|---|
| 情報 | アクションの詳細を表示します。 |
| Edit | アクションのプロパティを直接編集できます。 |
| 複製 | 変更パネルに存在する 1 つ以上のビューまたは VEC で参照および移動した 1 つ以上のビューにアクションを複製します。アクションは、必ずしも変更パネルに存在する必要はありません。<br/><br/>**注意：** 複製操作をおこなった後、参照を使用して VEC のビューに移動し、複製されたアクションが有効な操作であったかどうかを確認する必要があります。アクションがビューに適用できない場合、エラーが表示されます。 |
| 移動 | 変更パネルに既に存在するページの読み込みイベントまたはその他のビューにアクションを移動します。<br/><br/>**ページ読み込みイベント：** ページ読み込みイベントに対応するアクションは、Web アプリケーションの最初のページ読み込みに適用されます。<br/><br/>**注意：移動操作をおこなった後**、参照を使用して VEC のビューに移動し、移動が有効な操作かどうかを確認する必要があります。アクションがビューに適用できない場合、エラーが表示されます。 |
| Delete | アクションを削除します。 |

## SPAの例での VEC の使用

この節では、Visual Experience Composer を使用して A/B アクティビティや XT アクティビティのアクションや変更を作成する 3 つの例を説明します。

### 例 1:「ホーム」ビューを更新

このドキュメントでは、以前、「home」という名前のビューがホームサイト全体に対して定義されていました。 マーケティングチームは、次の方法で「home」ビューを更新します。

* 「**買い物かごに追加**」ボタンと「**いいね！**」ボタンを明るい青色の部分に変更します。 これは、ページの読み込み中に発生する可能性があります。ヘッダーのコンポーネントの変更が伴うからです。
* **2019 年の最新の製品** ラベルを **2019 年の最もホットな製品** に変更し、テキストの色を紫に変更します。

VEC でこれらの更新をおこなうには、**構成** を選択し、変更を「ホーム」ビューに適用します。

![](assets/vec-home.png)

### 例 2:製品ラベルの変更

「products-page-2」表示の場合、マーケティングチームは、**価格** ラベルを **販売価格** に変更し、ラベルの色を赤に変更したいと考えています。

VEC でこれらの更新をおこなうには、次の手順が必要です。

1. VEC で「**参照**」を選択します。
2. サイトの上部ナビゲーションで **製品** を選択します。
3. **「Load More**」を 1 回選択して、2 行目の製品を表示します。
4. VEC で **構成** を選択します。
5. アクションを適用して、テキストラベルを **販売価格** に変更し、色を赤に変更します。

![](assets/vec-products-page-2.png)

### 例 3:配信設定のスタイル設定のパーソナライズ

ビューは、状態やラジオボタンのオプションなど、詳細なレベルで定義できます。 このドキュメントでは、「チェックアウト — 通常」と「チェックアウト — 速達」の配信設定に関するビューを定義しました。 マーケティングチームは、「チェックアウト時に速達配送を選択」ビューのボタンの色を赤に変更したいと考えています。

VEC でこれらの更新をおこなうには、次の手順が必要です。

1. VEC で「**参照**」を選択します。
2. サイトの買い物かごに商品を追加します。
3. サイトの右上隅にある買い物かごアイコンを選択します。
4. 「**注文をチェックアウト**」を選択します。
5. **「配信設定**」の下の「**高速配信**」ラジオボタンを選択します。
6. VEC で **構成** を選択します。
7. **支払** ボタンの色を赤に変更します。

>[!NOTE]
>
>「checkout-express」ビューは、「**速達配送**」ラジオボタンが選択されるまで、変更パネルに表示されません。 これは、`sendEvent()` 関数が **「速達配送**」ラジオボタンが選択されたときに実行されるので、VEC はラジオボタンが選択されるまで「チェックアウト時速達配送」の表示を認識しないからです。

![](assets/vec-delivery-preference.png)
