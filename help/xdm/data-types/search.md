---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；XDM；フィールド；スキーマ；スキーマ；検索；データタイプ；データタイプ；データタイプ；
solution: Experience Platform
title: データタイプを検索
description: 検索エクスペリエンスデータモデル（XDM）データタイプについて説明します。
exl-id: 9893cb67-b0c7-4f91-a0d4-96f7b87d9510
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '160'
ht-degree: 18%

---

# [!UICONTROL  検索 ] データタイプ

[!UICONTROL  検索 ] は、web 検索アクティビティに関する情報を含んだ標準の Experience Data Model （XDM）データタイプです。

<img src="../images/data-types/search.PNG" width="500" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `isPaid` | ブール値 | 検索が有料かどうかを示すために使用されます。 |
| `keywords` | 文字列 | 検索のキーワード。 |
| `pageDepth` | 整数 | 検索結果のページの深さ。 |
| `position` | 整数 | 検索結果ページのリストの位置またはランク。 |
| `searchEngine` | 文字列 | 検索で使用される検索エンジン。 |
| `searchEngineID` | 文字列 | 検索エンジンの識別に使用されるアプリケーション固有の識別子。 |
| `slot` | 文字列 | 検索結果が表示されるページの名前付きセクション。 このプロパティの値は、定義した既知の列挙値（`top`、`side`、`bottom` など）の 1 つと等しくする必要があります。 |

{style="table-layout:auto"}

データタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/components/datatypes/search.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/components/datatypes/search.schema.json)
