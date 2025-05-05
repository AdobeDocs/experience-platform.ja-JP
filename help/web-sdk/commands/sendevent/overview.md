---
title: sendEvent
description: Adobe Experience Platform Edge Networkにデータを送信します。
exl-id: 83de368d-78d4-4e28-aadd-afaea1ca091d
source-git-commit: 9ea7b678f5cfa19c7fd1e3ba6633cdeed4084b18
workflow-type: tm+mt
source-wordcount: '257'
ht-degree: 0%

---

# `sendEvent`

`sendEvent` コマンドは、Adobeにデータを送信して、パーソナライズされたコンテンツ、ID およびオーディエンスの宛先を取得する主な方法です。 [`xdm`](xdm.md) オブジェクトを使用して、Adobe Experience Platform スキーマにマッピングするデータを送信します。 [`data`](data.md) オブジェクトを使用して、XDM 以外のデータを送信します。 データストリームマッパーを使用して、このオブジェクト内のデータをスキーマフィールドに合わせることができます。

## Web SDK タグ拡張機能を使用したイベントデータの送信

イベントデータの送信は、Adobe Experience Platform Data Collection タグインターフェイスのルール内のアクションとして実行されます。

1. Adobe IDの資格情報を使用して [experience.adobe.com](https://experience.adobe.com) にログインします。
1. **[!UICONTROL データ収集]**/**[!UICONTROL タグ]** に移動します。
1. 目的のタグプロパティを選択します。
1. **[!UICONTROL ルール]** に移動し、目的のルールを選択します。
1. [!UICONTROL &#x200B; アクション &#x200B;] で、既存のアクションを選択するか、アクションを作成します。
1. 「[!UICONTROL &#x200B; 拡張機能 &#x200B;]」ドロップダウンフィールドを **[!UICONTROL Adobe Experience Platform Web SDK]** に設定し、「[!UICONTROL &#x200B; アクションタイプ &#x200B;] を **[!UICONTROL イベントを送信]** に設定します。
1. 必要なフィールドを設定し、「**[!UICONTROL 変更を保持]**」をクリックして、公開ワークフローを実行します。

## Web SDK JavaScript ライブラリを使用したイベントデータの送信

設定済みの Web SDK インスタンスを呼び出す際に、`sendEvent` コマンドを実行します。 `sendEvent` コマンドを呼び出す前に、必ず [`configure`](../configure/overview.md) コマンドを呼び出してください。

```js
alloy("sendEvent", {
  "data": dataObject,
  "documentUnloading": false,
  "edgeConfigOverrides": { "datastreamId": "0dada9f4-fa94-4c9c-8aaf-fdbac6c56287" },
  "renderDecisions": true,
  "type": "commerce.purchases",
  "xdm": adobeDataLayer.getState(reference)
});
```

## 応答オブジェクト

このコマンドを使用して [ 応答を処理 ](../command-responses.md) する場合、応答オブジェクトで次のプロパティを使用できます。

* **`propositions`**:Edge Networkによって返される提案の配列。 自動的にレンダリングされる提案には、`true` に設定され `renderAttempted` フラグが含まれます。
* **`inferences`**：このユーザーに関する機械学習情報を含む、推論オブジェクトの配列。
* **`destinations`**:Edge Networkによって返される対象オブジェクトの配列。
