---
title: Web SDK での表示イベントの管理
description: この記事では、表示イベントの概要と、Web SDK での表示イベントの使用方法について説明します。
exl-id: 7150ad6e-7693-4f4d-917e-8d08a39a0b41
source-git-commit: c2e308b5e743f07062be9a34e23c4bc700b27463
workflow-type: tm+mt
source-wordcount: '351'
ht-degree: 0%

---

# Web SDK での表示イベントの管理

Web SDK は、表示イベントを使用して、特定のパーソナライゼーションコンテンツがページに表示された際に、パーソナライゼーションまたは Analytics サービスに通知します。

表示イベントを送信すると、パーソナライゼーション指標の精度が向上し、ページに表示される内容の正確な概要が得られます。

Web SDK では、次の 2 つの方法で表示イベントを送信できます。

* [自動](#send-automatically)に含まれるのは、パーソナライズされたコンテンツがページにレンダリングされた直後です。 方法に関するドキュメントを参照してください。 [パーソナライズされたコンテンツをレンダリング](rendering-personalization-content.md) を参照してください。
* [手動](#send-sendEvent-calls)、以降 `sendEvent` 呼び出し。

>[!NOTE]
>
>イベントの `applyPropositions` 関数に置き換えます。

## 表示イベントを自動的に送信する {#send-automatically}

イベントはパーソナライゼーションが読み込まれた直後に送信されるので、表示イベントを送信すると、より正確な Analytics 指標が自動的に提供されます。 また、この実装では、実装方法がより効率的になります。

パーソナライズされたコンテンツがページにレンダリングされた後で表示イベントを自動的に送信するには、次のパラメーターを設定する必要があります。

* `renderDecisions: true`
* `personalization.sendDisplayNotifications: true` または指定されていません

Web SDK は、 `sendEvent` を呼び出します。

## 後続の sendEvent 呼び出しで表示イベントを送信する {#send-sendEvent-calls}

比較対象 [自動的に](#send-automatically) 表示イベントの送信 ( 後続の `sendEvent` 呼び出しでは、この呼び出しにページ読み込みに関する詳細を含めることもできます。 これは、パーソナライズされたコンテンツを要求する際に利用できなかった、追加の情報である可能性があります。

また、 `sendEvent` 呼び出しを使用すると、Adobe Analyticsの使用時に発生するバウンス率のエラーを最小限に抑えることができます。

>[!IMPORTANT]
>
>手動でレンダリングした提案を使用する場合、表示イベントは、 `sendEvent` 呼び出し。 この場合、表示イベントを自動的に送信することはできません。

### 自動的にレンダリングされた提案の表示イベントを送信 {#auto-rendered-propositions}

自動的にレンダリングされた提案に対して表示イベントを送信するには、 `sendEvent` 呼び出し

* `renderDecisions: true`
* `personalization.sendDisplayNotifications: false` （ページヒットの上位）

表示イベントを送信するには、 `sendEvent` 次を使用 `personalization.includePendingDisplayNotifications: true`

### 手動でレンダリングした提案の表示イベントを送信 {#manually-rendered-propositions}

手動でレンダリングした提案の表示イベントを送信するには、それらを `_experience.decisioning.propositions` XDM フィールド ( `id`, `scope`、および `scopeDetails` 提案からのフィールド。

さらに、 `include _experience.decisioning.propositionEventType.display` ～に向かって `1`.
