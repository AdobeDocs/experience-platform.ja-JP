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

この `sendEvent` コマンドは、Adobeにデータを送信し、パーソナライズされたコンテンツ、ID およびオーディエンスの宛先を取得する主な方法です。 の使用 [`xdm`](xdm.md) Adobe Experience Platform スキーマにマッピングするデータを送信するオブジェクト。 の使用 [`data`](data.md) xdm 以外のデータを送信するオブジェクト。 データストリームマッパーを使用して、このオブジェクト内のデータをスキーマフィールドに合わせることができます。

## Web SDK タグ拡張機能を使用したイベントデータの送信

イベントデータの送信は、Adobe Experience Platform Data Collection タグインターフェイスのルール内のアクションとして実行されます。

1. へのログイン [experience.adobe.com](https://experience.adobe.com) Adobe IDの資格情報を使用します。
1. に移動します。 **[!UICONTROL データ収集]** > **[!UICONTROL タグ]**.
1. 目的のタグプロパティを選択します。
1. に移動します。 **[!UICONTROL ルール]**&#x200B;を選択してから、目的のルールを選択します。
1. 次の下 [!UICONTROL アクション]、既存のアクションを選択するか、アクションを作成します。
1. を [!UICONTROL 拡張機能] ドロップダウンフィールドの移動先 **[!UICONTROL Adobe Experience Platform Web SDK]**、を設定します。 [!UICONTROL アクションタイプ] 対象： **[!UICONTROL イベントを送信]**.
1. 目的のフィールドを設定し、 **[!UICONTROL 変更を保持]**&#x200B;次に、公開ワークフローを実行します。

## Web SDK JavaScript ライブラリを使用したイベントデータの送信

を実行 `sendEvent` コマンドは、設定した Web SDK のインスタンスを呼び出す際に使用します。 必ずを [`configure`](../configure/overview.md) コマンドを実行してから、 `sendEvent` コマンド。

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

以下を行う場合 [応答を処理](../command-responses.md) このコマンドを使用すると、応答オブジェクトで次のプロパティを使用できます。

* **`propositions`**:Edge Networkによって返される提案の配列。 自動的にレンダリングされる提案にはフラグが含まれます `renderAttempted` をに設定 `true`.
* **`inferences`**：推論オブジェクトの配列。このオブジェクトには、このユーザーに関する機械学習情報が含まれています。
* **`destinations`**:Edge Networkによって返される宛先オブジェクトの配列。
