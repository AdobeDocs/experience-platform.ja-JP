---
title: datastreamId
description: データの送信先のデータストリーム ID を決定します。
exl-id: 2d709f70-c014-4868-b2f5-17e8b88343d1
source-git-commit: b9fea4833c4fe869b27c1270aafd3f7ed62d59db
workflow-type: tm+mt
source-wordcount: '216'
ht-degree: 1%

---

# `datastreamId`

`datastreamId` プロパティは、データの送信先のAdobe Experience Platform内の [&#x200B; データストリーム &#x200B;](/help/datastreams/overview.md) を決定する文字列です。 このプロパティは、データをAdobeに送信する際に必要です。 Web SDK バージョン 2.20.0 以前では、代わりに `edgeConfigId` を使用します。

データストリーム ID を見つけるには：

1. Adobe IDの資格情報を使用して [experience.adobe.com](https://experience.adobe.com) にログインします。
1. **[!UICONTROL Data Collection]**／**[!UICONTROL Datastreams]**&#x200B;に移動します。
1. 検索フィールドを使用して目的のデータストリームを見つけ、データストリーム ID の横にある **[!UICONTROL Copy]**![&#x200B; コピー &#x200B;](../../assets/copy.png) を選択します。

または、目的のデータストリーム名を選択でき、コピーするデータストリーム ID が右側の列に表示されます。

## コードの例

`datastreamID` コマンドを実行するときは、`configure` string プロパティを設定します。 このプロパティは、すべての web SDK実装で必要です。 このプロパティを省略すると、Web SDKはデータを送信するデータストリームを認識しないため、そのデータは恒久的に失われます。

```js
alloy("configure", {
  datastreamId: "ebebf826-a01f-4458-8cec-ef61de241c93",
  orgId: "ADB3LETTERSANDNUMBERS@AdobeOrg",
});
```

>[!NOTE]
>
>1 つのページに web SDKの複数のインスタンスを設定する場合、インスタンスごとに異なる `datastreamId` を設定する必要があります。

## Web SDK タグ拡張機能を使用してデータストリーム ID を選択します

タグを使用して各環境に目的のデータストリームを設定する方法については、Web SDK タグ拡張機能ドキュメントの [&#x200B; データストリーム設定 &#x200B;](/help/tags/extensions/client/web-sdk/configure/datastreams.md) を参照してください。 実稼動、ステージングおよび開発のタグ環境用に、様々なデータストリームにデータを送信できます。
