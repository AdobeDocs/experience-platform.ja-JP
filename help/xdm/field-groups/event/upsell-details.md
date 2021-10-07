---
title: アップセル詳細スキーマフィールドグループ
description: このドキュメントでは、「アップセル詳細」スキーマフィールドグループの概要を説明します。
source-git-commit: 4a74faad811d9b13f93799686df44f04a8d1b784
workflow-type: tm+mt
source-wordcount: '154'
ht-degree: 5%

---

# [!UICONTROL アップセル詳細] スキーマフィールドグループ

[!UICONTROL アップセ]  [[!DNL XDM ExperienceEvent] ](../../classes/experienceevent.md) ルトランザクションの詳細や、オファーを顧客に表示する様々な方法など、アップセルマーケティングイベントに関する情報を取得するために使用される、クラスの標準スキーマフィールドグループです。

フィールドグループには、1 つのオブジェクトタイプのフィールド `upsells` が用意されています。 このオブジェクトに含まれるプロパティについて、以下で説明します。

![アップセルの詳細構造](../../images/field-groups/upsell-details.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `upsellImpressions` | [ インプレッション ](../../data-types/impressions.md) の配列 | 顧客に対して記録されたインプレッション（デジタルビュー、アップセルオファーとのエンゲージメント）をリストする配列。 |
| `upsellTransaction` | [トランザクション](../../data-types/transaction.md) | アップセルの通貨トランザクションを示します。 |

{style=&quot;table-layout:auto&quot;}

フィールドグループについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力例](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/industry-verticals/experienceevent-upsell-details.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/industry-verticals/experienceevent-upsell-details.schema.json)
