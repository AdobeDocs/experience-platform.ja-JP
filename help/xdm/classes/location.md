---
title: 場所クラス
description: エクスペリエンスデータモデル（XDM）の Location クラスについて説明します。
badgePrivateBeta: label="Private Beta" type="Informative"
hide: true
hidefromtoc: true
source-git-commit: e01a6cd4cf42338f87222a5e2871cd6c3683309a
workflow-type: tm+mt
source-wordcount: '148'
ht-degree: 8%

---

# [!UICONTROL Location] クラス

エクスペリエンスデータモデル（XDM）では、[!UICONTROL  場所 ] クラスは、トラベルホールやスポーツアリーナなど、ライブイベントの場所情報をキャプチャします。

![Location クラス構造 ](../images/classes/location.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL 識別子] | `_id` | [!UICONTROL 文字列] | レコードに対してシステムで生成された一意の文字列識別子。 このフィールドは、個々のレコードの一意性を追跡、データの重複を防ぎ、ダウンストリームのサービスでそのレコードを検索するために使用されます。<br><br> このフィールドはシステムで生成されるので、データ取り込み時に明示的な値を指定する必要はありません。 ただし、必要に応じて、独自の一意の ID 値を指定することもできます。 |
| [!UICONTROL  場所の識別子 ] | `locationID` | [!UICONTROL 文字列] | 場所の一意の ID。 |
| [!UICONTROL  場所名 ] | `locationName` | [!UICONTROL 文字列] | 場所の名前。 |

クラスを [[!UICONTROL Location] フィールドグループ ](../field-groups/location/healthcare-location.md) で拡張して、場所に関する詳細を記述できます。
