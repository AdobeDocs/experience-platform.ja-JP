---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ;スキーマ;XDM；フィールド；スキーマ;スキーマ;Webページの詳細；データ型；データ型；Webページ
solution: Experience Platform
title: Webページの詳細データ型
topic-legacy: overview
description: このドキュメントでは、Webページの詳細に関するExperience Data Model(XDM)データ型の概要を説明します。
exl-id: 31108e57-d416-485b-a6c3-4ebc4f5b1152
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '408'
ht-degree: 10%

---

# [!UICONTROL Webページの] 詳細データ型

[!UICONTROL Webページの] 詳細は、ExperienceEventによって記録され、読み込まれて表示されたWebページの詳細を説明する、標準のExperience Data Model(XDM)データ型です。

このデータ型は、完全なページの詳細と、シングルページWebアプリケーション(SPA)の初期ページ読み込みを目的としています。 読み込まれたページで行われる操作で、新しいページの読み込みをトリガーしない場合は、[Web操作](./web-interactions.md)データ型を参照してください。

<img src="../images/data-types/web-page-details.PNG" width="500" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `pageViews` | [[!UICONTROL 測定]](./measure.md) | Webページ上の表示数。 |
| `URL` | 文字列 | Webページの標準URLまたは通常のURLです。 これは、ページへの実際のリーチに使用されるURLである場合とそうでない場合があります。 ページに到達するために使用したURLを記録するには、`webLink`を使用します。 URI形式は、[RFC 3986](https://tools.ietf.org/html/rfc3986)標準に従う必要があります。 |
| `isErrorPage` | Boolean | このプロパティでは、ページがエラーページであるかどうかを示すフラグを使用します。 このフラグは、Web インタラクションを広範に分類するために使用されます。エラーはアプリケーションによって定義され、HTTPエラーコードで提供されるページに対応する場合があります。 |
| `isHomePage` | ブール値 | このプロパティでは、ページがホームページかどうかを示すフラグを使用します。 このフラグは、Web インタラクションを広範に分類するために使用されます。ホームページの定義は、アプリケーションによって決まります。 |
| `name` | 文字列 | Webページの標準名です。 この名前は、ページタイトルやページコンテンツに直接関連付けられるわけではありませんが、分類のためにサイトのページを整理するために使用されます。 |
| `server` | 文字列 | Web ページをホストする標準または通常のサーバー。  これは、実際にページの操作を提供したホストまたはサーバーである場合とそうでない場合があります。 |
| `siteSection` | 文字列 | このWebページが存在するサイトセクションの標準名です。 これは、インタラクションを分類または分類するために使用できます。 |
| `viewName` | 文字列 | ページ内の表示の名前。 このプロパティは、通常、ページレイアウトの大部分を変更するタブやコントロールを持つ単一ページのアプリ、またはページで使用されます。 |

データ型の詳細については、パブリックXDMリポジトリを参照してください。

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/datatypes/web/webpagedetails.example.2.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/web/webpagedetails.schema.json)
