---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；Web ページの詳細；データタイプ；データタイプ；データタイプ；Web ページ
solution: Experience Platform
title: Web 情報データタイプ
description: Web 情報エクスペリエンスデータモデル（XDM）データタイプについて説明します。
exl-id: bfb00835-5908-4baf-af2a-6d845710e340
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '166'
ht-degree: 2%

---

# [!UICONTROL Web 情報 &#x200B;] データタイプ

[!UICONTROL Web 情報 &#x200B;] は、Web ページ、リファラー、オンページインタラクションに関連するリンクなど、World Wide Web チャネル固有のエクスペリエンスイベントを介して記録された情報を記述する、標準のエクスペリエンスデータモデル（XDM）データタイプです。

![](../images/data-types/web-information.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `webInteraction` | [[!UICONTROL Web インタラクション &#x200B;]](./web-interaction.md) | インタラクションに対応する web リンクまたは URL に関する詳細を説明します。 |
| `webPageDetails` | [[!UICONTROL Web ページの詳細 &#x200B;]](./webpage-details.md) | Web インタラクションが発生した Web ページに関する詳細を説明します。 |
| `webReferrer` | [!UICONTROL &#x200B; オブジェクト &#x200B;] | Web インタラクションのリファラーを表します。これは、現在の Web インタラクションが記録される直前に訪問者が訪問した URL です。 次のサブプロパティが含まれます。 <ul><li>`URL`: リファラー URL。</li><li>`type`: リファラータイプ。</li></ul> |

{style="table-layout:auto"}

データタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [&#x200B; 入力された例 &#x200B;](https://github.com/adobe/xdm/blob/master/components/datatypes/webinfo.example.1.json)
* [&#x200B; 完全なスキーマ &#x200B;](https://github.com/adobe/xdm/blob/master/components/datatypes/webinfo.schema.json)
