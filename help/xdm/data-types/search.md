---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ;スキーマ;XDM；フィールド；スキーマ;スキーマ；検索；データ型；データ型；
solution: Experience Platform
title: Search Data Type
topic-legacy: overview
description: このドキュメントでは、Search Experience Data Model(XDM)データタイプの概要を説明します。
exl-id: 9893cb67-b0c7-4f91-a0d4-96f7b87d9510
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '181'
ht-degree: 16%

---

# [!UICONTROL Searchdata] 型

 Searchは、Web検索アクティビティに関する情報を含む標準的なExperience Data Model(XDM)データ型です。

<img src="../images/data-types/search.PNG" width="500" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `isPaid` | Boolean | 検索が有料かどうかを示すために使用します。 |
| `keywords` | 文字列 | 検索のキーワード。 |
| `pageDepth` | 整数 | 検索結果のページの深さ。 |
| `position` | 整数 | 検索結果ページのリストの位置またはランク。 |
| `searchEngine` | 文字列 | 検索で使用される検索エンジン。 |
| `searchEngineID` | 文字列 | 検索エンジンの識別に使用されるアプリケーション固有の識別子。 |
| `slot` | 文字列 | 検索結果が表示されたページの名前付きセクション。 このプロパティの値は、`top`、`side`、`bottom`など、定義する既知の列挙値の1つと等しくなければなりません。 |

データ型の詳細については、パブリックXDMリポジトリを参照してください。

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/datatypes/search.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/search.schema.json)
