---
title: インプレッション数データタイプ
description: インプレッション XDM データタイプについて説明します。
exl-id: 1e758043-a41e-45f7-ae8b-514990d0649e
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '115'
ht-degree: 6%

---

# [!UICONTROL &#x200B; インプレッション数 &#x200B;] データタイプ

[!UICONTROL &#x200B; インプレッション &#x200B;] は、マーケティングインプレッションを表す標準 XDM データタイプです。マーケティングインプレッションは、広告、デジタル投稿、web ページなどの、コンテンツのデジタルビューまたはエンゲージメントの数を定量化するために使用される指標です。

![](../images/data-types/impressions.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `ID` | 文字列 | インプレッションの一意の ID。 |
| `displays` | 整数 | インプレッション項目が顧客に表示された回数。 |
| `selected` | 整数 | インプレッション項目が選択またはクリックされた回数。 |
| `type` | 文字列 | インプレッションのタイプ。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、公開 XDM リポジトリを参照してください。

* [&#x200B; 入力された例 &#x200B;](https://github.com/adobe/xdm/blob/master/components/datatypes/industry-verticals/impressions.example.1.json)
* [&#x200B; 完全なスキーマ &#x200B;](https://github.com/adobe/xdm/blob/master/components/datatypes/industry-verticals/impressions.schema.json)
