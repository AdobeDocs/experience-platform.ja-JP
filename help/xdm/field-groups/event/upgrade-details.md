---
title: アップグレード詳細スキーマフィールドグループ
description: このドキュメントでは、「アップグレードの詳細」スキーマフィールドグループの概要を説明します。
exl-id: cd3f4cd9-ee0e-4bdf-a630-dd2c3c3cc8c7
source-git-commit: afdac5ce2ed967b4688d456a586c946bc2cf4179
workflow-type: tm+mt
source-wordcount: '154'
ht-degree: 5%

---

# [!UICONTROL 詳細スキーマ] フィールドグループのアップグレード

[!UICONTROL アップグレード]  [[!DNL XDM ExperienceEvent] ](../../classes/experienceevent.md) トランザクションの詳細や、オファーを顧客に表示する様々な方法など、アップグレードマーケティングイベントに関する情報の取得に使用される、クラスの標準スキーマフィールドグループの詳細です。

フィールドグループには、1 つのオブジェクトタイプのフィールド `upgrades` が用意されています。 このオブジェクトに含まれるプロパティについて、以下で説明します。

![アップグレードの詳細構造](../../images/field-groups/upgrade-details.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `upgradeImpressions` | [ インプレッション ](../../data-types/impressions.md) の配列 | 顧客に対して記録されたインプレッション（デジタルビュー、アップグレードオファーに関するエンゲージメント）をリストする配列。 |
| `upgradeTransaction` | [トランザクション](../../data-types/transaction.md) | アップグレードの通貨トランザクションについて説明します。 |

{style=&quot;table-layout:auto&quot;}

フィールドグループについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力例](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/industry-verticals/experienceevent-upgrade-details.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/industry-verticals/experienceevent-upgrade-details.schema.json)
