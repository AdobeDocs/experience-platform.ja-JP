---
title: MediaAnalytics インタラクションの詳細スキーマフィールドグループ
description: MediaAnalytics インタラクションの詳細スキーマフィールドグループについて説明します。
exl-id: 1096d28a-5796-49cc-bd45-b3f5188f699e
source-git-commit: fded2f25f76e396cd49702431fa40e8e4521ebf8
workflow-type: tm+mt
source-wordcount: '234'
ht-degree: 2%

---

# [!UICONTROL MediaAnalytics インタラクションの詳細 &#x200B;] スキーマフィールドグループ

[!UICONTROL MediaAnalytics インタラクションの詳細 &#x200B;] は、[[!DNL XDM ExperienceEvent]  クラス ](../../classes/experienceevent.md) の標準スキーマフィールドグループです。 このフィールドグループを使用すると、様々なプラットフォームやチャネルをまたいでメディアコンテンツとのインタラクションを包括的に監視および分析する、強化されたデータフィールドをキャプチャできます。

![MediaAnalytics インタラクションの詳細 [!UICONTROL &#x200B; スキーマフィールドグループ &#x200B;] スキーマ図 ](../../images/field-groups/mediaanalytics-interaction.png)

| 表示名 | プロパティ | データタイプ | 説明 |
|---| --- | --- | --- |
| [!UICONTROL &#x200B; メディアコレクションの詳細 &#x200B;] | `mediaCollection` | [[!UICONTROL &#x200B; メディアコレクションの詳細 &#x200B;]](../../data-types/media-collection-details.md) | メディア項目のコレクションに関連する属性。 Media Collection フィールドを使用してデータをキャプチャし、他のAdobe サービスに送信してさらに処理を行います。 |
| [!UICONTROL &#x200B; メディアレポートの詳細 &#x200B;] | `mediaReporting` | [[!UICONTROL &#x200B; メディアレポートの詳細 &#x200B;]](../../data-types/media-reporting-details.md) | メディアコンテンツに関連付けられているレポートの詳細と指標。 * メディアレポートフィールドは、Adobe サービスが送信したメディアコレクションフィールドを分析するためにユーザーが使用します。 このデータは、他の特定のユーザー指標と共に計算され、レポートされます。 |
| [!UICONTROL &#x200B; メディアコレクションでダウンロードされたコンテンツイベントのリスト &#x200B;] | `mediaDownloadedEvents` | [[!UICONTROL mediaEvent]](../../data-types/media-event-information.md) の [!UICONTROL &#x200B; 配列 &#x200B;] | メディアコレクション内のコンテンツのダウンロードをトラッキングするイベント。 |

{style="table-layout:auto"}

>[!TIP]
>
>Media Edge API で使用されないフィールドを非表示にできます。 これらのフィールドを非表示にすると、スキーマが読みやすく理解しやすくなりますが、必須ではありません。 これらのフィールドは、[!UICONTROL MediaAnalytics インタラクションの詳細 &#x200B;] フィールドグループのフィールドのみを参照します。 Experience Platform UI での読みやすくするには、[Media Analytics ドキュメントで未使用フィールドを非表示にする方法 ](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/edge-recommended/media-edge-sdk/implementation-edge.html#set-up-the-schema-in-adobe-experience-platform) の手順に従います。

<!-- 
>[!NOTE]
>
>Schemas contain fields that are not used in every context or situation. They provide a potential blueprint to map an object. Schemas displayed for the Media Edge API Collection or Reporting data types only portray the relevant fields. You can manually select and deselect the fields that you want to use if you intend to use a schema for the Media Edge API interaction. You can find instructions on [hiding unnecessary fields](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/edge-recommended/media-edge-sdk/implementation-edge.html#set-up-the-schema-in-adobe-experience-platform) in the guide to install Media Analytics with Experience Platform Edge.
 -->
