---
title: 「プロファイルパートナーエンリッチメント（サンプル）」フィールドグループ
description: プロファイルパートナーエンリッチメント（サンプル）スキーマフィールドグループについて説明します。
exl-id: 02f7c358-3cf9-45cb-a5c5-e2cb1f140d93
source-git-commit: 8be502c9eea67119dc537a5d63a6c71e0bff1697
workflow-type: tm+mt
source-wordcount: '234'
ht-degree: 14%

---

# [!UICONTROL &#x200B; プロファイルパートナーエンリッチメント（サンプル） &#x200B;] スキーマフィールドグループ

[!UICONTROL &#x200B; プロファイルパートナーエンリッチメント（サンプル） &#x200B;] は、[[!DNL XDM Individual Profile]  クラス &#x200B;](../../classes/individual-profile.md) の標準スキーマフィールドグループです。 このフィールドグループを使用して、顧客プロファイルに対してパートナー主導のエンリッチメントに関連する追加データを提供します。

![[!UICONTROL &#x200B; プロファイルパートナーエンリッチメント（サンプル） &#x200B;] フィールドグループの図。](../../images/field-groups/profile-partner-enrichment-sample.png)

| 表示名 | プロパティ | データタイプ | 説明 |
|-----------------------------|------------------------|-----------|----------------------------------|
| [!UICONTROL ageRangeInHousehold] | `ageRangeInHousehold` | 文字列 | 世帯内の年齢範囲。 |
| [!UICONTROL &#x200B; アパレルアクセサリー &#x200B;] | `apparelAccessories` | 文字列 | アパレルとアクセサリーのデータ。 |
| [!UICONTROL &#x200B; 自転車 &#x200B;] | `bicycling` | 文字列 | 自転車関連情報。 |
| [!UICONTROL cableTv] | `cableTv` | 文字列 | ケーブルテレビに関する情報。 |
| [!UICONTROL &#x200B; 家事 &#x200B;] | `domestics` | 文字列 | 国内関連データ。 |
| [!UICONTROL &#x200B; エレクトロニクス &#x200B;] | `electronics` | 文字列 | エレクトロニクス関連情報 |
| [!UICONTROL &#x200B; 飲食物 &#x200B;] | `foodAndBeverage` | 文字列 | 食品および飲料データ。 |
| [!UICONTROL &#x200B; 履物 &#x200B;] | `footwear` | 文字列 | 履物関連情報 |
| [!UICONTROL &#x200B; 健康食品 &#x200B;] | `healthFoods` | 文字列 | 健康食品のデータ。 |
| [!UICONTROL &#x200B; ハイキング &#x200B;] | `hiking` | 文字列 | ハイキング関連の情報。 |
| [!UICONTROL householdId] | `householdId` | 文字列 | 世帯の一意の ID。 |
| [!UICONTROL individualId] | `individualId` | 文字列 | 個人の一意の ID。 |
| [!UICONTROL inferredCardHolder] | `inferredCardHolder` | 文字列 | 推測されるカード所有者の情報。 |
| [!UICONTROL inferredPremiumCardholder] | `inferredPremiumCardholder` | 文字列 | 推測されるプレミアムカード所有者の詳細。 |
| [!UICONTROL matchLevelFlag] | `matchLevelFlag` | 文字列 | レベル・フラグ・データを照合します。 |
| [!UICONTROL BuyerRating] | `buyerRating` | 文字列 | バイヤーの評価情報。 |
| [!UICONTROL DonerRating] | `donorRating` | 文字列 | ドナー評価の詳細。 |
| [!UICONTROL &#x200B; 投資格付け &#x200B;] | `investmentRating` | 文字列 | 投資評価データ。 |
| [!UICONTROL &#x200B; レスポンダー評価 &#x200B;] | `responderRating` | 文字列 | レスポンダー評価情報。 |
| [!UICONTROL &#x200B; 支出速度 &#x200B;] | `spendingVelocity` | 文字列 | 支出速度の詳細。 |
| [!UICONTROL &#x200B; 支出量 &#x200B;] | `spendingVolume` | 文字列 | 支出量情報。 |
| [!UICONTROL recordId] | `recordId` | 文字列 | 一意のレコード ID。 |
| [!UICONTROL residenceId] | `residenceId` | 文字列 | 居住地の一意の ID。 |
| [!UICONTROL &#x200B; セーリング &#x200B;] | `sailing` | 文字列 | セーリング関連データ。 |
| [!UICONTROL seasonalHolidayProducts] | `seasonalHolidayProducts` | 文字列 | 季節ごとの休日の商品情報。 |
| [!UICONTROL &#x200B; スキー &#x200B;] | `skiing` | 文字列 | スキー関連データ。 |
| [!UICONTROL &#x200B; テニス &#x200B;] | `tennis` | 文字列 | テニス関連の情報。 |
| [!UICONTROL tvShoppers] | `tvShoppers` | 文字列 | テレビの買い物客の情報。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、公開 XDM リポジトリの [&#x200B; 完全なスキーマ &#x200B;](https://github.com/adobe/xdm/blob/master/components/fieldgroups/profile/partner-profile-enrichment/profile-partner-enrichment-sample.schema.json) を参照してください。
