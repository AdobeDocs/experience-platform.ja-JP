---
title: Sitetool 詳細スキーマフィールドグループ
description: このドキュメントでは、「サイトツールの詳細」スキーマフィールドグループの概要を説明します。
exl-id: 472c0a3f-efda-49af-9490-f2de90b348c0
source-git-commit: 2fd35c4ac29f43391f9dc03c636d20558b701be7
workflow-type: tm+mt
source-wordcount: '195'
ht-degree: 12%

---

# [!UICONTROL サイトツールの詳細] スキーマフィールドグループ

[!UICONTROL サイトツールの詳細] は、 [[!DNL XDM ExperienceEvent] クラス](../../classes/experienceevent.md). フィールドグループには、 `sitetool` オブジェクトをスキーマに追加します。

![フィールドグループ構造](../../images/field-groups/sitetool-details.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `dataGatheringEvent` | オブジェクト | このイベントがデータ収集イベントであるかどうか、およびその他の関連する詳細を示します。 次のプロパティが含まれます。<ul><li>`data`:（マップ）クイズ、調査またはポールの送信イベントの一環として収集および送信される JSON データが含まれます。</li><li>`isTrue`:(Boolean) このイベントが、クイズ、調査、投票などのデータ収集イベントかどうかを示します。</li><li>`score`:（整数）イベント応答に基づいてアクターが保護するスコア。</li></ul> |
| `actor` | 文字列 | アクションを実行した人またはメンバー。 |
| `actorID` | 文字列 | アクションを実行した人物またはメンバーの一意の識別子。 |
| `isKeyEvent` | ブール値 | このイベントがキーイベントであるかどうかを示します。 |
| `name` | 文字列 | サイトツールの名前（chatbot、survey など）。 |
| `section` | 文字列 | メインやサブなど、サイトツールの関連するセクション。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、 [パブリック XDM リポジトリ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/industry-verticals/experienceevent-healthcare-sitetool.schema.json).
