---
title: ボット検出フィールドグループ
description: ボット検出フィールドグループ (XDM) スキーマフィールドグループについて説明します。
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '101'
ht-degree: 6%

---

# [!UICONTROL ボット検出] フィールドグループ

[!UICONTROL ボット検出] は、 [[!DNL XDM ExperienceEvent] クラス](../../classes/experienceevent.md). フィールドグループは、ボットが生成したトラフィックに関する情報を提供します。

![の図 [!UICONTROL ボット検出] フィールドグループを使用します。](../../images/field-groups/bot-detection-information.png)

| 表示名 | プロパティ | データタイプ | 説明 |
|----------------------------|-----------------|-----------|---------------------------------------------------------|
| [!UICONTROL ボット検出] | `botDetection` | オブジェクト | ボットが生成したトラフィックに関する情報を提供します。 |
| [!UICONTROL スコア] | `score` | 数値 | ボットの確率スコア (0 ～ 1)。 スコアが 0 の場合は、トラフィックがボットではないことを意味します。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力された例](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-bot-detection.example.1.json)
* [完全なスキーマ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-bot-detection.schema.json)

