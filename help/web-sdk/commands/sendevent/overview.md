---
title: sendEvent
description: Adobe Experience Platform Edge Network にデータを送信します。
source-git-commit: f75dcfc945be2f45c1638bdd4d670288aef6e1e6
workflow-type: tm+mt
source-wordcount: '257'
ht-degree: 0%

---


# `sendEvent`

The `sendEvent` コマンドは、Adobeにデータを送信し、パーソナライズされたコンテンツ、ID、オーディエンスの宛先を取得する主な方法です。 以下を使用します。 [`xdm`](xdm.md) オブジェクトを使用して、Adobe Experience Platformスキーマにマッピングされたデータを送信します。 以下を使用します。 [`data`](data.md) オブジェクトを使用して、XDM 以外のデータを送信します。 データストリームマッパーを使用すると、このオブジェクト内のデータをスキーマフィールドに合わせることができます。

## Web SDK タグ拡張機能を使用したイベントデータの送信

イベントデータの送信は、Adobe Experience Platformのデータ収集タグインターフェイスのルール内のアクションとして実行されます。

1. にログインします。 [experience.adobe.com](https://experience.adobe.com) Adobe ID資格情報を使用して。
1. に移動します。 **[!UICONTROL データ収集]** > **[!UICONTROL タグ]**.
1. 目的のタグプロパティを選択します。
1. に移動します。 **[!UICONTROL ルール]**&#x200B;を選択し、目的のルールを選択します。
1. の下 [!UICONTROL アクション]、既存のアクションを選択するか、アクションを作成します。
1. を設定します。 [!UICONTROL 拡張] ドロップダウンフィールド **[!UICONTROL Adobe Experience Platform Web SDK]**&#x200B;をクリックし、 [!UICONTROL アクションタイプ] から **[!UICONTROL イベントを送信]**.
1. 目的のフィールドを設定し、「 **[!UICONTROL 変更を保持]**&#x200B;を開き、パブリッシュワークフローを実行します。

## Web SDK JavaScript ライブラリを使用したイベントデータの送信

を実行します。 `sendEvent` コマンドを使用して、Web SDK の設定済みインスタンスを呼び出す際に呼び出すことができます。 必ず [`configure`](../configure/overview.md) コマンドを呼び出す前に `sendEvent` コマンドを使用します。

```js
alloy("sendEvent", {
  "data": dataObject,
  "documentUnloading": true,
  "edgeConfigOverrides": { "datastreamId": "0dada9f4-fa94-4c9c-8aaf-fdbac6c56287" },
  "renderDecisions": true,
  "type": "commerce.purchases",
  "xdm": adobeDataLayer.getState(reference)
});
```

## 応答オブジェクト

もしあなたが [応答を処理する](../command-responses.md) このコマンドを使用すると、次のプロパティが応答オブジェクトで使用できます。

* **`propositions`**:Edge ネットワークから返される提案の配列。 自動的にレンダリングされる提案には、フラグが含まれます `renderAttempted` に設定 `true`.
* **`inferences`**：このユーザーに関する機械学習情報を含む、推論オブジェクトの配列。
* **`destinations`**:Edge ネットワークから返される宛先オブジェクトの配列。
