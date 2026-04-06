---
title: Talon.One Sourceの概要
description: Adobe Experience PlatformのTalon.One ソースについて説明します
badge: ベータ版
last-substantial-update: 2026-04-06T00:00:00Z
exl-id: 92ed180a-6175-45e2-a831-0f40fd8606b0
source-git-commit: f3026e0a717c07d95f12e3aeaf380ddc1b87c712
workflow-type: tm+mt
source-wordcount: '439'
ht-degree: 2%

---

# [!DNL Talon.One]

>[!AVAILABILITY]
>
>[!DNL Talon.One] ソースはベータ版です。 ベータ版のソースの使用について詳しくは、ソースの概要の[条件](../../home.md#terms-and-conditions)を参照してください。

[!DNL Talon.One]を利用すると、顧客に合わせてパーソナライズされたマーケティング施策を簡単に作成、管理、最適化できます。 ディスカウントの実行、クーポンの配布、紹介プログラムの立ち上げ、ロイヤルティプログラムの設定、ゲーミフィケーションによるインセンティブの提供など、オーディエンスへのエンゲージメントとリワード獲得を支援する、拡張可能な単一のシステムから利用できます。

Adobe Experience Platform ソースカタログの[!DNL Talon.One] ソースを使用して、[!DNL Talon.One] アカウントからバッチデータとストリーミングロイヤルティデータの両方を取得できます。

* [Talon.One ストリーミングイベント](../../tutorials/ui/create/loyalty/talon-one-streaming.md)
* [Talon.One Batch Source コネクタ](../../tutorials/ui/create/loyalty/talon-one-batch.md)

>[!TIP]
>
>ロイヤルティデータとは、ロイヤルティポイント、使用済みロイヤルティポイント、現在の購入層、付与されたクーポン、紹介、実績などのエンドユーザーのロイヤルティプログラム情報を指します。

## 前提条件 {#prerequisites}

[!DNL Talon.One Batch Source Connector]を認証して接続するために、次の資格情報の値を指定してください。

| 資格情報 | 説明 | 例 |
| --- | --- | --- |
| ドメイン | [!DNL Talon.One] アプリケーション環境に関連付けられている一意のURL。 ドメインの入力時に、プロトコルまたはパスが含まれていないことを確認してください。 | `acmetalonone.us-east4` |
| [!DNL Talon.One]管理API キー | 管理API キーは、[!DNL Talon.One]の管理APIへのアクセスを認証および承認するために使用される資格情報です。 これは、次のような操作を処理します。 <ul><li>クーポンのインポート</li><li>キャンペーンデータの取得</li><li>アプリケーションとエンドポイントの管理</li></ul> | `ManagementKey-v1 {YOUR_MANAGEMENT_KEY}` |

## マッピング {#mapping}

固有の`effectType`値に基づいて各エフェクトオブジェクトをマッピングするには、データ準備`array_to_map`関数を使用できます。 これにより、順序のないエフェクトの配列を、要件に一致するキーと値のペアに簡単に変換できます。 ガイダンスについては、以下の例を参照してください。

また、Adobeが提供する標準化されたロイヤルティフィールドグループを使用して、ロイヤルティプログラムの概念を一貫した方法でモデル化することもできます。

>[!BEGINTABS]

>[!TAB  ロイヤルティの詳細]

これはXDM個人プロファイルの標準XDM フィールドグループで、イベントデータではなくレコード属性を取得することで、個人のロイヤルティメンバーシップのステータスを表すために使用されます。 プロファイルスキーマでこのフィールドグループを使用して、次の情報を取得します。

* **メンバーがプログラムに参加している人** （`loyaltyID`、`program`、`status`、`tier`）
* **現在およびライフタイム残高** （`points`、`lifetimePoints`、`expiredPoints`など）
* 重要な&#x200B;**メンバーシップ日** （`joinDate`、`upgradeDate`、`tierExpiryDate`）

>[!TAB  ロイヤルティイベントの詳細]

「ロイヤルティイベントの詳細」フィールドグループは、特定の取引で獲得または交換されたポイントなど、イベントレベルでロイヤルティアクティビティを取得するように設計されています。 このフィールドグループには、`xdm:points`、`xdm:pointsRedeemed`、`xdm:pointsAsOfDate`、`xdm:program`などのフィールドが含まれます。 Experience Event スキーマでこのイベントレベルのフィールドグループを使用して、次の情報を取得します。

* **イベントごとの移動** （ポイント数：獲得、交換、期限切れ）
* ロイヤルティクーポンまたは紹介によって推進された&#x200B;**割引**
* ロイヤルティプロバイダーとの紐付けの&#x200B;**プログラム ID**&#x200B;およびトランザクション ID。

>[!ENDTABS]

| ソース | 宛先 |
| ---- | --- |
| `array_to_map(data.effects, "effectType").addLoyaltyPoints.campaignId` | `_{TENANT_ID}.loyalty.pointsGained[0].promotionId` |
| `array_to_map(data.effects, "effectType").addLoyaltyPoints.props.value` | `_{TENANT_ID}.loyalty.pointsGained[0].value` |
| `array_to_map(data.effects, "effectType").deductLoyaltyPoints.campaignId` | `_{TENANT_ID}.loyalty.pointsRedemption[0].promotionId` |
| `array_to_map(data.effects, "effectType").acceptCoupon.campaignId` | `_{TENANT_ID}.loyalty.couponRedemption[0].campaignId` |
| `array_to_map(data.effects, "effectType").deductLoyaltyPoints.props.value` | `_{TENANT_ID}.loyalty.pointsRedemption[0].value` |
| `array_to_map(data.effects, "effectType").acceptCoupon.props.value` | `_{TENANT_ID}.loyalty.couponRedemption[0].id` |
| `array_to_map(data.effects, "effectType").setDiscount.campaignId` | `_{TENANT_ID}.loyalty.discounts[0].promotionId` |
| `array_to_map(data.effects, "effectType").setDiscount.props.value` | `_{TENANT_ID}.loyalty.discounts[0].value` |
| `data.created` | `timestamp` |
| `data.attributes.params.profileId` | `personID` |
| `data.attributes.integrationId` | `_id` |
| `data.attributes.params.cartItems[*].name` | `productListItems[*].name` |
| `data.attributes.params.cartItems[*].category` | `productListItems[*].productCategories[0].categoryID` |
| `data.attributes.params.cartItems[*].sku` | `productListItems[*].SKU` |

{style="table-layout:auto"}

## 次の手順

次のドキュメントを参照して、[!DNL Talon.One] アカウントをExperience Platformに接続し、バッチデータとストリーミングロイヤルティデータの両方を取り込む方法を確認してください。

* [Talon.One ストリーミングイベント](../../tutorials/ui/create/loyalty/talon-one-streaming.md)
* [Talon.One Batch Source コネクタ](../../tutorials/ui/create/loyalty/talon-one-batch.md)
