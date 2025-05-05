---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；XDM；フィールド；スキーマ；スキーマ；Web インタラクション；データタイプ；データタイプ；データタイプ；
solution: Experience Platform
title: Web インタラクションデータタイプ
description: Web インタラクションエクスペリエンスデータモデル（XDM）データタイプについて説明します。
exl-id: 772d96c5-9fa3-4fed-8b38-16b8e7101743
source-git-commit: e028fbb82b37b3940b308a860c26f8b5f9884d3a
workflow-type: tm+mt
source-wordcount: '169'
ht-degree: 10%

---

# [!UICONTROL Web インタラクション &#x200B;] データタイプ

[!UICONTROL Web インタラクション &#x200B;] は、最初のページ読み込みが完了した後に Web ページで発生したインタラクションに関する情報を記述する、標準のエクスペリエンスデータモデル（XDM）データタイプです。 単一ページ web アプリ（SPA）など、新しいページ読み込みをトリガーとしないリッチ web アプリケーションでのインタラクションを記録することを目的としています。

![web インタラクション画像 ](../images/data-types/web-interaction.PNG){width=500}

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `linkClicks` | [[!UICONTROL 測定]](./measure.md) | Web リンクのクリックをトラッキングする測定値。 |
| `URL` | 文字列 | この Web インタラクションに使用される実際のリンクまたは URL。 |
| `name` | 文字列 | この Web リンクに使用される基準となる名前。 これは分類目的で使用されます。 |
| `type` | 文字列 | リンクタイプ。 このプロパティは、次の列挙値のいずれかに等しい必要があります： <li> `download` </li> <li> `exit` </li> <li> `other` </li> |

{style="table-layout:auto"}

データタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/components/datatypes/deprecated/webinteraction.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/components/datatypes/deprecated/webinteraction.schema.json)
