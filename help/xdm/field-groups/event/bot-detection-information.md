---
title: ボット検出フィールドグループ
description: ボット検出フィールドグループ（XDM）スキーマフィールドグループについて説明します。
exl-id: 8ade14a8-9a34-4060-95b2-812d1a21deeb
source-git-commit: 8be502c9eea67119dc537a5d63a6c71e0bff1697
workflow-type: tm+mt
source-wordcount: '101'
ht-degree: 8%

---

# [!UICONTROL &#x200B; ボット検出 &#x200B;] フィールドグループ

[!UICONTROL &#x200B; ボット検出 &#x200B;] は、[[!DNL XDM ExperienceEvent]  クラス &#x200B;](../../classes/experienceevent.md) の標準スキーマフィールドグループです。 フィールドグループは、ボット生成トラフィックに関する情報を提供します。

![&#x200B; 「ボット検出 [!UICONTROL &#x200B; フィールドグループ &#x200B;] 図 &#x200B;](../../images/field-groups/bot-detection-information.png)

| 表示名 | プロパティ | データタイプ | 説明 |
|----------------------------|-----------------|-----------|---------------------------------------------------------|
| [!UICONTROL &#x200B; ボットの検出 &#x200B;] | `botDetection` | オブジェクト | ボット生成トラフィックに関する情報を提供します。 |
| [!UICONTROL スコア] | `score` | 数値 | ゼロから 1 までのボット確率スコア。 スコアが 0 の場合は、トラフィックがボットではないことを意味します。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、公開 XDM リポジトリを参照してください。

* [&#x200B; 入力された例 &#x200B;](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-bot-detection.example.1.json)
* [&#x200B; 完全なスキーマ &#x200B;](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-bot-detection.schema.json)
