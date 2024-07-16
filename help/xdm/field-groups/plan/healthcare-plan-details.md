---
title: ヘルスケアプランの詳細スキーマフィールドグループ
description: 「ヘルスケアプランの詳細」スキーマフィールドグループについて説明します。
exl-id: 5a480c5b-74f8-48dd-858a-5cf2628dc7f0
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '202'
ht-degree: 9%

---

# [!UICONTROL  ヘルスケアプランの詳細 ] スキーマフィールドグループ

[!UICONTROL  ヘルスケアプランの詳細 ] は、[[!UICONTROL  プラン ] クラス ](../../classes/plan.md) の標準スキーマフィールドグループです。 医療プランに関連するプロパティをキャプチャする単一のオブジェクトタイプのフィールド `healthcarePlanDetails` を提供します。

![](../../images/field-groups/plan/healthcare-plan-details.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `networkDetails` | オブジェクトの配列 | 保険業者が定義した、受取人が治療を求めることができるプロバイダーのネットワークの詳細を一覧表示します。このプロバイダーは「ネットワーク内」の料金でカバーされます。 各オブジェクトには、次のプロパティが含まれます。 <ul><li>`networkID`:（文字列）ネットワークの保険業者固有の ID。</li><li>`networkName`:（文字列）ネットワークの保険業者固有の名前。</li></ul> |
| `affiliations` | 文字列の配列 | プランに関連付けられているビジネスエンティティのリスト。 |
| `coverageType` | 文字列 | プランの補償タイプ。 使用できる値は次のとおりです。<ul><li>`medical`</li><li>`dental`</li><li>`vision`</li><li>`accident`</li></ul> |
| `isActive` | ブール値 | プランがアクティブかどうかを示します。 |
| `lastVerificationDate` | 日時 | プランが最後に検証された日付。 |
| `payerID` | 文字列 | 支払人の一意の ID （プランの保険プロバイダー）。 |
| `planLevel` | 文字列 | 計画レベルを示します。 使用できる値は次のとおりです。<ul><li>`primary`</li><li>`secondary`</li><li>`tertiary`</li><li>`quaternary`</li></ul> |
| `planType` | 文字列 | プランのタイプを示します。 使用できる値は次のとおりです。<ul><li>`hmo`</li><li>`epo`</li><li>`pos`</li><li>`hdhp`</li></ul> |
| `targetOwnerType` | 文字列 | プランの所有者のタイプ。 例としては、個人、グループ、組織などがあります。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、[ 公開 XDM リポジトリ ](https://github.com/adobe/xdm/blob/master/docs/reference/fieldgroups/plan/healthcare-plan-details.schema.json) を参照してください。
