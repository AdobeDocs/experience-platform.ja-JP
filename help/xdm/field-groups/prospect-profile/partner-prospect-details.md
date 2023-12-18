---
title: パートナー見込み客の詳細（例）フィールドグループ
description: 「パートナー見込み客の詳細（サンプル） 」フィールドグループ (XDM) スキーマフィールドグループについて説明します。
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '346'
ht-degree: 9%

---

# [!UICONTROL パートナー見込み客の詳細（例）] フィールドグループ

[!UICONTROL パートナー見込み客の詳細（例）] は、 [[!DNL XDM ExperienceEvent] クラス](../../classes/experienceevent.md). The [!UICONTROL パートナー見込み客の詳細（例）] には、見込み客のプロファイルに関連する様々な詳細のサンプルフレームワークが用意されています。 このフレームワークは、多様な見込み客関連情報を整理および管理するプロセスを合理化します。

このフィールドグループは、 [個々の見込み客プロファイルクラス](https://experienceleague.adobe.com/docs/experience-platform/xdm/classes/prospect.html) を含める必要があります。

![の図 [!UICONTROL パートナー見込み客の詳細（例）] フィールドグループを使用します。](../../images/field-groups/partner/partner-prospect-details-sample.png)

| 表示名 | プロパティ | データタイプ | 説明 |
|---------------------------------------|-----------------------------|-----------|--------------------------------------------------|
| [!UICONTROL ageRangeInHousehold] | `ageRangeInHousehold` | 文字列 | 世帯内の年齢範囲。 |
| [!UICONTROL apparelAccessories] | `apparelAccessories` | 文字列 | 衣料品/アクセサリーの好みまたは関与。 |
| [!UICONTROL 自転車] | `bicycling` | 文字列 | 自転車アクティビティへの関心または関与。 |
| [!UICONTROL cableTv] | `cableTv` | 文字列 | ケーブルテレビとの接続を示します。 |
| [!UICONTROL 家畜] | `domestics` | 文字列 | 家庭活動の好みまたはエンゲージメント。 |
| [!UICONTROL エレクトロニクス] | `electronics` | 文字列 | 電子デバイスへの関心または関与。 |
| [!UICONTROL foodAndBebrecd] | `foodAndBeverage` | 文字列 | 飲食品の好みや関与。 |
| [!UICONTROL 履き物] | `footwear` | 文字列 | 履物への関心または関与。 |
| [!UICONTROL healthFoods] | `healthFoods` | 文字列 | 健康食品への好みや関与。 |
| [!UICONTROL ハイキング] | `hiking` | 文字列 | ハイキングアクティビティへの関心または関与。 |
| [!UICONTROL householdId] | `householdId` | 文字列 | 世帯の一意の ID。 |
| [!UICONTROL individualId] | `individualId` | 文字列 | 個人の一意の ID。 |
| [!UICONTROL interredCardHolder] | `inferredCardHolder` | 文字列 | カード所有者であるという推論。 |
| [!UICONTROL interredPremiumCardholder] | `inferredPremiumCardholder]` | 文字列 | プレミアムカード所有者であるとの推論。 |
| [!UICONTROL matchLevelFlag] | `matchLevelFlag` | 文字列 | 一致するレベルの指標。 |
| [!UICONTROL BuyerRating] | `buyerRating` | 文字列 | 購入行動に関連する格付け。 |
| [!UICONTROL DonorRating] | `donorRating` | 文字列 | ドナーの行動に関連する評価。 |
| [!UICONTROL InvestmentRating] | `investmentRating` | 文字列 | 投資行動に関連する格付け。 |
| [!UICONTROL ResponderRating] | `responderRating` | 文字列 | 回答者の行動に関連する評価。 |
| [!UICONTROL SpendingVelocity] | `spendingVelocity` | 文字列 | 支出の速度または割合。 |
| [!UICONTROL SpentingVolume] | `spendingVolume` | 文字列 | 支出の量または量。 |
| [!UICONTROL recordId] | `recordId` | 文字列 | レコードの一意の ID。 |
| [!UICONTROL residenceId] | `residenceId` | 文字列 | 住居の一意の ID。 |
| [!UICONTROL セーリング] | `sailing` | 文字列 | 帆走活動に対する関心または関与を示します。 |
| [!UICONTROL seasonalHolidayProducts] | `seasonalHolidayProducts` | 文字列 | ホリデー商品の環境設定または関与を示します。 |
| [!UICONTROL スキー] | `skiing` | 文字列 | スキー活動への関心または関与を示します。 |
| [!UICONTROL テニス] | `tennis` | 文字列 | テニス活動への関心または関与を示します。 |
| [!UICONTROL tvShoppers] | `tvShoppers` | 文字列 | TV ショッピングとの関わりを示します。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、 [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/profile/partner-prospect/merkle/prospect-details-partner-sample.schema.json) パブリック XDM リポジトリーで使用できます。
