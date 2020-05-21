---
keywords: Experience Platform;home;intelligent services;popular topics
solution: Experience Platform
title: Intelligent Servicesで使用するデータの準備
topic: Intelligent Services
translation-type: tm+mt
source-git-commit: 8e24c7c50d700bc3644ce710f77073e537207a6f
workflow-type: tm+mt
source-wordcount: '1445'
ht-degree: 1%

---


# Intelligent Servicesで使用するデータの準備

インテリジェントサービスがマーケティングイベントデータからインサイトを見つけるには、そのデータがセマンティックに強化され、標準構造で維持されている必要があります。 Intelligent Servicesは、これを達成するためにExperience Data Model(XDM)スキーマを活用します。 特に、インテリジェントサービスで使用されるすべてのデータセットは、 **Consumer ExperienceEvent(CEE)** XDMスキーマに準拠している必要があります。

このドキュメントでは、複数のチャネルからこのスキーマへのマーケティングイベントデータのマッピング、スキーマ内の重要なフィールドの概要を説明し、データを構造に効果的にマッピングする方法を判断する際の一般的な手引きを示します。

## CEEスキーマについて

Consumer ExperienceEventスキーマでは、デジタルマーケティングイベント（Webまたはモバイル）、オンラインまたはオフラインのコマースアクティビティに関する個人の行動を説明します。 このスキーマの使用は、Intelligent Servicesで必要となるのは、意味的に適切に定義されたフィールド（列）であるためです。そうしないとデータが明確にならなくなる不明な名前は一切避けられます。

インテリジェントサービスは、このスキーマ内の複数の主要フィールドを利用してマーケティングイベントデータからインサイトを生成します。これらのすべてはルートレベルで見つかり、展開して必須サブフィールドを表示できます。

![](./images/data-preparation/schema-expansion.gif)

すべてのXDMスキーマと同様、CEEミックスインも拡張可能です。 つまり、CEEミックスインにはフィールドを追加でき、必要に応じて複数のスキーマに異なるバリエーションを含めることができます。

Mixinの完全な例は、 [public XDM repositoryにあります](https://github.com/adobe/xdm/blob/797cf4930d5a80799a095256302675b1362c9a15/docs/reference/context/experienceevent-consumer.schema.md)。以下の節で説明するキーフィールドの参照として使用してください。

## キーフィールド

以下の節では、CEEミックスイン内の主なフィールドを強調して、インテリジェントサービスが有用なインサイトを生成できるようにする必要があります。例えば、その他の例に関するリファレンスドキュメントの説明やリンクが含まれます。

>[!IMPORTANT] アトリビューションAIがデータを処理するには、 `xdm:channel` フィールド（下の最初の節で説明）が **必要ですが** 、顧客AIには必須フィールドがありません。 その他のすべての主要フィールドは強くお勧めしますが、必須ではありません。

### xdm:チャネル

このフィールドは、ExperienceEventに関連するマーケティングチャネルを表します。 このフィールドには、チャネルのタイプ、メディアのタイプ、場所のタイプに関する情報が含まれます。 **アトリビューションAIがデータを処理できるようにするには、このフィールド&#x200B;_を指定する必要があります_**。

![](./images/data-preparation/channel.png)

**スキーマ例**

```json
{
  "@id": "https://ns.adobe.com/xdm/channels/facebook-feed",
  "@type": "https://ns.adobe.com/xdm/channel-types/social",
  "xdm:mediaType": "earned",
  "xdm:mediaAction": "clicks"
}
```

各必須サブフィールドの詳細については、 `xdm:channel`エクスペリエンスチャネルスキーマ [](https://github.com/adobe/xdm/blob/797cf4930d5a80799a095256302675b1362c9a15/docs/reference/channels/channel.schema.md) 仕様を参照してください。 マッピングの例については、次の [表を参照してください](#example-channels)。

#### チャネルマッピングの例 {#example-channels}

次の表に、 `xdm:channel` スキーマにマッピングされたマーケティングチャネルの例を示します。

| Channel | `@type` | `mediaType` | `mediaAction` |
| --- | --- | --- | --- |
| 有料検索 | https:/<span>/ns.adobe | 支払った | clicks |
| Social - Marketing | https:/<span>/ns.adobe | 獲得した | clicks |
| 表示 | https:/<span>/ns.adobe | 支払った | clicks |
| 電子メール | https:/<span>/ns.adobe | 支払った | clicks |
| 内部転送者 | https:/<span>/ns.adobe | 所有 | clicks |
| Display ViewThrough | https:/<span>/ns.adobe | 支払った | インプレッション |
| QRコードのリダイレクト | https:/<span>/ns.adobe | 所有 | clicks |
| Mobile | https:/<span>/ns.adobe | 所有 | clicks |

### xdm:productListItems

このフィールドは、製品のSKU、名前、価格、数量など、顧客が選択した製品を表す項目の配列です。

![](./images/data-preparation/productListItems.png)

**スキーマ例**

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

各必須サブフィールドの詳細については、「 `xdm:productListItems`コマース詳細スキーマ [](https://github.com/adobe/xdm/blob/797cf4930d5a80799a095256302675b1362c9a15/docs/reference/context/experienceevent-commerce.schema.md) 」仕様を参照してください。

### xdm:commerce

このフィールドには、発注書番号や支払い情報など、ExperienceEventに関するコマース固有の情報が含まれます。

![](./images/data-preparation/commerce.png)

**スキーマ例**

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

各必須サブフィールドの詳細については、「 `xdm:commerce`コマース詳細スキーマ [](https://github.com/adobe/xdm/blob/797cf4930d5a80799a095256302675b1362c9a15/docs/reference/context/experienceevent-commerce.schema.md) 」仕様を参照してください。

### xdm:web

このフィールドは、インタラクション、ページの詳細、転送者など、ExperienceEventに関するWebの詳細を表します。

![](./images/data-preparation/web.png)

**スキーマ例**

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

に必要な各サブフィールドの詳細については、ExperienceEvent Web詳細スキーマ `xdm:productListItems`仕様を参照して [](https://github.com/adobe/xdm/blob/797cf4930d5a80799a095256302675b1362c9a15/docs/reference/context/experienceevent-web.schema.md) ください。

### xdm:marketing

このフィールドには、タッチポイントでアクティブなマーケティングアクティビティに関する情報が含まれています。

![](./images/data-preparation/marketing.png)

**スキーマ例**

```json
{
  "xdm:trackingCode": "marketingcampaign111",
  "xdm:campaignGroup": "50%_DISCOUNT",
  "xdm:campaignName": "50%_DISCOUNT_USA"
}
```

の必須サブフィールドの詳細については、 `xdm:productListItems`marketing sechma [](https://github.com/adobe/xdm/blob/797cf4930d5a80799a095256302675b1362c9a15/docs/reference/context/marketing.schema.md) 仕様を参照してください。

## データのマッピングと取り込み

マーケティングイベントのデータをCEEスキーマにマッピングできるかどうかを判断したら、次の手順は、インテリジェントサービスに取り込むデータを決定することです。 Intelligent Servicesで使用されるすべての履歴データは、4か月のデータの最小期間に加え、ルックバック期間として予定されている日数に該当する必要があります。

送信するデータの範囲を決定したら、Adobe Consulting Servicesに連絡して、データをスキーマにマッピングし、サービスに取り込む方法についてお問い合わせください。

Adobe Experience Platformの購読がいて、自分でデータをマッピングし取り込む場合は、次の節に示す手順に従います。

### Adobe Experience Platformの使用

>[!NOTE] 以下の手順では、購読がエクスペリエンスプラットフォームを使用する必要があります。 プラットフォームにアクセスできない場合は、 [次の手順](#next-steps) 「」セクションに進みます。

この節では、データをマッピングし、インテリジェントサービスで使用するためのエクスペリエンスプラットフォームに取り込むためのワークフローについて説明します。詳細な手順のチュートリアルへのリンクも含まれます。

#### CEEスキーマとデータセットの作成

取り込み用にデータを準備する際に開始が発生する準備が整ったら、最初の手順は、CEEミックスインを使用する新しいXDMスキーマを作成することです。 次のチュートリアルでは、UIまたはAPIで新しいスキーマを作成するプロセスについて説明します。

* [UIでのスキーマの作成](../xdm/tutorials/create-schema-ui.md)
* [APIでのスキーマの作成](../xdm/tutorials/create-schema-api.md)

>[!IMPORTANT] 上記のチュートリアルは、スキーマを作成するための一般的なワークフローに従っています。 スキーマのクラスを選択する場合は、 **XDM ExperienceEventクラスを使用する必要があります**。 このクラスを選択したら、CEEミックスインをスキーマに追加できます。

CEEミックスインをスキーマに追加した後、データ内の追加フィールドに必要に応じて他のミックスインを追加できます。

スキーマを作成して保存したら、そのスキーマに基づいて新しいデータセットを作成できます。 以下のチュートリアルでは、UIまたはAPIで新しいデータセットを作成するプロセスについて説明します。

* [UIでのデータセットの作成](../catalog/datasets/user-guide.md#create) (既存のスキーマを使用する場合のワークフローに従います)
* [APIでのデータセットの作成](../catalog/datasets/create.md)

#### データセット追加の主なID名前空間タグ

Adobeオーディエンスマネージャー、Adobe Analyticsまたは他の外部ソースからデータを取り込む場合は、データセットに `primaryIdentityNameSpace` タグを追加する必要があります。 これは、カタログサービスAPIにPATCHリクエストを行うことで行うことができます。

ローカルCSVファイルからデータを取り込む場合は、データの [マッピングと取り込みに関する次の節に進むことができます](#ingest)。

以下のAPI呼び出しの例に従う前に、カタログ開発ガイドの [はじめにの節](../catalog/api/getting-started.md) 、必要なヘッダーに関する重要な情報を参照してください。

**API形式**

```http
PATCH /dataSets/{DATASET_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{DATASET_ID}` | 前に作成したデータセットのID。 |

**リクエスト**

データを取り込む元のソースに応じて、リクエストペイロードに適切な `primaryIdentityNamespace` タグ値と `sourceConnectorId` タグ値を指定する必要があります。

次のリクエストで、オーディエンスマネージャーに適したタグ値が追加されます。

```shell
curl -X PATCH \
  https://platform.adobe.io/data/foundation/catalog/dataSets/5ba9452f7de80400007fc52a \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
        "tags": {
          "primaryIdentityNameSpace": ["mcid"],
          "sourceConnectorId": ["audiencemanager"],
        }
      }'
```

次のリクエストによって、Analyticsに適したタグ値が追加されます。

```shell
curl -X PATCH \
  https://platform.adobe.io/data/foundation/catalog/dataSets/5ba9452f7de80400007fc52a \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
        "tags": {
          "primaryIdentityNameSpace": ["aaid"],
          "sourceConnectorId": ["analytics"],
        }
      }'
```

>[!NOTE] プラットフォームでのID名前空間の操作について詳しくは、「 [ID名前空間の概要](../identity-service/namespaces.md)」を参照してください。

**応答**

正常に完了すると、更新されたデータセットのIDを含む配列が返されます。 このIDは、PATCH要求で送信されたIDと一致する必要があります。

```json
[
    "@/dataSets/5ba9452f7de80400007fc52a"
]
```

#### データのマッピングと取り込み {#ingest}

CEEスキーマとデータセットを作成したら、データテーブルをスキーマに開始マッピングし、そのデータをプラットフォームに取り込むことができます。 UIでCSVファイルを実行する方法については、CSVファイルのXDMスキーマ [への](../ingestion/tutorials/map-a-csv-file.md) マッピングに関するチュートリアルを参照してください。 データセットを取り込むと、同じデータセットを使用して他のデータファイルを取り込むことができます。

サポートされるサードパーティアプリケーションにデータを格納している場合は、 [ソースコネクタを作成してマーケティングイベントのデータをリアルタイムでプラットフォームに取り込むこともできます](../sources/home.md) 。

## 次の手順 {#next-steps}

このドキュメントでは、インテリジェントサービスで使用するデータを準備する際の一般的なガイダンスを提供しました。 使用事例に基づく追加のコンサルティングが必要な場合は、アドビのコンサルティングサポートにお問い合わせください。

顧客体験データのデータセットへの入力が完了すると、インテリジェントサービスを使用してインサイトを生成できます。 開始するには、次のドキュメントを参照してください。

* [アトリビューションAIの概要](./attribution-ai/overview.md)
* [顧客 AI の概要](./customer-ai/overview.md)
