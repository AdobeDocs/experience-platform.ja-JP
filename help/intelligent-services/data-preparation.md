---
keywords: Experience Platform；ホーム；インテリジェントサービス；人気のあるトピック；インテリジェントサービス；インテリジェントサービス
solution: Experience Platform, Intelligent Services
title: インテリジェントサービスで使用するデータの準備
topic-legacy: Intelligent Services
description: インテリジェントサービスでマーケティングイベントデータからインサイトを検出するには、データがセマンティックにエンリッチメントされ、標準構造で維持されている必要があります。 インテリジェントサービスでは、エクスペリエンスデータモデル (XDM) スキーマを使用してこれを実現します。
exl-id: 17bd7cc0-da86-4600-8290-cd07bdd5d262
source-git-commit: aa73f8f4175793e82d6324b7c59bdd44bf8d20f9
workflow-type: tm+mt
source-wordcount: '2766'
ht-degree: 1%

---

# [!DNL Intelligent Services] で使用するデータを準備する

[!DNL Intelligent Services] がマーケティングイベントデータからインサイトを見つけるには、データが意味的にエンリッチメントされ、標準構造で維持されている必要があります。 [!DNL Intelligent Services] これを実 [!DNL Experience Data Model] 現するために、(XDM) スキーマを活用します。特に、[!DNL Intelligent Services] で使用されるすべてのデータセットは、Consumer ExperienceEvent(CEE)XDM スキーマに準拠しているか、Adobe Analyticsコネクタを使用している必要があります。 また、顧客 AI はAdobe Audience Managerコネクタをサポートしています。

このドキュメントでは、マーケティングイベントデータを複数のチャネルから CEE スキーマにマッピングし、スキーマ内の重要なフィールドの情報を概説して、データを構造に効果的にマッピングする方法を決定します。 Adobe Analyticsデータを使用する予定がある場合は、[Adobe Analyticsデータの準備 ](#analytics-data) の節を参照してください。 Adobe Audience Managerデータ（顧客 AI のみ）を使用する予定がある場合は、[AdobeAudience Manager データの準備 ](#AAM-data) の節を参照してください。

## データ要件

[!DNL Intelligent Services] では、作成する目標に応じて、様々な量の履歴データが必要です。**すべての** [!DNL Intelligent Services] に対して準備するデータには、肯定的なカスタマージャーニー/イベントと否定的なカスタマージャーニー/イベントの両方を含める必要があります。 負のイベントと正のイベントの両方を持つことで、モデルの精度と精度が向上します。

例えば、顧客 AI を使用して製品を購入する傾向を予測する場合、顧客 AI のモデルには、成功した購入パスの例と失敗したパスの例の両方が必要です。 これは、モデルトレーニングの間、顧客 AI はどのイベントやジャーニーが購入につながるかを把握しようとするからです。 また、購入しなかった顧客が実行するアクション（買い物かごへの項目の追加時にジャーニーを停止した顧客など）も含まれます。 これらの顧客は同様の行動を示す場合がありますが、顧客 AI はインサイトを提供し、傾向スコアの向上につながる主な違いと要因を掘り下げることができます。 同様に、Attribution AIでは、タッチポイントの効果、上位のコンバージョンパス、タッチポイント位置による分類などの指標を表示するために、両方のタイプのイベントとジャーニーが必要です。

履歴Attribution AIの要件のその他の例と情報については、入出力ドキュメントの [ 顧客 AI](./customer-ai/input-output.md#data-requirements) または [ データ ](./attribution-ai/input-output.md#data-requirements) 履歴データの要件の節を参照してください。

### データの結び付けのガイドライン

可能な場合は、共通の ID をまたいでユーザーのイベントを結び付けることをお勧めします。 例えば、「id1」を持つユーザーデータが 10 件のイベントにわたって存在するとします。 その後、同じユーザーが cookie id を削除し、次の 20 件のイベントで「id2」として記録されます。 id1 と id2 が同じユーザーに対応することがわかっている場合は、30 件のイベントすべてを共通の ID で結び付けることをお勧めします。

これができない場合は、モデル入力データを作成する際に、各イベントのセットを別々のユーザーとして扱う必要があります。 これにより、モデルのトレーニングとスコアリングの最適な結果が得られます。

## ワークフローの概要

準備プロセスは、データがAdobe Experience Platformに格納されているか外部に格納されているかによって異なります。 この節では、どちらのシナリオでも、実行する必要がある手順の概要を説明します。

### 外部データの準備

データをExperience Platformの外部に保存する場合は、[Consumer ExperienceEvent スキーマ ](#cee-schema) の必須フィールドと関連フィールドにデータをマッピングする必要があります。 このスキーマをカスタムフィールドグループで拡張して、顧客データをより適切に取り込むことができます。 マッピングが完了すると、Consumer ExperienceEvent スキーマを使用してデータセットを作成し、[Platform](../ingestion/home.md) にデータを取り込むことができます。 その後、[!DNL Intelligent Service] を設定する際に CEE データセットを選択できます。

使用する [!DNL Intelligent Service] に応じて、異なるフィールドが必要になる場合があります。 データを使用できる場合は、フィールドにデータを追加することをお勧めします。 必須フィールドの詳細については、[Attribution AI](./attribution-ai/input-output.md) または [ 顧客 AI](./customer-ai/input-output.md) 入力/出力ガイドを参照してください。

### Adobe Analyticsデータの準備 {#analytics-data}

顧客 AI とAttribution AIは、Adobe Analyticsデータをネイティブでサポートします。 Adobe Analyticsデータを使用するには、ドキュメントで説明されている手順に従って [Analytics ソースコネクタ ](../sources/tutorials/ui/create/adobe-applications/analytics.md) を設定します。

ソースコネクタがデータをExperience Platformにストリーミングすると、インスタンスの設定時に、Adobe Analyticsをデータソースとして選択し、その後にデータセットを選択できます。 すべての必須スキーマフィールドグループと個々のフィールドは、接続設定時に自動的に作成されます。 データセットを CEE 形式に ETL（抽出、変換、読み込み）する必要はありません。

>[!IMPORTANT]
>
>Adobe Analyticsコネクタは、データのバックフィルに最大 4 週間かかります。 最近接続を設定した場合は、顧客またはAttribution AIに必要な最小長のデータがデータセットに含まれていることを確認してください。 [ 顧客 AI](./customer-ai/input-output.md#data-requirements) または [Attribution AI](./attribution-ai/input-output.md#data-requirements) の履歴データの節を確認し、予測目標に十分なデータがあることを確認してください。

### Adobe Audience Managerデータの準備（顧客 AI のみ） {#AAM-data}

顧客 AI はAdobe Audience Managerデータをネイティブにサポートします。 Audience Managerデータを使用するには、ドキュメントに記載されている手順に従って [Audience Managerソースコネクタ ](../sources/tutorials/ui/create/adobe-applications/audience-manager.md) を設定します。

ソースコネクタがデータをExperience Platformにストリーミングすると、顧客 AI の設定時に、Adobe Audience Managerをデータソースとして選択し、その後にデータセットを選択できます。 すべてのスキーマフィールドグループと個々のフィールドは、接続設定時に自動的に作成されます。 データセットを CEE 形式に ETL（抽出、変換、読み込み）する必要はありません。

>[!IMPORTANT]
>
>コネクタを最近設定した場合は、データセットの長さが最小限必要なデータであることを確認する必要があります。 顧客 AI の [ 入出力ドキュメント ](./customer-ai/input-output.md) の履歴データの節を確認し、予測目標に十分なデータがあることを確認してください。

### [!DNL Experience Platform] データの準備

データが既に [!DNL Platform] に保存され、Adobe AnalyticsまたはAdobe Audience Manager（顧客 AI のみ）のソースコネクタを介したストリーミングではない場合は、次の手順に従います。 CEE スキーマを理解することをお勧めします。

1. [Consumer ExperienceEvent スキーマ ](#cee-schema) の構造を確認し、データをそのフィールドにマッピングできるかどうかを判断します。
2. Adobeコンサルティングサービスに問い合わせて、データをスキーマにマッピングし [!DNL Intelligent Services] に取り込む方法について問い合わせてください。また、データを自分でマッピングする場合は、このガイドの手順 ](#mapping) に従ってください。[

## CEE スキーマについて {#cee-schema}

「消費者エクスペリエンスイベント」スキーマは、デジタルマーケティングイベント（Web またはモバイル）、オンラインまたはオフラインのコマースアクティビティに関連する個人の行動を記述します。 [!DNL Intelligent Services] では、意味的に適切に定義されたフィールド（列）があるので、このスキーマを使用する必要があります。そうしないと、データを明確にしなくなる不明な名前を避けることができます。

CEE スキーマは、すべての XDM ExperienceEvent スキーマと同様に、イベント（または一連のイベント）が発生したときのシステムの時系列ベースの状態（ポイントインタイムや関係する主体の ID など）を取り込みます。 エクスペリエンスイベントは、発生した事実の記録なので、不変で、発生した事実を集計や解釈なしで表します。

[!DNL Intelligent Services] このスキーマ内の複数の主要フィールドを利用して、マーケティングイベントデータからインサイトを生成します。すべてのデータはルートレベルで見つかり、展開して必要なサブフィールドを表示できます。

![](./images/data-preparation/schema-expansion.gif)

すべての XDM スキーマと同様に、CEE スキーマフィールドグループは拡張可能です。 つまり、CEE フィールドグループにフィールドを追加し、必要に応じて複数のスキーマに異なるバリエーションを含めることができます。

フィールドグループの完全な例は、[ パブリック XDM リポジトリ ](https://github.com/adobe/xdm/blob/797cf4930d5a80799a095256302675b1362c9a15/docs/reference/context/experienceevent-consumer.schema.md) にあります。 さらに、次の [JSON ファイル ](https://github.com/AdobeDocs/experience-platform.en/blob/master/help/intelligent-services/assets/CEE_XDM_sample_rows.json) を表示およびコピーして、CEE スキーマに準拠するためにデータを構造化する方法の例を確認できます。 独自のデータをスキーマにマッピングする方法を判断するには、次の節で概要を説明する主要フィールドについて学習する際に、これら両方の例を参照してください。

## キーフィールド

CEE フィールドグループ内には、[!DNL Intelligent Services] が有益なインサイトを生成するために使用する必要がある主なフィールドがいくつかあります。 この節では、これらのフィールドの使用例と期待されるデータについて説明し、その他の例に関する参照ドキュメントへのリンクを示します。

### 必須フィールド

すべてのキーフィールドを使用することを強くお勧めしますが、[!DNL Intelligent Services] が機能するには、**必須** のフィールドが 2 つあります。

* [プライマリ ID フィールド](#identity)
* [xdm:timestamp](#timestamp)
* [xdm:channel](#channel) (Attribution AIの場合のみ必須 )

#### プライマリID {#identity}

スキーマ内の 1 つのフィールドは、プライマリ ID フィールドとして設定する必要があります。これにより、[!DNL Intelligent Services] は、時系列データの各インスタンスを個々の個人にリンクできます。

データのソースと特性に基づいて、プライマリ ID として使用する最適なフィールドを決定する必要があります。 ID フィールドには、フィールドが値として想定する ID データのタイプを示す **ID 名前空間** を含める必要があります。 有効な名前空間値には次のものが含まれます。

* &quot;電子メール&quot;
* &quot;phone&quot;
* 「mcid」(Adobe Audience Manager ID 用 )
* 「aaid」(Adobe Analytics ID の場合 )

プライマリ ID として使用する必要があるフィールドが不明な場合は、Adobeコンサルティングサービスに問い合わせて、最適なソリューションを決定してください。 プライマリ ID が設定されていない場合、インテリジェントサービスアプリケーションは次のデフォルトの動作を使用します。

| デフォルト | Attribution AI | 顧客 AI |
| --- | --- | --- |
| ID 列 | `endUserIDs._experience.aaid.id` | `endUserIDs._experience.mcid.id` |
| 名前空間 | AAID | ECID |

プライマリ ID を設定するには、「**[!UICONTROL スキーマ]**」タブからスキーマに移動し、スキーマ名のハイパーリンクを選択して **[!DNL Schema Editor]** を開きます。

![スキーマに移動](./images/data-preparation/navigate_schema.png)

次に、プライマリ ID として設定するフィールドに移動し、それを選択します。 **[!UICONTROL フィールドのプロパティ]** メニューがそのフィールド用に開きます。

![フィールドの選択](./images/data-preparation/find_field.png)

**[!UICONTROL フィールドのプロパティ]** メニューで、「**[!UICONTROL ID]**」チェックボックスが表示されるまで下にスクロールします。 チェックボックスをオンにすると、選択した ID を **[!UICONTROL プライマリID]** として設定するオプションが表示されます。 このボックスも選択します。

![チェックボックスを選択](./images/data-preparation/set_primary_identity.png)

次に、ドロップダウン内の事前定義済みの名前空間のリストから **[!UICONTROL ID 名前空間]** を指定する必要があります。 この例では、Adobe Audience Manager ID `mcid.id` が使用されているので、ECID 名前空間が選択されています。 「**[!UICONTROL 適用]**」を選択して更新内容を確定し、右上隅の「**[!UICONTROL 保存]**」を選択して、変更内容をスキーマに保存します。

![変更を保存します。](./images/data-preparation/select_namespace.png)

#### xdm:timestamp {#timestamp}

このフィールドは、イベントが発生した日時を表します。 この値は、ISO 8601 標準に従って、文字列として指定する必要があります。

#### xdm:channel {#channel}

>[!NOTE]
>
>このフィールドは、Attribution AIを使用する場合にのみ必須です。

このフィールドは、ExperienceEvent に関連するマーケティングチャネルを表します。 「 」フィールドには、チャネルタイプ、メディアタイプ、場所タイプに関する情報が含まれます。

![](./images/data-preparation/channel.png)

**スキーマの例**

```json
{
  "@id": "https://ns.adobe.com/xdm/channels/facebook-feed",
  "@type": "https://ns.adobe.com/xdm/channel-types/social",
  "xdm:mediaType": "earned",
  "xdm:mediaAction": "clicks"
}
```

`xdm:channel` に必要な各サブフィールドに関する詳細は、[ エクスペリエンスチャネルスキーマ ](https://github.com/adobe/xdm/blob/797cf4930d5a80799a095256302675b1362c9a15/docs/reference/channels/channel.schema.md) の仕様を参照してください。 一部のマッピングの例については、](#example-channels) の下の [ 表を参照してください。

#### チャネルマッピングの例 {#example-channels}

次の表に、`xdm:channel` スキーマにマッピングされたマーケティングチャネルの例を示します。

| チャネル | `@type` | `mediaType` | `mediaAction` |
| --- | --- | --- | --- |
| 有料検索 | https:/<span>/ns.adobe.com/xdm/channel-types/search | 支払済み | clicks |
| ソーシャル — マーケティング | https:/<span>/ns.adobe | 獲得 | clicks |
| 表示 | https:/<span>/ns.adobe.com/xdm/channel-types/display | 支払済み | clicks |
| メール | https:/<span>/ns.adobe.com/xdm/channel-types/email | 支払済み | clicks |
| 内部リファラー | https:/<span>/ns.adobe.com/xdm/channel-types/direct | 所有 | clicks |
| Display ViewThrough | https:/<span>/ns.adobe.com/xdm/channel-types/display | 支払済み | impressions |
| QR コードのリダイレクト | https:/<span>/ns.adobe.com/xdm/channel-types/direct | 所有 | clicks |
| Mobile | https:/<span>/ns.adobe.com/xdm/channel-types/mobile | 所有 | clicks |

### 推奨フィールド

残りの主要フィールドについては、この節で説明します。 [!DNL Intelligent Services] が機能するためにこれらのフィールドが必ずしも必要なわけではありませんが、より豊富なインサイトを得るために、できるだけ多く使用することを強くお勧めします。

#### xdm:productListItems

このフィールドは、製品の SKU、名前、価格、数量など、顧客が選択した製品を表す項目の配列です。

![](./images/data-preparation/productListItems.png)

**スキーマの例**

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

`xdm:productListItems` に必要な各サブフィールドの詳細については、[ コマースの詳細スキーマ ](https://github.com/adobe/xdm/blob/797cf4930d5a80799a095256302675b1362c9a15/docs/reference/context/experienceevent-commerce.schema.md) 仕様を参照してください。

#### xdm:commerce

このフィールドには、発注書番号や支払い情報など、ExperienceEvent に関するコマース固有の情報が含まれます。

![](./images/data-preparation/commerce.png)

**スキーマの例**

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

`xdm:commerce` に必要な各サブフィールドの詳細については、[ コマースの詳細スキーマ ](https://github.com/adobe/xdm/blob/797cf4930d5a80799a095256302675b1362c9a15/docs/reference/context/experienceevent-commerce.schema.md) 仕様を参照してください。

#### xdm:web

このフィールドは、インタラクション、ページの詳細、リファラーなど、ExperienceEvent に関する Web の詳細を表します。

![](./images/data-preparation/web.png)

**スキーマの例**

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

`xdm:productListItems` に必要な各サブフィールドに関する詳細は、[ExperienceEvent Web 詳細スキーマ ](https://github.com/adobe/xdm/blob/797cf4930d5a80799a095256302675b1362c9a15/docs/reference/context/experienceevent-web.schema.md) の仕様を参照してください。

#### xdm:marketing

このフィールドには、タッチポイントでアクティブなマーケティングアクティビティに関する情報が含まれます。

![](./images/data-preparation/marketing.png)

**スキーマの例**

```json
{
  "xdm:trackingCode": "marketingcampaign111",
  "xdm:campaignGroup": "50%_DISCOUNT",
  "xdm:campaignName": "50%_DISCOUNT_USA"
}
```

`xdm:productListItems` に必要な各サブフィールドの詳細については、[ マーケティングの仕様 ](https://github.com/adobe/xdm/blob/797cf4930d5a80799a095256302675b1362c9a15/docs/reference/context/marketing.schema.md) を参照してください。

## データのマッピングと取得 {#mapping}

マーケティングイベントデータを CEE スキーマにマッピングできるかどうかを判断したら、次の手順は、[!DNL Intelligent Services] に取り込むデータを決定することです。 [!DNL Intelligent Services] で使用されるすべての履歴データは、最小 4 ヶ月分のデータ期間と、ルックバック期間として意図された日数に該当する必要があります。

送信するデータの範囲を決定したら、Adobeコンサルティングサービスに連絡して、データをスキーマにマッピングし、サービスに取り込むのに役立ちます。

[!DNL Adobe Experience Platform] サブスクリプションをお持ちで、データを自分でマッピングして取り込む場合は、次の節で説明する手順に従います。

### Adobe Experience Platformの使用

>[!NOTE]
>
>以下の手順では、購読登録が必要です。Experience Platform Platform にアクセスできない場合は、次の手順 ](#next-steps) の節に進んでください。[

この節では、[!DNL Intelligent Services] で使用するデータをマッピングしExperience Platformに取り込むワークフローについて説明します。詳細な手順に関するチュートリアルへのリンクも含まれます。

#### CEE スキーマとデータセットの作成

取り込み用のデータの準備を開始する準備が整ったら、まず CEE フィールドグループを使用する新しい XDM スキーマを作成します。 次のチュートリアルでは、UI または API で新しいスキーマを作成するプロセスを順を追って説明します。

* [UI でのスキーマの作成](../xdm/tutorials/create-schema-ui.md)
* [API でのスキーマの作成](../xdm/tutorials/create-schema-api.md)

>[!IMPORTANT]
>
>上記のチュートリアルは、スキーマを作成するための一般的なワークフローに従います。 スキーマのクラスを選択する場合は、**XDM ExperienceEvent クラス** を使用する必要があります。 このクラスを選択したら、CEE フィールドグループをスキーマに追加できます。

CEE フィールドグループをスキーマに追加した後、データ内の追加フィールドに必要に応じて、他のフィールドグループを追加できます。

スキーマを作成して保存したら、そのスキーマに基づいて新しいデータセットを作成できます。 次のチュートリアルでは、UI または API で新しいデータセットを作成するプロセスを順を追って説明します。

* [UI でのデータセットの作成](../catalog/datasets/user-guide.md#create) （既存のスキーマを使用する際のワークフローに従います）
* [API でのデータセットの作成](../catalog/datasets/create.md)

データセットを作成したら、**[!UICONTROL Datasets]** ワークスペース内の Platform UI で見つけることができます。

![](images/data-preparation/dataset-location.png)

#### データセットへの ID フィールドの追加

[!DNL Adobe Audience Manager]、[!DNL Adobe Analytics]、または他の外部ソースからデータを取り込む場合は、スキーマフィールドを ID フィールドとして設定できます。 スキーマフィールドを ID フィールドとして設定するには、スキーマ作成のための [UI チュートリアル ](../xdm/tutorials/create-schema-ui.md#identity-field) または [API チュートリアル ](../xdm/tutorials/create-schema-api.md#define-an-identity-descriptor) 内の ID フィールドの設定に関する節を参照してください。

ローカルの CSV ファイルからデータを取り込む場合は、[ マッピングと ](#ingest) データの取り込みに関する次の節に進んでください。

#### データのマッピングと取得 {#ingest}

CEE スキーマとデータセットを作成したら、データテーブルとスキーマのマッピングを開始し、そのデータを Platform に取り込むことができます。 UI で CSV ファイルを実行する手順については、[CSV ファイルの XDM スキーマへのマッピング ](../ingestion/tutorials/map-a-csv-file.md) に関するチュートリアルを参照してください。 次の [ サンプルの JSON ファイル ](https://github.com/AdobeDocs/experience-platform.en/blob/master/help/intelligent-services/assets/CEE_XDM_sample_rows.json) を使用して、独自のデータを使用する前に取り込みプロセスをテストできます。

データセットが入力されると、同じデータセットを使用して追加のデータファイルを取り込むことができます。

データがサポートされるサードパーティアプリケーションに格納されている場合は、[ ソースコネクタ ](../sources/home.md) を作成して、マーケティングイベントデータを [!DNL Platform] にリアルタイムで取り込むこともできます。

## 次の手順 {#next-steps}

このドキュメントでは、[!DNL Intelligent Services] で使用するデータの準備に関する一般的なガイダンスを提供しました。 使用事例に基づいて追加のコンサルティングが必要な場合は、Adobeコンサルティングサポートにお問い合わせください。

データセットに顧客体験データを正常に入力したら、[!DNL Intelligent Services] を使用してインサイトを生成できます。 開始するには、次のドキュメントを参照してください。

* [Attribution AI の概要](./attribution-ai/overview.md)
* [顧客 AI の概要](./customer-ai/overview.md)
