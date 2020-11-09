---
title: 'Adobe TargetとAdobe Experience PlatformWeb SDK '
seo-title: Adobe Experience PlatformウェブSDKとAdobe Targetの使用
description: Adobe Targetを使用してExperience PlatformWeb SDKを使用し、パーソナライズされたコンテンツをレンダリングする方法を学びます
seo-description: Adobe Targetを使用してExperience PlatformWeb SDKを使用し、パーソナライズされたコンテンツをレンダリングする方法を学びます
keywords: target;adobe target;xdm views; views;single page applications;SPA;SPA lifecycle;client-side;AB testing;AB;Experience targeting;XT;VEC
translation-type: tm+mt
source-git-commit: 8aeeef09602386f219fd8284b332469c04e88ffb
workflow-type: tm+mt
source-wordcount: '1671'
ht-degree: 14%

---


# シングルページアプリケーションの実装

Adobe Experience PlatformWeb SDKは、シングルページアプリケーション(SPA)などの次世代のクライアント側テクノロジー上でパーソナライゼーションを実行できる豊富な機能を備えています。

従来の Web サイトは、「ページ間」ナビゲーションモデル（別名マルチページアプリケーション）を使用していました。このモデルでは、Web サイトデザインは URL と密接に結合され、ある Web ページから別のページへのトランジションにはページ読み込みが必要になります。

代わりに、シングルページアプリケーションなどの最新のWebアプリケーションは、多くの場合ページのリロードとは無関係に、ブラウザーUIのレンダリングを迅速に使用するモデルを採用しています。 これらのエクスペリエンスは、スクロール、クリック、カーソルの移動など、顧客の操作によってトリガーされます。 最新のWebのパラダイムが進化してきたので、従来の汎用イベント（ページ読み込みなど）との関連性が、パーソナライゼーションや実験を展開するようにはなりません。

![](assets/spa-vs-traditional-lifecycle.png)

## SPA向けプラットフォームWeb SDKの利点

シングルページアプリ用にAdobe Experience PlatformWeb SDKを使用する利点を次に示します。

* ページ読み込み時にすべてのオファーをキャッシュし、複数のサーバー呼び出しを単一のサーバー呼び出しに減らす機能。
* オファーは、従来のサーバー呼び出しによって導入された遅延時間なく、キャッシュを介して即座に表示されるので、サイトでのユーザーエクスペリエンスが大幅に向上します。
* 1行のコードと1回限りの開発者のセットアップにより、マーケターはSPA上のVisual Experience Composer(VEC)を使用して、A/Bおよびエクスペリエンスターゲット設定(XT)アクティビティを作成し、実行できます。

## XDM表示とシングルページアプリ

SPA の Adobe Target VEC は、ビューと呼ばれる新しい概念を活用します。ビューとはビジュアル要素の論理的集合体で、全体として SPA のエクスペリエンスを形作ります。したがって、単一のページアプリは、ユーザーの操作に基づいて、URLではなく表示を介した移行と見なすことができます。 通常、ビューはサイト全体またはサイト内のグループ化されたビジュアル要素を表せます。

表示の詳細を説明するために、次の例では、「React」に実装された仮定のオンラインeコマースサイトを使用して、例の表示を調べます。

ホームサイトに移動した後、ヒーロー画像は、イースターセールと、そのサイトで利用可能な最新の商品を宣伝します。 この場合、ホーム画面全体に対して表示を定義できます。 この表示は、単に「ホーム」と呼ぶことができます。

![](assets/example-views.png)

As the customer becomes more interested in the products that the business is selling, they decide to click the **Products** link. ホームサイトと同様、製品サイト全体をビューとして定義できます。この表示は、「products-all」という名前にできます。

![](assets/example-products-all.png)

表示は、サイト全体またはサイト上の視覚要素のグループとして定義できるので、製品サイトに表示される4つの製品をグループ化し、表示と見なすことができます。 この表示は「products」という名前にできます。

![](assets/example-products.png)

顧客が「 **ロードする製品を増やす** 」ボタンをクリックしてサイト上の他の製品を調査する場合、WebサイトのURLは変わりませんが、表示される2行目の製品のみを表す表示をここに作成できます。 表示名は「products-page-2」にできます。

![](assets/example-load-more.png)

顧客はサイトからいくつかの製品を購入することを決定し、チェックアウト画面に進みます。 チェックアウトサイトでは、「通常」配信または「高速」配信を選択できます。 表示は、サイト上の任意のグループの視覚要素にすることができるので、配信の環境設定用に表示を作成し、「配信の環境設定」と呼ぶことができます。

![](assets/example-check-out.png)

表示の概念はこれよりもずっと遠くに広げられる。 これらは、サイトで定義できる表示の例に過ぎません。

## XDM表示の実装

XDM表示は、Adobe Targetで活用して、マーケターがVisual Experience Composerを使用してSPA上でA/BテストとXTテストを実行できるようにします。 1回限りの開発者の設定を完了するには、次の手順を実行する必要があります。

1. [Adobe Experience PlatformWeb SDKのインストール](../../fundamentals/installing-the-sdk.md)
2. 個人用に設定するシングルページアプリ内のすべてのXDM表示を特定します。
3. XDM表示を定義した後、ABまたはXT VECアクティビティを配信するために、に `sendEvent()` 設定した関数と、対応するXDM表示をシングルページアプリで実装 `renderDecisions``true` します。 XDM表示を渡す必要があり `xdm.web.webPageDetails.viewName`ます。 この手順に従うと、マーケターはVisual Experience Composerを利用して、これらのXDMに対してA/BテストとXTテストを開始できます。

   ```javascript
   alloy("sendEvent",  { 
     "renderDecisions": true, 
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
>最初の `sendEvent()` 呼び出し時に、エンドユーザーにレンダリングする必要のあるすべてのXDM表示が取得され、キャッシュされます。 XDM表示が渡された以降の `sendEvent()` 呼び出しは、キャッシュから読み取られ、サーバーコールなしでレンダリングされます。

## `sendEvent()` 関数の例

この節では、仮定的なeコマースSPA用にReactで `sendEvent()` 関数を呼び出す方法を示す3つの例の概要を説明します。

### 例1:A/Bテストホームページ

マーケティングチームは、ホームページ全体でA/Bテストを実行する必要があります。

![](assets/use-case-1.png)

ホームサイト全体でA/Bテストを実行するに `sendEvent()` は、XDMを次のように `viewName` 設定して呼び出す必要がありま `home`す。

```javascript
function onViewChange() { 
  
  var viewName = window.location.hash; // or use window.location.pathName if router works on path and not hash 

  viewName = viewName || 'home'; // view name cannot be empty 

  // Sanitize viewName to get rid of any trailing symbols derived from URL 

  if (viewName.startsWith('#') || viewName.startsWith('/')) { 
    viewName = viewName.substr(1); 
  }
   
  alloy("sendEvent",  { 
    "renderDecisions": true, 
    "xdm": { 
      "web": { 
        "webPageDetails": { 
          "viewName":"home" 
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

### 例2:パーソナライズされた製品

マーケティングチームは、ユーザーが「もっと **ロードする」をクリックした後で価格ラベルの色を赤に変更して、2番目の行の製品をパーソナライズしたいと考えています**。

![](assets/use-case-2.png)

```javascript
function onViewChange(viewName) { 

  alloy("sendEvent",  { 
    "renderDecisions": true, 
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

### 例3:A/Bテスト配信の環境設定

The marketing team want to run an A/B test to see whether changing the color of the button from blue to red when **Express Delivery** is selected can boost conversions (as opposed to keeping the button color blue for both delivery options).

![](assets/use-case-3.png)

選択した配信の好みに応じてサイト上のコンテンツをパーソナライズするために、配信の好みごとに表示を作成することができる。 「 **標準」配信を選択した場合** 、表示に「checkout-normal」という名前を付けることができます。 If **Express Delivery** is selected, the View can be named &quot;checkout-express&quot;.

```javascript
function onViewChange(viewName) { 

  alloy("sendEvent",  { 
    "renderDecisions": true, 
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

## SPA用のVisual Experience Composerの使用

XDM表示の定義が完了し、それらのXDM表示が渡さ`sendEvent()` れた状態で実装されると、VECはこれらの表示を検出し、A/BまたはXTアクティビティのアクションや変更を作成できるようになります。

>[!NOTE]
>
>SPAでVECを使用するには、 [Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-target-vec-helper/) または [Chrome](https://chrome.google.com/webstore/detail/adobe-target-vec-helper/ggjpideecfnbipkacplkhhaflkdjagak) VEC Helper Extensionをインストールしてアクティベートする必要があります。

### 変更パネル

変更パネルは、特定の表示に対して作成されたアクションをキャプチャします。 表示のすべてのアクションは、その表示の下にグループ化されます。

![](assets/modifications-panel.png)

### Actions

アクションをクリックすると、このアクションが適用されるサイトの要素がハイライトされます。Each VEC action created under a View has the following icons: **Information**, **Edit**, **Clone**, **Move**, and **Delete**. これらのアイコンの詳細については、次の表で説明します。

![](assets/action-icons.png)

| アイコン | 説明 |
|---|---|
| 情報 | アクションの詳細を表示します。 |
| Edit | アクションのプロパティを直接編集できます。 |
| 複製 | 変更パネルに存在する 1 つ以上のビューまたは VEC で参照および移動した 1 つ以上のビューにアクションを複製します。アクションは、必ずしも変更パネルに存在する必要はありません。<br/><br/>**注意：** クローン・オペレーションの作成後、VECの表示に[参照]を使用して移動し、クローン・アクションが有効なオペレーションであったかどうかを確認する必要があります。 アクションがビューに適用できない場合、エラーが表示されます。 |
| 移動 | 変更パネルに既に存在するページ読み込みイベントまたはその他のビューにアクションを移動します。<br/><br/>**ページ型イベント:** ページ読み込みイベントに対応するアクションは、Webアプリケーションの最初のページ読み込みに適用されます。 <br/><br/>**注意：**&#x200B;移動操作が行われた後、「参照」を使用してVEC内の表示に移動し、移動が有効な操作であったかどうかを確認する必要があります。 アクションがビューに適用できない場合、エラーが表示されます。 |
| Delete | アクションを削除します。 |

## SPAでのVECの使用例

ここでは、Visual Experience Composerを使用してA/BまたはXTアクティビティのアクションや変更を作成する3つの例について説明します。

### 例1:「ホーム」表示の更新

以前のドキュメントでは、「home」という名前の表示がホームサイト全体に対して定義されていました。 次に、マーケティングチームは「ホーム」表示を次のように更新します。

* 「カート **」** ボタン **と「いいね！」ボタンを** 、青の明るい部分に変更します。 これは、ヘッダーのコンポーネントの変更が含まれるので、ページの読み込み中に発生する必要があります。
* Change the **Latest Products for 2019** label to **Hottest Products for 2019** and change the text color to purple.

To make these updates in the VEC, select **Compose** and apply those changes to the &quot;home&quot; view.

![](assets/vec-home.png)

### 例2:製品ラベルの変更

「products-page-2」表示の場合、マーケティングチームは **Price** ラベルを **Sale Price** （販売価格）に変更し、ラベルの色を赤に変更したいと考えています。

VECでこれらの更新を行うには、次の手順が必要です。

1. VECで「 **参照** 」を選択します。
2. サイトの上部ナビゲーションで **「製品** 」を選択します。
3. Select **Load More** once to view the second row of products.
4. VECで **構成** (Compose)を選択します。
5. Apply actions to change the text label to **Sale Price** and the color to red.

![](assets/vec-products-page-2.png)

### 例3:配信設定のカスタマイズ

表示は、状態やラジオボタンのオプションなど、詳細なレベルで定義できます。 以前は、このドキュメントの表示は、配信プリファレンスの「checkout-normal」と「checkout-express」に対して定義されていました。 マーケティングチームは、「checkout-express」表示のボタンの色を赤に変更したいと考えています。

VECでこれらの更新を行うには、次の手順が必要です。

1. VECで「 **参照** 」を選択します。
2. 製追加品を買い物かごに追加する必要があります。
3. サイトの右上隅にある買い物かごアイコンを選択します。
4. 「注文を **チェックアウト**」を選択します。
5. 「 **配信プリファレンス」(** Preferences **)の「高速配信**」(Express Client)ラジオボタンを選択します。
6. VECで **構成** (Compose)を選択します。
7. [ **支払** ]ボタンの色を赤に変更します。

>[!NOTE]
>
>「checkout-express」表示は、「 **Express配信** 」ラジオボタンが選択されるまで、変更パネルに表示されません。 これは、この`sendEvent()` 機能が実行されるのは、「 **高速配信** 」ラジオボタンが選択されたときです。したがって、VECは、ラジオボタンが選択されるまで「チェックアウト高速」表示を認識しないからです。

![](assets/vec-delivery-preference.png)