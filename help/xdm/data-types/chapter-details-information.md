---
title: チャプター詳細情報データタイプ
description: チャプター詳細情報エクスペリエンスデータモデル (XDM) データタイプについて説明します。
source-git-commit: 65f3dcf1cacfbc4e8a598244810d238bd88f64bd
workflow-type: tm+mt
source-wordcount: '195'
ht-degree: 6%

---

# [!UICONTROL チャプターの詳細情報] データタイプ

[!UICONTROL チャプターの詳細情報] は、メディアコンテンツ内のチャプターまたはセグメントに関連する様々な属性を記述する標準 Experience Data Model(XDM) データ型です。 以下を使用します。 [!UICONTROL チャプターの詳細情報] チャプター名、期間、位置、ID、再生ステータス（開始/完了）、各チャプターの閲覧時間などの詳細を取り込むためのデータタイプ。

![「チャプター詳細情報」データ型の図です。](../images/data-types/chapter-details-information.png)

| 表示名 | プロパティ | データタイプ | 説明 |
|---------------------------|---------------|-----------|---------------------------------------------------|
| [!UICONTROL チャプター名] | `friendlyName` | 文字列 | チャプターまたはセグメントの名前。 |
| [!UICONTROL チャプターの長さまたは期間] | `length` | 整数 | **必須** チャプターの長さ（秒）。 |
| [!UICONTROL チャプターオフセット] | `offset` | 整数 | **必須** 開始時からのコンテンツ内のチャプターのオフセット（秒）。 |
| [!UICONTROL チャプター位置] | `index` | 整数 | **必須** コンテンツ内のチャプターの位置（インデックス、整数）。 |
| [!UICONTROL チャプター ID] | `ID` | 文字列 | 自動生成されたチャプターの ID。 |
| [!UICONTROL チャプター開始済み] | `isStarted` | ブール値 | チャプターが開始されたかどうか。 |
| [!UICONTROL チャプター完了] | `isCompleted` | ブール値 | チャプターが完了したかどうか。 |
| [!UICONTROL チャプター再生時間] | `timePlayed` | 整数 | チャプターの閲覧時間（秒）。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、 [パブリック XDM リポジトリ](https://github.com/adobe/xdm/blob/master/components/datatypes/chapterdetails.schema.json)
