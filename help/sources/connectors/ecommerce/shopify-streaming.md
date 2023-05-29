---
title: ストリーミングソースを Shopify
description: ソース接続とデータフローを作成して、Shopify インスタンスからAdobe Experience Platformにストリーミングデータを取り込む方法を説明します。
badge: ベータ
last-substantial-update: 2023-04-26T00:00:00Z
exl-id: 4c83c08d-c744-4167-9e3b-ed9a995943f4
source-git-commit: e37c00863249e677f1645266859bf40fe6451827
workflow-type: tm+mt
source-wordcount: '682'
ht-degree: 3%

---

# [!DNL Shopify Streaming]

>[!NOTE]
>
>[!DNL Shopify Streaming] ソースはベータ版です。詳しくは、 [ソースの概要](../../home.md#terms-and-conditions) ベータラベル付きのソースの使用に関する詳細

Adobe Experience Platformは、ストリーミングアプリケーションからデータを取り込む機能を備えています。 ストリーミングプロバイダーのサポートには以下が含まれます。 [!DNL Shopify].

## 前提条件 {#prerequisites}

次の節では、を使用する前に完了する必要がある前提条件の手順について説明します。 [!DNL Shopify Streaming] ソース。

有効な [!DNL Shopify] に接続するためのパートナーアカウント [!DNL Shopify] API まだパートナーアカウントをお持ちでない場合は、 [[!DNL Shopify] パートナーダッシュボード](https://www.shopify.com/partners).

### アプリケーションを作成

有効な [!DNL Shopify] パートナーアカウントを使用する場合は、パートナーダッシュボードを使用して先に進み、アプリを作成できます。 でアプリを作成する方法に関する包括的な手順については、 [!DNL Shopify]、 [[!DNL Shopify] 入門ガイド](https://www.shopify.com/partners/blog/17056443-how-to-generate-a-shopify-api-token).

アプリを作成したら、 **クライアント ID** および **クライアント秘密鍵** から **クライアント資格情報** タブ [!DNL Shopify] パートナーダッシュボード。 次の手順では、クライアント ID とクライアント秘密鍵を使用して、認証コードとアクセストークンを取得します。

### 認証コードの取得

次に、ドメインの `myshopify.com` ブラウザーへの URL。API キー、スコープ、リダイレクト URI を定義するクエリ文字列。

この URL の形式は次のとおりです。

**API 形式**

```http
https://{SHOP}.myshopify.com/admin/oauth/authorize?client_id={API_KEY}&scope={SCOPES}&redirect_uri={REDIRECT_URI}
```

| パラメーター | 説明 |
| --- | --- |
| `shop` | サブドメイン `myshopify.com` URL。 |
| `api_key` | お使いの [!DNL Shopify] クライアント ID。 クライアント ID は、 **クライアント資格情報** タブ [!DNL Shopify] パートナーダッシュボード。 |
| `scopes` | 定義するアクセスのタイプ。 たとえば、スコープを `scope=write_orders,read_customers` ：オーダーの変更と顧客の読み取りの権限を許可します。 |
| `redirect_uri` | アクセストークンを生成するスクリプトの URL です。 |

**リクエスト**

```http
https://connnectors-test.myshopify.com/admin/oauth/authorize?client_id=l6fiviermmzpram5i1spfub99shms3j9&scope=write_orders,read_customers&redirect_uri=https://acme.com
```

**応答**

リクエストが成功した場合は、アクセストークンの生成に必要な認証コードを含むリダイレクト URL が返されます。

```http
https://www.acme.com/?code=k6j2palgrbljja228ou8c20fmn7w41gz&hmac=68c9163f772eecbc8848c90f695bca0460899c125af897a6d2b0ebbd59d3a43b&shop=connnectors-test.myshopify.com&state=123456×tamp=1658305460
```

### アクセストークンの取得

これで、クライアント ID、クライアント秘密鍵、認証コードが揃い、アクセストークンを取得できます。 アクセストークンを取得するには、ドメインの `myshopify.com` この URL を [!DNL Shopify's] API エンドポイント： `/admin/oauth/access_token`.

**API 形式**

```https
POST /{SHOP}.myshopify.com/admin/oauth/access_token
```

**リクエスト**

次のリクエストでは、 [!DNL Shopify] インスタンス。

```shell
curl -X POST \
  'https://connnectors-test.myshopify.com/admin/oauth/access_token' \
  -H 'developer-token: {DEVELOPER_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'Cookie: _master_udr=xxx; request_method=POST'
  -d '{
    "client_id": "l6fiviermmzpram5i1spfub99shms3j9",
    "client_secret": "dajn3caxz9s7ti624ncyv_m4f60jnwi3ii3y3k",
    "code": "k6j2palgrbljja228ou8c20fmn7w41gz"
}'
```

**応答**

正常な応答は、アクセストークンと権限範囲を返します。

```json
{
  "access_token": "shpca_wjhifwfc91psjtldysxd6rqli371tx54",
  "scope": "write_orders,read_customers"
}
```

## ストリーミング用の Webhook の作成 [!DNL Shopify] データ {#webhook}

Web フックを使用すると、アプリケーションを [!DNL Shopify] ショップで特定のイベントが発生した後に、データを取得したりアクションを実行したりします。 ストリーミング用 [!DNL Shopify] データをExperience Platformに送信する場合、Web フックを使用して、http エンドポイントとサブスクリプション用のトピックを定義できます。

**リクエスト**

次のリクエストを実行すると、 [!DNL Shopify Streaming] データ。

```shell
curl -X POST \
  'https://connnectors-test.myshopify.com/admin/api/2022-07/webhooks.json' \
  -H 'X-Shopify-Access-Token: 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' \
  -H 'Content-Type: application/json' \; request_method=POST' \
  -d '{
  "webhook": {
    "address": "https://dcs.adobedc.net/collection/9d411a24aa3c0a3eded92bac6c64d0da986ee7a8212f87168c5fb42d9ddc3227",
    "topic": "orders/create",
    "format": "json"
  }
}'
```

| パラメーター | 説明 |
| --- | --- | 
| `webhook.address` | ストリーミングメッセージが送信される HTTP エンドポイント。 |
| `webhook.topic` | Webhook サブスクリプションのトピック。 詳しくは、 [[!DNL Shopify] webhook イベントトピックガイド](https://shopify.dev/docs/api/admin-rest/2023-04/resources/webhook#event-topics). |
| `webhook.format` | データの形式。 |

**応答**

正常な応答は、Webhook に関する情報 ( `id`、アドレス、およびその他のメタデータ情報。

```json
{
  "webhook": {
    "id": 1091138715786,
    "address": "https://dcs.adobedc.net/collection/9d411a24aa3c0a3eded92bac6c64d0da986ee7a8212f87168c5fb42d9ddc3227",
    "topic": "orders/create",
    "created_at": "2022-07-20T07:15:23-04:00",
    "updated_at": "2022-07-20T07:15:23-04:00",
    "format": "json",
    "fields": [],
    "metafield_namespaces": [],
    "api_version": "2021-10",
    "private_metafield_namespaces": []
  }
}
```

### 制限事項 {#limitations}

以下は、 [!DNL Shopify Streaming] ソース。

* 同じリソースに対して異なるトピックを配信する順序を調整できるかどうかは保証されません。 例えば、 `products/update` ウェブフックが配信される前に `products/create` Webhook.
* Webhook イベントをエンドポイントに少なくとも 1 回配信するように Webhook を設定できます。 つまり、1 つのエンドポイントが同じイベントを複数回受け取る場合があります。 重複する Webhook イベントをスキャンするには、 `X-Shopify-Webhook-Id` ヘッダーから前のイベントへ。
* [!DNL Shopify] は、HTTP 2xx ステータス応答を成功通知として処理します。 その他のステータスコードの応答は、失敗と見なされます。 [!DNL Shopify] には、失敗した Webhook 通知の再試行メカニズムが用意されています。 存在する場合 **5 秒待った後の応答なし**, [!DNL Shopify] 接続を再試行します **19 回** 次の段階で **48 時間**. 再試行期間の終わりまでに応答がない場合は、 [!DNL Shopify] ウェブフックを削除します。

## 次の手順

次のチュートリアルでは、 [!DNL Shopify Streaming] API と UI を使用してExperience Platformにソースを追加します。

* [フローサービス API を使用して、Shopify ストリーミングソース接続とデータフローを作成する](../../tutorials/api/create/ecommerce/shopify-streaming.md)
* [UI での Shopify ストリーミングソース接続とデータフローの作成](../../tutorials/ui/create/ecommerce/shopify-streaming.md)
