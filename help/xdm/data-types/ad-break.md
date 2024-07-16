---
title: 広告ブレーク データタイプ
description: 広告ブレーク Experience Data Model （XDM）データタイプについて説明します。
exl-id: dfe0c386-8459-440d-95b5-b2139fac0fc3
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '102'
ht-degree: 21%

---

# [!UICONTROL  広告ブレーク ] データタイプ

[!UICONTROL  広告ブレーク ] は、タイムド広告がタイムドメディアにどのように挿入されるかを説明する、標準の Experience Data Model （XDM）データタイプです。

![ データタイプ構造 ](../images/data-types/ad-break.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `_dc.title` | 文字列 | 広告ブレークのわかりやすい名前。 |
| `_id` | 文字列 | 広告ブレークの一意の ID。 |
| `offset` | 整数 | プライマリコンテンツの開始時からの広告ブレークのオフセット (秒)。 |

{style="table-layout:auto"}

データタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/components/datatypes/marketing/advertising-break.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/components/datatypes/marketing/advertising-break.schema.json)
