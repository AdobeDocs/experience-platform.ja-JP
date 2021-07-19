---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；Webページの詳細；データ型；データ型；データ型；Webページ
solution: Experience Platform
title: Webページの詳細データタイプ
topic-legacy: overview
description: このドキュメントでは、Webページの詳細に関するエクスペリエンスデータモデル(XDM)データタイプの概要を説明します。
exl-id: 31108e57-d416-485b-a6c3-4ebc4f5b1152
source-git-commit: 12c3f440319046491054b3ef3ec404798bb61f06
workflow-type: tm+mt
source-wordcount: '411'
ht-degree: 11%

---

# [!UICONTROL Webページの詳細デ] ータタイプ

[!UICONTROL Webページの詳] 細は、ExperienceEventによって記録される、読み込まれて表示されたWebページに関する詳細を記述する標準のExperience Data Model(XDM)データ型です。

このデータ型は、シングルページWebアプリケーション(SPA)の完全なページの詳細と最初のページ読み込みを目的としています。 読み込まれたページで発生するインタラクションで、新しいページ読み込みをトリガーしない場合は、[Webインタラクション](./web-interaction.md)データ型を参照してください。

<img src="../images/data-types/web-page-details.PNG" width="500" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `pageViews` | [[!UICONTROL 測定]](./measure.md) | Webページのビュー数。 |
| `URL` | 文字列 | Webページの標準URLまたは通常のURL。 これは、ページに到達するために実際に使用されるURLである場合とそうでない場合があります。 ページに到達するために使用するURLを記録するには、`webLink`を使用します。 URI形式は、[RFC 3986](https://tools.ietf.org/html/rfc3986)標準に従う必要があります。 |
| `isErrorPage` | Boolean | このプロパティは、ページがエラーページであるかどうかを示すフラグを使用します。 このフラグは、Web インタラクションを広範に分類するために使用されます。エラーはアプリケーションによって定義され、HTTPエラーコードで提供されるページに対応できます。 |
| `isHomePage` | Boolean | このプロパティは、ページがホームページであるかどうかを示すフラグを使用します。 このフラグは、Web インタラクションを広範に分類するために使用されます。ホームページの定義は、アプリケーションによって決まります。 |
| `name` | 文字列 | Webページの規範的な名前。 この名前は、必ずしもページタイトルやページコンテンツに直接関連付けるわけではありませんが、分類のためにサイトのページを整理するために使用されます。 |
| `server` | 文字列 | Web ページをホストする標準または通常のサーバー。  これは、実際にページインタラクションを提供したホストまたはサーバーである場合とそうでない場合があります。 |
| `siteSection` | 文字列 | このWebページが存在するサイトセクションの規範的な名前。 これは、インタラクションを分類または分類するために使用できます。 |
| `viewName` | 文字列 | ページ内のビューの名前。 このプロパティは、通常、ページレイアウトの大部分を変更するタブやコントロールを持つシングルページアプリケーションやページで使用されます。 |

{style=&quot;table-layout:auto&quot;}

データタイプについて詳しくは、パブリックXDMリポジトリを参照してください。

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/datatypes/deprecated/webpagedetails.example.2.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/deprecated/webpagedetails.schema.json)
