---
title: Web SDK での表示イベントの管理
description: この記事では、表示イベントの概要と、Web SDK での使用方法について説明します。
exl-id: 7150ad6e-7693-4f4d-917e-8d08a39a0b41
source-git-commit: 4c7313afdce6645ab638b2998573e5a4f7c5de8f
workflow-type: tm+mt
source-wordcount: '351'
ht-degree: 0%

---

# Web SDK での表示イベントの管理

表示イベントは、特定のパーソナライゼーションコンテンツがページに表示されたときに、パーソナライゼーションサービスまたは分析サービスに通知するために Web SDK で使用されます。

表示イベントを送信すると、パーソナライゼーション指標の精度が向上し、ページ上でユーザーに表示される内容の正確な概要を把握できます。

Web SDK を使用すると、次の 2 つの方法で表示イベントを送信できます。

* パーソナライズされたコンテンツがページにレンダリングされた直後、[&#x200B; 自動的に &#x200B;](#send-automatically)。 詳しくは、パーソナライズされたコンテンツの [&#x200B; レンダリング &#x200B;](rendering-personalization-content.md) 方法に関するドキュメントを参照してください。
* [&#x200B; 手動 &#x200B;](#send-sendEvent-calls)、後続の `sendEvent` 呼び出しを使用して実行します。

>[!NOTE]
>
>`applyPropositions` 関数を呼び出しても、表示イベントは自動的には送信されません。

## 表示イベントの自動送信 {#send-automatically}

表示イベントを送信すると、パーソナライゼーションが読み込まれた直後にイベントが送信されるので、より正確な分析指標が自動的に提供されます。 また、この実装により、より効率的な実装方法が提供されます。

パーソナライズされたコンテンツがページにレンダリングされた後に表示イベントを自動的に送信するには、次のパラメーターを設定する必要があります。

* `renderDecisions: true`
* `personalization.sendDisplayEvent: true` または指定されていません

Web SDK は、`sendEvent` 呼び出しの結果パーソナライゼーションがレンダリングされた直後に表示イベントを送信します。

## 後続の sendEvent 呼び出しで表示イベントを送信 {#send-sendEvent-calls}

表示イベントを [&#x200B; 自動的に &#x200B;](#send-automatically) 送信する場合と比較して、後続の `sendEvent` 呼び出しに含める場合は、呼び出しにページの読み込みに関する詳細な情報を含めることもできます。 これは、パーソナライズされたコンテンツのリクエスト時には利用できなかった追加情報の場合があります。

さらに、`sendEvent` 呼び出しで表示イベントを送信することで、Adobe Analyticsを使用する際のバウンス率エラーを最小限に抑えることができます。

>[!IMPORTANT]
>
>手動でレンダリングされた提案を使用する場合、表示イベントは `sendEvent` 呼び出しを介してのみサポートされます。 この場合、表示イベントを自動的に送信することはできません。

### 自動的にレンダリングされた提案の表示イベントを送信 {#auto-rendered-propositions}

自動的にレンダリングされた提案の表示イベントを送信するには、`sendEvent` 呼び出しで次のパラメーターを設定する必要があります。

* `renderDecisions: true`
* ページヒット数の上位に対する `personalization.sendDisplayEvent: false`

表示イベントを送信するには、`personalization.includeRenderedPropositions: true` で `sendEvent` を呼び出します

### 手動でレンダリングされた提案の表示イベントを送信 {#manually-rendered-propositions}

手動でレンダリングした提案の表示イベントを送信するには、提案の `id`、`scope`、`scopeDetails` のフィールドを含め、`_experience.decisioning.propositions` XDM フィールドにそれらを含める必要があります。

さらに、「`include _experience.decisioning.propositionEventType.display`」フィールドを `1` に設定します。
