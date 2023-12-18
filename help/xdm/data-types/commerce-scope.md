---
title: コマーススコープのデータタイプ
description: Commerce Scope Experience Data Model(XDM) データタイプについて説明します。
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '125'
ht-degree: 6%

---

# [!UICONTROL コマース範囲] データタイプ

[!UICONTROL コマース範囲] は標準の Experience Data Model(XDM) データ型です。XDM は、コマースエコシステム内でイベントが発生した場所の識別子を定義します。 環境、Web サイト、ストア、ストアの表示を区別します。

![コマーススコープのデータタイプを示す図。](../images/data-types/commerce-scope.png)

| 表示名 | プロパティ | データタイプ | 説明 |
|---------------------------------|-------------------|-----------|-------------------------------------------------------|
| [!UICONTROL 環境 ID] | `environmentID` | 文字列 | 環境 ID。 32 桁の英数字 ID。 |
| [!UICONTROL Web サイトコード] | `websiteCode` | 文字列 | 環境内の一意の Web サイトコード。 |
| [!UICONTROL ストアコード] | `storeCode` | 文字列 | Web サイト内の一意のストアコード。 |
| [!UICONTROL ストアビューコード] | `storeViewCode` | 文字列 | ストア内の一意のストア表示コード。 |

{style="table-layout:auto"}

データタイプについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力された例](https://github.com/adobe/xdm/blob/master/components/datatypes/commercescope.example.1.json)
* [完全なスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/commercescope.schema.json)
