---
title: sendEvent
description: Adobe Experience Platform Edge Networkにデータを送信します。
exl-id: 83de368d-78d4-4e28-aadd-afaea1ca091d
source-git-commit: c6a2b9700f0a688f65fec9febf5622c6c7b6aafa
workflow-type: tm+mt
source-wordcount: '175'
ht-degree: 0%

---

# `sendEvent`

`sendEvent` コマンドは、データをAdobeに送信する主な方法です。 その応答オブジェクトは、パーソナライズされたコンテンツ、ID およびオーディエンスの宛先を取得する主な方法です。 [`xdm`](xdm.md) オブジェクトを使用して、Adobe Experience Platform スキーマにマッピングするデータを送信します。 [`data`](data.md) オブジェクトを使用して、XDM 以外のデータを送信します。 Adobeにデータを送信する際のペイロードの上限は 64 KB です。

設定済みの Web SDK インスタンスを呼び出す際に、`sendEvent` コマンドを実行します。 [`configure`](../configure/overview.md) コマンドを呼び出す前に、必ず `sendEvent` コマンドを呼び出してください。

```js
alloy("sendEvent", {
  data: dataObject,
  documentUnloading: false,
  edgeConfigOverrides: { datastreamId: "0dada9f4-fa94-4c9c-8aaf-fdbac6c56287" },
  personalization: { decisionScopes: ["hero-banner"]},
  renderDecisions: true,
  type: "commerce.purchases",
  xdm: adobeDataLayer.getState(reference)
});
```

## 応答オブジェクト

このコマンドを使用して [&#x200B; 応答を処理 &#x200B;](../command-responses.md) する場合、応答オブジェクトで次のプロパティを使用できます。

* **`propositions`**:Edge Networkから返される提案の配列。 自動的にレンダリングされる提案には、`renderAttempted` に設定され `true` フラグが含まれます。
* **`inferences`**：このユーザーに関する機械学習情報を含む、推論オブジェクトの配列。
* **`destinations`**:Edge Networkによって返される宛先オブジェクトの配列。

## Web SDK タグ拡張機能を使用してイベントを送信

このコマンドに相当する web SDK タグ拡張機能は、[**[!UICONTROL Send event]**](/help/tags/extensions/client/web-sdk/actions/send-event.md#data-fields) のアクションです。
