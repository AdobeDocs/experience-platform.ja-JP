---
solution: Experience Platform
title: セグメント定義クラス
description: エクスペリエンスデータモデル（XDM）のセグメント定義クラスについて説明します。
exl-id: c0f7b04c-2266-4d08-89a1-67ba758a51a7
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '370'
ht-degree: 1%

---

# [!UICONTROL &#x200B; セグメント定義 &#x200B;] クラス

「[!UICONTROL &#x200B; セグメント定義 &#x200B;]」は、セグメント定義の詳細を取り込んだ標準のエクスペリエンスデータモデル（XDM）クラスです。 クラスには、オーディエンスの ID や名前などの必須フィールドのほか、その他のオプション属性が含まれています。 このクラスは、外部システムからAdobe Experience Platformにセグメント定義を取り込む場合に使用します。

>[!NOTE]
>
>このクラスは、セグメント定義自体に関する情報を取り込む場合にのみ使用してください。 プロファイルデータ内のオーディエンスメンバーシップ情報を取得するには、[!UICONTROL XDM 個人プロファイル &#x200B;] スキーマの [ セグメントメンバーシップの詳細フィールドグループ ](../field-groups/profile/segmentation.md) を使用する必要があります。

![](../images/classes/segment-definition.png)

| プロパティ | 説明 |
| --- | --- |
| `_repo` | 次の [!UICONTROL DateTime] フィールドを含むオブジェクト。 <ul><li>`createDate`：データが最初に取り込まれた日時など、データストア内でリソースが作成された日時。</li><li>`modifyDate`: リソースが最後に変更された日時。</li></ul> |
| `_id` | レコードに対してシステムで生成された一意の文字列識別子。 このフィールドは、個々のレコードの一意性を追跡、データの重複を防ぎ、ダウンストリームのサービスでそのレコードを検索するために使用されます。<br><br> このフィールドはシステムで生成されるので、データ取り込み時に明示的な値は指定されません。 ただし、必要に応じて、独自の一意の ID 値を指定することもできます。<br><br> このフィールドは **個人に関連する ID を表すものではなく** データ記録そのものを表していることを見極めることが重要です。 人物に関する ID データは、代わりに [ID フィールド ](../schema/composition.md#identity) に降格させるべきです。 |
| `createdByBatchID` | レコードを作成する原因となった取り込まれたバッチの ID。 |
| `description` | セグメント定義の説明。 |
| `identityMap` | オーディエンスが適用される個人用の名前空間 ID のセットを含む map フィールド。 そのユースケースについては、[ スキーマ構成の基本 ](../schema/composition.md#identityMap) の ID マップの節を参照してください。 |
| `modifiedByBatchID` | レコードを更新する原因となった、最後に取り込まれたバッチの ID。 |
| `repositoryCreatedBy` | レコードを作成したユーザーの ID。 |
| `repositoryLastModifiedBy` | レコードを最後に変更したユーザーの ID。 |
| `segmentName` | **（必須）** セグメント定義の名前。 |
| `segmentStatus` | 外部システムからのオーディエンスのステータス。 以下の値を使用できます。 <ul><li>`ACTIVE`</li><li>`INACTIVE`</li><li>`DELETED`</li><li>`DRAFT`</li><li>`REVOKED`</li></ul> |
| `version` | セグメント定義の最新のバージョン番号。 |

{style="table-layout:auto"}
