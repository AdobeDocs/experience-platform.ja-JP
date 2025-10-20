---
title: Talon.One Sourceの概要
description: Adobe Experience Platformの Talon.One ソースについて説明します
badge: ベータ版
hide: true
hidefromtoc: true
source-git-commit: 558a9d6ff3222acbf77edea0a82ef50725cd6203
workflow-type: tm+mt
source-wordcount: '286'
ht-degree: 3%

---

# [!DNL Talon.One]

>[!AVAILABILITY]
>
>[!DNL Talon.One] のソースはベータ版です。 ベータラベル付きソースの使用について詳しくは、ソースの概要の [&#x200B; 利用条件 &#x200B;](../../home.md#terms-and-conditions) を参照してください。

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