---
title: MediaAnalytics インタラクション詳細スキーマフィールドグループ
description: MediaAnalytics インタラクション詳細スキーマフィールドグループについて説明します。
exl-id: 1096d28a-5796-49cc-bd45-b3f5188f699e
source-git-commit: b81afb8f6c4eaedb19a58b6fe3896286f1486804
workflow-type: tm+mt
source-wordcount: '233'
ht-degree: 1%

---

# [!UICONTROL MediaAnalytics インタラクションの詳細] スキーマフィールドグループ

[!UICONTROL MediaAnalytics インタラクションの詳細] は、 [[!DNL XDM ExperienceEvent] クラス](../../classes/experienceevent.md). このフィールドグループを使用して、様々なプラットフォームやチャネルにわたるメディアコンテンツとのやり取りを包括的に監視および分析する、エンリッチメントされたデータフィールドをキャプチャします。

![のスキーマ図 [!UICONTROL MediaAnalytics インタラクションの詳細] スキーマフィールドグループを使用します。](../../images/field-groups/mediaanalytics-interaction.png)

| 表示名 | プロパティ | データタイプ | 説明 |
|---| --- | --- | --- |
| [!UICONTROL メディアコレクションの詳細] | `mediaCollection` | [[!UICONTROL メディアコレクションの詳細]](../../data-types/media-collection-details.md) | メディア項目のコレクションに関連する属性。 メディアコレクションフィールドを使用してデータをキャプチャし、他のAdobe サービスに送信してさらに処理します。 |
| [!UICONTROL メディアレポートの詳細] | `mediaReporting` | [[!UICONTROL メディアレポートの詳細]](../../data-types/media-reporting-details.md) | メディアコンテンツに関連付けられているレポートの詳細と指標。 *メディアレポートフィールドは、Adobe サービスが送信したメディアコレクションフィールドを分析するために使用します。 このデータは、他の特定のユーザー指標と共に、計算され、レポートされます。 |
| [!UICONTROL メディアコレクションダウンロードされたコンテンツイベントのリスト] | `mediaDownloadedEvents` | [!UICONTROL 配列] / [[!UICONTROL mediaEvent]](../../data-types/media-event-information.md) | メディアコレクション内のコンテンツのダウンロードを追跡するイベント。 |

{style="table-layout:auto"}

>[!TIP]
>
>Media Edge API で使用されていないフィールドを非表示にできます。 これらのフィールドを非表示にすると、スキーマが読み取りやすく、理解しやすくなりますが、必須ではありません。 これらのフィールドは、 [!UICONTROL MediaAnalytics インタラクションの詳細] fieldgroup. Platform UI の読みやすさを向上させるには、 [未使用フィールドを非表示にする方法に関する Media Analytics ドキュメント](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/edge-recommended/media-edge-sdk/implementation-edge.html#set-up-the-schema-in-adobe-experience-platform).

<!-- 
>[!NOTE]
>
>Schemas contain fields that are not used in every context or situation. They provide a potential blueprint to map an object. Schemas displayed for the Media Edge API Collection or Reporting data types only portray the relevant fields. You can manually select and deselect the fields that you want to use if you intend to use a schema for the Media Edge API interaction. You can find instructions on [hiding unnecessary fields](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/edge-recommended/media-edge-sdk/implementation-edge.html#set-up-the-schema-in-adobe-experience-platform) in the guide to install Media Analytics with Experience Platform Edge.
 -->
