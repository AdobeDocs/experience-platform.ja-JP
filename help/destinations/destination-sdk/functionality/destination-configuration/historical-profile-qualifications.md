---
description: Destination SDKで作成された宛先でサポートされる履歴プロファイルの絞り込みについて説明します。
title: プロファイル選定履歴
source-git-commit: 65a658208b48a50184e55a6d64cdf7ad6de0f04f
workflow-type: tm+mt
source-wordcount: '214'
ht-degree: 10%

---


# プロファイル選定履歴

Destination SDKを通じて作成されたすべての宛先は、デフォルトで履歴プロファイル認定をサポートします。 つまり、ユーザーが最初に宛先に対するアクティベーションデータフローを設定したとき、最初の書き出しには、そのセグメントに対してこれまでに認定されたセグメントのすべてのメンバーが含まれます。

この動作は、 `"backfillHistoricalProfileData":true` パラメーターを設定してください。

>[!IMPORTANT]
>
>過去のプロファイル認定は、Destination SDKおよび `backfillHistoricalProfileData` パラメーターはユーザーが設定できません。

## サポートされる統合のタイプ {#supported-integration-types}

このページで説明する機能をサポートする統合のタイプについて詳しくは、次の表を参照してください。

| 統合タイプ | 機能をサポート |
|---|---|
| リアルタイム（ストリーミング）統合 | ○ |
| ファイルベース（バッチ）の統合 | ○ |



<!-- 
|Parameter | Type | Description|
|---------|----------|------|
|`backfillHistoricalProfileData` | Boolean | Controls whether historical profile data is exported when segments are activated to the destination. <br> <ul><li> `true`: [!DNL Platform] sends the historical user profiles that qualified for the segment before the segment is activated. </li><li> `false`: [!DNL Platform] only includes user profiles that qualify for the segment after the segment is activated. </li></ul> |

{style="table-layout:auto"} -->


## 次の手順 {#next-steps}

この記事を読むと、Experience Platformが最初に宛先にセグメントを書き出す際に、アクティブ化されたセグメントの対象として認定されたすべてのプロファイルの過去の母集団が、自動的に書き出されることになります。 このオプションは、Destination SDKまたはExperience PlatformUI では設定できません。

その他の宛先コンポーネントについて詳しくは、次の記事を参照してください。

* [顧客認証](customer-authentication.md)
* [OAuth 2 認証](oauth2-authentication.md)
* [顧客データフィールド](customer-data-fields.md)
* [UI 属性](ui-attributes.md)
* [スキーマ設定](schema-configuration.md)
* [ID 名前空間の設定](identity-namespace-configuration.md)
* [サポートされるマッピング設定](supported-mapping-configurations.md)
* [宛先配信](destination-delivery.md)
* [オーディエンスメタデータの設定](audience-metadata-configuration.md)
* [集計ポリシー](aggregation-policy.md)
* [バッチ設定](batch-configuration.md)