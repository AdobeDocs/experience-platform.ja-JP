---
title: XDM ビジネスマーケティングリストクラス
description: このドキュメントでは、エクスペリエンスデータモデル (XDM) の XDM ビジネスマーケティングリストクラスの概要を説明します。
exl-id: 510c5608-054d-4bed-91eb-22d84b5dc625
source-git-commit: 50e5fe8573d828f88867ed33fe86e974c85de60a
workflow-type: tm+mt
source-wordcount: '310'
ht-degree: 6%

---

# [!UICONTROL XDM ビジネスマーケティングリスト] クラス

>[!IMPORTANT]
>
>このクラスは、 [Real-time Customer Data Platform B2B エディション](../../../rtcdp/b2b-overview.md). このクラスがに参加するには、Real-time CDP B2B Edition へのアクセス権が必要です。 [リアルタイム顧客プロファイル](../../../profile/home.md).

[!UICONTROL XDM ビジネスマーケティングリスト] は、マーケティングリストの最低限必要なプロパティを取り込む、標準のエクスペリエンスデータモデル (XDM) クラスです。 マーケティングリストを使用すると、製品を購入する可能性が最も高い見込み客を優先することができます。

![UI に表示される XDM Business Marketing List クラスの構造](../../images/classes/b2b/business-marketing-list.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `extSourceSystemAudit` | [[!UICONTROL 外部ソースシステム監査属性]](../../data-types/external-source-system-audit-attributes.md) | マーケティングリストが外部ソースシステムから取得された場合、このオブジェクトはそのシステムの監査属性をキャプチャします。 |
| `marketingListKey` | [[!UICONTROL B2B ソース]](../../data-types/b2b-source.md) | マーケティングリストエンティティの複合識別子。 |
| `_id` | 文字列 | レコードの一意の ID。 これは、 `marketingListID`. |
| `isDeleted` | ブール値 | このマーケティングリストエンティティがMarketo Engageで削除されたかどうかを示します。<br><br>を使用する場合、 [Marketoソースコネクタ](../../../sources/connectors/adobe-applications/marketo/marketo.md)を指定した場合、Marketoで削除されたレコードは自動的にリアルタイム顧客プロファイルに反映されます。 ただし、これらのプロファイルに関連するレコードは、データレイク内で引き続き保持される場合があります。 設定別 `isDeleted` から `true`の場合は、フィールドを使用して、データレイクに対するクエリを実行する際に、ソースから削除されたレコードを除外できます。 |
| `marketingListDescription` | 文字列 | マーケティングリストの説明。 |
| `marketingListID` | 文字列 | マーケティングリストエンティティの一意の ID。 |
| `marketingListName` | 文字列 | マーケティングリストの名前。 |

{style=&quot;table-layout:auto&quot;}

詳しくは、 [リアルタイム CDP B2B エディションのスキーマの関係](../../tutorials/relationship-b2b.md) このクラスが他の B2B クラスと概念的にどのように関連しているか、およびAdobe Experience Platform UI でこれらの関係を確立する方法を学びます。
