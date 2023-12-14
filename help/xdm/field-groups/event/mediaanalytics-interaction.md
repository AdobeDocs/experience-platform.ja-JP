---
title: MediaAnalytics インタラクション詳細スキーマフィールドグループ
description: MediaAnalytics インタラクション詳細スキーマフィールドグループについて説明します。
source-git-commit: 65f3dcf1cacfbc4e8a598244810d238bd88f64bd
workflow-type: tm+mt
source-wordcount: '137'
ht-degree: 3%

---

# [!UICONTROL MediaAnalytics インタラクションの詳細] スキーマフィールドグループ

[!UICONTROL MediaAnalytics インタラクションの詳細] は、 [[!DNL XDM ExperienceEvent] クラス](../../classes/experienceevent.md). このフィールドグループを使用して、様々なプラットフォームやチャネルにわたるメディアコンテンツとのやり取りを包括的に監視および分析する、エンリッチメントされたデータフィールドをキャプチャします。

![のスキーマ図 [!UICONTROL MediaAnalytics インタラクションの詳細] スキーマフィールドグループを使用します。](../../images/field-groups/mediaanalytics-interaction.png)

| 表示名 | プロパティ | データタイプ | 説明 |
|---| --- | --- | --- |
| [!UICONTROL メディアコレクションの詳細] | `mediaCollection` | [[!UICONTROL メディアの詳細情報]](../../data-types/media-details-information.md) | メディア項目のコレクションに関連する属性。 |
| [!UICONTROL メディアレポートの詳細] | `mediaReporting` | [[!UICONTROL メディアの詳細情報]](../../data-types/media-details-information.md) | メディアコンテンツに関連付けられているレポートの詳細と指標。 |
| [!UICONTROL メディアコレクションダウンロードされたコンテンツイベントのリスト] | `mediaDownloadedEvents` | [!UICONTROL 配列] / [[!UICONTROL mediaEvent]](../../data-types/media-event-information.md) | メディアコレクション内のコンテンツのダウンロードを追跡するイベント。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、 [パブリック XDM リポジトリ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-media-analytics.schema.json)
