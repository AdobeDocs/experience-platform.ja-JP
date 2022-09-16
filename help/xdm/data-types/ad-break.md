---
title: 広告ブレークのデータタイプ
description: このドキュメントでは、広告ブレークエクスペリエンスデータモデル (XDM) データタイプの概要を説明します。
exl-id: dfe0c386-8459-440d-95b5-b2139fac0fc3
source-git-commit: 2fd35c4ac29f43391f9dc03c636d20558b701be7
workflow-type: tm+mt
source-wordcount: '128'
ht-degree: 7%

---

# [!UICONTROL 広告ブレーク] データタイプ

[!UICONTROL 広告ブレーク] は、時間指定広告が時間指定メディアに挿入される方法を示す標準的な Experience Data Model(XDM) データ型です。

![データタイプの構造](../images/data-types/ad-break.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `_dc.title` | 文字列 | 広告ブレークのわかりやすい名前。 |
| `_id` | 文字列 | 広告ブレークの一意の識別子。 |
| `offset` | 整数 | プライマリコンテンツの開始時からの広告ブレークのオフセット（秒）。 |

{style=&quot;table-layout:auto&quot;}

データタイプについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力された例](https://github.com/adobe/xdm/blob/master/components/datatypes/marketing/advertising-break.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/marketing/advertising-break.schema.json)
