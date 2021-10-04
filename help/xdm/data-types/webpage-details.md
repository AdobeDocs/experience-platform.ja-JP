---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；Web ページの詳細；データ型；データ型；データ型；Web ページ
solution: Experience Platform
title: Web ページの詳細データ型
topic-legacy: overview
description: このドキュメントでは、Web ページの詳細に関するエクスペリエンスデータモデル (XDM) データタイプの概要を説明します。
exl-id: 31108e57-d416-485b-a6c3-4ebc4f5b1152
source-git-commit: 12c3f440319046491054b3ef3ec404798bb61f06
workflow-type: tm+mt
source-wordcount: '411'
ht-degree: 11%

---

# [!UICONTROL Web ページの詳] 細データ型

[!UICONTROL Web ページの詳] 細は、ExperienceEvent で記録される、読み込まれて表示された Web ページに関する詳細を説明する標準のエクスペリエンスデータモデル (XDM) データ型です。

このデータ型は、単一ページ Web アプリケーション (SPA) の完全なページの詳細と最初のページ読み込みを目的としています。 読み込まれたページで発生し、新しいページ読み込みをトリガーしないインタラクションについては、[Web インタラクション ](./web-interaction.md) データ型を参照してください。

<img src="../images/data-types/web-page-details.PNG" width="500" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `pageViews` | [[!UICONTROL 測定]](./measure.md) | Web ページのビュー数。 |
| `URL` | 文字列 | Web ページの標準または通常の URL。 これは、ページに到達するために実際に使用される URL である場合とそうでない場合があります。 ページに到達するために使用した URL を記録するには、`webLink` を使用します。 URI 形式は、[RFC 3986](https://tools.ietf.org/html/rfc3986) 標準に従う必要があります。 |
| `isErrorPage` | Boolean | このプロパティは、ページがエラーページであるかどうかを示すフラグを使用します。 このフラグは、web インタラクションを広範に分類するために使用されます。エラーはアプリケーションによって定義され、HTTP エラーコードで提供されるページに対応できます。 |
| `isHomePage` | Boolean | このプロパティは、ページがホームページであるかどうかを示すフラグを使用します。 このフラグは、web インタラクションを広範に分類するために使用されます。ホームページの定義は、アプリケーションによって決まります。 |
| `name` | 文字列 | Web ページの規範的な名前。 この名前は、必ずしもページタイトルやページコンテンツに直接関連付けるわけではありませんが、分類のためにサイトのページを整理するために使用されます。 |
| `server` | 文字列 | Web ページをホストする標準または通常のサーバー。  これは、実際にページの操作を提供したホストまたはサーバーである場合とそうでない場合があります。 |
| `siteSection` | 文字列 | この Web ページが存在するサイトセクションの規範的な名前。 これは、インタラクションを分類または分類するために使用できます。 |
| `viewName` | 文字列 | ページ内のビューの名前。 このプロパティは、通常、ページレイアウトの大部分を変更するタブやコントロールを持つ単一ページアプリケーションやページで使用されます。 |

{style=&quot;table-layout:auto&quot;}

データタイプについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力例](https://github.com/adobe/xdm/blob/master/components/datatypes/deprecated/webpagedetails.example.2.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/deprecated/webpagedetails.schema.json)
