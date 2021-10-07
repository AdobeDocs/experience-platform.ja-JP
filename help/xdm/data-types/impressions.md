---
title: Impressions データタイプ
description: このドキュメントでは、インプレッション XDM データタイプの概要を説明します。
source-git-commit: 7fc16546176d196582a3cdfcee51f799eeef9788
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 7%

---

#  Impressionsdata type

 インプレッションとは、マーケティングインプレッションを記述する標準の XDM データ型で、広告、デジタル投稿、Web ページなどのコンテンツのデジタルビュー数やアクション数を定量化する指標です。

![](../images/data-types/impressions.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `ID` | 文字列 | インプレッションの一意の ID。 |
| `displays` | 整数 | 顧客にインプレッション項目が表示された回数。 |
| `selected` | 整数 | インプレッション項目が選択またはクリックされた回数。 |
| `type` | 文字列 | インプレッションのタイプ。 |

{style=&quot;table-layout:auto&quot;}

フィールドグループについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力例](https://github.com/adobe/xdm/blob/master/components/datatypes/industry-verticals/impressions.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/industry-verticals/impressions.schema.json)
