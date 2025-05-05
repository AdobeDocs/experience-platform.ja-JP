---
title: Stripe
description: 支払いデータをStripeアカウントからAdobe Experience Platformに取り込む方法を説明します
badge: ベータ版
exl-id: 191d217e-036d-491a-b7dd-abcad74625ba
source-git-commit: 62bcaa532cdec68a2f4f62e5784c35b91b7d5743
workflow-type: tm+mt
source-wordcount: '809'
ht-degree: 9%

---

# [!DNL Stripe]

>[!NOTE]
>
>[!DNL Stripe] ソースはベータ版です。ベータラベル付きソースの使用について詳しくは、[ ソースの概要 ](../../home.md#terms-and-conditions) を参照してください。

あらゆる規模の数千の企業が、オンラインと対面の両方で [!DNL Stripe] を活用して支払いを受け入れ、新たな収益源を生み出し、Adobe Experience Platform、Adobe Commerce、[!DNL Magento Open Source] の支援を受けてグローバルに拡大しています。

Experience Platformの [!DNL Stripe] ソースを使用して、顧客の購入フロー中に取り込まれたデータを取り込みます。 取り込んだら、このデータを使用してパーソナライズされたオファーを作成し、より豊富なビジネスインサイトを活用します。

>[!TIP]
>
>Experience Platformの [!DNL Stripe] ソースに関するご質問は、adobe-partnership<span>@stripe.com[!DNL Stripe] お問い合わせください。

>[!BEGINSHADEBOX]

**[!DNL Stripe] ソースのユースケースのサンプル**

ビジネスでは、顧客はオンラインストアで商品を購入できます。その際、[!DNL Klarna]、[!DNL Afterpay]、[!DNL Affirm] または [!DNL Zip] を使用して **今すぐ購入** および **後で支払う** オプションを使用できます。

[!DNL Stripe] データソースを使用して **今すぐ購入** および **後で支払う** オプションの使用状況を分析し、これらの顧客に対してパーソナライズされたオファーを試します。 例えば、チェックアウト前に買い物かごのアイテム数を増やすために、アドオン項目を推奨することを検討します。

>[!ENDSHADEBOX]

[!DNL Stripe] ソースアカウントの設定、必要な資格情報の取得、スキーマの作成方法について詳しくは、以下のドキュメントを参照してください。

## 前提条件 {#prerequisites}

次の節では、[!DNL Stripe] アカウントをExperience Platformに接続する前に完了する必要がある前提条件の設定について説明します。

### アクセストークンの取得

* [!DNL Stripe] メールアドレスとパスワードを使用して [&#128279;](https://dashboard.stripe.com/login) ダッシュボード [!DNL Stripe]  にログインします。
* [!DNL Developers] ダッシュボードで、「**[!DNL API keys for developers]**」を選択します。
* 「**API キー**」タブで、「**[!DNL Reveal test key]**」を選択してアクセストークンを表示します。
* [!DNL Flow Service] API またはExperience PlatformUI を使用して [!DNL Stripe] アカウントをExperience Platformに接続する際に、このトークンをアクセストークンとして使用できるようになりました。

### 必要な資格情報の収集

[!DNL Stripe] アカウントをExperience Platformに接続するには、次の認証資格情報の値を指定する必要があります。

>[!BEGINTABS]

>[!TAB API]

[!DNL Flow Service] API を使用して [!DNL Stripe] アカウントに接続する場合は、次の資格情報を指定する必要があります。

| 資格情報 | 説明 |
| --- | --- |
| `accessToken` | [!DNL Stripe] OAuth 2 更新コード認証トークン。 |
| `connectionSpec.id` | [!DNL Stripe] ソースの接続仕様 ID。 この ID は、`cc2c31d6-7b8c-4581-b49f-5c8698aa3ab3` として固定されます。 |

>[!TAB UI]

Experience Platformユーザーインターフェイスを使用して [!DNL Stripe] アカウントに接続する場合は、次の資格情報を指定する必要があります。

| 資格情報 | 説明 |
| --- | --- |
| アクセストークン | [!DNL Stripe] OAuth 2 更新コード認証トークン。 |

>[!ENDTABS]

[!DNL Stripe] API の使用について詳しくは、[[!DNL Stripe] API キーに関するドキュメント ](https://docs.stripe.com/keys) を参照してください。

### エクスペリエンスデータモデル（XDM）スキーマの作成

[!DNL Stripe] ソースは、次のリソースパスからのデータの取り込みをサポートしています。

* 料金
* 購読
* 払戻
* 残高トランザクション
* 顧客
* 価格

データセットを記述する XDM スキーマを作成する必要があります。このデータセットには、[!DNL Stripe] からExperience Platformに送信されるフィールドとデータタイプを格納できます。

>[!BEGINTABS]

>[!TAB  料金 ]

[!DNL Stripe] では、**料金** はあなたの [!DNL Stripe] にお金を移動しようとする試みを表します。 特定の請求属性について詳しくは、[[!DNL Stripe]  料金に関する API ガイド ](https://docs.stripe.com/api/charges) を参照してください。

+++選択してStripe料金オブジェクトを表示します

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

ま [!DNL Stripe]、**購読** を使用して、顧客に定期的に請求できます。 特定の購読属性について詳しくは、購読に関する [[!DNL Stripe] API ガイド ](https://docs.stripe.com/api/subscriptions) を参照してください。

+++選択してStripe購読オブジェクトを表示します

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

>[!TAB  払戻し ]

[!DNL Stripe] では、以前に作成した料金を払い戻す **払い戻し** を使用できます。 特定の払い戻し属性について詳しくは、[[!DNL Stripe]  払い戻しに関する API ガイド ](https://docs.stripe.com/api/refunds) を参照してください。

+++選択してStripe払戻オブジェクトを表示します

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

>[!TAB  残高取引 ]

[!DNL Stripe] えば、**残高取引** は、[!DNL Stripe] 勘定間の資金の移動を表します。 特定の残高取引属性について詳しくは [&#128279;](https://docs.stripe.com/api/balance_transactions) 残高取引に関する [!DNL Stripe] API ガイド」を参照してください。

+++オンにすると、Stripe残高取引オブジェクトが表示されます

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

>[!TAB  顧客 ]

[!DNL Stripe] では、**顧客** はビジネスの特定の顧客を表します。 特定の顧客属性について詳しくは、[[!DNL Stripe]  顧客に関する API ガイド ](https://docs.stripe.com/api/customers) を参照してください。

+++選択して、Stripe顧客オブジェクトを表示します

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

>[!TAB  価格 ]

[!DNL Stripe] では、**価格** は、製品の繰り返し購入と 1 回限りの購入の両方に対する単価、通貨、オプションの請求サイクルを表します。 特定の価格属性について詳しくは、価格に関する [[!DNL Stripe] API ガイド ](https://docs.stripe.com/api/prices) を参照してください。

+++選択してStripe価格オブジェクトを表示します

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

### Experience Platformーに対する権限の設定

[!DNL Stripe] アカウントをExperience Platformに接続するには、アカウントで **[!UICONTROL ソースの表示]** および **[!UICONTROL ソースの管理]** 権限の両方が有効になっている必要があります。 必要な権限を取得するには、製品管理者にお問い合わせください。 詳しくは、[ アクセス制御 UI ガイド ](../../../access-control/ui/overview.md) を参照してください。

## 次の手順

前提条件の設定が完了したら、に進んで、[!DNL Stripe] データを接続してExperience Platformに取り込むことができます。 API またはユーザーインターフェイスを使用して [!DNL Stripe] 支払いデータをExperience Platformに取り込む方法については、次のガイドを参照してください。

* [Flow Service API を使用して、StripeアカウントからExperience Platformに支払いデータを取り込みます ](../../tutorials/api/create/payments/stripe.md)。
* [ ユーザーインターフェイスを使用した、StripeアカウントからExperience Platformへの支払いデータの取り込み ](../../tutorials/ui/create/payments/stripe.md)。
