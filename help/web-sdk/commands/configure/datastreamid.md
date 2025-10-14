---
title: datastreamId
description: データの送信先のデータストリーム ID を決定します。
exl-id: 2d709f70-c014-4868-b2f5-17e8b88343d1
source-git-commit: 8fc0fd96f13f0642f7671d0e0f4ecfae8ab6761f
workflow-type: tm+mt
source-wordcount: '297'
ht-degree: 0%

---

# `datastreamId`

`datastreamId` プロパティは、データの送信先のAdobe Experience Platform内の [&#x200B; データストリーム &#x200B;](../../../datastreams/overview.md) を決定する文字列です。 このプロパティは、データをAdobeに送信する際に必要です。 Web SDK バージョン 2.20.0 以前では、代わりに `edgeConfigId` を使用します。

データストリーム ID を見つけるには：

1. Adobe IDの資格情報を使用して [experience.adobe.com](https://experience.adobe.com) にログインします。
1. **[!UICONTROL データ収集]**/**[!UICONTROL データストリーム]** に移動します。
1. 検索フィールドを使用して目的のデータストリームを見つけ、データストリーム ID の横にある **[!UICONTROL コピー]** ![&#x200B; コピー &#x200B;](../../assets/copy.png) を選択します。

または、目的のデータストリーム名を選択でき、コピーするデータストリーム ID が右側の列に表示されます。

## Web SDK タグ拡張機能を使用してデータストリーム ID を選択します

使用可能なデータストリームのリストから選択するか、（タグ拡張機能の設定 [&#x200B; 時にデータストリーム ID を直接入力し &#x200B;](/help/tags/extensions/client/web-sdk/web-sdk-extension-configuration.md) す。

1. Adobe IDの資格情報を使用して [experience.adobe.com](https://experience.adobe.com) にログインします。
1. **[!UICONTROL データ収集]**/**[!UICONTROL タグ]** に移動します。
1. 目的のタグプロパティを選択します。
1. **[!UICONTROL 拡張機能]** に移動し、[!UICONTROL Adobe Experience Platform Web SDK **[!UICONTROL カードの]** 設定 &#x200B;] をクリックします。
1. [!UICONTROL &#x200B; データストリーム &#x200B;] セクションを見つけ、データストリームを決定する目的の方法を選択します。
   * リストから選択する場合は、サンドボックスとデータストリームをそれぞれのドロップダウンリストから選択します。
   * 値を入力する場合は、目的のデータストリーム ID を入力します。
1. 「**[!UICONTROL 保存]**」をクリックして、変更を公開します。

実稼動、ステージングおよび開発のタグ環境用に、様々なデータストリームにデータを送信できます。

## Web SDK JavaScript ライブラリを使用してデータストリーム ID を選択します

`configure` コマンドを実行するときは、`datastreamID` string プロパティを設定します。 このプロパティは、すべての Web SDK 実装で必要です。 このプロパティを省略すると、Web SDK はデータを送信するデータストリームを認識しないため、そのデータは恒久的に失われます。

```js
alloy("configure", {
  datastreamId: "ebebf826-a01f-4458-8cec-ef61de241c93",
  orgId: "ADB3LETTERSANDNUMBERS@AdobeOrg",
});
```

1 つのページに Web SDK の複数のインスタンスを設定する場合、インスタンスごとに異なる `datastreamId` を設定する必要があります。
