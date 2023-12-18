---
title: アップグレード詳細スキーマフィールドグループ
description: 「アップグレードの詳細」スキーマフィールドグループについて説明します。
exl-id: cd3f4cd9-ee0e-4bdf-a630-dd2c3c3cc8c7
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '126'
ht-degree: 3%

---

# [!UICONTROL アップグレードの詳細] スキーマフィールドグループ

[!UICONTROL アップグレードの詳細] は、 [[!DNL XDM ExperienceEvent] クラス](../../classes/experienceevent.md) トランザクションに関する詳細や、オファーが顧客に表示された様々な方法など、アップグレードマーケティングイベントに関する情報を取得するために使用します。

フィールドグループには、単一のオブジェクトタイプのフィールドが用意されています。 `upgrades`. このオブジェクトに含まれるプロパティについて、以下で説明します。

![アップグレードの詳細構造](../../images/field-groups/upgrade-details.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `upgradeImpressions` | の配列 [Impressions](../../data-types/impressions.md) | 顧客に対して記録されたインプレッション（デジタルビュー、アップグレードオファーを含むエンゲージメント）をリストする配列。 |
| `upgradeTransaction` | [トランザクション](../../data-types/transaction.md) | アップグレードの通貨トランザクションを記述します。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力された例](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/industry-verticals/experienceevent-upgrade-details.example.1.json)
* [完全なスキーマ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/industry-verticals/experienceevent-upgrade-details.schema.json)
