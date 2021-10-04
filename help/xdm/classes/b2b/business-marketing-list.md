---
title: XDM ビジネスマーケティングリストクラス
description: このドキュメントでは、エクスペリエンスデータモデル (XDM) の XDM ビジネスマーケティングリストクラスの概要を説明します。
exl-id: 510c5608-054d-4bed-91eb-22d84b5dc625
source-git-commit: b5cdd72238f7b4519de1c789f4294b9698415327
workflow-type: tm+mt
source-wordcount: '213'
ht-degree: 8%

---

# [!UICONTROL XDM Business Marketing List] クラス（ベータ版）

>[!IMPORTANT]
>
>このクラスは、現在ベータ版のリアルタイム顧客データプラットフォーム B2B エディションの一部です。 ドキュメントと機能は変更される場合があります。

[!UICONTROL XDM Business Marketing List は、] マーケティングリストの最低限必要なプロパティを取得する、標準のエクスペリエンスデータモデル (XDM) クラスです。マーケティングリストを使用すると、製品を購入する可能性が最も高い見込み客を優先して優先できます。

![](../../images/classes/b2b/business-marketing-list.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `extSourceSystemAudit` | [[!UICONTROL 外部ソース・システム監査属性]](../../data-types/external-source-system-audit-attributes.md) | マーケティングリストが外部ソースシステムから取得された場合、このオブジェクトはそのシステムの監査属性を取り込みます。 |
| `marketingListKey` | [[!UICONTROL B2B ソース]](../../data-types/b2b-source.md) | マーケティングリストエンティティの複合識別子。 |
| `_id` | 文字列 | レコードの一意の識別子。 これは、`marketingListID` とは別のシステム生成値です。 |
| `marketingListDescription` | 文字列 | マーケティングリストの説明。 |
| `marketingListID` | 文字列 | マーケティングリストエンティティの一意の ID。 |
| `marketingListName` | 文字列 | マーケティングリストの名前。 |

{style=&quot;table-layout:auto&quot;}

このクラスが概念的に他の B2B クラスと関連付けられる方法と、Adobe Experience Platform UI でこれらの関係を確立する方法については、Real-time CDP B2B Edition](../../tutorials/relationship-b2b.md) の [ スキーマの関係に関するガイドを参照してください。
