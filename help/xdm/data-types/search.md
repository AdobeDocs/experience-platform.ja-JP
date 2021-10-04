---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；検索；データ型；データ型；
solution: Experience Platform
title: 検索データタイプ
topic-legacy: overview
description: このドキュメントでは、Search Experience Data Model(XDM) データタイプの概要を説明します。
exl-id: 9893cb67-b0c7-4f91-a0d4-96f7b87d9510
source-git-commit: 39d04cf482e862569277211d465bb2060a49224a
workflow-type: tm+mt
source-wordcount: '184'
ht-degree: 17%

---

#  Searchdata type

 検索は、Web 検索アクティビティに関する情報を含む、標準的な Experience Data Model(XDM) データ型です。

<img src="../images/data-types/search.PNG" width="500" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `isPaid` | Boolean | 検索が有料かどうかを示すために使用します。 |
| `keywords` | 文字列 | 検索のキーワード。 |
| `pageDepth` | 整数 | 検索結果のページの深さ。 |
| `position` | 整数 | 検索結果ページでのリストの位置またはランク。 |
| `searchEngine` | 文字列 | 検索で使用される検索エンジン。 |
| `searchEngineID` | 文字列 | 検索エンジンの識別に使用されるアプリケーション固有の識別子。 |
| `slot` | 文字列 | 検索結果が表示されたページの名前付きセクション。 このプロパティの値は、`top`、`side`、`bottom` など、定義済みの既知の列挙値の 1 つと等しい必要があります。 |

{style=&quot;table-layout:auto&quot;}

データタイプについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力例](https://github.com/adobe/xdm/blob/master/components/datatypes/search.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/search.schema.json)
