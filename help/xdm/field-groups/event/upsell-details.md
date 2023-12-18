---
title: アップセル詳細スキーマフィールドグループ
description: アップセル詳細スキーマフィールドグループについて説明します。
exl-id: 6b69805d-03bc-489b-945a-03e61b99842e
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '126'
ht-degree: 3%

---

# [!UICONTROL アップセルの詳細] スキーマフィールドグループ

[!UICONTROL アップセルの詳細] は、 [[!DNL XDM ExperienceEvent] クラス](../../classes/experienceevent.md) アップセルマーケティングイベントに関する情報（トランザクションに関する詳細や、オファーが顧客に表示された様々な方法など）を取得するために使用します。

フィールドグループには、単一のオブジェクトタイプのフィールドが用意されています。 `upsells`. このオブジェクトに含まれるプロパティについて、以下で説明します。

![アップセルの詳細構造](../../images/field-groups/upsell-details.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `upsellImpressions` | の配列 [Impressions](../../data-types/impressions.md) | 顧客に記録されたインプレッション（デジタルビュー、アップセルオファーを含むエンゲージメント）をリストする配列。 |
| `upsellTransaction` | [トランザクション](../../data-types/transaction.md) | アップセルの通貨トランザクションを記述します。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力された例](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/industry-verticals/experienceevent-upsell-details.example.1.json)
* [完全なスキーマ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/industry-verticals/experienceevent-upsell-details.schema.json)
