---
keywords: Experience Platform；ホーム；インテリジェントサービス；人気の高いトピック；インテリジェントサービス；インテリジェントサービス
solution: Experience Platform, Intelligent Services
title: インテリジェントサービスで使用するデータの準備
topic: Intelligent Services
description: インテリジェントサービスがマーケティングイベントデータからインサイトを見つけるには、そのデータがセマンティックに強化され、標準構造で維持されている必要があります。 Intelligent Servicesでは、Experience Data Model(XDM)スキーマを使用してこれを実現します。
exl-id: 17bd7cc0-da86-4600-8290-cd07bdd5d262
translation-type: tm+mt
source-git-commit: 867c97d58f3496cb9e9e437712f81bd8929ba99f
workflow-type: tm+mt
source-wordcount: '2400'
ht-degree: 1%

---

# [!DNL Intelligent Services]で使用するデータを準備する

[!DNL Intelligent Services]がマーケティングイベントデータからインサイトを見つけるには、そのデータがセマンティックに強化され、標準構造で維持されている必要があります。 [!DNL Intelligent Services] これを達成す [!DNL Experience Data Model] るためには、XDM(R)スキーマを活用します。特に、[!DNL Intelligent Services]で使用されるすべてのデータセットは、Consumer ExperienceEvent(CEE)XDMスキーマに準拠するか、Adobe Analyticsコネクタを使用する必要があります。 また、カスタマーAIはAdobe Audience Managerコネクタをサポートしています。

このドキュメントでは、複数のチャネルからマーケティングイベントのデータをCEEスキーマにマッピングし、スキーマ内の重要なフィールドの概要を説明し、データを構造に効果的にマッピングする方法を判断する際の一般的な手順を説明します。 Adobe Analyticsのデータを使用する場合は、[Adobe Analyticsのデータ準備](#analytics-data)のセクションを表示してください。 Adobe Audience Managerデータ（お客様のAIのみ）を使用する予定がある場合は、[Adobeオーディエンスマネージャデータ準備](#AAM-data)のセクションに表示してください。

## ワークフローの概要

準備プロセスは、データがAdobe Experience Platformに保存されているか、外部に保存されているかによって異なります。 この節では、いずれかのシナリオで必要となる手順について要約します。

### 外部データの準備

データが[!DNL Experience Platform]の外部に保存されている場合は、次の手順に従います。

1. 専用のAzure BLOBストレージコンテナのアクセス資格情報を要求するには、Adobeコンサルティングサービスに問い合わせてください。
1. アクセス資格情報を使用して、データをBlobコンテナにアップロードします。
1. Adobeコンサルティングサービスで作業し、データを[Consumer ExperienceEventスキーマ](#cee-schema)にマッピングし、[!DNL Intelligent Services]に取り込みます。

### Adobe Analyticsデータの準備{#analytics-data}

顧客AIとAttribution AIは、Adobe Analyticsデータをネイティブでサポートします。 Adobe Analyticsデータを使用するには、ドキュメントに記載されている手順に従って[Analyticsソースコネクタ](../sources/tutorials/ui/create/adobe-applications/analytics.md)を設定します。

ソースコネクタがデータをExperience Platformにストリーミングすると、インスタンス設定時に、データソースとしてAdobe Analyticsを選択し、その後データセットを選択することができます。 すべての必須スキーマフィールドとミックスインは、接続の設定時に自動的に作成されます。 データセットをCEE形式にETL（抽出、変換、ロード）する必要はありません。

>[!IMPORTANT]
>
>Adobe Analyticsコネクターは、データをバックフィルするのに最大4週間かかります。 接続を最近設定した場合は、顧客やAttribution AIに必要な最小長のデータがデータセットに含まれていることを確認する必要があります。 [顧客AI](./customer-ai/input-output.md#data-requirements)または[Attribution AI](./attribution-ai/input-output.md#data-requirements)の履歴データのセクションを確認し、予測目標に十分なデータがあることを確認してください。

### Adobe Audience Managerデータの準備（お客様向けAIのみ） {#AAM-data}

顧客AIは、Adobe Audience Managerデータをネイティブでサポートします。 Audience Managerデータを使用するには、ドキュメントに記載されている手順に従って[Audience Managerソースコネクタ](../sources/tutorials/ui/create/adobe-applications/audience-manager.md)を設定します。

ソースコネクタがデータをExperience Platformにストリーミングすると、Customer AIの設定時に、データソースとしてAdobe Audience Managerを選択し、その後データセットを選択することができます。 すべての必須スキーマフィールドとミックスインは、接続の設定時に自動的に作成されます。 データセットをCEE形式にETL（抽出、変換、ロード）する必要はありません。

>[!IMPORTANT]
>
>コネクターを最近設定した場合は、データセットに必要な最小長のデータが含まれていることを確認する必要があります。 顧客AIの[入力/出力ドキュメント](./customer-ai/input-output.md)の履歴データの節を確認し、予測目標に十分なデータがあることを確認してください。

### [!DNL Experience Platform] データ準備

データが既に[!DNL Platform]に保存されており、Adobe AnalyticsまたはAdobe Audience Manager（Customer AIのみ）のソースコネクタを通じてストリーミングしない場合は、次の手順に従います。 お客様AIを利用する予定の場合は、CEEスキーマを理解することをお勧めします。

1. [コンシューマーエクスペリエンスイベントスキーマ](#cee-schema)の構造を確認し、データをフィールドにマップできるかどうかを判断します。
2. Adobeのコンサルティングサービスに連絡して、データをスキーマにマッピングし、[!DNL Intelligent Services]に取り込みます。自分でデータをマッピングする場合は、[このガイドの手順](#mapping)に従ってください。

## CEEスキーマについて{#cee-schema}

Consumer ExperienceEventスキーマでは、デジタルマーケティングイベント（Webまたはモバイル）、オンラインまたはオフラインのコマースアクティビティに関する個人の行動を説明します。 [!DNL Intelligent Services]では、意味的に適切に定義されたフィールド（列）があるので、このスキーマを使用する必要があります。そうしないとデータの明確性が損なわれる不明な名前を避けることができます。

CEEスキーマは、すべてのXDM ExperienceEventスキーマと同様に、ポイントインタイムや関係する件名のIDなど、イベント(またはイベントのセット)が発生したときのシステムの時系列ベースの状態を取り込みます。 エクスペリエンスイベントは、何が起こったかの実際の記録であり、そのため、集約や解釈なしに起こったことを不変で、表します。

[!DNL Intelligent Services] このスキーマ内のいくつかの主要フィールドを利用して、マーケティングイベントデータからインサイトを生成します。これらのすべてはルートレベルにあり、展開して必須サブフィールドを表示できます。

![](./images/data-preparation/schema-expansion.gif)

すべてのXDMスキーマと同様、CEEミックスインも拡張可能です。 つまり、CEEミックスインにはフィールドを追加でき、必要に応じて複数のスキーマに異なるバリエーションを含めることができます。

mixinの完全な例は、[パブリックXDMリポジトリ](https://github.com/adobe/xdm/blob/797cf4930d5a80799a095256302675b1362c9a15/docs/reference/context/experienceevent-consumer.schema.md)にあります。 また、次の[JSONファイル](https://github.com/AdobeDocs/experience-platform.en/blob/master/help/intelligent-services/assets/CEE_XDM_sample_rows.json)を表示してコピーすると、CEEスキーマに準拠するようにデータを構造化する方法の例を確認できます。 独自のデータをスキーマにマップする方法を決定するには、次の節で説明する主なフィールドについて学習する際に、これらの例を両方参照してください。

## キーフィールド

CEEミックスインにはいくつかの重要なフィールドがあり、[!DNL Intelligent Services]が有用な洞察を生成するために利用する必要があります。 この節では、これらのフィールドの使用事例と期待されるデータについて説明し、その他の例に関するリファレンスドキュメントへのリンクを示します。

### 必須フィールド

すべてのキーフィールドの使用を強くお勧めしますが、[!DNL Intelligent Services]が機能するためには、**必須**&#x200B;のフィールドが2つあります。

* [プライマリIDフィールド](#identity)
* [xdm:timestamp](#timestamp)
* [xdm:チャネル](#channel) (Attribution AIの場合のみ必須)

#### プライマリID {#identity}

スキーマ内のフィールドの1つは、プライマリIDフィールドとして設定する必要があります。これにより、[!DNL Intelligent Services]が時系列データの各インスタンスを個々の人にリンクできるようになります。

データのソースと特性に基づいて、主IDとして使用する最適なフィールドを決定する必要があります。 IDフィールドには、フィールドが値として期待するIDデータの種類を示す&#x200B;**ID名前空間**&#x200B;を含める必要があります。 有効な名前空間値には次のものがあります。

* &quot;電子メール&quot;
* &quot;phone&quot;
* &quot;mcid&quot;(Adobe Audience ManagerID用)
* &quot;aid&quot;(Adobe AnalyticsID用)

どのフィールドを主IDとして使用すべきかがわからない場合は、Adobeコンサルティングサービスに問い合わせて、最適なソリューションを決定してください。 プライマリIDが設定されていない場合、インテリジェントサービスアプリケーションは次のデフォルト動作を使用します。

| デフォルト | Attribution AI | 顧客 AI |
| --- | --- | --- |
| ID列 | `endUserIDs._experience.aaid.id` | `endUserIDs._experience.mcid.id` |
| 名前空間 | AAID | ECID |

プライマリIDを設定するには、「**[!UICONTROL スキーマ]**」タブからスキーマに移動し、スキーマ名のハイパーリンクを選択して&#x200B;**[!DNL Schema Editor]**&#x200B;を開きます。

![スキーマに移動](./images/data-preparation/navigate_schema.png)

次に、主要IDにするフィールドに移動し、それを選択します。 **[!UICONTROL フィールドのプロパティ]**&#x200B;メニューがそのフィールド用に開きます。

![フィールドを選択](./images/data-preparation/find_field.png)

**[!UICONTROL フィールドプロパティ]**&#x200B;メニューで、**[!UICONTROL 「ID」]**&#x200B;チェックボックスが見つかるまで下にスクロールします。 チェックボックスをオンにすると、選択したIDを&#x200B;**[!UICONTROL プライマリID]**&#x200B;として設定するオプションが表示されます。 このボックスも選択します。

![チェックボックスを選択](./images/data-preparation/set_primary_identity.png)

次に、ドロップダウン内の事前定義済みの名前空間のリストから&#x200B;**[!UICONTROL ID名前空間]**&#x200B;を指定する必要があります。 この例では、Adobe Audience ManagerID `mcid.id`が使用されているため、ECID名前空間が選択されています。 **[!UICONTROL 「]**&#x200B;適用」を選択して更新を確認し、右上隅の「**[!UICONTROL 保存]**」を選択してスキーマに変更を保存します。

![変更を保存する](./images/data-preparation/select_namespace.png)

#### xdm:timestamp {#timestamp}

このフィールドは、イベントが発生した日時を表します。 この値は、ISO 8601規格に従って、文字列として指定する必要があります。

#### xdm:channel {#channel}

>[!NOTE]
>
>このフィールドは、Attribution AIを使用する場合にのみ必須です。

このフィールドは、ExperienceEventに関連するマーケティングチャネルを表します。 このフィールドには、チャネルのタイプ、メディアのタイプ、場所のタイプに関する情報が含まれます。

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

`xdm:channel`に必要な各サブフィールドの詳細については、[エクスペリエンスチャネルスキーマ](https://github.com/adobe/xdm/blob/797cf4930d5a80799a095256302675b1362c9a15/docs/reference/channels/channel.schema.md)仕様を参照してください。 一部のマッピングの例については、](#example-channels)の下の[テーブルを参照してください。

#### チャネルマッピングの例{#example-channels}

次の表に、`xdm:channel`スキーマにマッピングされたマーケティングチャネルの例を示します。

| チャネル | `@type` | `mediaType` | `mediaAction` |
| --- | --- | --- | --- |
| 有料検索 | https:/<span>/ns.adobe.com/xdm/チャネルタイプ/search | 支払った | clicks |
| Social - Marketing | https:/<span>/ns.adobe.com/xdm/チャネルタイプ/social | 獲得した | クリック |
| 表示 | https:/<span>/ns.adobe.com/xdm/チャネルタイプ/表示 | 支払った | クリック |
| Email | https:/<span>/ns.adobe.com/xdm/チャネルタイプ/email | 支払った | クリック |
| 内部転送者 | https:/<span>/ns.adobe.com/xdm/チャネルタイプ/direct | 所有 | クリック |
| Display ViewThrough | https:/<span>/ns.adobe.com/xdm/チャネルタイプ/表示 | 支払った | インプレッション |
| QRコードのリダイレクト | https:/<span>/ns.adobe.com/xdm/チャネルタイプ/direct | 所有 | クリック |
| Mobile | https:/<span>/ns.adobe.com/xdm/チャネルタイプ/モバイル | 所有 | クリック |

### 推奨フィールド

この節では、その他の主要フィールドについて説明します。 [!DNL Intelligent Services]が機能するために必ずしもこれらのフィールドは必要ありませんが、より豊富な洞察を得るためには、できるだけ多くのフィールドを使用することを強くお勧めします。

#### xdm:productListItems

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

`xdm:productListItems`に必要な各サブフィールドの詳細については、[コマースの詳細スキーマ](https://github.com/adobe/xdm/blob/797cf4930d5a80799a095256302675b1362c9a15/docs/reference/context/experienceevent-commerce.schema.md)仕様を参照してください。

#### xdm:commerce

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

`xdm:commerce`に必要な各サブフィールドの詳細については、[コマースの詳細スキーマ](https://github.com/adobe/xdm/blob/797cf4930d5a80799a095256302675b1362c9a15/docs/reference/context/experienceevent-commerce.schema.md)仕様を参照してください。

#### xdm:web

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

`xdm:productListItems`に必要な各サブフィールドの詳細については、[ExperienceEvent Web詳細スキーマ](https://github.com/adobe/xdm/blob/797cf4930d5a80799a095256302675b1362c9a15/docs/reference/context/experienceevent-web.schema.md)仕様を参照してください。

#### xdm:marketing

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

`xdm:productListItems`に必要な各サブフィールドの詳細については、[marketing sechma](https://github.com/adobe/xdm/blob/797cf4930d5a80799a095256302675b1362c9a15/docs/reference/context/marketing.schema.md)仕様を参照してください。

## データのマッピングと取り込み{#mapping}

マーケティングイベントのデータをCEEスキーマにマッピングできるかどうかを判断したら、次の手順は[!DNL Intelligent Services]に取り込むデータを決定することです。 [!DNL Intelligent Services]で使用されるすべての履歴データは、4か月のデータの最小時間枠内に収まり、その後、ルックバック期間として使用される日数を加える必要があります。

送信するデータの範囲を決定したら、Adobeコンサルティングサービスに連絡して、データをスキーマにマッピングし、サービスに取り込む方法についてお問い合わせください。

[!DNL Adobe Experience Platform]購読をお持ちで、自分でデータをマッピングして取り込む場合は、次の節に示す手順に従ってください。

### Adobe Experience Platformの使用

>[!NOTE]
>
>以下の手順では、Experience Platformを購読する必要があります。 プラットフォームにアクセスできない場合は、[次の手順](#next-steps)のセクションに進んでください。

この節では、[!DNL Intelligent Services]で使用するデータをマッピングし、Experience Platformに取り込むためのワークフローについて説明します。詳細な手順のチュートリアルへのリンクも含まれます。

#### CEEスキーマとデータセットの作成

取り込み用にデータを準備する際に開始が発生する準備が整ったら、最初の手順は、CEEミックスインを使用する新しいXDMスキーマを作成することです。 次のチュートリアルでは、UIまたはAPIで新しいスキーマを作成するプロセスについて説明します。

* [UIでのスキーマの作成](../xdm/tutorials/create-schema-ui.md)
* [APIでのスキーマの作成](../xdm/tutorials/create-schema-api.md)

>[!IMPORTANT]
>
>上記のチュートリアルは、スキーマを作成するための一般的なワークフローに従っています。 スキーマ用のクラスを選択する場合は、**XDM ExperienceEventクラス**&#x200B;を使用する必要があります。 このクラスを選択したら、CEEミックスインをスキーマに追加できます。

CEEミックスインをスキーマに追加した後、データ内の追加フィールドに必要に応じて他のミックスインを追加できます。

スキーマを作成して保存したら、そのスキーマに基づいて新しいデータセットを作成できます。 以下のチュートリアルでは、UIまたはAPIで新しいデータセットを作成するプロセスについて説明します。

* [UIでのデータセットの作成](../catalog/datasets/user-guide.md#create) (既存のスキーマを使用する場合のワークフローに従います)
* [APIでのデータセットの作成](../catalog/datasets/create.md)

データセットの作成後は、**[!UICONTROL Datasets]**&#x200B;ワークスペース内のプラットフォームUIで見つけることができます。

![](images/data-preparation/dataset-location.png)

#### データセット追加のIDフィールド

[!DNL Adobe Audience Manager]、[!DNL Adobe Analytics]、または他の外部ソースからデータを取り込む場合は、スキーマフィールドをIDフィールドとして設定できます。 スキーマフィールドをIDフィールドとして設定するには、スキーマ作成用の[UIチュートリアル](../xdm/tutorials/create-schema-ui.md#identity-field)または[APIチュートリアル](../xdm/tutorials/create-schema-api.md#define-an-identity-descriptor)内のIDフィールドの設定に関する節を表示します。

ローカルCSVファイルからデータを取り込む場合は、[マッピングと取り込みデータ](#ingest)の次のセクションに進むことができます。

#### データのマッピングと取り込み{#ingest}

CEEスキーマとデータセットを作成したら、データテーブルをスキーマに開始マッピングし、そのデータをプラットフォームに取り込むことができます。 UIでのCSVファイルの実行方法については、[CSVファイルのXDMスキーマへのマッピング](../ingestion/tutorials/map-a-csv-file.md)のチュートリアルを参照してください。 以下の[サンプルJSONファイル](https://github.com/AdobeDocs/experience-platform.en/blob/master/help/intelligent-services/assets/CEE_XDM_sample_rows.json)を使用して、独自のデータを使用する前にインジェストプロセスをテストできます。

データセットを取り込むと、同じデータセットを使用して他のデータファイルを取り込むことができます。

サポートされているサードパーティアプリケーションにデータが保存されている場合は、[ソースコネクタ](../sources/home.md)を作成してマーケティングイベントのデータを[!DNL Platform]にリアルタイムで取り込むように選択することもできます。

## 次の手順 {#next-steps}

このドキュメントは、[!DNL Intelligent Services]で使用するデータの準備に関する一般的なガイダンスを提供しました。 使用事例に基づく追加のコンサルティングが必要な場合は、Adobeコンサルティングサポートにお問い合わせください。

顧客体験データのデータセットへの入力が完了したら、[!DNL Intelligent Services]を使用してインサイトを生成できます。 開始するには、次のドキュメントを参照してください。

* [Attribution AI の概要](./attribution-ai/overview.md)
* [Customer AI の概要](./customer-ai/overview.md)
