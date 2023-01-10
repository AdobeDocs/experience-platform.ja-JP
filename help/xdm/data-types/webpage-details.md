---
keywords: Experience Platform；ホーム；人気の高いトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；Web ページの詳細；データ型；データ型；データ型；web ページ
solution: Experience Platform
title: Web ページの詳細データタイプ
description: このドキュメントでは、Web ページの詳細に関するエクスペリエンスデータモデル (XDM) データタイプの概要を説明します。
exl-id: 31108e57-d416-485b-a6c3-4ebc4f5b1152
source-git-commit: 60c0bd62b4effaa161c61ab304718ab8c20a06e1
workflow-type: tm+mt
source-wordcount: '411'
ht-degree: 11%

---

# [!UICONTROL Web ページの詳細] データタイプ

[!UICONTROL Web ページの詳細] は、ExperienceEvent によって記録される、読み込まれて表示された Web ページに関する詳細を記述する標準 Experience Data Model(XDM) データ型です。

このデータ型は、単一ページ Web アプリケーション (SPA) の完全なページの詳細と最初のページ読み込みを目的としています。 読み込まれたページで新しいページの読み込みをトリガーしないインタラクションが発生する場合は、 [web インタラクション](./web-interaction.md) データタイプ。

<img src="../images/data-types/web-page-details.PNG" width="500" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `pageViews` | [[!UICONTROL 測定]](./measure.md) | Web ページのビュー数。 |
| `URL` | 文字列 | Web ページの基準となる URL または通常の URL。 これは、ページに到達するために使用された実際の URL である場合とそうでない場合があります。 ページに到達するために使用した URL を記録するには、以下を使用します。 `webLink`. URI 形式は、 [RFC 3986](https://tools.ietf.org/html/rfc3986) 標準。 |
| `isErrorPage` | ブール値 | このプロパティは、ページがエラーページかどうかを示すフラグを使用します。 このフラグは、web インタラクションを広範に分類するために使用されます。エラーはアプリケーションによって定義され、HTTP エラーコードで提供されるページに対応できます。 |
| `isHomePage` | ブール値 | このプロパティは、ページがホームページかどうかを示すフラグを使用します。 このフラグは、web インタラクションを広範に分類するために使用されます。ホームページの定義は、アプリケーションによって決まります。 |
| `name` | 文字列 | Web ページの基準となる名前。 この名前は、必ずしもページタイトルやページコンテンツに直接関連付けられるわけではありませんが、分類目的でサイトのページを整理するために使用されます。 |
| `server` | 文字列 | Web ページをホストする標準または通常のサーバー。  実際にページインタラクションを提供したホストまたはサーバーである場合とそうでない場合があります。 |
| `siteSection` | 文字列 | この Web ページが存在するサイトセクションの基準となる名前。 これは、インタラクションを分類または分類するために使用できます。 |
| `viewName` | 文字列 | ページ内のビューの名前。 このプロパティは、通常、ページレイアウトの大部分を変更するタブやコントロールを持つシングルページアプリケーションやページで使用されます。 |

{style=&quot;table-layout:auto&quot;}

データタイプについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力された例](https://github.com/adobe/xdm/blob/master/components/datatypes/deprecated/webpagedetails.example.2.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/deprecated/webpagedetails.schema.json)
