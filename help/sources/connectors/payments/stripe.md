---
title: Stripe
description: StripeアカウントからAdobe Experience Platformに支払データを取り込む方法を説明します
hide: true
hidefromtoc: true
badge: ベータ版
source-git-commit: b5e791882ddb7cb8c87c15d4812470b3bbc9a72e
workflow-type: tm+mt
source-wordcount: '809'
ht-degree: 9%

---

# [!DNL Stripe]

>[!NOTE]
>
>[!DNL Stripe] ソースはベータ版です。詳しくは、 [ソースの概要](../../home.md#terms-and-conditions) ベータラベル付きのソースの使用に関する詳細

あらゆる規模の数千の企業が [!DNL Stripe] オンラインとオンラインの両方で支払いを受け入れ、新しい収入源を生み出し、Adobe Experience Platform、Adobe Commerce、および [!DNL Magento Open Source].

以下を使用します。 [!DNL Stripe] ソース ( 顧客による購入フロー中にキャプチャされたデータを取り込むExperience Platform)。 取り込まれたら、このデータを使用してパーソナライズされたオファーを作成し、豊富なビジネスインサイトを活用します。

>[!TIP]
>
>」を参照してください。 [!DNL Stripe] ソースオンExperience Platform、連絡先 [!DNL Stripe] at adobe-partnership<span>@stripe.com.

>[!BEGINSHADEBOX]

**の使用例 [!DNL Stripe] ソース**

お客様のビジネスでは、顧客がオンラインストアで商品を購入する際に、 **購入する** および **後払いする** ( [!DNL Klarna], [!DNL Afterpay], [!DNL Affirm]または [!DNL Zip]) をクリックします。

以下を使用します。 [!DNL Stripe] 使用を分析するデータソース **購入する** および **後払いする** オプションを選択し、これらのお客様に対してパーソナライズされたオファーを試してみてください。 例えば、チェックアウト前に買い物かご内の品目数を増やすためのアドオン品目をレコメンデーションすることを検討します。

>[!ENDSHADEBOX]

の設定方法については、以下のドキュメントをお読みください。 [!DNL Stripe] ソースアカウント、必要な資格情報を取得し、スキーマを作成します。

## 前提条件 {#prerequisites}

次の節では、接続前に完了する必要がある前提条件の設定に関する情報を示します。 [!DNL Stripe] アカウントからExperience Platformへ。

### アクセストークンの取得

* にログインします。 [[!DNL Stripe] dashboard](https://dashboard.stripe.com/login) を使用して、 [!DNL Stripe] 電子メールアドレスとパスワード。
* Adobe Analytics の [!DNL Developers] ダッシュボード、選択 **[!DNL API keys for developers]**.
* の下 **API キー** タブ、選択 **[!DNL Reveal test key]** をクリックして、アクセストークンを表示します。
* これで、このトークンを、 [!DNL Stripe] アカウントからExperience Platformへ、次のいずれかを使用 [!DNL Flow Service] API またはExperience PlatformUI。

### 必要な資格情報の収集

接続するには [!DNL Stripe] アカウントからExperience Platformに、次の認証資格情報の値を指定する必要があります。

>[!BEGINTABS]

>[!TAB API]

接続する際に、次の資格情報を入力する必要があります。 [!DNL Stripe] アカウントを [!DNL Flow Service] API.

| 資格情報 | 説明 |
| --- | --- |
| `accessToken` | お使いの [!DNL Stripe] OAuth 2 更新コード認証トークン。 |
| `connectionSpec.id` | の接続仕様 ID [!DNL Stripe] ソース。 この ID は次のように修正されました。 `cc2c31d6-7b8c-4581-b49f-5c8698aa3ab3`. |

>[!TAB UI]

接続する際に、次の資格情報を入力する必要があります。 [!DNL Stripe] アカウントを使用します。Experience Platformユーザーインターフェイスを使用します。

| 資格情報 | 説明 |
| --- | --- |
| アクセストークン | お使いの [!DNL Stripe] OAuth 2 更新コード認証トークン。 |

>[!ENDTABS]

の使用に関する詳細 [!DNL Stripe] API、 [[!DNL Stripe] API キーに関するドキュメント](https://docs.stripe.com/keys).

### エクスペリエンスデータモデル (XDM) スキーマの作成

The [!DNL Stripe] ソースは、次のリソースパスからのデータの取り込みをサポートしています。

* 料金
* 購読
* 払い戻し
* 残高トランザクション
* 顧客
* 価格

データセットを記述する XDM スキーマを作成する必要があります。このデータセットには、送信元のフィールドとデータタイプを格納できます。 [!DNL Stripe] をExperience Platformに追加します。

>[!BEGINTABS]

>[!TAB 料金]

In [!DNL Stripe], **料金** お金をあなたの中に移す試みを表す [!DNL Stripe]. 詳しくは、 [[!DNL Stripe] 料金に関する API ガイド](https://docs.stripe.com/api/charges) 特定の請求属性の詳細を参照してください。

+++「 」を選択して、StripeCharge オブジェクトを表示します。

```json
{
  "id": "ch_3MmlLrLkdIwHu7ix0snN0B15",
  "object": "charge",
  "amount": 1099,
  "amount_captured": 1099,
  "amount_refunded": 0,
  "application": null,
  "application_fee": null,
  "application_fee_amount": null,
  "balance_transaction": "txn_3MmlLrLkdIwHu7ix0uke3Ezy",
  "billing_details": {
    "address": {
      "city": null,
      "country": null,
      "line1": null,
      "line2": null,
      "postal_code": null,
      "state": null
    },
    "email": null,
    "name": null,
    "phone": null
  },
  "calculated_statement_descriptor": "Stripe",
  "captured": true,
  "created": 1679090539,
  "currency": "usd",
  "customer": null,
  "description": null,
  "disputed": false,
  "failure_balance_transaction": null,
  "failure_code": null,
  "failure_message": null,
  "fraud_details": {},
  "invoice": null,
  "livemode": false,
  "metadata": {},
  "on_behalf_of": null,
  "outcome": {
    "network_status": "approved_by_network",
    "reason": null,
    "risk_level": "normal",
    "risk_score": 32,
    "seller_message": "Payment complete.",
    "type": "authorized"
  },
  "paid": true,
  "payment_intent": null,
  "payment_method": "card_1MmlLrLkdIwHu7ixIJwEWSNR",
  "payment_method_details": {
    "card": {
      "brand": "visa",
      "checks": {
        "address_line1_check": null,
        "address_postal_code_check": null,
        "cvc_check": null
      },
      "country": "US",
      "exp_month": 3,
      "exp_year": 2024,
      "fingerprint": "mToisGZ01V71BCos",
      "funding": "credit",
      "installments": null,
      "last4": "4242",
      "mandate": null,
      "network": "visa",
      "three_d_secure": null,
      "wallet": null
    },
    "type": "card"
  },
  "receipt_email": null,
  "receipt_number": null,
  "receipt_url": "https://pay.stripe.com/receipts/payment/CAcaFwoVYWNjdF8xTTJKVGtMa2RJd0h1N2l4KOvG06AGMgZfBXyr1aw6LBa9vaaSRWU96d8qBwz9z2J_CObiV_H2-e8RezSK_sw0KISesp4czsOUlVKY",
  "refunded": false,
  "review": null,
  "shipping": null,
  "source_transfer": null,
  "statement_descriptor": null,
  "statement_descriptor_suffix": null,
  "status": "succeeded",
  "transfer_data": null,
  "transfer_group": null
}
```

+++

>[!TAB サブスクリプション]

In [!DNL Stripe]を使用する場合、 **購読** を使用して、顧客に対して定期的な料金を請求できます。 詳しくは、 [[!DNL Stripe] 購読に関する API ガイド](https://docs.stripe.com/api/subscriptions) を参照してください。

+++「 」を選択して、購読Stripe・オブジェクトを表示します。

```json
{
  "id": "sub_1MowQVLkdIwHu7ixeRlqHVzs",
  "object": "subscription",
  "application": null,
  "application_fee_percent": null,
  "automatic_tax": {
    "enabled": false,
    "liability": null
  },
  "billing_cycle_anchor": 1679609767,
  "billing_thresholds": null,
  "cancel_at": null,
  "cancel_at_period_end": false,
  "canceled_at": null,
  "cancellation_details": {
    "comment": null,
    "feedback": null,
    "reason": null
  },
  "collection_method": "charge_automatically",
  "created": 1679609767,
  "currency": "usd",
  "current_period_end": 1682288167,
  "current_period_start": 1679609767,
  "customer": "cus_Na6dX7aXxi11N4",
  "days_until_due": null,
  "default_payment_method": null,
  "default_source": null,
  "default_tax_rates": [],
  "description": null,
  "discount": null,
  "ended_at": null,
  "invoice_settings": {
    "issuer": {
      "type": "self"
    }
  },
  "items": {
    "object": "list",
    "data": [
      {
        "id": "si_Na6dzxczY5fwHx",
        "object": "subscription_item",
        "billing_thresholds": null,
        "created": 1679609768,
        "metadata": {},
        "plan": {
          "id": "price_1MowQULkdIwHu7ixraBm864M",
          "object": "plan",
          "active": true,
          "aggregate_usage": null,
          "amount": 1000,
          "amount_decimal": "1000",
          "billing_scheme": "per_unit",
          "created": 1679609766,
          "currency": "usd",
          "interval": "month",
          "interval_count": 1,
          "livemode": false,
          "metadata": {},
          "nickname": null,
          "product": "prod_Na6dGcTsmU0I4R",
          "tiers_mode": null,
          "transform_usage": null,
          "trial_period_days": null,
          "usage_type": "licensed"
        },
        "price": {
          "id": "price_1MowQULkdIwHu7ixraBm864M",
          "object": "price",
          "active": true,
          "billing_scheme": "per_unit",
          "created": 1679609766,
          "currency": "usd",
          "custom_unit_amount": null,
          "livemode": false,
          "lookup_key": null,
          "metadata": {},
          "nickname": null,
          "product": "prod_Na6dGcTsmU0I4R",
          "recurring": {
            "aggregate_usage": null,
            "interval": "month",
            "interval_count": 1,
            "trial_period_days": null,
            "usage_type": "licensed"
          },
          "tax_behavior": "unspecified",
          "tiers_mode": null,
          "transform_quantity": null,
          "type": "recurring",
          "unit_amount": 1000,
          "unit_amount_decimal": "1000"
        },
        "quantity": 1,
        "subscription": "sub_1MowQVLkdIwHu7ixeRlqHVzs",
        "tax_rates": []
      }
    ],
    "has_more": false,
    "total_count": 1,
    "url": "/v1/subscription_items?subscription=sub_1MowQVLkdIwHu7ixeRlqHVzs"
  },
  "latest_invoice": "in_1MowQWLkdIwHu7ixuzkSPfKd",
  "livemode": false,
  "metadata": {},
  "next_pending_invoice_item_invoice": null,
  "on_behalf_of": null,
  "pause_collection": null,
  "payment_settings": {
    "payment_method_options": null,
    "payment_method_types": null,
    "save_default_payment_method": "off"
  },
  "pending_invoice_item_interval": null,
  "pending_setup_intent": null,
  "pending_update": null,
  "quantity": 1,
  "schedule": null,
  "start_date": 1679609767,
  "status": "active",
  "test_clock": null,
  "transfer_data": null,
  "trial_end": null,
  "trial_settings": {
    "end_behavior": {
      "missing_payment_method": "create_invoice"
    }
  },
  "trial_start": null
}
```

+++

>[!TAB 払い戻し]

In [!DNL Stripe]を使用する場合、 **払い戻し** ：以前に作成した手数料を返金します。 詳しくは、 [[!DNL Stripe] 返金に関する API ガイド](https://docs.stripe.com/api/refunds) 特定の払い戻し属性の詳細については、を参照してください。

+++「 」を選択して返金Stripeを表示

```json
{
  "id": "re_1Nispe2eZvKYlo2Cd31jOCgZ",
  "object": "refund",
  "amount": 1000,
  "balance_transaction": "txn_1Nispe2eZvKYlo2CYezqFhEx",
  "charge": "ch_1NirD82eZvKYlo2CIvbtLWuY",
  "created": 1692942318,
  "currency": "usd",
  "destination_details": {
    "card": {
      "reference": "123456789012",
      "reference_status": "available",
      "reference_type": "acquirer_reference_number",
      "type": "refund"
    },
    "type": "card"
  },
  "metadata": {},
  "payment_intent": "pi_1GszsK2eZvKYlo2CfhZyoZLp",
  "reason": null,
  "receipt_number": null,
  "source_transfer_reversal": null,
  "status": "succeeded",
  "transfer_reversal": null
}
```

+++

>[!TAB 残高トランザクション]

In [!DNL Stripe], **貸借取引** あなたの間の資金の動きを表す [!DNL Stripe] アカウント。 詳しくは、 [[!DNL Stripe] 残高トランザクションに関する API ガイド](https://docs.stripe.com/api/balance_transactions) を参照してください。

+++選択して、[ トランザクション残高のStripe] オブジェクトを表示します

```json
{
  "id": "txn_1MiN3gLkdIwHu7ixxapQrznl",
  "object": "balance_transaction",
  "amount": -400,
  "available_on": 1678043844,
  "created": 1678043844,
  "currency": "usd",
  "description": null,
  "exchange_rate": null,
  "fee": 0,
  "fee_details": [],
  "net": -400,
  "reporting_category": "transfer",
  "source": "tr_1MiN3gLkdIwHu7ixNCZvFdgA",
  "status": "available",
  "type": "transfer"
}
```

+++

>[!TAB 顧客]

In [!DNL Stripe], **顧客** は、ビジネスの特定の顧客を表します。 特定の顧客属性について詳しくは、 [[!DNL Stripe] お客様向け API ガイド](https://docs.stripe.com/api/customers) を参照してください。

+++「 」を選択して、顧客のStripe・オブジェクトを表示します。

```json
{
  "id": "cus_NffrFeUfNV2Hib",
  "object": "customer",
  "address": null,
  "balance": 0,
  "created": 1680893993,
  "currency": null,
  "default_source": null,
  "delinquent": false,
  "description": null,
  "discount": null,
  "email": "jennyrosen@example.com",
  "invoice_prefix": "0759376C",
  "invoice_settings": {
    "custom_fields": null,
    "default_payment_method": null,
    "footer": null,
    "rendering_options": null
  },
  "livemode": false,
  "metadata": {},
  "name": "Jenny Rosen",
  "next_invoice_sequence": 1,
  "phone": null,
  "preferred_locales": [],
  "shipping": null,
  "tax_exempt": "none",
  "test_clock": null
}
```

+++

>[!TAB 価格]

In [!DNL Stripe], **価格** は、製品の定期購入と 1 回限りの購入の両方について、単価、通貨、およびオプションの請求サイクルを表します。 詳しくは、 [[!DNL Stripe] 価格に関する API ガイド](https://docs.stripe.com/api/prices) を参照してください。

+++「 」を選択して、Stripe価格オブジェクトを表示します。

```json
{
  "id": "price_1MoBy5LkdIwHu7ixZhnattbh",
  "object": "price",
  "active": true,
  "billing_scheme": "per_unit",
  "created": 1679431181,
  "currency": "usd",
  "custom_unit_amount": null,
  "livemode": false,
  "lookup_key": null,
  "metadata": {},
  "nickname": null,
  "product": "prod_NZKdYqrwEYx6iK",
  "recurring": {
    "aggregate_usage": null,
    "interval": "month",
    "interval_count": 1,
    "trial_period_days": null,
    "usage_type": "licensed"
  },
  "tax_behavior": "unspecified",
  "tiers_mode": null,
  "transform_quantity": null,
  "type": "recurring",
  "unit_amount": 1000,
  "unit_amount_decimal": "1000"
}
```

+++

>[!ENDTABS]


### IP アドレス許可リスト

ソースコネクタを操作する前に、IP アドレスのリストを許可リストに追加する必要があります。 地域固有の IP アドレスを許可リストに追加しないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下する場合があります。 詳しくは、[IP アドレスの許可リスト](../../ip-address-allow-list.md)ページを参照してください。

### Experience Platformの権限の設定

次の両方を持つ必要があります。 **[!UICONTROL ソースを表示]** および **[!UICONTROL ソースの管理]** アカウントに対して有効になっている権限で、 [!DNL Stripe] アカウントからExperience Platformへ。 製品管理者に問い合わせて、必要な権限を取得してください。 詳しくは、 [アクセス制御 UI ガイド](../../../access-control/ui/overview.md).

## 次の手順

前提条件の設定が完了したら、次に進み、 [!DNL Stripe] データをExperience Platformに送信します。 取り込み方法については、次のガイドを参照してください [!DNL Stripe] API またはユーザーインターフェイスを使用してExperience Platformに支払うデータ：

* [フローサービス API を使用して、StripeアカウントからExperience Platformに支払データを取り込みます](../../tutorials/api/create/payments/stripe.md).
* [ユーザーインターフェイスを使用して、StripeアカウントからExperience Platformに支払データを取り込みます](../../tutorials/ui/create/payments/stripe.md).