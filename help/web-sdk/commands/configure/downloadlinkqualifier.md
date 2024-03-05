---
title: downloadLinkQualifier
description: 自動リンクトラッキングでのダウンロードリンクの認定方法を決定するのに役立ちます。
source-git-commit: b6e084d2beed58339191b53d0f97b93943154f7c
workflow-type: tm+mt
source-wordcount: '228'
ht-degree: 0%

---

# `downloadLinkQualifier`

次を使用して自動リンクトラッキングを有効にした場合： [`clickCollectionEnabled`](clickcollectionenabled.md)、 `downloadLinkQualifier` プロパティは、URL がダウンロードリンクと見なされる条件を決定するのに役立ちます。

このプロパティは正規表現の文字列です。 クリックした URL がこの正規表現と一致する場合、 `xdm.web.webInteraction.type` が `"download"`. また、リンクに `download` HTML属性。 次の場合 `clickCollectionEnabled` が有効になっていない場合、このプロパティは何もしません。

## Web SDK タグ拡張を使用してリンク修飾子をダウンロード

を有効にします。 **[!UICONTROL クリックデータの収集を有効にする]** 」チェックボックスをオンにして、以下に目的のテキストを入力します。 **[!UICONTROL リンク修飾子をダウンロード]** when [タグ拡張の設定](/help/tags/extensions/client/web-sdk/web-sdk-extension-configuration.md).

1. にログインします。 [experience.adobe.com](https://experience.adobe.com) Adobe ID資格情報を使用して。
1. に移動します。 **[!UICONTROL データ収集]** > **[!UICONTROL タグ]**.
1. 目的のタグプロパティを選択します。
1. に移動します。 **[!UICONTROL 拡張機能]**&#x200B;を選択し、次に **[!UICONTROL 設定]** の [!UICONTROL Adobe Experience Platform Web SDK] カード。
1. 下にスクロールして、 [!UICONTROL データ収集] 「 」セクションで、「 」チェックボックスをオンにします。 **[!UICONTROL クリックデータの収集を有効にする]**.
1. 有効にすると、 **[!UICONTROL リンク修飾子をダウンロード]** テキストボックスが表示されます。 必要な値を入力します。 また、正規表現をテストし、デフォルト値に戻すためのボタンも使用できます。
1. クリック **[!UICONTROL 保存]**&#x200B;をクリックし、変更を公開します。

## Web SDK JavaScript ライブラリを使用してリンク修飾子をダウンロード

を設定します。 `downloadLinkQualifier` 文字列（実行時） `configure` コマンドを使用します。 このプロパティを省略した場合、デフォルトでは次の値になります。

`\.(exe|zip|wav|mp3|mov|mpg|avi|wmv|pdf|doc|docx|xls|xlsx|ppt|pptx)$`

ダウンロードリンクを絞り込むために異なる条件を使用する場合は、このプロパティを目的の正規表現値に設定します。

```js
alloy("configure", {
  "edgeConfigId": "ebebf826-a01f-4458-8cec-ef61de241c93",
  "orgId": "ADB3LETTERSANDNUMBERS@AdobeOrg",
  "clickCollectionEnabled": true,
  "downloadLinkQualifier": "\.(exe|zip|wav|mp3|mov|mpg|avi|wmv|pdf|doc|docx|xls|xlsx|ppt|pptx)$"
});
```
