---
title: インプレッションデータタイプ
description: このドキュメントでは、インプレッション XDM データタイプの概要を説明します。
exl-id: 1e758043-a41e-45f7-ae8b-514990d0649e
source-git-commit: afdac5ce2ed967b4688d456a586c946bc2cf4179
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 7%

---

# [!UICONTROL Impressions] データタイプ

[!UICONTROL Impressions] はマーケティングインプレッションを表す標準的な XDM データ型です。マーケティングインプレッションは、広告、デジタル投稿、Web ページなどのコンテンツのデジタルビューやエンゲージメントの数を定量化するために使用される指標です。

![](../images/data-types/impressions.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `ID` | 文字列 | インプレッションの一意の ID。 |
| `displays` | 整数 | インプレッション項目が顧客に表示された回数。 |
| `selected` | 整数 | インプレッション項目が選択またはクリックされた回数。 |
| `type` | 文字列 | インプレッションのタイプ。 |

{style=&quot;table-layout:auto&quot;}

フィールドグループについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力された例](https://github.com/adobe/xdm/blob/master/components/datatypes/industry-verticals/impressions.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/industry-verticals/impressions.schema.json)
