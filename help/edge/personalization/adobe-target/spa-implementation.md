---
title: Adobe Experience PlatformWeb SDKのシングルページアプリケーション実装
description: Adobe Targetを使用して、Adobe Experience PlatformWeb SDKのシングルページアプリケーション(SPA)実装を作成する方法を学びます。
keywords: ターゲット;adobeターゲット;xdm表示;表示；シングルページアプリ；SPA;SPAライフサイクル；クライアント側；ABテスト；AB；エクスペリエンスのターゲット設定；XT;VEC
translation-type: tm+mt
source-git-commit: 69f2e6069546cd8b913db453dd9e4bc3f99dd3d9
workflow-type: tm+mt
source-wordcount: '1665'
ht-degree: 12%

---


# シングルページアプリケーションの実装

Adobe Experience PlatformWeb SDKは、シングルページアプリケーション(SPA)など、次世代のクライアント側テクノロジーでパーソナライゼーションを実行できる豊富な機能を備えています。

従来の Web サイトは、「ページ間」ナビゲーションモデル（別名マルチページアプリケーション）を使用していました。このモデルでは、Web サイトデザインは URL と密接に結合され、ある Web ページから別のページへのトランジションにはページ読み込みが必要になります。

代わりに、シングルページアプリケーションなどの最新のWebアプリケーションでは、多くの場合ページのリロードとは無関係に、ブラウザーUIのレンダリングを迅速に使用するモデルが採用されています。 これらのエクスペリエンスは、スクロール、クリック、カーソルの移動など、顧客の操作によってトリガーされます。 最新のWebのパラダイムが進化してきたので、従来の汎用イベント（ページ読み込みなど）との関連性が、パーソナライゼーションや実験を展開するようにはなりません。

![](assets/spa-vs-traditional-lifecycle.png)

## SPA向けプラットフォームWeb SDKの利点

シングルページアプリケーションでAdobe Experience PlatformWeb SDKを使用する利点を次に示します。

* ページ読み込み時にすべてのオファーをキャッシュし、複数のサーバー呼び出しを単一のサーバー呼び出しに減らす機能。
* オファーは、従来のサーバー呼び出しによって導入された遅延時間なく、キャッシュを介して即座に表示されるので、サイトでのユーザーエクスペリエンスが大幅に向上します。
* 1行のコードと1回限りの開発者のセットアップにより、マーケターはSPA上のVisual Experience Composer(VEC)を使用して、A/Bおよびエクスペリエンスターゲット設定(XT)アクティビティを作成し、実行できます。

## XDM表示とシングルページアプリケーション

SPA向けAdobe TargetVECは、表示と呼ばれる概念を利用している。SPAエクスペリエンスを構成する視覚的要素の論理的なグループ。 したがって、単一ページのアプリは、ユーザーの操作に基づいて、URLではなく表示を介した移行と見なすことができます。 通常、ビューはサイト全体またはサイト内のグループ化されたビジュアル要素を表せます。

表示の詳細を説明するために、次の例では、「React」に実装された仮定のオンラインeコマースサイトを使用して、例の表示を調べます。

ホームサイトに移動した後、ヒーロー画像は、イースターセールと、そのサイトで利用可能な最新の商品を宣伝します。 この場合、ホーム画面全体に対して表示を定義できます。 この表示は、単に「ホーム」と呼ぶことができます。

![](assets/example-views.png)

顧客は、ビジネスが販売する製品に対する関心が高まるにつれ、**製品**&#x200B;リンクをクリックすることにします。 ホームサイトと同様、製品サイト全体をビューとして定義できます。この表示は、「products-all」という名前にできます。

![](assets/example-products-all.png)

表示は、サイト全体またはサイト上の視覚要素のグループとして定義できるので、製品サイトに表示される4つの製品をグループ化し、表示と見なすことができます。 この表示は「products」という名前にできます。

![](assets/example-products.png)

顧客が「**さらに読み込む**」ボタンをクリックしてサイト上の他の製品を探索するとき、WebサイトのURLは変わりませんが、表示される2行目の製品のみを表す表示をここに作成できます。 表示名は「products-page-2」にできます。

![](assets/example-load-more.png)

顧客はサイトからいくつかの製品を購入することを決定し、チェックアウト画面に進みます。 チェックアウトサイトでは、「通常」配信または「高速」配信を選択できます。 表示は、サイト上の任意のグループの視覚要素にすることができるので、配信の環境設定用に表示を作成し、「配信の環境設定」と呼ぶことができます。

![](assets/example-check-out.png)

表示の概念はこれよりもずっと遠くに広げられる。 これらは、サイトで定義できる表示の例に過ぎません。

## XDM表示の実装

XDM表示は、Adobe Targetで活用して、マーケターがVisual Experience Composerを使用してSPA上でA/BテストとXTテストを実行できるようにします。 1回限りの開発者の設定を完了するには、次の手順を実行する必要があります。

1. [Adobe Experience PlatformWeb SDK](../../fundamentals/installing-the-sdk.md)をインストール
2. 個人用に設定したいシングルページアプリケーション内のすべてのXDM表示を特定します。
3. XDM表示を定義した後、ABまたはXT VECアクティビティを配信するために、`renderDecisions`を`true`に設定し、対応するXDM表示をシングルページアプリで実装します。 `sendEvent()`XDM表示は`xdm.web.webPageDetails.viewName`に渡す必要があります。 この手順により、マーケターはVisual Experience Composerを利用して、これらのXDMに対するA/BテストとXTテストを開始できます。

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
>最初の`sendEvent()`呼び出し時に、エンドユーザに対してレンダリングする必要のあるすべてのXDM表示が取得され、キャッシュされます。 XDM表示が渡された後の`sendEvent()`呼び出しは、キャッシュから読み出され、サーバーコールなしでレンダリングされます。

## `sendEvent()` 関数の例

この節では、Reactで仮定のeコマースSPA用に`sendEvent()`関数を呼び出す方法を示す3つの例を概説します。

### 例1:A/Bテストホームページ

マーケティングチームは、ホームページ全体でA/Bテストを実行する必要があります。

![](assets/use-case-1.png)

ホームサイト全体でA/Bテストを実行するには、`sendEvent()`を呼び出し、XDM `viewName`を`home`に設定する必要があります。

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

### 例2:パーソナライズされた製品

マーケティングチームは、ユーザーが&#x200B;**「さらに**&#x200B;を読み込む」をクリックした後に、価格ラベルの色を赤に変更して、2行目の製品をパーソナライズしたいと考えています。

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

### 例3:A/Bテスト配信の環境設定

**高速配信**&#x200B;が選択されている場合にボタンの色を青から赤に変更すると、(両方の配信オプションのボタンの色を青に保つのではなく)コンバージョンが高くなるかどうかをA/Bテストを実行します。

![](assets/use-case-3.png)

選択した配信の好みに応じてサイト上のコンテンツをパーソナライズするために、配信の好みごとに表示を作成することができる。 **通常の配信**&#x200B;を選択した場合、表示には「checkout-normal」という名前を付けることができます。 「**高速配信**」が選択されている場合は、表示に「checkout-express」という名前を付けることができます。

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

## SPA用のVisual Experience Composerの使用

XDM表示の定義が完了し、それらのXDM表示が渡された`sendEvent()`を実装すると、VECはこれらの表示を検出し、A/BまたはXTアクティビティに対するアクションや変更を作成できるようになります。

>[!NOTE]
>
>SPAでVECを使用するには、[Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-target-vec-helper/)または[Chrome](https://chrome.google.com/webstore/detail/adobe-target-vec-helper/ggjpideecfnbipkacplkhhaflkdjagak) VEC Helper Extensionをインストールしてアクティブ化する必要があります。

### 変更パネル

変更パネルは、特定の表示に対して作成されたアクションをキャプチャします。 表示のすべてのアクションは、その表示の下にグループ化されます。

![](assets/modifications-panel.png)

### アクション

アクションをクリックすると、このアクションが適用されるサイトの要素がハイライトされます。表示の下で作成される各VECアクションには、次のアイコンがあります。**情報**、**編集**、**クローン**、**移動**、**削除**。 これらのアイコンの詳細については、次の表で説明します。

![](assets/action-icons.png)

| アイコン | 説明 |
|---|---|
| 情報 | アクションの詳細を表示します。 |
| Edit | アクションのプロパティを直接編集できます。 |
| 複製 | 変更パネルに存在する 1 つ以上のビューまたは VEC で参照および移動した 1 つ以上のビューにアクションを複製します。アクションは、必ずしも変更パネルに存在する必要はありません。<br/><br/>**注意：クローン操作を行った** 後、VEC内の表示に[参照]を使用して移動し、そのクローン操作が有効な操作であったかどうかを確認する必要があります。アクションがビューに適用できない場合、エラーが表示されます。 |
| 移動 | 変更パネルに既に存在するページ読み込みイベントまたはその他のビューにアクションを移動します。<br/><br/>**ページ読み込みイベント：ページ読み込みイベントに対応する** すべてのアクションは、Webアプリケーションの最初のページ読み込みに適用されます。<br/><br/>**注意：**&#x200B;移動操作が行われた後、「参照」を使用してVEC内の表示に移動し、移動が有効な操作であったかどうかを確認する必要があります。アクションがビューに適用できない場合、エラーが表示されます。 |
| Delete | アクションを削除します。 |

## SPAでのVECの使用例

ここでは、Visual Experience Composerを使用してA/BまたはXTアクティビティのアクションや変更を作成する3つの例について説明します。

### 例1:「ホーム」表示の更新

以前のドキュメントでは、「home」という名前の表示がホームサイト全体に対して定義されていました。 次に、マーケティングチームは「ホーム」表示を次のように更新します。

* **追加を買い物かご**&#x200B;に、**いいね！**&#x200B;ボタンを、より明るい青色の共有に変更します。 これは、ヘッダーのコンポーネントの変更が含まれるので、ページの読み込み中に発生する必要があります。
* **2019年の最新製品**&#x200B;のラベルを&#x200B;**2019年の最新製品**&#x200B;に変更し、テキストの色を紫に変更します。

VECでこれらの更新を行うには、**「構成**」を選択し、変更を「ホーム」表示に適用します。

![](assets/vec-home.png)

### 例2:製品ラベルの変更

「products-page-2」表示の場合、マーケティングチームは、**価格**&#x200B;ラベルを&#x200B;**販売価格**&#x200B;に変更し、ラベルの色を赤に変更することを希望します。

VECでこれらの更新を行うには、次の手順が必要です。

1. VECで「**参照**」を選択します。
2. サイトの上部ナビゲーションで&#x200B;**製品**&#x200B;を選択します。
3. 「**もっと**&#x200B;読み込む」を1回選択して、2行目の製品を表示します。
4. VECで「**構成**」を選択します。
5. テキストラベルを&#x200B;**販売価格**&#x200B;に変更し、色を赤に変更するアクションを適用します。

![](assets/vec-products-page-2.png)

### 例3:配信設定のカスタマイズ

表示は、状態やラジオボタンのオプションなど、詳細なレベルで定義できます。 以前は、このドキュメントの表示は、配信プリファレンスの「checkout-normal」と「checkout-express」に対して定義されていました。 マーケティングチームは、「checkout-express」表示のボタンの色を赤に変更したいと考えています。

VECでこれらの更新を行うには、次の手順が必要です。

1. VECで「**参照**」を選択します。
2. 製追加品を買い物かごに追加する必要があります。
3. サイトの右上隅にある買い物かごアイコンを選択します。
4. 「**注文をチェックアウト**」を選択します。
5. **配信の環境設定**&#x200B;の下にある「配信&#x200B;**高速設定**」ラジオボタンを選択します。
6. VECで「**構成**」を選択します。
7. **ペイ**&#x200B;ボタンの色を赤に変更します。

>[!NOTE]
>
>「checkout-express」表示は、「**高速配信**」ラジオボタンが選択されるまで、変更パネルに表示されません。 これは、`sendEvent()`関数が実行されるのは、**「高速配信」**&#x200B;ラジオボタンが選択されたときです。したがって、VECは、ラジオボタンが選択されるまで「checkout-express」表示を認識しないからです。

![](assets/vec-delivery-preference.png)
