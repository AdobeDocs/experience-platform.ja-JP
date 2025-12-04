---
title: パーソナライズ機能
description: 様々なユーザーに異なるコンテンツをレンダリングし、ユーザー向けにパーソナライズされたエクスペリエンスを作成します。
exl-id: 4bd370c8-8d8d-469e-9666-b5b6d0e3a660
source-git-commit: 54cd49bc1e6f23e3fb6d539274ea16d36ea21386
workflow-type: tm+mt
source-wordcount: '1244'
ht-degree: 0%

---

# `personalization`

`personalization` オブジェクトは、リクエストされるパーソナライゼーションの決定（オファーまたは提案）と、リクエストおよび応答でのそれらの処理方法を設定します。 ユーザーごとに表示されるコンテンツをカスタマイズできる原動力になるので、Adobe TargetまたはAdobe Journey Optimizerの実装で特に価値があります。

```js
alloy("sendEvent", {
  personalization: {
    decisionScopes: ["hero-banner"],
    surfaces: ["web://example.com"],
    schemas: ["https://ns.adobe.com/personalization/dom-action"],
    sendDisplayEvent: true,
    sendDisplayNotifications: true,
    includePendingDisplayNotifications: true,
    defaultPersonalizationEnabled: false
  },
  renderDecisions: true,
  xdm: adobeDataLayer.getState(reference)
});
```

`personalization` オブジェクトには、次のプロパティが含まれています。

## `personalization.decisionScopes`

`decisionScopes` プロパティは、パーソナライゼーションの決定を取得して返すように Web SDKに指示する文字列の配列です。 配列内の各項目は、パーソナライズされたコンテンツが必要な場所、コンテキスト、または論理的な配置を識別します。

このプロパティは、ページの特定の領域やコンポーネント用にパーソナライズされたコンテンツを明示的に取得する場合に役立ちます。 これは、ユーザーのナビゲーションやビューの変更に応じて異なるオファーセットが必要になる可能性のある単一ページアプリケーションで特に役立ちます。 また、このプロパティを使用して、ユーザーに関連する UI 要素のオファーのみを取得することで、パフォーマンスを最適化できます。

```js
personalization: {
  decisionScopes: ["hero-banner", "cart-offer"]
}
```

Adobe Targetでは、各決定範囲は mbox またはアクティビティにマッピングされます。 Adobe Journey Optimizerでは、各決定範囲は、決定ベースの web コンテンツのプレースメントまたはキャンペーンにマッピングされます。 Offer Decisioningでは、決定範囲は、訪問者が受け取る必要があるオファーや提案にマッピングされます。

>[!TIP]
>
>グローバルスコープをリクエスト（またはブロック）する場合は、[`defaultPersonalizationEnabled`](#personalizationdefaultpersonalizationenabled) で指定する代わりに `decisionScopes` を使用します。

## `personalization.surfaces`

`surfaces` プロパティは、パーソナライゼーションがリクエストされるチャネル、デバイスまたはコンテキストを手動で定義する [&#x200B; サーフェス URI](https://experienceleague.adobe.com/en/docs/journey-optimizer/using/channels/code-based-experience/configure-code-based-channel/code-based-surface#surface-uri) 文字列の配列です。 クロスチャネルエコシステム内のドメイン、アプリ、デバイスプラットフォームなど、異なるデジタルエクスペリエンスを区別できます。 デフォルトでは、ライブラリは現在のページからデフォルトサーフェスを推測します。 このプロパティを使用して、現在のページで自動的に推測されるサーフェスを上書きできます。

このプロパティは、クロスチャネルパーソナライゼーションを使用する際に役立ち、異なるチャネル間でパーソナライゼーションの仕組みを区別する必要があります。 これにより、同じAdobe Experience Platform組織の下で異なるサイトに対して異なるオファーを作成できます。

```js
personalization: {
  surfaces: ["web://example.com", "web://support.example.com/contact"]
}
```

このプロパティは、Adobe Journey Optimizer キャンペーンやサーフェス管理で設定されたサーフェスと一致するので、Journey Optimizerで基本的に使用されます。

## `personalization.schemas`

`schemas` プロパティは、Edge Networkからリクエストされるパーソナライゼーションコンテンツのタイプをフィルタリングするスキーマ URI 文字列の配列です。 このプロパティを設定すると、指定したコンテンツタイプ定義に一致するオファーのみを含めるように、Adobeから受け取る応答が制限されます。 省略すると、ライブラリは、一致した範囲またはサーフェスに対して使用可能なすべてのスキーマのオファーをリクエストします。

このプロパティを使用すると、応答サイズを最適化し、web サイトやアプリケーションが処理できるオファータイプのみを受け取るようにします。 また、パーソナライズされたコンテンツを特定の方法（DOM アクションのみ、または JSON オブジェクトのみ）でレンダリングする単一ページアプリケーションで使用する場合にも役立ちます。

```js
personalization: {
  schemas: [
    "https://ns.adobe.com/personalization/dom-action",
    "https://ns.adobe.com/personalization/html-content-item"
  ]
}
```

次のスキーマ URI がサポートされています。

* **`https://ns.adobe.com/personalization/dom-action`**：直接の DOM アクションであるオファーで、通常はAdobe Targetの Visual Experience Composer によって生成されます。 これらは、追加のコードを使用せずにページ上の要素を自動的に操作する命令を含んでいます。 これは、自動レンダリングされる web パーソナライゼーションの標準です。
* **`https://ns.adobe.com/personalization/html-content-item`**：文字列として配信される生のHTMLを含むオファー。 通常、実装では、ページ上の目的の場所にこのコンテンツを挿入し、DOM アクションよりも詳細に制御できるようにします。 バナー、スニペットまたはモーダルコンテンツに一般的に使用されます。
* **`https://ns.adobe.com/personalization/json-content-item`**:JSON オブジェクトとして書式設定されたオファー。 最も一般的に使用されるのは、HTMLや DOM の変更ではなく構造化データを想定している API ベースの実装やフロントエンドです。
* **`https://ns.adobe.com/personalization/redirect-item`**：別の URL にリダイレクトするオファー ランディングページやオンボーディングフローなど、ターゲティングまたは決定ロジックに基づいてユーザーを新しいページに移動するために使用します。
* **`https://ns.adobe.com/personalization/ruleset-item`**: クライアントサイドのルールエンジンを強化するビジネスロジックのブロックを提供します。 論理条件と結果（if/then パーソナライゼーションロジック）を定義する、バージョン管理されたルールセットを含みます。
* **`https://ns.adobe.com/personalization/message/in-app`**:Adobe Journey Optimizerのアプリ内メッセージ専用にフォーマットされたオファー。通常、モーダル、バナー、ポップアップまたはオーバーレイとしてレンダリングされます。
* **`https://ns.adobe.com/personalization/message/content-card`**:Adobe Journey Optimizer コンテンツカード専用に書式設定されたオファー。web アプリやモバイルアプリでの永続的またはインボックススタイルのフィード用に設計されています。
* **`https://ns.adobe.com/personalization/message/native-alert`**:Adobe Journey Optimizerのネイティブなアラート専用にフォーマットされたオファーで、プラットフォームのネイティブな通知ダイアログがトリガーされる。
* **`https://ns.adobe.com/personalization/measurement`**：パーソナライズされたオファーに対するクリック数およびインタラクションを追跡するために使用します。 レンダリングできるコンテンツを含まない。
* **`https://ns.adobe.com/personalization/eventHistoryOperation`**：ローカルストレージで訪問者のイベント履歴を変更するためのスキーマ。 SDK によって内部的に使用され、配信またはブロックされたエクスペリエンスのトラッキングに使用されます。 レンダリングできるコンテンツを含まない。
* **`https://ns.adobe.com/personalization/default-content-item`**：フォールバックまたはデフォルトのコンテンツ。通常、パーソナライズされたオファーが適格でない場合。 これにより、資格のないユーザーも引き続きコンテンツを受け取り、エクスペリエンスの一貫性を維持できます。

## `personalization.sendDisplayEvent`

`sendDisplayEvent` プロパティは、パーソナライズされたコンテンツがページにレンダリングされた直後に、表示通知イベントがEdge Networkに自動的に送信されるかどうかを指定するブール値です。 省略すると、デフォルト値は `true` になります。 パーソナライズされたコンテンツがインプレッショントラッキング用にレンダリングされたことを示したくない場合は、この変数を `false` に設定します。

この変数を `false` に設定する最も一般的な使用例は、実装の他の場所に表示イベントをフラグする別のコマンドを送信する予定がある場合です。 一部の実装では、インプレッションと Analytics の両方のイベントが発生します。このプロパティを使用すると、インプレッション数を増分する `sendEvent` コマンドを完全に制御できます。

```js
personalization: {
  sendDisplayEvent: true
}
```

>[!NOTE]
>
>Web SDKの以前のバージョン（バージョン 2.12.0 以前）では、代わりに `sendDisplayNotifications` を使用しています。

## `personalization.includePendingDisplayNotifications`

`includePendingDisplayNotifications` プロパティは、保留中の表示通知が現在の `sendEvent` 呼び出しにバンドルされるかどうかを制御するブール値です。 保留中のディスプレイ通知は、レンダリング済みで、まだディスプレイのイベントとしてEdge Networkにレポートされていない、パーソナライズされたコンテンツのインプレッションです。 パーソナライズされたコンテンツのレンダリングと `sendEvent` 呼び出しは互いに非同期である可能性があるので、このプロパティは、単一ページアプリケーションを使用する場合に役立ちます。

このプロパティのデフォルト値は `false` です。 保留中の表示通知をバッチおよびフラッシュして、インプレッションが正確に記録されるようにする場合は、このプロパティを `true` に設定します。 従来の Web サイトなどの同期実装では、通常、このプロパティを設定する必要はありません。

```js
personalization: {
  includePendingDisplayNotifications: true
}
```

## `personalization.defaultPersonalizationEnabled`

`defaultPersonalizationEnabled` プロパティは、Web SDKがこの `__view__` コマンドに対してデフォルトのページ全体のパーソナライゼーションスコープ（`sendEvent`）およびサーフェスをリクエストする方法を明示的に制御するブール値です。 デフォルトでは、ページの読み込み後の最初の `sendEvent` コマンドで、Web SDKは、デフォルトのページ全体のパーソナライゼーション範囲と関連するサーフェスに対してオファーをリクエストします。 後続の `sendEvent` コマンドは、デフォルトのパーソナライゼーションをリクエストしません。 このプロパティを使用して、その動作を上書きできます。 これは、ユーザーがサイトに移動したときにデフォルトのパーソナライゼーションを再度リクエストしたい単一ページアプリケーション実装で役立ちます。 オファーの取得を複製せずに表示イベントを _のみ_ 送信する場合にも、このプロパティを使用できます。

```js
personalization: {
  defaultPersonalizationEnabled: false
}
```

このプロパティは、設定に応じて次のロジックを使用します。

* **未設定**：まだリクエストされていない場合に、デフォルトのパーソナライゼーションをリクエストします。 デフォルトのパーソナライゼーションは通常、ページの読み込み後の最初の `sendEvent` にリクエストされ、同じページでの後続の `sendEvent` 呼び出しでは再度リクエストされません。 このプロパティを設定すると、この動作がオーバーライドされます。
* **`true`**：ページ読み込み後にこの `sendEvent` コマンドが最初でない場合でも、ページ範囲とデフォルトサーフェスを明示的にリクエストします。 このプロパティを `true` に設定するのは、単一ページアプリケーションのシナリオなど、デフォルトのパーソナライゼーションリクエストを強制する必要がある場合に最適です。
* **`false`**：ページ読み込み後にこの `sendEvent` コマンドが最初に使用された場合でも、ページ範囲とデフォルトサーフェスのリクエストを明示的に抑制します。 このプロパティを `false` に設定するのが最適なのは、特定の `sendEvent` コマンドで新しいオファーをリクエストせず、代わりに Analytics にデータを送信したり、表示イベントを送信したりする場合です。

## Web SDK タグ拡張機能を使用したPersonalization コンポーネント

このプロパティに相当する Web SDK タグ拡張機能は、&#39;[**[!UICONTROL Personalization]**](/help/tags/extensions/client/web-sdk/actions/send-event.md#personalization-fields)&#39; アクションを構成する場合の [!UICONTROL Send event] セクションです。
