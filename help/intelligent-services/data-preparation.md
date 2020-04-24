---
keywords: Experience Platform;home;intelligent services;popular topics
solution: Experience Platform
title: Intelligent Servicesで使用するデータの準備
topic: Intelligent Services
translation-type: tm+mt
source-git-commit: 1b367eb65d1e592412d601d089725671e42b7bbd

---


# Intelligent Servicesで使用するデータの準備

インテリジェントサービスがマーケティングイベントデータからの洞察を見つけるには、そのデータがセマンティックに強化され、標準構造で維持されている必要があります。 Intelligent Servicesは、これを達成するためにExperience Data Model(XDM)スキーマを活用します。 特に、Intelligent Servicesで使用されるすべてのデータセットは、 **Consumer ExperienceEvent(CEE)** XDMスキーマに準拠している必要があります。

このドキュメントでは、複数のチャネルのマーケティングイベントのデータをこのスキーマにマッピングし、スキーマ内の重要なフィールドの情報をまとめ、データを効率的に構造にマッピングする方法を判断する際の一般的な手引きを示します。

## CEEについてのスキーマ

Consumer ExperienceEventスキーマは、デジタルマーケティングイベント（Webまたはモバイル）、オンラインまたはオフラインのコマースアクティビティに関する個人の行動を説明します。 このスキーマの使用は、意味的に適切に定義されたフィールド（列）があるので、Intelligent Servicesでは必須です。そうしないと、データの明確性が低下する不明な名前は避けられます。

インテリジェントサービスは、このスキーマ内の複数の主要なフィールドを利用してマーケティングイベントのデータからインサイトを生成します。これらのすべてはルートレベルで見つかり、展開して必須のサブフィールドを表示できます。

![](./images/data-preparation/schema-expansion.gif)

すべてのXDMスキーマと同様、CEEミックスインも拡張可能です。 つまり、CEEミックスインにフィールドを追加し、必要に応じて複数のスキーマに異なるバリエーションを含めることができます。

mixinの完全な例は、 [public XDM repositoryにあります](https://github.com/adobe/xdm/blob/797cf4930d5a80799a095256302675b1362c9a15/docs/reference/context/experienceevent-consumer.schema.md)。以下の節で概要を説明するキーフィールドの参照として使用してください。

## キーフィールド

以下の節では、CEEミックスイン内の主要なフィールドを強調し、インテリジェントサービスが有用なインサイトを生成するために利用する必要があります。例えば、その他の例に関する説明や参照ドキュメントへのリンクが含まれます。

### xdm:チャネル

このフィールドは、ExperienceEventに関連するマーケティングチャネルを表します。 このフィールドには、チャネルの種類、メディアの種類、場所の種類に関する情報が含まれます。 **アトリビュー&#x200B;_ション_AIがデータを処理するには、このフィールドを指定する必要があります**。

![](./images/data-preparation/channel.png)

**サンプルスキーマ**

```json
{
  "@id": "https://ns.adobe.com/xdm/channels/facebook-feed",
  "@type": "https://ns.adobe.com/xdm/channel-types/social",
  "xdm:mediaType": "earned",
  "xdm:mediaAction": "clicks"
}
```

の必須サブフィールドの詳細については、エクスペリエンスの `xdm:channel`チャネルスキーマ仕様を参照してく [ださ](https://github.com/adobe/xdm/blob/797cf4930d5a80799a095256302675b1362c9a15/docs/reference/channels/channel.schema.md) い。 マッピングの例については、以下の表を参照 [してください](#example-channels)。

#### チャネルマッピングの例 {#example-channels}

次の表に、スキーマにマッピングされたマーケティングチャネルの例を示 `xdm:channel` します。

| Channel | `@type` | `mediaType` | `mediaAction` |
| --- | --- | --- | --- |
| 有料検索 | https:/<span>/ns.adobe.com/xdm/チャネルタイプ/search | 支払った | clicks |
| Social - Marketing | https:/<span>/ns.adobe.com/xdm/チャネルタイプ/social | 獲得した | clicks |
| 表示 | https:/<span>/ns.adobe.com/xdm/チャネルタイプ/display | 支払った | clicks |
| 電子メール | https:/<span>/ns.adobe.com/xdm/チャネルタイプ/email | 支払った | clicks |
| 内部転送者 | https:/<span>/ns.adobe.com/xdm/チャネルタイプ/direct | 所有 | clicks |
| ビュースルーを表示 | https:/<span>/ns.adobe.com/xdm/チャネルタイプ/display | 支払った | インプレッション |
| QRコードのリダイレクト | https:/<span>/ns.adobe.com/xdm/チャネルタイプ/direct | 所有 | clicks |
| Mobile | https:/<span>/ns.adobe.com/xdm/チャネルタイプ/mobile | 所有 | clicks |

### xdm:productListItems

このフィールドは、製品のSKU、名前、価格、数量など、顧客が選択した製品を表す項目の配列です。

![](./images/data-preparation/productListItems.png)

**サンプルスキーマ**

```json
[
  {
    "xdm:SKU": "1002352692",
    "xdm:name": "24-Watt 8-Light Chrome Integrated LED Bath Light",
    "xdm:currencyCode": "USD",
    "xdm:quantity": 1,
    "xdm:priceTotal": 159.45
  },
  {
    "xdm:SKU": "3398033623",
    "xdm:name": "16ft RGB LED Strips",
    "xdm:currencyCode": "USD",
    "xdm:quantity": 1,
    "xdm:priceTotal": 79.99
  }
]
```

の必須サブフィールドの詳細については、コマースの詳細 `xdm:productListItems`スキーマ仕様を [参照してください](https://github.com/adobe/xdm/blob/797cf4930d5a80799a095256302675b1362c9a15/docs/reference/context/experienceevent-commerce.schema.md) 。

### xdm:commerce

このフィールドには、発注書番号や支払い情報など、ExperienceEventに関するコマース固有の情報が含まれます。

![](./images/data-preparation/commerce.png)

**サンプルスキーマ**

```json
{
    "xdm:order": {
      "xdm:purchaseID": "a8g784hjq1mnp3",
      "xdm:purchaseOrderNumber": "123456",
      "xdm:payments": [
        {
          "xdm:transactionID": "transactid-a111",
          "xdm:paymentAmount": 59,
          "xdm:paymentType": "credit_card",
          "xdm:currencyCode": "USD"
        },
        {
          "xdm:transactionId": "transactid-a222",
          "xdm:paymentAmount": 100,
          "xdm:paymentType": "gift_card",
          "xdm:currencyCode": "USD"
        }
      ],
      "xdm:currencyCode": "USD",
      "xdm:priceTotal": 159
    },
    "xdm:purchases": {
      "xdm:value": 1
    }
  }
```

の必須サブフィールドの詳細については、コマースの詳細 `xdm:commerce`スキーマ仕様を [参照してください](https://github.com/adobe/xdm/blob/797cf4930d5a80799a095256302675b1362c9a15/docs/reference/context/experienceevent-commerce.schema.md) 。

### xdm:web

このフィールドは、インタラクション、ページの詳細、転送者など、ExperienceEventに関連するWebの詳細を表します。

![](./images/data-preparation/web.png)

**サンプルスキーマ**

```json
{
  "xdm:webPageDetails": {
    "xdm:siteSection": "Shopping Cart",
    "xdm:server": "example.com",
    "xdm:name": "Purchase Confirmation",
    "xdm:URL": "https://www.example.com/orderConf",
    "xdm:errorPage": false,
    "xdm:homePage": false,
    "xdm:pageViews": {
      "xdm:value": 1
    }
  },
  "xdm:webReferrer": {
    "xdm:URL": "https://www.example.com/checkout",
    "xdm:referrerType": "internal"
  }
}
```

各必須サブフィールドの詳細については、ExperienceEvent Webの詳細 `xdm:productListItems`スキーマ [](https://github.com/adobe/xdm/blob/797cf4930d5a80799a095256302675b1362c9a15/docs/reference/context/experienceevent-web.schema.md) 。

### xdm:marketing

このフィールドには、タッチポイントでアクティブなマーケティングアクティビティに関する情報が含まれています。

![](./images/data-preparation/marketing.png)

**サンプルスキーマ**

```json
{
  "xdm:trackingCode": "marketingcampaign111",
  "xdm:campaignGroup": "50%_DISCOUNT",
  "xdm:campaignName": "50%_DISCOUNT_USA"
}
```

各必須サブフィールドの詳細については、「マーケティング `xdm:productListItems`の仕様」を参照し [てください](https://github.com/adobe/xdm/blob/797cf4930d5a80799a095256302675b1362c9a15/docs/reference/context/marketing.schema.md) 。

## データのマッピングと取り込み

マーケティングイベントのデータをCEEスキーマにマッピングできるかどうかを判断したら、データをインテリジェントサービスに取り込むプロセスを開始できます。 データをアドビのコンサルティングサービスにマッピングし、スキーマに取り込む方法については、アドビのコンサルティングサービスにお問い合わせください。

Adobe Experience Platform購読をお持ちで、データを自分でマッピングして取り込む場合は、次の節で説明する手順に従います。

### Adobe Experience Platformの使用

>[!NOTE] 次の手順では、エクスペリエンスプラットフォームへの購読が必要です。 プラットフォームにアクセスできない場合は、次の手順に進ん [でください](#next-steps) 。

この節では、詳細な手順のチュートリアルへのリンクを含め、Intelligent Servicesで使用するデータをExperience Platformにマッピングして取り込むためのワークフローについて説明します。

#### CEEデータセットとスキーマの作成

取り込むデータを準備する開始の準備が整ったら、まずCEEミックスインを使用する新しいXDMスキーマを作成します。 次のチュートリアルでは、UIまたはAPIで新しいスキーマを作成するプロセスを説明します。

* [UIでのスキーマの作成](../xdm/tutorials/create-schema-ui.md)
* [APIでのスキーマの作成](../xdm/tutorials/create-schema-api.md)

>[!IMPORTANT] 上記のチュートリアルは、ワークフローを作成するための一般的なスキーマです。 スキーマのクラスを選択する場合は、 **XDM ExperienceEventクラスを使用する必要があります**。 このクラスを選択したら、CEEミックスインをスキーマに追加。

CEEミックスインをスキーマに追加した後、データ内の追加のフィールドに必要に応じて他のミックスインを追加できます。

データセットを作成して保存した後、スキーマに基づいて新しいデータセットをスキーマします。 次のチュートリアルでは、UIまたはAPIで新しいデータセットを作成するプロセスについて説明します。

* [UIでのデータセットの作成](../catalog/datasets/user-guide.md#create) （既存のデータセットの使用ワークフローに従います）
* [APIでのデータセットの作成](../catalog/datasets/create.md)

#### データのマッピングと取り込み

CEEスキーマとデータセットを作成したら、データテーブルをスキーマに開始マッピングし、そのデータをプラットフォームに取り込むことができます。 UIでCSVファイルを実行す [る手順については、CSVファイルをXDMスキーマにマッピングする](../ingestion/tutorials/map-a-csv-file.md) （英語のみ）のチュートリアルを参照してください。 データセットを設定したら、同じデータセットを使用して追加のデータファイルを取り込むことができます。

## 次の手順 {#next-steps}

このドキュメントでは、インテリジェントサービスで使用するデータの準備に関する一般的なガイダンスを提供しました。 使用事例に基づいて追加のコンサルティングが必要な場合は、アドビのコンサルティングサポートにお問い合わせください。

データセットに顧客体験データを正しく埋め込むと、インテリジェントサービスを使用してインサイトを生成できます。 使用を開始するには、次のドキュメントを参照してください。

* [アトリビューションAIの概要](./attribution-ai/overview.md)
* [顧客 AI の概要](./customer-ai/overview.md)
