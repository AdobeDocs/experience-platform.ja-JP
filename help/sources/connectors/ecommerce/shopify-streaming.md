---
title: Shopify ストリーミングSource
description: ソース接続とデータフローを作成して、Shopify インスタンスからAdobe Experience Platformにストリーミングデータを取り込む方法を説明します
badge: ベータ版
last-substantial-update: 2023-04-26T00:00:00Z
exl-id: ae991913-68b5-4bbb-b8a5-e566d67a4c1a
source-git-commit: b4334b4f73428f94f5a7e5088f98e2459afcaf3c
workflow-type: tm+mt
source-wordcount: '671'
ht-degree: 3%

---

# [!DNL Shopify Streaming]

>[!NOTE]
>
>[!DNL Shopify Streaming] ソースはベータ版です。ベータラベル付きソースの使用について詳しくは、[ ソースの概要 ](../../home.md#terms-and-conditions) を参照してください。

Adobe Experience Platformは、ストリーミングアプリケーションからのデータ取り込みをサポートしています。 ストリーミングプロバイダーのサポートには、[!DNL Shopify] が含まれます。

## 前提条件 {#prerequisites}

次の節では、[!DNL Shopify Streaming] ソースを使用する前に完了する必要のある手順の概要を説明します。

[!DNL Shopify] API に接続するには、有効な [!DNL Shopify] パートナーアカウントが必要です。 パートナーアカウントをお持ちでない場合は、[[!DNL Shopify]  パートナーダッシュボード ](https://www.shopify.com/partners) を使用して登録してください。

### アプリケーションの作成

有効な [!DNL Shopify] パートナーアカウントを使用すると、パートナーダッシュボードで続行してアプリを作成できます。 [!DNL Shopify] でアプリを作成する方法の包括的な手順については、[[!DNL Shopify]  はじめる前にガイド ](https://www.shopify.com/partners/blog/17056443-how-to-generate-a-shopify-api-token) を参照してください。

アプリが作成されたら、[!DNL Shopify] ーザーパートナーダッシュボードの「**クライアント資格情報**」タブから **クライアント ID** および **クライアント秘密鍵** を取得します。 クライアント ID とクライアント秘密鍵は、次の手順で認証コードとアクセストークンを取得するために使用されます。

### 認証コードの取得

次に、ドメインの `myshopify.com` URL を、API キー、範囲、リダイレクト URI を定義するクエリ文字列と共にブラウザーに入力して、認証コードを取得します。

この URL の形式は次のとおりです。

**API 形式**

```http
https://{SHOP}.myshopify.com/admin/oauth/authorize?client_id={API_KEY}&scope={SCOPES}&redirect_uri={REDIRECT_URI}
```

| パラメーター | 説明 |
| --- | --- |
| `shop` | サブドメインの `myshopify.com` URL。 |
| `api_key` | [!DNL Shopify] クライアント ID。 クライアント ID は、[!DNL Shopify] ーザーパートナーダッシュボードの「**クライアント資格情報**」タブから取得できます。 |
| `scopes` | 定義するアクセスのタイプ。 例えば、範囲を `scope=write_orders,read_customers` として設定すると、注文の変更や顧客の読み取りを行う権限を付与できます。 |
| `redirect_uri` | アクセストークンを生成するスクリプトの URL |

**リクエスト**

```http
https://connnectors-test.myshopify.com/admin/oauth/authorize?client_id=l6fiviermmzpram5i1spfub99shms3j9&scope=write_orders,read_customers&redirect_uri=https://acme.com
```

**応答**

正常な応答は、アクセストークンの生成に必要な認証コードを含む、リダイレクト URL を返します。

```http
https://www.acme.com/?code=k6j2palgrbljja228ou8c20fmn7w41gz&hmac=68c9163f772eecbc8848c90f695bca0460899c125af897a6d2b0ebbd59d3a43b&shop=connnectors-test.myshopify.com&state=123456×tamp=1658305460
```

### アクセストークンの取得

これで、クライアント ID、クライアント秘密鍵、認証コードが用意できたので、アクセストークンを取得できます。 アクセストークンを取得するには、API エンドポイント `/admin/oauth/access_token` を使用してこの URL を追加する際に、ドメインの `myshopify.com` URL[!DNL Shopify's]POSTリクエストを行います。

**API 形式**

```https
POST /{SHOP}.myshopify.com/admin/oauth/access_token
```

**リクエスト**

次のリクエストでは、[!DNL Shopify] インスタンスのアクセストークンが生成されます。

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

応答が成功すると、アクセストークンと権限範囲が返されます。

```json
{
  "access_token": "shpca_wjhifwfc91psjtldysxd6rqli371tx54",
  "scope": "write_orders,read_customers"
}
```

## [!DNL Shopify] データをストリーミングするための Webhook の作成 {#webhook}

Webhook を使用すると、アプリケーションは [!DNL Shopify] データとの同期を維持したり、ショップで特定のイベントが発生した後にアクションを実行したりできます。 [!DNL Shopify] データをExperience Platformにストリーミングする場合は、webhook を使用して、http エンドポイントとサブスクリプションのトピックを定義できます。

**リクエスト**

次のリクエストは、[!DNL Shopify Streaming] データの Webhook を作成します。

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
| `webhook.address` | ストリーミングメッセージが送信される http エンドポイント。 |
| `webhook.topic` | Webhook サブスクリプションのトピック。 詳しくは、[[!DNL Shopify] webhook イベントトピックガイド ](https://shopify.dev/docs/api/admin-rest/2023-04/resources/webhook#event-topics) を参照してください。 |
| `webhook.format` | データの形式。 |

**応答**

応答が成功すると、Webhook に関する情報（対応する `id`、アドレス、その他のメタデータ情報を含む）が返されます。

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

[!DNL Shopify Streaming] ソースで Webhook を使用する際に発生する可能性のある既知の制限事項を以下に示します。

* 同じリソースに対して異なるトピックの配信順を並べ替えることができるとは限りません。 例えば、`products/create` Webhook より前に `products/update` Webhook が配信される可能性があります。
* Webhook イベントを少なくとも 1 回はエンドポイントに配信するように Webhook を設定できます。 つまり、1 つのエンドポイントが同じイベントを複数回受け取る可能性があります。 `X-Shopify-Webhook-Id` ヘッダーを以前のイベントと比較することで、重複する Webhook イベントをスキャンできます。
* [!DNL Shopify] は、HTTP 2xx ステータス応答を成功した通知として扱います。 その他のステータスコード応答は、失敗と見なされます。 [!DNL Shopify] は、失敗した webhook 通知の再試行メカニズムを提供します。 **5 秒待っても応答がない** 場合は、[!DNL Shopify] は次の **48 時間** の間、接続を **19 回** 再試行します。 再試行期間の終わりまでに応答がまだない場合、[!DNL Shopify] は Webhook を削除します。

## 次の手順

次のチュートリアルでは、API と UI を使用して [!DNL Shopify Streaming] ソースをExperience Platformに接続する方法の手順を説明します。

* [Flow Service API を使用した Shopify ストリーミングソース接続とデータフローの作成](../../tutorials/api/create/ecommerce/shopify-streaming.md)
* [UI での Shopify ストリーミングソース接続とデータフローの作成](../../tutorials/ui/create/ecommerce/shopify-streaming.md)
