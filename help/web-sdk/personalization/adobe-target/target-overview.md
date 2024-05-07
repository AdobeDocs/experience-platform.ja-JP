---
title: パーソナライゼーションのための Web SDK でのAdobe Targetの使用
description: Adobe Targetを使用して、Experience PlatformWeb SDK でパーソナライズされたコンテンツをレンダリングする方法を説明します
exl-id: 021171ab-0490-4b27-b350-c37d2a569245
source-git-commit: a34204eb58ed935831d26caf062ebb486039669f
workflow-type: tm+mt
source-wordcount: '1354'
ht-degree: 4%

---

# 使用方法 [!DNL Adobe Target] および [!DNL Web SDK] パーソナライゼーションの場合

[!DNL Adobe Experience Platform] [!DNL Web SDK] では、で管理されたパーソナライズされたエクスペリエンスを配信およびレンダリングできます。 [!DNL Adobe Target] を web チャネルに追加します。 という WYSIWYG エディターを使用できます [Visual Experience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html) （VEC）、または非ビジュアルインターフェイス、 [フォームベースの Experience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html)を作成し、アクティブ化して、アクティビティとパーソナライゼーションエクスペリエンスを配信します。

>[!IMPORTANT]
>
>を使用して Target 実装を Platform Web SDK に移行する方法を説明します [Target を at.js 2.x から Platform Web SDK に移行する](https://experienceleague.adobe.com/docs/platform-learn/migrate-target-to-websdk/introduction.html?lang=ja) チュートリアル。
>
>を使用した Target の初めての実装方法を説明します [Web SDK を使用したAdobe Experience Cloudの実装](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/overview.html?lang=ja) チュートリアル。 Target について詳しくは、チュートリアルの節を参照してください。 [Platform Web SDK を使用した Target の設定](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/applications-setup/setup-target.html).


次の機能はテスト済みで、現在でサポートされています [!DNL Target]:

* [A/B テスト](https://experienceleague.adobe.com/docs/target/using/activities/abtest/test-ab.html)
* [A4T インプレッションおよびコンバージョンレポート](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t.html?lang=ja)
* [Automated Personalization アクティビティ](https://experienceleague.adobe.com/docs/target/using/activities/automated-personalization/automated-personalization.html)
* [エクスペリエンスのターゲット設定アクティビティ](https://experienceleague.adobe.com/docs/target/using/activities/automated-personalization/automated-personalization.html)
* [多変量分析テスト（MVT）](https://experienceleague.adobe.com/docs/target/using/activities/multivariate-test/multivariate-testing.html)
* [Recommendations アクティビティ](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations.html?lang=ja)
* [Target のネイティブインプレッションとコンバージョンレポート](https://experienceleague.adobe.com/docs/target/using/reports/reports.html)
* [VEC サポート](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html)

## [!DNL Web SDK] システム図

次の図は、のワークフローを理解するのに役立ちます [!DNL Target] および [!DNL Web SDK] edge decisioning.

![Platform Web SDK を使用したAdobe Target Edge Decisioning の図](assets/target-platform-web-sdk-new.png)

| 呼び出し | 詳細 |
| --- | --- |
| 1 | デバイスは、 [!DNL Web SDK]. この [!DNL Web SDK] xdm データ、データストリーム環境 ID、渡されたパラメーター、Edge NetworkID を含むリクエストを顧客に送信します（オプション）。 ページ（またはコンテナ）は事前非表示になっています。 |
| 2 | Edge Networkはリクエストをエッジサービスに送信し、訪問者 ID、同意、その他の訪問者コンテキスト情報（位置情報やデバイスにわかりやすい名前など）を使用してリクエストを強化します。 |
| 3 | Edge Networkがエンリッチメントされたパーソナライゼーションリクエストをに送信します [!DNL Target] 訪問者 ID および渡し込みパラメーターを持つエッジ。 |
| 4 | プロファイルスクリプトは、を実行してにフィードします [!DNL Target] プロファイルストレージ。 プロファイルストレージはからセグメントを取得します [!UICONTROL オーディエンスライブラリ] （例えば、から共有されたセグメント [!DNL Adobe Analytics], [!DNL Adobe Audience Manager], [!DNL Adobe Experience Platform]）に設定します。 |
| 5 | URL リクエストパラメーターとプロファイルデータに基づいて、 [!DNL Target] 現在のページビューと今後の事前読み込みビューの訪問者に表示するアクティビティとエクスペリエンスを決定します。 [!DNL Target] 次に、これをEdge Networkに返します。 |
| 6 | a. Edge Networkがパーソナライゼーションの応答をページに送り返します。オプションで、パーソナライゼーションをさらに強化するためのプロファイル値も返されます。 現在のページ上のパーソナライズされたコンテンツは、デフォルトコンテンツのちらつきなしでできるだけ早く表示されます。<br>b. シングルページアプリケーション（SPA）でユーザーアクションの結果として表示されるビューのパーソナライズされたコンテンツは、キャッシュされるので、ビューがトリガーされたときに追加のサーバー呼び出しをおこなわずに即座にコンテンツを適用できます。 <br>c. Edge Networkは、訪問者 ID と Cookie のその他の値（同意、セッション ID、ID、Cookie チェック、パーソナライゼーションなど）を送信します。 |
| 7 | Web SDK が、通知をデバイスからEdge Networkに送信します。 |
| 8 | Edge Networkは次へ進む [!UICONTROL Analytics for Target] （A4T）の詳細（アクティビティ、エクスペリエンス、コンバージョンメタデータ）をに変換します [!DNL Analytics] エッジ。 |

## 有効化 [!DNL Adobe Target]

を有効にする [!DNL Target]、次の手順を実行します。

1. Enable （有効） [!DNL Target] が含まれる [データストリーム](../../../datastreams/overview.md) 適切なクライアントコードを指定します。
1. を追加 `renderDecisions` イベントへのオプション。

その後、オプションで、次のオプションも追加できます。

* **`decisionScopes`**：イベントにこのオプションを追加して、特定のアクティビティ（フォームベースのコンポーザーで作成されたアクティビティで役立つ）を取得します。
* **[事前非表示のスニペット](../manage-flicker.md)**：ページの特定の部分のみを非表示にします。

## Adobe Target VEC の使用

で VEC を使用するには [!DNL Web SDK] の実装、インストールおよびアクティベーション [Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-target-vec-helper/) または [Chrome](https://chrome.google.com/webstore/detail/adobe-target-vec-helper/ggjpideecfnbipkacplkhhaflkdjagak) VEC Helper 拡張機能。

詳しくは、を参照してください [Visual Experience Composer ヘルパー拡張機能](https://experienceleague.adobe.com/docs/target/using/experiences/vec/troubleshoot-composer/vec-helper-browser-extension.html) が含まれる *Adobe Target ガイド*.

## パーソナライズされたコンテンツのレンダリング

参照： [パーソナライゼーションコンテンツのレンダリング](../rendering-personalization-content.md) を参照してください。

## XDM でのオーディエンス

のオーディエンスを定義する場合 [!DNL Target] を介して配信されるアクティビティ [!DNL Web SDK], [XDM](https://experienceleague.adobe.com/docs/experience-platform/xdm/home.html?lang=ja) 定義し、使用する必要があります。 XDM スキーマ、クラス、スキーマフィールドグループを定義したら、 [!DNL Target] ターゲティング用に XDM データで定義されたオーディエンスルール。 内 [!DNL Target]、XDM データはに表示されます [!UICONTROL Audience Builder] をカスタムパラメーターとして使用します。 XDM は、ドット表記（など）を使用してシリアル化されます。 `web.webPageDetails.name`）に設定します。

以下がある場合： [!DNL Target] カスタムパラメーターまたはユーザープロファイルを使用する定義済みオーディエンスを持つアクティビティの場合、SDK 経由で正しく配信されません。 カスタムパラメーターやユーザープロファイルを使用する代わりに、XDM を使用する必要があります。 ただし、を介してサポートされている標準のオーディエンスターゲティングフィールドは次のとおりです [!DNL Web SDK] XDM は必要ありません。 これらのフィールドは、 [!DNL Target] XDM を必要としない UI:

* ターゲットライブラリ
* ジオ
* ネットワーク
* オペレーティングシステム
* サイトのページ
* ブラウザー
* トラフィックソース
* 時間枠

詳しくは、を参照してください [オーディエンスのカテゴリ](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/target-rules.html) が含まれる *Adobe Target ガイド*.

### レスポンストークン

レスポンストークンは、GoogleやFacebookなどのサードパーティにメタデータを送信するために使用されます。 レスポンストークンは、 `meta` 内のフィールド `propositions` -> `items`. 次に例を示します。

```json
{
  "id": "AT:eyJhY3Rpdml0eUlkIjoiMTI2NzM2IiwiZXhwZXJpZW5jZUlkIjoiMCJ9",
  "scope": "__view__",
  "scopeDetails": ...,
  "renderAttempted": true,
  "items": [
    {
      "id": "0",
      "schema": "https://ns.adobe.com/personalization/dom-action",
      "meta": {
        "experience.id": "0",
        "activity.id": "126736",
        "offer.name": "Default Content",
        "offer.id": "0"
      }
    }
  ]
}
```

応答トークンを収集するには、を購読する必要があります。 `alloy.sendEvent` promise、反復処理 `propositions`詳細を抽出します `items` -> `meta`.

ごと `proposition` 持つ `renderAttempted` 次のかどうかを示すブール値フィールド `proposition` はレンダリングされたかどうか。 以下のサンプルを参照してください。

```js
alloy("sendEvent",
  {
    "renderDecisions": true,
    "decisionScopes": [
      "hero-container"
    ]
  }).then(result => {
    const { propositions } = result;

    // filter rendered propositions
    const renderedPropositions = propositions.filter(proposition => proposition.renderAttempted === true);

    // collect the item metadata that represents the response tokens
    const collectMetaData = (items) => {
      return items.filter(item => item.meta !== undefined).map(item => item.meta);
    }

    const pageLoadResponseTokens = renderedPropositions
      .map(proposition => collectMetaData(proposition.items))
      .filter(e => e.length > 0)
      .flatMap(e => e);
  });
  
```

自動レンダリングが有効な場合、提案の配列には次が含まれます。

#### ページ読み込み時：

* フォームベースの Composer ベース `propositions` （を使用） `renderAttempted` フラグの設定 `false`
* を使用した Visual Experience Composer ベースの提案 `renderAttempted` フラグの設定 `true`
* を使用したシングルページアプリケーションビューの Visual Experience Composer ベースの提案 `renderAttempted` フラグの設定 `true`

#### 表示時 – 変更（キャッシュされたビュー用）:

* を使用したシングルページアプリケーションビューの Visual Experience Composer ベースの提案 `renderAttempted` フラグの設定 `true`

自動レンダリングが無効の場合、提案の配列には次が含まれます。

#### ページ読み込み時：

* [!DNL Form-based Composer]ベース `propositions` （を使用） `renderAttempted` フラグの設定 `false`
* [!DNL Visual Experience Composer]を使用したベースの提案 `renderAttempted` フラグの設定 `false`
* [!DNL Visual Experience Composer]を使用した単一ページアプリケーションビューのベースの提案 `renderAttempted` フラグの設定 `false`

#### 表示時 – 変更（キャッシュされたビュー用）:

* を使用したシングルページアプリケーションビューの Visual Experience Composer ベースの提案 `renderAttempted` フラグの設定 `false`

### 単一プロファイルの更新

この [!DNL Web SDK] プロファイルをに更新できます [!DNL Target] プロファイルおよびを [!DNL Web SDK] エクスペリエンスイベントとして。

を更新するには [!DNL Target] プロファイルで、プロファイルデータが次と共に渡されることを確認します。

* 次の下 `"data {"`
* 次の下 `"__adobe.target"`
* プレフィックス `"profile."`

| キー | タイプ | 説明 |
| --- | --- | --- |
| `renderDecisions` | ブール値 | DOM アクションを解釈するかどうかをパーソナライゼーションコンポーネントに指示します |
| `decisionScopes` | 配列 `<String>` | 決定を取得する範囲のリスト |
| `xdm` | オブジェクト | Web SDK にエクスペリエンスイベントとして格納される、XDM 形式のデータ |
| `data` | オブジェクト | に送信された任意のキーと値のペア [!DNL Target] target クラスの下のソリューション。 |

標準 [!DNL Web SDK] このコマンドを使用するコードは次のようになります。

**コンテンツがエンドユーザーに表示されるまで、プロファイルまたはエンティティパラメーターの保存を遅らせる**

コンテンツが表示されるまでプロファイルの属性の記録を遅延させるには、次のように設定します `data.adobe.target._save=false` ご要望の通りです。

例えば、Web サイトには、Web サイト上の 3 つのカテゴリリンク（男性、女性および子供）に対応する 3 つの決定範囲が含まれており、ユーザーが最終的に訪問したカテゴリを追跡したいとします。 これらのリクエストを送信するには、 `__save` フラグの設定 `false` コンテンツがリクエストされた際にカテゴリが保持されるのを回避する。 コンテンツを視覚化したら、適切なペイロードを送信します（ `eventToken` および `stateToken`）に設定する必要があります。

<!--Save profile or entity attributes by default with:

```js
alloy ( "sendEvent" , {
  renderDecisions : true,
  data : {
    __adobe : {
      target : {
        "__save" : true // Optional. __save=true is the default 
        "profile.gender" : "female",
        "profile.age" : 30,
        "entity.name" : "T-shirt",
        "entity.id" : "1234",
      }
    }
  }
} ) ; 
```
-->

次の例では、trackEvent スタイルのメッセージを送信し、プロファイルスクリプトを実行して、属性を保存し、イベントを直ちに記録します。

```js
alloy ( "sendEvent" , {
  renderDecisions : true,
  data : {
    __adobe : {
      target : {
        "profile.gender" : "female",
        "profile.age" : 30,
        "entity.name" : "T-shirt" ,
        "entity.id" : "1234" ,
        "track": {
          "scopes": [ "mbox1", "mbox2"],
          "type": "display|click|..."
        }
      }
    }
  }
} ) ;
```

>[!NOTE]
>
>次の場合 `__save` ディレクティブは省略され、プロファイルとエンティティの属性を保存すると、リクエストの残りがパーソナライゼーションのプリフェッチであっても、リクエストが実行されたかのように直ちに処理されます。 この `__save` ディレクティブは、プロファイル属性とエンティティ属性にのみ関連しています。 トラック オブジェクトが存在する場合、 `__save` ディレクティブは無視されます。 データは直ちに保存され、通知が記録されます。

**`sendEvent`プロファイルデータを使用**

```js
alloy("sendEvent", {
   renderDecisions: true|false,
   xdm: { // Experience Event XDM data },
   data: { // Freeform data }
});
```

**プロファイル属性をAdobe Targetに送信する方法：**

```js
alloy("sendEvent", {
  "renderDecisions": true,
  "data": {
    "__adobe": {
      "target": {
        "profile.gender": "female",
        "profile.age": 30
      }
    }
  }
});
```

## お勧めをリクエスト

以下の表にリストを示します [!DNL Recommendations] 属性とそれぞれが [!DNL Web SDK]:

| カテゴリ | 属性 | サポートステータス |
| --- | --- | --- |
| Recommendations - デフォルトのエンティティ属性 | entity.id | サポートあり |
|  | entity.name | サポートあり |
|  | entity.categoryId | サポートあり |
|  | entity.pageUrl | サポートあり |
|  | entity.thumbnailUrl | サポートあり |
|  | entity.message | サポートあり |
|  | entity.value | サポートあり |
|  | entity.inventory | サポートあり |
|  | entity.brand | サポートあり |
|  | entity.margin | サポートあり |
|  | entity.event.detailsOnly | サポートあり |
| Recommendations - カスタムエンティティの属性 | entity.yourCustomAttributeName | サポートあり |
| Recommendations – 予約済みの mbox/ページパラメーター | excludedIds | サポートあり |
|  | cartIds | サポートあり |
|  | productPurchasedId | サポートあり |
| カテゴリ親和性のページまたは項目カテゴリ | user.categoryId | サポートあり |

**Recommendations属性をAdobe Targetに送信する方法：**

```js
alloy("sendEvent", {
  "renderDecisions": true,
  "data": {
    "__adobe": {
      "target": {
        "entity.id": "123",
        "entity.genre": "Drama"
      }
    }
  }
});
```

## デバッグ

mboxTrace と mboxDebug は非推奨（廃止予定）になりました。 ～の方法を用いる [Web SDK のデバッグ](/help/web-sdk/use-cases/debugging.md) その代わり。

## 用語

__提案：__ 対象： [!DNL Adobe Target]、提案は、アクティビティから選択されたエクスペリエンスに関連付けられます。

__スキーマ：__ 決定のスキーマは、のオファーのタイプです。 [!DNL Adobe Target].

__範囲：__ 決定の範囲。 対象： [!DNL Adobe Target]範囲は mBox です。 グローバル mBox は `__view__` スコープ。

__XDM:__ XDM は、ドット表記にシリアル化された後、に配置されます [!DNL Adobe Target] mBox パラメーターとして。
