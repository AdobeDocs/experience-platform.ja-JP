---
title: Commerce Scope データタイプ
description: Commerce Scope Experience Data Model （XDM）データタイプについて説明します。
exl-id: c2888c3a-a49c-43c4-8d36-0a485cb76a58
source-git-commit: 8be502c9eea67119dc537a5d63a6c71e0bff1697
workflow-type: tm+mt
source-wordcount: '125'
ht-degree: 8%

---

# [!UICONTROL Commerce スコープ &#x200B;] データ型

[!UICONTROL Commerce スコープ &#x200B;] は、コマースエコシステム内でイベントが発生した場所の識別子を定義する、標準のエクスペリエンスデータモデル（XDM）データタイプです。 環境、web サイト、ストア、ストアビューを区別します。

![Commerce Scope データタイプの図。](../images/data-types/commerce-scope.png)

| 表示名 | プロパティ | データタイプ | 説明 |
|---------------------------------|-------------------|-----------|-------------------------------------------------------|
| [!UICONTROL &#x200B; 環境 ID] | `environmentID` | 文字列 | 環境 ID。 32 桁の英数字 ID。 |
| [!UICONTROL Web サイトコード &#x200B;] | `websiteCode` | 文字列 | 環境内の一意の Web サイトコード。 |
| [!UICONTROL &#x200B; ストアコード &#x200B;] | `storeCode` | 文字列 | Web サイト内の一意のストアコード。 |
| [!UICONTROL &#x200B; ストア表示コード &#x200B;] | `storeViewCode` | 文字列 | ストア内の一意のストア表示コード。 |

{style="table-layout:auto"}

データタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [&#x200B; 入力された例 &#x200B;](https://github.com/adobe/xdm/blob/master/components/datatypes/commercescope.example.1.json)
* [&#x200B; 完全なスキーマ &#x200B;](https://github.com/adobe/xdm/blob/master/components/datatypes/commercescope.schema.json)
