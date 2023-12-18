---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；検索；データ型；データ型；
solution: Experience Platform
title: データタイプを検索
description: Search Experience Data Model(XDM) データタイプについて説明します。
exl-id: 9893cb67-b0c7-4f91-a0d4-96f7b87d9510
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '160'
ht-degree: 18%

---

# [!UICONTROL 検索] データタイプ

[!UICONTROL 検索] は、Web 検索アクティビティに関する情報が含まれる標準の Experience Data Model(XDM) データ型です。

<img src="../images/data-types/search.PNG" width="500" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `isPaid` | ブール値 | 検索が有料かどうかを示すために使用します。 |
| `keywords` | 文字列 | 検索のキーワード。 |
| `pageDepth` | 整数 | 検索結果のページの深さ。 |
| `position` | 整数 | 検索結果ページでのリストの位置またはランク。 |
| `searchEngine` | 文字列 | 検索で使用される検索エンジン。 |
| `searchEngineID` | 文字列 | 検索エンジンの識別に使用されるアプリケーション固有の識別子。 |
| `slot` | 文字列 | 検索結果が表示されたページの名前付きセクション。 このプロパティの値は、定義した既知の列挙値（例： ）の 1 つと等しい必要があります `top`, `side`または `bottom`. |

{style="table-layout:auto"}

データタイプについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力された例](https://github.com/adobe/xdm/blob/master/components/datatypes/search.example.1.json)
* [完全なスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/search.schema.json)
