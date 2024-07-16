---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；Web ページの詳細；データタイプ；データタイプ；データタイプ；Web ページ
solution: Experience Platform
title: Web ページの詳細データタイプ
description: Web ページの詳細についてエクスペリエンスデータモデル（XDM）データタイプを説明します。
exl-id: 31108e57-d416-485b-a6c3-4ebc4f5b1152
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '383'
ht-degree: 11%

---

# [!UICONTROL Web ページの詳細 ] データタイプ

[!UICONTROL Web ページの詳細 ] は、標準のエクスペリエンスデータモデル（XDM）データタイプで、ExperienceEvent によって記録されたように、読み込まれて表示された web ページに関する詳細を記述するものです。

データタイプは、シングルページ web アプリケーション（SPA）のページの完全な詳細と初期ページの読み込みを目的としています。 読み込まれたページで発生し、新しいページ読み込みをトリガーしないインタラクションについては、[web インタラクション ](./web-interaction.md) データタイプを参照してください。

<img src="../images/data-types/web-page-details.PNG" width="500" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `pageViews` | [[!UICONTROL 測定]](./measure.md) | Web ページのビュー数。 |
| `URL` | 文字列 | 基準となる、または通常の web ページの URL。 これは、ページへの到達に使用された実際の URL である場合とそうでない場合があります。 ページに到達するために使用された URL を記録するには、`webLink` を使用します。 URI 形式は、[RFC 3986](https://tools.ietf.org/html/rfc3986) 標準に従う必要があります。 |
| `isErrorPage` | ブール値 | このプロパティは、ページがエラーページかどうかを示すフラグを使用します。 このフラグは、web インタラクションを大まかに分類するために使用されます。 エラーはアプリケーションによって定義され、HTTP エラーコードで提供されるページに対応できます。 |
| `isHomePage` | ブール値 | このプロパティは、ページがホームページかどうかを示すフラグを使用します。 このフラグは、web インタラクションを大まかに分類するために使用されます。 ホームページの定義は、アプリケーションによって決まります。 |
| `name` | 文字列 | Web ページの基準となる名前。この名前は、必ずしもページのタイトルではなく、またページコンテンツと直接関連付けられてもいませんが、分類目的でサイトのページを整理するために使用されます。 |
| `server` | 文字列 | Web ページをホストする、基準となる、または通常のサーバー。 ページインタラクションを実際に提供したホストやサーバーである場合もあれば、そうでない場合もあります。 |
| `siteSection` | 文字列 | この Web ページが存在するサイトセクションの基準となる名前。 これを使用して、インタラクションを分類または分類できます。 |
| `viewName` | 文字列 | ページ内のビューの名前。 このプロパティは、通常、ページ レイアウトの大部分を変更するタブまたはコントロールを持つ単一ページ アプリケーションまたはページで使用されます。 |

{style="table-layout:auto"}

データタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/components/datatypes/deprecated/webpagedetails.example.2.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/components/datatypes/deprecated/webpagedetails.schema.json)
