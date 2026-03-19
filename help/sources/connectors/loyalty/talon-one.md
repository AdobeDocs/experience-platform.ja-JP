---
title: Talon.One Sourceの概要
description: Adobe Experience Platformの Talon.One ソースについて説明します
badge: ベータ版
hide: true
hidefromtoc: true
exl-id: 92ed180a-6175-45e2-a831-0f40fd8606b0
source-git-commit: 5ceef18d479854aa4b633e7e5e393a6698a05b2e
workflow-type: tm+mt
source-wordcount: '439'
ht-degree: 2%

---

# [!DNL Talon.One]

>[!AVAILABILITY]
>
>[!DNL Talon.One] のソースはベータ版です。 ベータラベル付きソースの使用について詳しくは、ソースの概要の [ 利用条件 ](../../home.md#terms-and-conditions) を参照してください。

[!DNL Talon.One] を使用すると、顧客に合わせてパーソナライズされたマーケティングキャンペーンを簡単に作成、管理および最適化できます。 この強力なプラットフォームを使用すると、割引の実行、クーポンの配布、紹介プログラムの開始、ロイヤルティプログラムの設定、ゲーミフィケーションのインセンティブの提供を、すべて 1 つのスケーラブルなシステムで行えます。お客様がオーディエンスと関わり、報酬を得ることができるように設計されています。

Adobe Experience Platform ソースカタログの [!DNL Talon.One] ソースを使用すると、[!DNL Talon.One] アカウントからバッチデータとストリーミングロイヤルティデータの両方を取り込むことができます。

* [Talon.One ストリーミングイベント](../../tutorials/ui/create/loyalty/talon-one-streaming.md)
* [Talon.One バッチ Source コネクタ](../../tutorials/ui/create/loyalty/talon-one-batch.md)

>[!TIP]
>
>ロイヤルティデータとは、エンドユーザーのロイヤルティプログラム情報（ロイヤルティポイント、使用したロイヤルティポイント、現在の階層、付与されたクーポン、紹介、成果など）を指します。

## 前提条件 {#prerequisites}

[!DNL Talon.One Batch Source Connector] ーザーを認証して接続するために、次の資格情報の値を指定します。

| 資格情報 | 説明 | 例 |
| --- | --- | --- |
| ドメイン | [!DNL Talon.One] アプリケーション環境に関連付けられた一意の URL。 ドメインを入力する際は、プロトコルまたはパスを含めないでください。 | `acmetalonone.us-east4` |
| [!DNL Talon.One] Management API キー | 管理 API キーは、[!DNL Talon.One] の管理 API へのアクセスを認証および承認するために使用される資格情報です。 これにより、次のような操作が処理されます。 <ul><li>クーポンのインポート</li><li>キャンペーンデータを取得中</li><li>アプリケーションとエンドポイントの管理</li></ul> | `ManagementKey-v1 {YOUR_MANAGEMENT_KEY}` |

## マッピング {#mapping}

各エフェクトオブジェクトを固有の `effectType` 値に基づいてマッピングするには、データ準備 `array_to_map` 関数を使用します。 これにより、順序なしのエフェクトの配列を、要件に合ったキーと値のペアに簡単に変換できます。 詳しくは、以下の例を参照してください。

また、Adobeが提供する標準化されたロイヤルティフィールドグループを使用して、一貫した方法でロイヤルティプログラムの概念をモデル化することもできます。

>[!BEGINTABS]

>[!TAB  ロイヤルティの詳細 ]

これは、XDM 個人プロファイルの標準 XDM フィールドグループで、イベントデータではなくレコード属性を取得して、個人のロイヤルティメンバーシップステータスを記述するために使用されます。 プロファイルスキーマでこのフィールドグループを使用して、次の項目を取得します。

* **メンバー** プログラムに参加しているユーザー（`loyaltyID`、`program`、`status`、`tier`）
* **現在およびライフタイムの残高** （`points`、`lifetimePoints`、`expiredPoints` など）
* キー **メンバーシップの日付** （`joinDate`、`upgradeDate`、`tierExpiryDate`）

>[!TAB  ロイヤルティイベントの詳細 ]

ロイヤルティイベントの詳細フィールドグループは、特定のトランザクションで獲得または引き換えられたポイントなど、イベントレベルのロイヤルティアクティビティを取り込むように設計されています。 このフィールドグループには、`xdm:points`、`xdm:pointsRedeemed`、`xdm:pointsAsOfDate`、`xdm:program` などのフィールドが含まれます。 エクスペリエンスイベントスキーマでこのイベントレベルのフィールドグループを使用して、次の項目をキャプチャします。

* **イベントごとの移動** ポイント（獲得、交換、期限切れ）
* ロイヤルティクーポンまたは参照によって請求された **割引**
* ロイヤルティプロバイダーとの紐付けの **プログラム ID** およびトランザクション ID。

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

[!DNL Talon.One] アカウントをExperience Platformに接続し、バッチとストリーミングの両方のロイヤルティデータを取り込む方法については、次のドキュメントをお読みください。

* [Talon.One ストリーミングイベント](../../tutorials/ui/create/loyalty/talon-one-streaming.md)
* [Talon.One バッチ Source コネクタ](../../tutorials/ui/create/loyalty/talon-one-batch.md)
