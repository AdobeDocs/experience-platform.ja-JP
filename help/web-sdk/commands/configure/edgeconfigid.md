---
title: edgeConfigId
description: データの送信先のデータストリーム ID を決定します。
source-git-commit: f75dcfc945be2f45c1638bdd4d670288aef6e1e6
workflow-type: tm+mt
source-wordcount: '288'
ht-degree: 0%

---

# `edgeConfigId`

The `edgeConfigId` プロパティは、 [datastream](../../../datastreams/overview.md) Adobe Experience Platformで、データの送信先となるユーザー名です。 このプロパティは、データをAdobeに送信する際に必要です。

データストリーム ID を探すには、次の手順に従います。

1. にログインします。 [experience.adobe.com](https://experience.adobe.com) Adobe ID資格情報を使用して。
1. に移動します。 **[!UICONTROL データ収集]** > **[!UICONTROL データストリーム]**.
1. 検索フィールドを使用して目的のデータストリームを見つけ、「 」を選択します。 **[!UICONTROL コピー]** ![コピー](../../assets/copy.png) をクリックします。

また、目的のデータストリーム名を選択すると、コピーする右側の列にデータストリーム ID が表示されます。

## Web SDK タグ拡張機能を使用してデータストリーム ID を選択します。

使用可能なデータストリームのリストから選択するか、 [タグ拡張の設定](/help/tags/extensions/client/web-sdk/web-sdk-extension-configuration.md).

1. にログインします。 [experience.adobe.com](https://experience.adobe.com) Adobe ID資格情報を使用して。
1. に移動します。 **[!UICONTROL データ収集]** > **[!UICONTROL タグ]**.
1. 目的のタグプロパティを選択します。
1. に移動します。 **[!UICONTROL 拡張機能]**&#x200B;を選択し、次に **[!UICONTROL 設定]** の [!UICONTROL Adobe Experience Platform Web SDK] カード。
1. 次を見つけます。 [!UICONTROL データストリーム] 」セクションで、データストリームを決定する目的の方法を選択します。
   * リストからを選択する場合は、各ドロップダウンリストからサンドボックスとデータストリームを選択します。
   * 値を入力する場合は、目的のデータストリーム ID を入力します。
1. クリック **[!UICONTROL 保存]**&#x200B;をクリックし、変更を公開します。

実稼動、ステージングおよび開発のタグ環境用に、様々なデータストリームにデータを送信できます。

## Web SDK JavaScript ライブラリを使用したデータストリーム ID の選択

を設定します。 `edgeConfigId` 文字列プロパティ（実行時） `configure` コマンドを使用します。 このプロパティは、すべての Web SDK 実装に必要です。 このプロパティを省略した場合、Web SDK はデータの送信先のデータストリームを認識せず、そのデータが永久的に失われます。

```js
alloy("configure", {
  "edgeConfigId": "ebebf826-a01f-4458-8cec-ef61de241c93",
  "orgId": "ADB3LETTERSANDNUMBERS@AdobeOrg",
});
```

1 つのページで Web SDK の複数のインスタンスを設定する場合は、 `edgeConfigId` インスタンスごとに
