---
title: Web SDKでの表示イベントの管理
description: 表示イベントの概要と、Web SDKでの使用方法について説明します。
exl-id: 7150ad6e-7693-4f4d-917e-8d08a39a0b41
source-git-commit: db7e6df1b1a0eb19518d9c6ccd6e6bb9131d5a3e
workflow-type: tm+mt
source-wordcount: '314'
ht-degree: 0%

---

# Web SDKでの表示イベントの管理

表示イベントは、特定のパーソナライゼーションコンテンツがページに表示されたときに、パーソナライゼーションサービスまたは分析サービスに通知するために、web SDKで使用されます。 表示イベントを送信すると、パーソナライゼーション指標の精度が向上し、ページ上でユーザーに表示される内容の正確な概要を把握できます。

>[!NOTE]
>
>`applyPropositions` 関数を呼び出しても、表示イベントは自動的には送信されません。

## 表示イベントの自動送信

表示イベントを送信すると、パーソナライゼーションが読み込まれた直後にイベントが送信されるので、より正確な分析指標が自動的に提供されます。 また、この実装により、より効率的な実装方法が提供されます。

パーソナライズされたコンテンツがページにレンダリングされた後に表示イベントを自動的に送信するには、次のパラメーターを設定する必要があります。

* `renderDecisions: true`
* `personalization.sendDisplayEvent: true` または指定されていません

Web SDKは、`sendEvent` 呼び出しの結果としてパーソナライゼーションがレンダリングされた直後に表示イベントを送信します。

## 後続の sendEvent 呼び出しで表示イベントを送信

表示イベントを自動的に送信する場合と比較して、後続の `sendEvent` 呼び出しに表示イベントを含める場合は、呼び出しにページの読み込みに関する詳細な情報を含めることもできます。 これは、パーソナライズされたコンテンツのリクエスト時には利用できなかった追加情報の場合があります。

さらに、`sendEvent` 呼び出しで表示イベントを送信することで、Adobe Analyticsを使用する際のバウンス率エラーを最小限に抑えることができます。

>[!IMPORTANT]
>
>手動でレンダリングされた提案を使用する場合、表示イベントは `sendEvent` 呼び出しを介してのみサポートされます。 この場合、表示イベントを自動的に送信することはできません。

### 自動的にレンダリングされた提案の表示イベントを送信

自動的にレンダリングされた提案の表示イベントを送信するには、`sendEvent` 呼び出しで次のパラメーターを設定する必要があります。

* `renderDecisions: true`
* ページヒット数の上位に対する `personalization.sendDisplayEvent: false`

表示イベントを送信するには、`sendEvent` で `personalization.includeRenderedPropositions: true` を呼び出します

### 手動でレンダリングされた提案の表示イベントを送信

手動でレンダリングした提案の表示イベントを送信するには、提案の `_experience.decisioning.propositions`、`id`、`scope` のフィールドを含め、`scopeDetails` XDM フィールドにそれらを含める必要があります。

さらに、「`include _experience.decisioning.propositionEventType.display`」フィールドを `1` に設定します。
