---
title: 「Partner Prospect Details （Sample）」フィールド・グループ
description: パートナー見込み客詳細（サンプル）フィールドグループ（XDM）スキーマフィールドグループについて説明します。
exl-id: 2de1eb7a-2e44-4417-9bdd-7a8a4b2d3a7f
source-git-commit: 8be502c9eea67119dc537a5d63a6c71e0bff1697
workflow-type: tm+mt
source-wordcount: '346'
ht-degree: 11%

---

# [!UICONTROL  パートナー見込み客詳細（サンプル） ] フィールドグループ

[!UICONTROL  パートナー見込み客詳細（サンプル） ] は、[[!DNL XDM ExperienceEvent]  クラス ](../../classes/experienceevent.md) の標準スキーマフィールドグループです。 [!UICONTROL  パートナー見込み客詳細（サンプル） ] は、見込み客のプロファイルに関連する様々な詳細のサンプルフレームワークを提供します。 このフレームワークにより、多様な見込み客関連の情報を整理および管理するプロセスが合理化されます。

このフィールドグループは、パートナーのコンテキスト内で [ 個人見込み客プロファイルクラス ](https://experienceleague.adobe.com/docs/experience-platform/xdm/classes/prospect.html) を拡張します。

![[!UICONTROL  パートナー見込み客の詳細（サンプル） ] フィールドグループの図。](../../images/field-groups/partner/partner-prospect-details-sample.png)

| 表示名 | プロパティ | データタイプ | 説明 |
|---------------------------------------|-----------------------------|-----------|--------------------------------------------------|
| [!UICONTROL ageRangeInHousehold] | `ageRangeInHousehold` | 文字列 | 世帯内の年齢範囲。 |
| [!UICONTROL  アパレルアクセサリー ] | `apparelAccessories` | 文字列 | アパレル/アクセサリーの好みや関与。 |
| [!UICONTROL  自転車 ] | `bicycling` | 文字列 | 自転車活動への関心または関与。 |
| [!UICONTROL cableTv] | `cableTv` | 文字列 | ケーブル テレビとのエンゲージメントを示します。 |
| [!UICONTROL  家事 ] | `domestics` | 文字列 | 国内の活動における好みまたはエンゲージメント。 |
| [!UICONTROL  エレクトロニクス ] | `electronics` | 文字列 | 電子デバイスへの関心またはエンゲージメント。 |
| [!UICONTROL  飲食物 ] | `foodAndBeverage` | 文字列 | 好み、または食品や飲料への関与。 |
| [!UICONTROL  履物 ] | `footwear` | 文字列 | 履物に対する関心または関与。 |
| [!UICONTROL  健康食品 ] | `healthFoods` | 文字列 | 健康食品の好みまたは関与。 |
| [!UICONTROL  ハイキング ] | `hiking` | 文字列 | ハイキングアクティビティへの関心または関与。 |
| [!UICONTROL householdId] | `householdId` | 文字列 | 世帯の一意の ID。 |
| [!UICONTROL individualId] | `individualId` | 文字列 | 個人の一意の ID。 |
| [!UICONTROL inferredCardHolder] | `inferredCardHolder` | 文字列 | カード所有者であるという推論。 |
| [!UICONTROL inferredPremiumCardholder] | `inferredPremiumCardholder]` | 文字列 | プレミアムカード所有者であるという推論。 |
| [!UICONTROL matchLevelFlag] | `matchLevelFlag` | 文字列 | マッチングレベルの指標。 |
| [!UICONTROL BuyerRating] | `buyerRating` | 文字列 | 購入行動に関連する評価。 |
| [!UICONTROL DonerRating] | `donorRating` | 文字列 | ドナーの行動に関する評価。 |
| [!UICONTROL  投資格付け ] | `investmentRating` | 文字列 | 投資行動に関する格付け。 |
| [!UICONTROL  レスポンダー評価 ] | `responderRating` | 文字列 | レスポンダーの行動に関連するレーティング。 |
| [!UICONTROL  支出速度 ] | `spendingVelocity` | 文字列 | 支出のスピードまたはレート。 |
| [!UICONTROL  支出量 ] | `spendingVolume` | 文字列 | 支出の量または量。 |
| [!UICONTROL recordId] | `recordId` | 文字列 | レコードの一意の ID。 |
| [!UICONTROL residenceId] | `residenceId` | 文字列 | 住居の一意の ID。 |
| [!UICONTROL  セーリング ] | `sailing` | 文字列 | セーリングアクティビティへの関心または関与を示します。 |
| [!UICONTROL seasonalHolidayProducts] | `seasonalHolidayProducts` | 文字列 | ホリデー商品の好みまたは関与を示します。 |
| [!UICONTROL  スキー ] | `skiing` | 文字列 | スキー活動への関心または関与を示します。 |
| [!UICONTROL  テニス ] | `tennis` | 文字列 | テニス活動への関心または関与を示します。 |
| [!UICONTROL tvShoppers] | `tvShoppers` | 文字列 | テレビ ショッピングとのエンゲージメントを示します。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、公開 XDM リポジトリの [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/profile/partner-prospect/merkle/prospect-details-partner-sample.schema.json) を参照してください。
