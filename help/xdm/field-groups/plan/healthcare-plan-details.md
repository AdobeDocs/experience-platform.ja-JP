---
title: ヘルスケアプラン詳細スキーマフィールドグループ
description: このドキュメントでは、「ヘルスケアプランの詳細」スキーマフィールドグループの概要を説明します。
source-git-commit: 3937963ceee8502b0669a3f007fd38ecf2824e9b
workflow-type: tm+mt
source-wordcount: '219'
ht-degree: 11%

---

# [!UICONTROL 医療プランの詳細] スキーマフィールドグループ

[!UICONTROL 医療プランの詳細] は、 [[!UICONTROL プラン] クラス](../../classes/plan.md). 単一のオブジェクトタイプフィールドを提供します。 `healthcarePlanDetails` 医療計画に関連するプロパティをキャプチャする

![](../../images/field-groups/plan/healthcare-plan-details.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `networkDetails` | オブジェクトの配列 | 保険業者が定義した、受益者が治療を受けることのできるプロバイダーのネットワークの詳細をリストし、「ネットワーク内」の料金でカバーされます。 各オブジェクトには、次のプロパティが含まれます。 <ul><li>`networkID`:(String) ネットワークの保険業者固有の ID。</li><li>`networkName`:(String) 保険会社のネットワーク固有の名前。</li></ul> |
| `affiliations` | 文字列の配列 | プランに関連する事業体のリスト。 |
| `coverageType` | 文字列 | プランの対象タイプ。 使用できる値は次のとおりです。<ul><li>`medical`</li><li>`dental`</li><li>`vision`</li><li>`accident`</li></ul> |
| `isActive` | ブール値 | プランがアクティブかどうかを示します。 |
| `lastVerificationDate` | 日時 | プランが最後に検証された日付。 |
| `payerID` | 文字列 | 支払者の一意の識別子（つまり、プランの保険プロバイダー）。 |
| `planLevel` | 文字列 | 計画レベルを示します。 使用できる値は次のとおりです。<ul><li>`primary`</li><li>`secondary`</li><li>`tertiary`</li><li>`quaternary`</li></ul> |
| `planType` | 文字列 | プランのタイプを示します。 使用できる値は次のとおりです。<ul><li>`hmo`</li><li>`epo`</li><li>`pos`</li><li>`hdhp`</li></ul> |
| `targetOwnerType` | 文字列 | プランの所有者のタイプ。 例としては、個人、グループ、組織などがあります。 |

{style=&quot;table-layout:auto&quot;}

フィールドグループについて詳しくは、 [パブリック XDM リポジトリ](https://github.com/adobe/xdm/blob/master/docs/reference/fieldgroups/plan/healthcare-plan-details.schema.json).
