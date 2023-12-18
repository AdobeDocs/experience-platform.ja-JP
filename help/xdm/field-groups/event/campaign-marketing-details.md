---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；スキーマ；XDM;ExperienceEvent；フィールド；スキーマ；スキーマデザイン；フィールドグループ；フィールドグループ；
solution: Experience Platform
title: キャンペーンマーケティング詳細スキーマフィールドグループ
description: 「キャンペーンマーケティング詳細」スキーマフィールドグループについて説明します。
exl-id: be08b38b-68a0-4a74-9b8f-0344a0637395
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '124'
ht-degree: 20%

---

# [!UICONTROL キャンペーンマーケティングの詳細] スキーマフィールドグループ

>[!NOTE]
>
>複数のスキーマフィールドグループの名前が変更されました。 詳しくは、[フィールドグループ名の更新](../name-updates.md)のドキュメントを参照してください。

[!UICONTROL キャンペーンマーケティングの詳細] は、 [[!DNL XDM ExperienceEvent] クラス](../../classes/experienceevent.md)：キャンペーングループ、名前、トラッキングコードなど、マーケティングキャンペーン情報の説明に使用されます。

![](../../images/field-groups/campaign-marketing-details.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `marketing` | [マーケティング](../../data-types/marketing.md) | マーケティングキャンペーン情報（キャンペーングループ、名前、トラッキングコードなど）を表すオブジェクト。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力された例](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-marketing.example.1.json)
* [完全なスキーマ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-marketing.schema.json)
