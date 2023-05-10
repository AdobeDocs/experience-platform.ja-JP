---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；検索；データ型；データ型；
solution: Experience Platform
title: データタイプを検索
description: このドキュメントでは、Search Experience Data Model(XDM) データタイプの概要を説明します。
exl-id: 9893cb67-b0c7-4f91-a0d4-96f7b87d9510
source-git-commit: 5caa4c750c9f786626f44c3578272671d85b8425
workflow-type: tm+mt
source-wordcount: '181'
ht-degree: 16%

---

# [!UICONTROL 検索] データタイプ

[!UICONTROL 検索] は、Web 検索アクティビティに関する情報を含む標準 Experience Data Model(XDM) データ型です。

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
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/search.schema.json)
