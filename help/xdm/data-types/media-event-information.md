---
title: メディアイベント情報のデータタイプ
description: メディアイベント情報エクスペリエンスデータモデル（XDM）データタイプについて説明します。
exl-id: 91bb7f28-b629-4044-b687-768c545ac8a2
source-git-commit: b81afb8f6c4eaedb19a58b6fe3896286f1486804
workflow-type: tm+mt
source-wordcount: '107'
ht-degree: 5%

---

# [!UICONTROL &#x200B; メディアイベント情報 &#x200B;] データタイプ

[!UICONTROL &#x200B; メディアイベント情報 &#x200B;] は、エクスペリエンスイベントに関連するメディアの詳細情報を記述する、標準の Experience Data Model （XDM）データタイプです。

![ メディアイベント情報データタイプの図。](../images/data-types/media-event-information.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `mediaCollection` | [!UICONTROL mediaDetails] | メディアは、エクスペリエンスイベントに関連する詳細情報です。 このデータタイプは、[ メディアデータ収集 ](./media-collection-details.md) と [ メディアデータレポート ](./media-reporting-details.md) の両方に使用されます。 |
| `mediaEventTimestamp` | [!UICONTROL 文字列] | メディア・イベントが発生した時刻。 |
| `mediaEventType` | [!UICONTROL 文字列] | メディアイベントタイプ。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、[ 公開 XDM リポジトリ ](https://github.com/adobe/xdm/blob/master/components/datatypes/mediaevent.schema.json) を参照してください。
