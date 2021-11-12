---
title: XDM ビジネスマーケティングリストクラス
description: このドキュメントでは、エクスペリエンスデータモデル (XDM) の XDM ビジネスマーケティングリストクラスの概要を説明します。
exl-id: 510c5608-054d-4bed-91eb-22d84b5dc625
source-git-commit: edf7afc5db219430232a3226dc691570b50a32bd
workflow-type: tm+mt
source-wordcount: '185'
ht-degree: 5%

---

# [!UICONTROL XDM ビジネスマーケティングリスト] クラス

[!UICONTROL XDM ビジネスマーケティングリスト] は、マーケティングリストの最低限必要なプロパティを取り込む、標準のエクスペリエンスデータモデル (XDM) クラスです。 マーケティングリストを使用すると、製品を購入する可能性が最も高い見込み客を優先することができます。

![](../../images/classes/b2b/business-marketing-list.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `extSourceSystemAudit` | [[!UICONTROL 外部ソースシステム監査属性]](../../data-types/external-source-system-audit-attributes.md) | マーケティングリストが外部ソースシステムから取得された場合、このオブジェクトはそのシステムの監査属性をキャプチャします。 |
| `marketingListKey` | [[!UICONTROL B2B ソース]](../../data-types/b2b-source.md) | マーケティングリストエンティティの複合識別子。 |
| `_id` | 文字列 | レコードの一意の ID。 これは、 `marketingListID`. |
| `marketingListDescription` | 文字列 | マーケティングリストの説明。 |
| `marketingListID` | 文字列 | マーケティングリストエンティティの一意の ID。 |
| `marketingListName` | 文字列 | マーケティングリストの名前。 |

{style=&quot;table-layout:auto&quot;}

詳しくは、 [リアルタイム CDP B2B エディションのスキーマの関係](../../tutorials/relationship-b2b.md) このクラスが他の B2B クラスと概念的にどのように関連しているか、およびAdobe Experience Platform UI でこれらの関係を確立する方法を学びます。
