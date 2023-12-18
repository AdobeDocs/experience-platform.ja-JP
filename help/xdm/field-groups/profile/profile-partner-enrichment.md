---
title: プロファイルパートナーエンリッチメント（サンプル）フィールドグループ
description: 「プロファイルパートナーエンリッチメント（サンプル） 」スキーマフィールドグループについて説明します。
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '234'
ht-degree: 13%

---


# [!UICONTROL プロファイルパートナーエンリッチメント（サンプル）] スキーマフィールドグループ

[!UICONTROL プロファイルパートナーエンリッチメント（サンプル）] は、 [[!DNL XDM Individual Profile] クラス](../../classes/individual-profile.md). このフィールドグループを使用して、顧客プロファイルのパートナー主導のエンリッチメントに関する追加のデータを提供します。

![の図 [!UICONTROL プロファイルパートナーエンリッチメント（サンプル）] フィールドグループを使用します。](../../images/field-groups/profile-partner-enrichment-sample.png)

| 表示名 | プロパティ | データタイプ | 説明 |
|-----------------------------|------------------------|-----------|----------------------------------|
| [!UICONTROL ageRangeInHousehold] | `ageRangeInHousehold` | 文字列 | 年齢は世帯内に及ぶ。 |
| [!UICONTROL apparelAccessories] | `apparelAccessories` | 文字列 | 衣料品およびアクセサリデータ。 |
| [!UICONTROL 自転車] | `bicycling` | 文字列 | 自転車に関する情報。 |
| [!UICONTROL cableTv] | `cableTv` | 文字列 | ケーブルテレビに関する情報。 |
| [!UICONTROL 家畜] | `domestics` | 文字列 | 国内関連データ。 |
| [!UICONTROL エレクトロニクス] | `electronics` | 文字列 | エレクトロニクス関連の情報。 |
| [!UICONTROL foodAndBebrecd] | `foodAndBeverage` | 文字列 | 飲食品のデータ。 |
| [!UICONTROL 履き物] | `footwear` | 文字列 | 履物関連の情報。 |
| [!UICONTROL healthFoods] | `healthFoods` | 文字列 | 健康食品のデータ。 |
| [!UICONTROL ハイキング] | `hiking` | 文字列 | ハイキング関連の情報。 |
| [!UICONTROL householdId] | `householdId` | 文字列 | 世帯の一意の ID。 |
| [!UICONTROL individualId] | `individualId` | 文字列 | 個人の一意の ID。 |
| [!UICONTROL interredCardHolder] | `inferredCardHolder` | 文字列 | 推測されるカード所有者情報。 |
| [!UICONTROL interredPremiumCardholder] | `inferredPremiumCardholder` | 文字列 | 推測されるプレミアムカード所有者の詳細。 |
| [!UICONTROL matchLevelFlag] | `matchLevelFlag` | 文字列 | 一致レベルフラグのデータ。 |
| [!UICONTROL BuyerRating] | `buyerRating` | 文字列 | 購入者の評価情報。 |
| [!UICONTROL DonorRating] | `donorRating` | 文字列 | ドナーの評価の詳細。 |
| [!UICONTROL InvestmentRating] | `investmentRating` | 文字列 | 投資評価データ。 |
| [!UICONTROL ResponderRating] | `responderRating` | 文字列 | 回答者の評価情報です。 |
| [!UICONTROL SpendingVelocity] | `spendingVelocity` | 文字列 | 支出速度の詳細。 |
| [!UICONTROL SpentingVolume] | `spendingVolume` | 文字列 | 購買数量の情報。 |
| [!UICONTROL recordId] | `recordId` | 文字列 | 一意のレコード識別子。 |
| [!UICONTROL residenceId] | `residenceId` | 文字列 | 居住用の一意の ID。 |
| [!UICONTROL セーリング] | `sailing` | 文字列 | 帆走関連のデータ。 |
| [!UICONTROL seasonalHolidayProducts] | `seasonalHolidayProducts` | 文字列 | 季節別の休日の製品情報。 |
| [!UICONTROL スキー] | `skiing` | 文字列 | スキー関連のデータ。 |
| [!UICONTROL テニス] | `tennis` | 文字列 | テニス関連の情報。 |
| [!UICONTROL tvShoppers] | `tvShoppers` | 文字列 | TV 買い物客の情報。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、 [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/profile/partner-profile-enrichment/profile-partner-enrichment-sample.schema.json) パブリック XDM リポジトリーで使用できます。
