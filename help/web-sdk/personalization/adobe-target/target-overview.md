---
title: Adobe Targetと Web SDK を使用してパーソナライゼーションを実行する
description: Adobe Targetを使用してExperience PlatformWeb SDK でパーソナライズされたコンテンツをレンダリングする方法を説明します
exl-id: 021171ab-0490-4b27-b350-c37d2a569245
source-git-commit: b6e084d2beed58339191b53d0f97b93943154f7c
workflow-type: tm+mt
source-wordcount: '1158'
ht-degree: 5%

---

# 用途 [!DNL Adobe Target] および [!DNL Web SDK] パーソナライゼーションの

[!DNL Adobe Experience Platform] [!DNL Web SDK] では、で管理されるパーソナライズされたエクスペリエンスを配信およびレンダリングできます [!DNL Adobe Target] を web チャネルに追加します。 WYSIWYG エディタを使用できます。このエディタは、 [Visual Experience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html) (VEC)、または非ビジュアルインターフェイス [フォームベースの Experience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html)を使用して、アクティビティとパーソナライゼーションエクスペリエンスを作成、アクティブ化および配信します。

>[!IMPORTANT]
>
>を使用して Target 実装を Platform Web SDK に移行する方法について説明します。 [at.js 2.x から Platform Web SDK への Target の移行](https://experienceleague.adobe.com/docs/platform-learn/migrate-target-to-websdk/introduction.html?lang=ja) チュートリアル
>
>Target を初めて実装する際に、 [Web SDK を使用したAdobe Experience Cloudの実装](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/overview.html?lang=ja) チュートリアル Target に固有の情報については、「 」というタイトルのチュートリアルの節を参照してください。 [Platform Web SDK での Target の設定](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/applications-setup/setup-target.html).


次の機能はテスト済みで、現在はでサポートされています。 [!DNL Target]:

* [A/B テスト](https://experienceleague.adobe.com/docs/target/using/activities/abtest/test-ab.html)
* [A4T インプレッションとコンバージョンのレポート](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t.html?lang=ja)
* [Automated Personalizationアクティビティ](https://experienceleague.adobe.com/docs/target/using/activities/automated-personalization/automated-personalization.html)
* [エクスペリエンスのターゲット設定アクティビティ](https://experienceleague.adobe.com/docs/target/using/activities/automated-personalization/automated-personalization.html)
* [多変量分析テスト (MVT)](https://experienceleague.adobe.com/docs/target/using/activities/multivariate-test/multivariate-testing.html)
* [Recommendationsアクティビティ](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations.html)
* [ネイティブ Target のインプレッションおよびコンバージョンレポート](https://experienceleague.adobe.com/docs/target/using/reports/reports.html)
* [VEC サポート](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html)

## [!DNL Web SDK] システム図

次の図は、 [!DNL Target] および [!DNL Web SDK] エッジ判定。

![Platform Web SDK を使用したAdobe Target Edge Decisioning の図](./assets/target-platform-web-sdk.png)

| 呼び出し | 詳細 |
| --- | --- |
| 1 | デバイスが [!DNL Web SDK]. The [!DNL Web SDK] は、XDM データ、Datastreams 環境 ID、渡されたパラメーターおよび顧客 ID（オプション）を使用して、エッジネットワークにリクエストを送信します。 ページ（またはコンテナ）は事前に非表示になっています。 |
| 2 | エッジネットワークは、エッジサービスにリクエストを送信し、訪問者 ID、同意、その他の訪問者のコンテキスト情報（位置情報やデバイスにわかりやすい名前など）でエンリッチメントします。 |
| 3 | エッジネットワークは、エンリッチメントされたパーソナライゼーションリクエストを [!DNL Target] エッジに貼り付け、訪問者 ID および渡されたパラメーターを含める必要があります。 |
| 4 | プロファイルスクリプトが実行されてから、 [!DNL Target] プロファイルストレージ。 プロファイルストレージは、 [!UICONTROL オーディエンスライブラリ] ( 例えば、 [!DNL Adobe Analytics], [!DNL Adobe Audience Manager]、 [!DNL Adobe Experience Platform]) をクリックします。 |
| 5 | URL リクエストパラメーターとプロファイルデータに基づき、 [!DNL Target] は、現在のページビューと今後のプリフェッチされたビューに対して、訪問者に対して表示するアクティビティとエクスペリエンスを決定します。 [!DNL Target] 次に、これを edge ネットワークに送り返します。 |
| 6 | a. Edge ネットワークは、パーソナライゼーション応答を（オプションで、追加のパーソナライゼーションに関するプロファイル値を含めて）ページに送り返します。 デフォルトコンテンツがちらつくことなく、可能な限り迅速に現在のページ上のパーソナライズされたコンテンツが表示されます。<br>b.シングルページアプリケーション (SPA) のユーザーアクションの結果として表示されるビュー用にパーソナライズされたコンテンツはキャッシュされるので、ビューがトリガーされたときに追加のサーバー呼び出しなしで即座に適用できます。 <br>。Edge ネットワークは、訪問者 ID とその他の値を Cookie で送信します（同意、セッション ID、ID、Cookie チェック、パーソナライゼーションなど）。 |
| 7 | エッジネットワークが転送されます [!UICONTROL Analytics for Target] (A4T) [!DNL Analytics] エッジです。 |

## 有効化 [!DNL Adobe Target]

有効にするには [!DNL Target]、次の操作を実行します。

1. 有効にする [!DNL Target] の [datastream](../../../datastreams/overview.md) を適切なクライアントコードに置き換えます。
1. 次を追加： `renderDecisions` オプションをイベントに追加できます。

その後、オプションで、次のオプションも追加できます。

* **`decisionScopes`**：イベントにこのオプションを追加して、特定のアクティビティ（フォームベースのコンポーザーで作成されたアクティビティで役立つ）を取得します。
* **[スニペットを事前に非表示にする](../manage-flicker.md)**：ページの特定の部分のみを非表示にします。

## Adobe Target VEC の使用

VEC を [!DNL Web SDK] 実装、インストール、アクティブ化 [Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-target-vec-helper/) または [クロム](https://chrome.google.com/webstore/detail/adobe-target-vec-helper/ggjpideecfnbipkacplkhhaflkdjagak) VEC ヘルパー拡張機能。

詳しくは、 [Visual Experience Composer ヘルパー拡張機能](https://experienceleague.adobe.com/docs/target/using/experiences/vec/troubleshoot-composer/vec-helper-browser-extension.html) （内） *Adobe Targetガイド*.

## パーソナライズされたコンテンツのレンダリング

詳しくは、 [パーソナライゼーションコンテンツのレンダリング](../rendering-personalization-content.md) を参照してください。

## XDM のオーディエンス

のオーディエンスを定義する際 [!DNL Target] 経由で配信されるアクティビティ [!DNL Web SDK], [XDM](https://experienceleague.adobe.com/docs/experience-platform/xdm/home.html?lang=ja) を定義して使用する必要があります。 XDM スキーマ、クラス、スキーマフィールドグループを定義したら、 [!DNL Target] ターゲティング用に XDM データで定義されるオーディエンスルール。 Within [!DNL Target]に値を指定しない場合、XDM データは [!UICONTROL Audience Builder] をカスタムパラメーターとして設定します。 XDM は、ドット表記 ( 例えば、 `web.webPageDetails.name`) をクリックします。

次の条件を満たしている場合： [!DNL Target] アクティビティが事前に定義されたオーディエンスでカスタムパラメーターまたはユーザープロファイルを使用する場合、SDK を介して正しく配信されません。 カスタムパラメーターやユーザープロファイルを使用する代わりに、XDM を使用する必要があります。 ただし、 [!DNL Web SDK] XDM を必要としない これらのフィールドは、 [!DNL Target] XDM を必要としない UI:

* ターゲットライブラリ
* ジオ
* ネットワーク
* オペレーティングシステム
* サイトのページ
* ブラウザー
* トラフィックソース
* 時間枠

詳しくは、 [オーディエンスのカテゴリ](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/target-rules.html) （内） *Adobe Targetガイド*.

### レスポンストークン

レスポンストークンは、GoogleやFacebookなどのサードパーティにメタデータを送信するために使用されます。 レスポンストークンは、 `meta` ～の中に入る `propositions` -> `items`. 次に例を示します。

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

レスポンストークンを収集するには、サブスクライブする必要があります。 `alloy.sendEvent` 約束、反復する `propositions`」をクリックし、次の場所から詳細を抽出します。 `items` -> `meta`.

毎 `proposition` には、 `renderAttempted` ブール型フィールドで、 `proposition` がレンダリングされたかどうか。 以下のサンプルを参照してください。

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

自動レンダリングが有効な場合、提案配列には次の情報が含まれます。

#### ページ読み込み時：

* フォームベースのコンポーザーベース `propositions` 次を使用 `renderAttempted` フラグをに設定 `false`
* を使用した Visual Experience Composer ベースの提案 `renderAttempted` フラグをに設定 `true`
* を使用したシングルページアプリケーションビューに対する Visual Experience Composer ベースの提案 `renderAttempted` フラグをに設定 `true`

#### 表示時 — 変更時（キャッシュされた表示の場合）:

* を使用したシングルページアプリケーションビューに対する Visual Experience Composer ベースの提案 `renderAttempted` フラグをに設定 `true`

自動レンダリングが無効になっている場合、提案配列には次の情報が含まれます。

#### ページ読み込み時：

* [!DNL Form-based Composer] — ベース `propositions` 次を使用 `renderAttempted` フラグをに設定 `false`
* [!DNL Visual Experience Composer]に基づく提案 `renderAttempted` フラグをに設定 `false`
* [!DNL Visual Experience Composer]を使用したシングルページアプリビューのベースの提案 `renderAttempted` フラグをに設定 `false`

#### 表示時 — 変更時（キャッシュされた表示の場合）:

* を使用したシングルページアプリケーションビューに対する Visual Experience Composer ベースの提案 `renderAttempted` フラグをに設定 `false`

### 単一プロファイルの更新

The [!DNL Web SDK] を使用すると、プロファイルを [!DNL Target] プロファイルと [!DNL Web SDK] エクスペリエンスイベントとして。

を更新するには [!DNL Target] プロファイルで、プロファイルデータが次の情報と共に渡されていることを確認します。

* の下 `"data {"`
* の下 `"__adobe.target"`
* プレフィックス `"profile."`

| キー | タイプ | 説明 |
| --- | --- | --- |
| `renderDecisions` | ブール値 | パーソナライゼーションコンポーネントに DOM アクションを解釈するかどうかを指定します |
| `decisionScopes` | 配列 `<String>` | 決定を取得するスコープのリスト |
| `xdm` | オブジェクト | エクスペリエンスイベントとして Web SDK に格納される XDM で形式設定されたデータ |
| `data` | オブジェクト | に送信される任意のキーと値のペア [!DNL Target] target クラスのソリューション。 |

標準 [!DNL Web SDK] このコマンドを使用するコードは、次のようになります。

**`sendEvent`プロファイルデータと共に**

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

## 推奨をリクエスト

次の表にリストを示します。 [!DNL Recommendations] 属性と、それぞれが [!DNL Web SDK]:

| カテゴリ | 属性 | サポートステータス |
| --- | --- | --- |
| Recommendations — デフォルトのエンティティ属性 | entity.id | サポートあり |
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
| Recommendations — カスタムエンティティの属性 | entity.yourCustomAttributeName | サポートあり |
| Recommendations — 予約済みの mbox/ページパラメーター | excludedIds | サポートあり |
|  | cartIds | サポートあり |
|  | productPurchasedId | サポートあり |
| カテゴリ親和性のページまたは品目カテゴリ | user.categoryId | サポートあり |

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

mboxTrace と mboxDebug は非推奨（廃止予定）となりました。 次のメソッドを使用： [Web SDK のデバッグ](/help/web-sdk/use-cases/debugging.md) 代わりに、

## 用語

__提案：__ In [!DNL Adobe Target]の提案は、アクティビティから選択されたエクスペリエンスと相関関係を持ちます。

__スキーマ：__ 決定のスキーマは、 [!DNL Adobe Target].

__範囲：__ 決定の範囲。 In [!DNL Adobe Target]の場合、範囲は mBox です。 グローバル mBox は、 `__view__` 範囲。

__XDM:__ XDM はドット表記にシリアル化され、次に [!DNL Adobe Target] を mbox パラメーターとして設定します。
