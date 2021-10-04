---
solution: Experience Platform
title: セグメント定義クラス
topic-legacy: overview
description: このドキュメントでは、エクスペリエンスデータモデル (XDM) のセグメント定義クラスの概要を説明します。
exl-id: c0f7b04c-2266-4d08-89a1-67ba758a51a7
source-git-commit: 632ea4e2a94bfcad098a5fc5a5ed8985c0f41e0e
workflow-type: tm+mt
source-wordcount: '374'
ht-degree: 1%

---

# [!UICONTROL セグメント定] 義クラス

「[!UICONTROL  セグメント定義 ]」は、セグメント定義の詳細を取り込む標準のエクスペリエンスデータモデル (XDM) クラスです。 このクラスには、セグメントの ID や名前などの必須フィールドと、その他のオプションの属性が含まれます。 このクラスは、外部システムからAdobe Experience Platformにセグメント定義を取り込む場合に使用する必要があります。

>[!NOTE]
>
>このクラスは、セグメント定義自体に関する情報を取り込む場合にのみ使用する必要があります。 プロファイルデータ内でセグメントメンバーシップ情報を取り込むには、[!UICONTROL XDM Individual Profile] スキーマの [Segment Membership Details フィールドグループ ](../field-groups/profile/segmentation.md) を使用する必要があります。

![](../images/classes/segment-definition.png)

| プロパティ | 説明 |
| --- | --- |
| `_repo` | 次の [!UICONTROL DateTime] フィールドを含むオブジェクト： <ul><li>`createDate`:データが最初に取り込まれた日時など、リソースがデータストアに作成された日時。</li><li>`modifyDate`:リソースが最後に変更された日時。</li></ul> |
| `_id` | レコードの、システムで生成された一意の文字列識別子。 このフィールドは、個々のレコードの一意性を追跡し、データの重複を防ぎ、ダウンストリームサービスでそのレコードを検索するために使用します。<br><br>このフィールドはシステムで生成されるので、データの取り込み時に明示的な値を指定することはありません。ただし、必要に応じて独自の一意の ID 値を指定することもできます。<br><br>このフィールドは、個人に関連する ID ではな **** く、データ自体のレコードを表すことを区別することが重要です。個人に関連する ID データは、代わりに [ID フィールド ](../schema/composition.md#identity) に置き換える必要があります。 |
| `createdByBatchID` | レコードを作成する原因となった取得済みバッチの ID。 |
| `description` | セグメント定義の説明。 |
| `identityMap` | セグメントが適用される個人の名前空間付き ID のセットを含む map フィールド。 使用例について詳しくは、「[ スキーマ構成の基本 ](../schema/composition.md#identityMap)」の ID マップに関する節を参照してください。 |
| `modifiedByBatchID` | レコードを更新した最後に取得したバッチの ID。 |
| `repositoryCreatedBy` | レコードを作成したユーザーの ID。 |
| `repositoryLastModifiedBy` | レコードを最後に変更したユーザーの ID。 |
| `segmentName` | **（必須）** セグメント定義の名前。 |
| `segmentStatus` | 外部システムからのセグメントのステータス。 次の値を使用できます。 <ul><li>`ACTIVE`</li><li>`INACTIVE`</li><li>`DELETED`</li><li>`DRAFT`</li><li>`REVOKED`</li></ul> |
| `version` | セグメント定義の最新バージョン番号。 |
