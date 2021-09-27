---
title: XDM Business Marketing List Class
description: このドキュメントでは、エクスペリエンスデータモデル(XDM)のXDMビジネスマーケティングリストクラスの概要を説明します。
source-git-commit: 19bb39b66f3a3eb93fd0138ac021568021d77b0f
workflow-type: tm+mt
source-wordcount: '164'
ht-degree: 4%

---

# [!UICONTROL XDM Business Marketing ] Listclass

>[!NOTE]
>
>このクラスは、リアルタイム顧客データプラットフォームのB2Bエディションにアクセスできる組織でのみ使用できます。

[!UICONTROL XDM Business Marketing Listは、] マーケティングリストの最低限必要なプロパティを取り込む、標準のエクスペリエンスデータモデル(XDM)クラスです。マーケティングリストを使用すると、製品を購入する可能性が最も高い見込み顧客を優先できます。

![](../../images/classes/b2b/business-marketing-list.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `extSourceSystemAudit` | [[!UICONTROL 外部ソース・システム監査属性]](../../data-types/external-source-system-audit-attributes.md) | マーケティングリストが外部ソースシステムから取得された場合、このオブジェクトはそのシステムの監査属性を取り込みます。 |
| `marketingListKey` | [[!UICONTROL B2Bソース]](../../data-types/b2b-source.md) | マーケティングリストエンティティの複合識別子。 |
| `_id` | 文字列 | レコードの一意の識別子。 これは、`marketingListID`とは別のシステム生成値です。 |
| `marketingListDescription` | 文字列 | マーケティングリストの説明。 |
| `marketingListID` | 文字列 | マーケティングリストエンティティの一意のID。 |
| `marketingListName` | 文字列 | マーケティングリストの名前。 |
