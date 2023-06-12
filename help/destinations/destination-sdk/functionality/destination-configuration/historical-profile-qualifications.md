---
description: Destination SDK で作成された宛先でサポートされるプロファイル選定履歴について説明します。
title: プロファイル選定履歴
source-git-commit: 65a658208b48a50184e55a6d64cdf7ad6de0f04f
workflow-type: tm+mt
source-wordcount: '214'
ht-degree: 100%

---


# プロファイル選定履歴

Destination SDK を通じて作成したすべての宛先は、デフォルトでプロファイル選定履歴をサポートしています。つまり、ユーザーが最初に宛先に対するアクティベーションデータフローを設定すると、最初の書き出しには、これまでにそのセグメントに選定されたことのある、セグメントのすべてのメンバーが含まれます。

この動作は、宛先設定の `"backfillHistoricalProfileData":true` パラメーターで定義されます。

>[!IMPORTANT]
>
>プロファイル選定履歴は、Destination SDK で作成されたすべての宛先で有効になっており、ユーザーは `backfillHistoricalProfileData` パラメーターを設定できません。

## サポートされる統合タイプ {#supported-integration-types}

このページで説明される機能をサポートする統合のタイプについて詳しくは、以下の表を参照してください。

| 統合タイプ | 機能のサポート |
|---|---|
| リアルタイム（ストリーミング）統合 | ○ |
| ファイルベースの（バッチ）統合 | ○ |



<!-- 
|Parameter | Type | Description|
|---------|----------|------|
|`backfillHistoricalProfileData` | Boolean | Controls whether historical profile data is exported when segments are activated to the destination. <br> <ul><li> `true`: [!DNL Platform] sends the historical user profiles that qualified for the segment before the segment is activated. </li><li> `false`: [!DNL Platform] only includes user profiles that qualify for the segment after the segment is activated. </li></ul> |

{style="table-layout:auto"} -->


## 次の手順 {#next-steps}

この記事を読むことで、Experience Platform では、最初にセグメントを宛先に書き出す際に、これまでにアクティブ化されたセグメントに選定されたことのあるすべてのプロファイルの母集団履歴が自動的に書き出されることを理解しました。このオプションは、Destination SDK や Experience Platform UI では設定できません。

その他の宛先コンポーネントについて詳しくは、以下の記事を参照してください。

* [顧客認証](customer-authentication.md)
* [OAuth 2 認証](oauth2-authentication.md)
* [顧客データフィールド](customer-data-fields.md)
* [UI 属性](ui-attributes.md)
* [スキーマ設定](schema-configuration.md)
* [ID 名前空間設定](identity-namespace-configuration.md)
* [サポートされるマッピング設定](supported-mapping-configurations.md)
* [宛先配信](destination-delivery.md)
* [オーディエンスメタデータ設定](audience-metadata-configuration.md)
* [集計ポリシー](aggregation-policy.md)
* [バッチ設定](batch-configuration.md)