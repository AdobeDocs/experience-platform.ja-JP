---
solution: Experience Platform
title: セグメント定義クラス
topic-legacy: overview
description: このドキュメントでは、エクスペリエンスデータモデル(XDM)のセグメント定義クラスの概要を説明します。
exl-id: c0f7b04c-2266-4d08-89a1-67ba758a51a7
source-git-commit: 632ea4e2a94bfcad098a5fc5a5ed8985c0f41e0e
workflow-type: tm+mt
source-wordcount: '374'
ht-degree: 0%

---

# [!UICONTROL セグメント] 定義クラス

&quot;[!UICONTROL セグメント定義]&quot;は、セグメント定義の詳細を取り込む標準的なExperience Data Model(XDM)クラスです。 このクラスには、セグメントのIDや名前などの必須フィールドと、他のオプションの属性が含まれます。 このクラスは、外部システムからセグメント定義をAdobe Experience Platformに取り込む場合に使用します。

>[!NOTE]
>
>このクラスは、セグメント定義自体に関する情報を取り込む目的でのみ使用します。 プロファイルデータ内のセグメントメンバーシップ情報を取り込むには、[!UICONTROL XDM個人プロファイル]スキーマで[セグメントメンバーシップの詳細フィールドグループ](../field-groups/profile/segmentation.md)を使用する必要があります。

![](../images/classes/segment-definition.png)

| プロパティ | 説明 |
| --- | --- |
| `_repo` | 次の[!UICONTROL DateTime]フィールドを含むオブジェクト： <ul><li>`createDate`:データが最初に取り込まれた日時など、データストアでリソースが作成された日時。</li><li>`modifyDate`:リソースが最後に変更された日時。</li></ul> |
| `_id` | レコードに対する、システム生成の一意の文字列識別子。 このフィールドは、個々のレコードの一意性の追跡、データの重複の防止およびダウンストリームサービスでのそのレコードの検索に使用します。<br><br>このフィールドはシステム生成なので、データ取り込み中に明示的な値を指定することはありません。ただし、必要に応じて独自の一意のID値を指定することもできます。<br><br>このフィールドは、個々の人に関連するアイデンティティ **ではなく、データの記録自体を表しているのではな** いことを区別することが重要です。個人に関するIDデータは、代わりに[IDフィールド](../schema/composition.md#identity)に左右する必要があります。 |
| `createdByBatchID` | レコードを作成した、取り込んだバッチのID。 |
| `description` | セグメント定義の説明。 |
| `identityMap` | セグメントが適用される個人の名前空間IDのセットが含まれるmapフィールド。 使用事例の詳細については、[スキーマ構成の基本](../schema/composition.md#identityMap)のIDマップに関する節を参照してください。 |
| `modifiedByBatchID` | レコードを更新した最後に取り込んだバッチのID。 |
| `repositoryCreatedBy` | レコードを作成したユーザーのID。 |
| `repositoryLastModifiedBy` | レコードを最後に変更したユーザーのID。 |
| `segmentName` | **（必須）** セグメント定義の名前。 |
| `segmentStatus` | 外部システムからのセグメントのステータス。 次の値を指定できます。 <ul><li>`ACTIVE`</li><li>`INACTIVE`</li><li>`DELETED`</li><li>`DRAFT`</li><li>`REVOKED`</li></ul> |
| `version` | セグメント定義の最新バージョン番号。 |
