---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；XDM；フィールド；スキーマ；スキーマ；検索；データタイプ；データタイプ；データタイプ；
solution: Experience Platform
title: データタイプを検索
description: 検索エクスペリエンスデータモデル（XDM）データタイプについて説明します。
exl-id: 9893cb67-b0c7-4f91-a0d4-96f7b87d9510
source-git-commit: e028fbb82b37b3940b308a860c26f8b5f9884d3a
workflow-type: tm+mt
source-wordcount: '164'
ht-degree: 18%

---

# [!UICONTROL &#x200B; 検索 &#x200B;] データタイプ

[!UICONTROL &#x200B; 検索 &#x200B;] は、web 検索アクティビティに関する情報を含んだ標準の Experience Data Model （XDM）データタイプです。

![&#x200B; 検索画像 &#x200B;](../images/data-types/search.PNG){width=500}

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

* [&#x200B; 入力された例 &#x200B;](https://github.com/adobe/xdm/blob/master/components/datatypes/search.example.1.json)
* [&#x200B; 完全なスキーマ &#x200B;](https://github.com/adobe/xdm/blob/master/components/datatypes/search.schema.json)
