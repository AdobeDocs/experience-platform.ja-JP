---
title: サイトツール詳細スキーマフィールドグループ
description: サイトツールの詳細スキーマフィールドグループについて説明します。
exl-id: 472c0a3f-efda-49af-9490-f2de90b348c0
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '181'
ht-degree: 6%

---

# [!UICONTROL &#x200B; サイトツールの詳細 &#x200B;] スキーマフィールドグループ

[!UICONTROL &#x200B; サイトツールの詳細 &#x200B;] は、[[!DNL XDM ExperienceEvent]  クラス &#x200B;](../../classes/experienceevent.md) の標準スキーマフィールドグループです。 フィールドグループは、スキーマに単一の `sitetool` オブジェクトを提供し、スキーマは、サイトツールによって収集された情報を取得します。

![&#x200B; フィールドグループ構造 &#x200B;](../../images/field-groups/sitetool-details.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `dataGatheringEvent` | オブジェクト | このイベントがデータ収集イベントであるかどうか、およびその他の関連する詳細を示します。 次のプロパティが含まれます。<ul><li>`data`: （Map）クイズ、調査、またはポーリング送信イベントの一部として収集および送信される JSON データが含まれています。</li><li>`isTrue`: （ブール値）このイベントが、クイズ、調査、投票などのデータ収集イベントであるかどうかを示します。</li><li>`score`: （整数）イベント応答に基づいてアクターによって保護されたスコア。</li></ul> |
| `actor` | 文字列 | アクションを実行したユーザーまたはメンバー。 |
| `actorID` | 文字列 | アクションを実行したユーザー/メンバーの一意の ID。 |
| `isKeyEvent` | ブール値 | このイベントがキーイベントであるかどうかを示します。 |
| `name` | 文字列 | チャットボットやアンケートなどのサイトツールの名前。 |
| `section` | 文字列 | main や sub など、サイトツールの関連セクション。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、[&#x200B; 公開 XDM リポジトリ &#x200B;](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/industry-verticals/experienceevent-healthcare-sitetool.schema.json) を参照してください。
