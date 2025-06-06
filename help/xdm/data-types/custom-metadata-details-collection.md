---
title: カスタムメタデータの詳細コレクションのデータタイプ
description: カスタムメタデータ詳細収集エクスペリエンスデータモデル（XDM）データタイプについて説明します。
exl-id: e2fa65ea-b738-43c6-90f1-1968dd83b963
source-git-commit: 799a384556b43bc844782d8b67416c7eea77fbf0
workflow-type: tm+mt
source-wordcount: '111'
ht-degree: 15%

---

# [!UICONTROL &#x200B; カスタムメタデータの詳細 &#x200B;] コレクションのデータタイプ

[!UICONTROL &#x200B; カスタムメタデータの詳細 &#x200B;] コレクションは、カスタムメタデータを保存するための構造を定義する、標準のエクスペリエンスデータモデル（XDM）データタイプです。 [!UICONTROL &#x200B; カスタムメタデータの詳細 &#x200B;] コレクション データタイプを使用して、コンテンツやインタラクションに関連付けられたカスタムメタデータの名前や値などの詳細をキャプチャします。

![ カスタムメタデータの詳細コレクション データタイプの図。](../images/data-types/the-custom-metadata-collection.png)

| 表示名 | プロパティ | データタイプ | 必須 | 説明 |
|--------------------------------------------|------------------|-----------|----------|-------------------------------|
| [!UICONTROL &#x200B; カスタムメタデータフィールド名 &#x200B;] | `name` | 文字列 | × | カスタムフィールドの名前。 |
| [!UICONTROL &#x200B; カスタムメタデータフィールドの値 &#x200B;] | `value` | 文字列 | × | カスタムフィールドの値。 |

{style="table-layout:auto"}
