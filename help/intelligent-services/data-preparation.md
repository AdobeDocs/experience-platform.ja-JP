---
keywords: Experience Platform；ホーム；インテリジェントサービス；人気のトピック；インテリジェントサービス；インテリジェントサービス
solution: Experience Platform
title: インテリジェントサービスで使用するデータの準備
description: インテリジェントサービスでマーケティングイベントデータからインサイトを発見するには、データを意味的にエンリッチメントし、標準構造で維持する必要があります。 インテリジェントサービスでは、これを実現するためにエクスペリエンスデータモデル（XDM）スキーマを使用します。
exl-id: 17bd7cc0-da86-4600-8290-cd07bdd5d262
source-git-commit: 73dea391f8fcb1d2d491c814b453afb4e538459d
workflow-type: tm+mt
source-wordcount: '2951'
ht-degree: 2%

---

# [!DNL Intelligent Services] で使用するデータの準備

マーケティングイベントデータからインサイトを発見する [!DNL Intelligent Services] めには、データを意味的にエンリッチメントし、標準構造に維持する必要があります。 これを実現 [!DNL Intelligent Services] るには、[!DNL Experience Data Model] （XDM）スキーマを活用します。 特に、[!DNL Intelligent Services] で使用するすべてのデータセットは、コンシューマーエクスペリエンスイベント（CEE） XDM スキーマに準拠するか、Adobe Analytics コネクタを使用する必要があります。 さらに、顧客 AI はAdobe Audience Manager コネクタをサポートします。

このドキュメントでは、複数のチャネルから CEE スキーマにマーケティングイベントデータをマッピングする際の一般的なガイダンスを提供し、スキーマ内の重要なフィールドに関する情報を概説し、データをその構造に効果的にマッピングする方法を決定するのに役立ちます。 Adobe Analyticsのデータを使用する予定がある場合は、[Adobe Analyticsのデータ準備 ](#analytics-data) の節を参照してください。 Adobe Audience Manager データ（顧客 AI のみ）を使用する予定がある場合は、[Adobe Audience Manger のデータ準備 ](#AAM-data) の節を参照してください。

## データ要件

作成 [!DNL Intelligent Services] る目標に応じて、異なる量の履歴データが必要になります。 いずれにせよ、準備する **すべて** データには [!DNL Intelligent Services] ポジティブとネガティブの両方のカスタマージャーニー/イベントを含める必要があります。 負のイベントと正のイベントの両方を持つことで、モデルの精度と精度が向上します。

例えば、顧客 AI を使用して製品の購入傾向を予測する場合、顧客 AI のモデルには、購入が成功したパスの例と失敗したパスの例の両方が必要です。 これは、モデルのトレーニング中、顧客 AI がイベントやジャーニーが購入につながるかを理解しようとするためです。 これには、買い物かごへの商品の追加でジャーニーを停止した個人など、購入しなかった顧客が実行したアクションも含まれます。 これらの顧客は同様の行動を示す場合がありますが、顧客 AI はインサイトを提供し、傾向スコアの上昇につながる主な違いや要因を掘り下げることができます。 同様に、アトリビューション AI では、タッチポイントの有効性、トップコンバージョンパス、タッチポイントポジション別の分類などの指標を表示するために、両方のタイプのイベントとジャーニーが必要です。

履歴データ要件に関するその他の例と情報については、入力/出力ドキュメントの [ 顧客 AI](./customer-ai/data-requirements.md#data-requirements) または [ アトリビューション AI](./attribution-ai/input-output.md#data-requirements) 履歴データ要件の節を参照してください。

### データのステッチのガイドライン

可能であれば、共通の ID でユーザーのイベントを結び付けることをお勧めします。 例えば、10 個のイベントをまたいで「id1」のユーザーデータを持つことができます。 その後、同じユーザーが cookie id を削除し、次の 20 件のイベントで「id2」として記録されます。 id1 と id2 が同じユーザーに対応することがわかっている場合は、30 個のイベントをすべて共通の id で結び付けることをお勧めします。

これが不可能な場合は、モデル入力データを作成する際に、イベントの各セットを異なるユーザーとして扱う必要があります。 これにより、モデルのトレーニングとスコアリングで最適な結果が得られます。

## ワークフローの概要

準備プロセスは、データがAdobe Experience Platformに保存されているか、外部に保存されているかによって異なります。 この節では、どちらのシナリオでも実行する必要がある手順の概要を説明します。

### 外部データの準備

データがExperience Platform外に保存されている場合は、データを [Consumer ExperienceEvent スキーマ ](#cee-schema) の必須フィールドと関連フィールドにマッピングする必要があります。 このスキーマをカスタムフィールドグループで拡張して、顧客データをより適切に取り込むことができます。 マッピングが完了したら、Consumer ExperienceEvent スキーマを使用してデータセットを作成し、[ データをExperience Platformに取り込む ](../ingestion/home.md) ことができます。 [!DNL Intelligent Service] の設定時に、CEE データセットを選択できます。

使用する [!DNL Intelligent Service] に応じて、異なるフィールドが必要になる場合があります。 利用可能なデータがある場合は、フィールドにデータを追加することがベストプラクティスです。 必須フィールドについて詳しくは、[ アトリビューション AI](./attribution-ai/input-output.md) または [ 顧客 AI](./customer-ai/data-requirements.md) データ要件ガイドを参照してください。

### Adobe Analytics データの準備 {#analytics-data}

顧客 AI とアトリビューション AI は、Adobe Analytics データをネイティブにサポートします。 Adobe Analytics データを使用するには、[Analytics ソースコネクタ ](../sources/tutorials/ui/create/adobe-applications/analytics.md) を設定するためのドキュメントで説明されている手順に従ってください。

ソースコネクタでデータがExperience Platformにストリーミングされると、インスタンス設定の際に、Adobe Analyticsをデータソースとして選択し、続いてデータセットを選択できるようになります。 すべての必須スキーマフィールドグループと個々のフィールドは、接続の設定時に自動的に作成されます。 データセットを CEE 形式に ETL （抽出、変換、読み込み）する必要はありません。

Adobe Analytics ソースコネクタを通じてAdobe Experience Platformに送信されたデータをAdobe Analytics データと比較すると、いくつかの不一致に気付く場合があります。 Analytics Source コネクタでは、エクスペリエンスデータモデル（XDM）スキーマへの変換中に行が削除される可能性があります。 行全体が変換に適さない理由としては、タイムスタンプの欠落、ユーザー ID の欠落、無効または大きなユーザー ID、無効な分析値など、複数の理由が考えられます。

詳細と例については、[Adobe AnalyticsとCustomer Journey Analytics データの比較 ](https://www.adobe.com/go/compare-aa-data-to-cja-data) のドキュメントを参照してください。 この記事は、データの整合性に関する懸念に妨げられることなく、お客様とチームがAdobe Experience Platform データをインテリジェントサービスに使用できるように、これらの違いを診断し、解決することを目的としています。

Adobe Experience Platform Query Services で、channel.typeAtSource クエリを使用して開始と終了のタイムスタンプの間の合計レコード数を次のように実行し、マーケティングチャネル別にカウントを見つけます。

```SELECT channel.typeAtSource as typeAtSource,
       Count(_id) AS Records 
FROM  df_hotel
WHERE timestamp>=from_utc_timestamp('2021-05-15','UTC')
        AND timestamp<from_utc_timestamp('2022-01-10','UTC')
        AND timestamp IS NOT NULL
        AND enduserids._experience.aaid.id IS NOT NULL
GROUP BY channel.typeAtSource
```

>[!IMPORTANT]
>
>Adobe Analytics コネクタによるデータのバックフィルには最大 4 週間かかります。 最近接続を設定した場合は、データセットに、顧客 AI またはアトリビューション AI に必要な最小データ長があることを確認する必要があります。 [ 顧客 AI](./customer-ai/data-requirements.md#data-requirements) または [ アトリビューション AI](./attribution-ai/input-output.md#data-requirements) の履歴データの節を確認し、予測目標を達成するために十分なデータがあることを確認してください。

### Adobe Audience Manager データ準備（顧客 AI のみ） {#AAM-data}

顧客 AI は、Adobe Audience Manager データをネイティブにサポートします。 Audience Manager データを使用するには、[Audience Manager ソースコネクタ ](../sources/tutorials/ui/create/adobe-applications/audience-manager.md) を設定するためのドキュメントで概説されている手順に従ってください。

ソースコネクタでデータがExperience Platformにストリーミングされると、顧客 AI 設定の際に、Adobe Audience Managerをデータソースとして選択し、続いてデータセットを選択できるようになります。 接続の設定時に、すべてのスキーマフィールドグループと個々のフィールドが自動的に作成されます。 データセットを CEE 形式に ETL （抽出、変換、読み込み）する必要はありません。

>[!IMPORTANT]
>
>最近コネクタを設定した場合は、データセットに必要なデータの最小の長さがあることを確認する必要があります。 顧客 AI については、[ 入力/出力ドキュメント ](./customer-ai/data-requirements.md) の履歴データの節を確認し、予測目標を達成するために十分なデータがあることを確認してください。

### [!DNL Experience Platform] データの準備

データが既に [!DNL Experience Platform] に保存されていて、Adobe AnalyticsまたはAdobe Audience Manager（顧客 AI のみ）ソースコネクタを介してストリーミングされていない場合は、次の手順に従います。 CEE スキーマを理解することをお勧めします。

1. [Consumer ExperienceEvent スキーマ ](#cee-schema) の構造を確認し、データをフィールドにマッピングできるかどうかを判断します。
2. Adobe Consulting サービスに連絡して、データをスキーマにマッピングして [!DNL Intelligent Services] に取り込むか、データを自分でマッピングする場合は [ このガイドの手順に従ってください ](#mapping)。

## CEE スキーマについて {#cee-schema}

消費者 ExperienceEvent スキーマは、デジタルマーケティングイベント（web またはモバイル）やオンラインまたはオフラインのコマースアクティビティに関連する個人の行動を記述します。 [!DNL Intelligent Services] では、このスキーマを使用する必要があります。これは、このスキーマのフィールド（列）が意味的に明確に定義されているので、データが不明瞭になる未知の名前を避けるためです。

CEE スキーマは、すべての XDM ExperienceEvent スキーマと同様に、イベント（または一連のイベント）が発生したときのシステムの時系列ベースの状態（主体が関与する時点や ID など）をキャプチャします。 エクスペリエンスイベントは、何が起きたかの事実の記録であり、したがって、それらは不変であり、集計や解釈なしに起こったことを表します。

[!DNL Intelligent Services] のスキーマ内のいくつかの主要なフィールドを利用して、マーケティングイベントデータからインサイトを生成します。これらはすべてルートレベルで見つかり、必要なサブフィールドを表示するように展開できます。

![ ナビゲーションとサブフィールドの詳細を示す、Adobe Experience Platform UI でのスキーマ拡張のデモ ](./images/data-preparation/schema-expansion.gif)

すべての XDM スキーマと同様に、CEE スキーマフィールドグループは拡張可能です。 つまり、CEE フィールドグループにフィールドを追加したり、必要に応じて様々なバリエーションを複数のスキーマに含めたりできます。

フィールドグループの完全な例は、[ 公開 XDM リポジトリ ](https://github.com/adobe/xdm/blob/797cf4930d5a80799a095256302675b1362c9a15/docs/reference/context/experienceevent-consumer.schema.md) にあります。 さらに、CEE スキーマに準拠したデータの構造を示す例として、次の [JSON ファイル ](https://github.com/AdobeDocs/experience-platform.en/blob/master/help/intelligent-services/assets/CEE_XDM_sample_rows.json) を表示してコピーできます。 独自のデータをスキーマにマッピングする方法を決定するには、これらの両方の例を参照し、以下のセクションで概要を説明するキーフィールドについて確認します。

## キーフィールド

CEE フィールドグループには、[!DNL Intelligent Services] が有用なインサイトを生成するために利用する必要があるキーフィールドがいくつかあります。 この節では、ユースケースとこれらのフィールドに必要なデータについて説明し、その他の例のリファレンスドキュメントへのリンクを提供します。

### 必須フィールド

すべての主要なフィールドを使用することを強くお勧めしますが、[!DNL Intelligent Services] ーザーが機能するために **必須** の 2 つのフィールドがあります。

* [プライマリ ID フィールド](#identity)
* [xdm:timestamp](#timestamp)
* [xdm:channel](#channel) （アトリビューション AI の場合のみ必須）

#### プライマリ ID {#identity}

スキーマのフィールドの 1 つを、時系列データの各インスタンスを個々のユーザーにリンクで [!DNL Intelligent Services] るプライマリ ID フィールドとして設定する必要があります。

データのソースと特性に基づいて、プライマリ ID として使用する最適なフィールドを決定する必要があります。 ID フィールドには、フィールドが想定する ID データのタイプを値として示す **ID 名前空間** を含める必要があります。 有効な名前空間の値の一部を次に示します。

>[!NOTE]
>
>Experience Cloud ID （ECID）は、MCID とも呼ばれ、名前空間で引き続き使用されます。

* &quot;電子メール&quot;
* &quot;電話&quot;
* 「mcid」（Adobe Audience Manager ID の場合）
* 「aaid」（Adobe Analytics ID の場合）

プライマリ ID として使用するフィールドが不明な場合は、Adobe Consulting サービスに連絡して最適なソリューションを判断してください。 プライマリ ID が設定されていない場合、インテリジェントサービスアプリケーションは次のデフォルトの動作を使用します。

| デフォルト | アトリビューション AI | 顧客 AI |
| --- | --- | --- |
| ID 列 | `endUserIDs._experience.aaid.id` | `endUserIDs._experience.mcid.id` |
| 名前空間 | AAID | ECID |

プライマリ ID を設定するには、「**[!UICONTROL スキーマ]**」タブからスキーマに移動し、スキーマ名のハイパーリンクを選択して **[!DNL Schema Editor]** を開きます。

![Adobe Experience Platform UI でのスキーマへの移動。](./images/data-preparation/navigate_schema.png)

次に、プライマリ ID として使用するフィールドに移動し、そのフィールドを選択します。 そのフィールドの **[!UICONTROL フィールドプロパティ]** メニューが開きます。

![Adobe Experience Platform UI で目的のフィールドを選択するプロセス。](./images/data-preparation/find_field.png)

**[!UICONTROL フィールドプロパティ]** メニューで、「**[!UICONTROL ID]**」チェックボックスが見つかるまで下にスクロールします。 チェックボックスをオンにすると、選択した ID を **[!UICONTROL プライマリ ID]** として設定するオプションが表示されます。 このボックスも選択します。

![Adobe Experience Platform UI でプライマリ ID を設定するチェックボックス。](./images/data-preparation/set_primary_identity.png)

次に、ドロップダウンの定義済み名前空間のリストから **[!UICONTROL ID 名前空間]** を指定する必要があります。この例では、Adobe Audience Manager ID `mcid.id` が使用されているので、ECID 名前空間が選択されています。 **[!UICONTROL 適用]** を選択して更新を確認し、右上隅の **[!UICONTROL 保存]** を選択して、スキーマに対する変更を保存します。

![Adobe Experience Platform UI で ECID 名前空間の選択を示すドロップダウンメニュー。](./images/data-preparation/select_namespace.png)

#### xdm:timestamp {#timestamp}

このフィールドは、イベントが発生した日時を表します。 この値は、ISO 8601 標準に従って、文字列として指定する必要があります。

#### xdm:channel {#channel}

>[!NOTE]
>
>このフィールドは、アトリビューション AI を使用する場合にのみ必須です。

このフィールドは、ExperienceEvent に関連するマーケティングチャネルを表します。 フィールドには、チャネルタイプ、メディアタイプ、場所タイプに関する情報が含まれます。

![type、mediaType、mediaAction などのサブフィールドを含む、xdm:channel フィールドの構造を示す図。](./images/data-preparation/channel.png)

**スキーマの例**

```json
{
  "@id": "https://ns.adobe.com/xdm/channels/facebook-feed",
  "@type": "https://ns.adobe.com/xdm/channel-types/social",
  "xdm:mediaType": "earned",
  "xdm:mediaAction": "clicks"
}
```

`xdm:channel` の必須サブフィールドのそれぞれについて詳しくは、[ エクスペリエンスチャネルスキーマ ](https://github.com/adobe/xdm/blob/797cf4930d5a80799a095256302675b1362c9a15/docs/reference/channels/channel.schema.md) 仕様を参照してください。 マッピングの例については、以下の [ 表 ](#example-channels) を参照してください。

#### チャネルマッピングの例 {#example-channels}

次の表に、`xdm:channel` スキーマにマッピングされるマーケティングチャネルの例を示します。

| チャネル | `@type` | `mediaType` | `mediaAction` |
| --- | --- | --- | --- |
| 有料検索 | https:/<span>/ns.adobe.com/xdm/channel-types/search | 有料 | クリック数 |
| ソーシャル – マーケティング | https:/<span>/ns.adobe.com/xdm/channel-types/social | 獲得済み | クリック数 |
| 表示 | https:/<span>/ns.adobe.com/xdm/channel-types/display | 有料 | クリック数 |
| メール | https:/<span>/ns.adobe.com/xdm/channel-types/email | 有料 | クリック数 |
| 内部リファラー | https:/<span>/ns.adobe.com/xdm/channel-types/direct | 所有 | クリック数 |
| ビュースルーを表示 | https:/<span>/ns.adobe.com/xdm/channel-types/display | 有料 | 印象 |
| QR コードのリダイレクト | https:/<span>/ns.adobe.com/xdm/channel-types/direct | 所有 | クリック数 |
| Mobile | https:/<span>/ns.adobe.com/xdm/channel-types/mobile | 所有 | クリック数 |

### 推奨フィールド

その他の主要なフィールドの概要については、この節で説明します。 これらのフィールドは [!DNL Intelligent Services] が機能するために必ずしも必要ではありませんが、より豊富なインサイトを得るには、できるだけ多く使用することを強くお勧めします。

#### xdm:productListItems

このフィールドは、顧客が選択した製品を表す項目の配列で、製品 SKU、名前、価格、数量が含まれます。

![xdm:productListItems フィールド（SKU、名前、currencyCode、数量、priceTotal などのサブフィールドを含む） ](./images/data-preparation/productListItems.png)

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

`xdm:productListItems` の必須サブフィールドのそれぞれについて詳しくは、[ コマースの詳細スキーマ ](https://github.com/adobe/xdm/blob/797cf4930d5a80799a095256302675b1362c9a15/docs/reference/context/experienceevent-commerce.schema.md) 仕様を参照してください。

#### xdm:commerce

このフィールドには、発注番号や支払い情報など、ExperienceEvent に関するコマース固有の情報が含まれています。

![ 注文、購入、支払いなどのサブフィールドを含む、xdm:commerce フィールドの構造。](./images/data-preparation/commerce.png)

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

`xdm:commerce` の必須サブフィールドのそれぞれについて詳しくは、[ コマースの詳細スキーマ ](https://github.com/adobe/xdm/blob/797cf4930d5a80799a095256302675b1362c9a15/docs/reference/context/experienceevent-commerce.schema.md) 仕様を参照してください。

#### xdm:web

このフィールドは、インタラクション、ページの詳細、リファラーなど、ExperienceEvent に関する web の詳細を表します。

![xdm:web フィールド（webPageDetails や webReferrer などのサブフィールドを含む ](./images/data-preparation/web.png)

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

`xdm:productListItems` の必須サブフィールドのそれぞれについて詳しくは、[ExperienceEvent web details schema](https://github.com/adobe/xdm/blob/797cf4930d5a80799a095256302675b1362c9a15/docs/reference/context/experienceevent-web.schema.md) 仕様を参照してください。

#### xdm:marketing

このフィールドには、タッチポイントでアクティブなマーケティングアクティビティに関連する情報が含まれています。

![trackingCode、campaignGroup、campaignName などのサブフィールドを含む、xdm:marketing フィールドの構造。](./images/data-preparation/marketing.png)

**スキーマの例**

```json
{
  "xdm:trackingCode": "marketingcampaign111",
  "xdm:campaignGroup": "50%_DISCOUNT",
  "xdm:campaignName": "50%_DISCOUNT_USA"
}
```

`xdm:productListItems` の必須サブフィールドのそれぞれについて詳しくは、[marketing sechma](https://github.com/adobe/xdm/blob/797cf4930d5a80799a095256302675b1362c9a15/docs/reference/context/marketing.schema.md) 仕様を参照してください。

## データのマッピングと取り込み {#mapping}

マーケティングイベントデータを CEE スキーマにマッピングできるかどうかを決定したら、次の手順は、[!DNL Intelligent Services] に取り込むデータを決定することです。 [!DNL Intelligent Services] で使用するすべての履歴データは、4 か月のデータの最小期間に、ルックバック期間として意図した日数を加えた期間内にある必要があります。

送信するデータの範囲を決めたら、Adobe Consulting サービスに連絡して、データをスキーマにマッピングし、サービスに取り込みます。

[!DNL Adobe Experience Platform] サブスクリプションがあり、データを自分でマッピングして取り込む場合は、以下の節で説明する手順に従ってください。

### Adobe Experience Platformの使用

>[!NOTE]
>
>以下の手順では、Experience Platformを購読する必要があります。 Experience Platformへのアクセス権がない場合は、[ 次の手順 ](#next-steps) の節に進みます。

この節では、[!DNL Intelligent Services] で使用するデータをExperience Platformにマッピングおよび取り込むためのワークフローの概要を説明し、詳細な手順を示すチュートリアルへのリンクを示します。

#### CEE スキーマとデータセットの作成

取り込み用のデータの準備を開始する準備が整ったら、まず、CEE フィールドグループを採用する新しい XDM スキーマを作成します。 次のチュートリアルでは、UI または API で新しいスキーマを作成するプロセスについて説明します。

* [UI でのスキーマの作成](../xdm/tutorials/create-schema-ui.md)
* [API でのスキーマの作成](../xdm/tutorials/create-schema-api.md)

>[!IMPORTANT]
>
>上記のチュートリアルは、スキーマを作成するための一般的なワークフローに従います。 スキーマのクラスを選択する場合は、**XDM ExperienceEvent クラス** を使用する必要があります。 このクラスを選択したら、CEE フィールドグループをスキーマに追加できます。

CEE フィールドグループをスキーマに追加した後、データ内の追加フィールドに対して、必要に応じて他のフィールドグループを追加できます。

スキーマを作成して保存したら、そのスキーマに基づいて新しいデータセットを作成できます。 次のチュートリアルでは、UI または API で新しいデータセットを作成するプロセスについて順を追って説明します。

* [UI でのデータセットの作成 ](../catalog/datasets/user-guide.md#create) （既存のスキーマを使用する場合のワークフローに従う）
* [API でのデータセットの作成](../catalog/datasets/create.md)

データセットを作成したら、Experience Platform UI の **[!UICONTROL データセット]** ワークスペース内で見つけることができます。

![](images/data-preparation/dataset-location.png)

#### データセットに ID フィールドを追加する

[!DNL Adobe Audience Manager]、[!DNL Adobe Analytics]、または別の外部ソースからデータを取り込む場合は、スキーマフィールドを ID フィールドとして設定するオプションがあります。 スキーマフィールドを ID フィールドとして設定するには、スキーマを作成するための [UI チュートリアル ](../xdm/tutorials/create-schema-ui.md#identity-field) または [API チュートリアル ](../xdm/tutorials/create-schema-api.md#define-an-identity-descriptor) の ID フィールドの設定に関する節を参照してください。

ローカルの CSV ファイルからデータを取り込む場合は、次の節 [ データのマッピングと取り込み ](#ingest) に進みます。

#### データのマッピングと取り込み {#ingest}

CEE スキーマとデータセットを作成したら、データテーブルからスキーマへのマッピングを開始し、そのデータをExperience Platformに取り込むことができます。 UI でこれをおこなう手順については、[CSV ファイルを XDM スキーマにマッピングする ](../ingestion/tutorials/map-csv/overview.md) に関するチュートリアルを参照してください。 独自のデータを使用する前に、次の [ サンプル JSON ファイル ](https://github.com/AdobeDocs/experience-platform.en/blob/master/help/intelligent-services/assets/CEE_XDM_sample_rows.json) を使用して、取り込みプロセスをテストできます。

データセットにデータを入力したら、同じデータセットを使用して追加のデータファイルを取り込むことができます。

サポートされているサードパーティアプリケーションにデータが保存されている場合は、[ ソースコネクタ ](../sources/home.md) を作成して、マーケティングイベントデータをリアルタイムで [!DNL Experience Platform] に取り込むこともできます。

## 次の手順 {#next-steps}

このドキュメントでは、[!DNL Intelligent Services] で使用するデータの準備に関する一般的なガイダンスを提供しました。 ユースケースに基づいて追加のコンサルティングが必要な場合は、Adobe Consulting サポートにお問い合わせください。

データセットにカスタマーエクスペリエンスデータを正常に入力したら、[!DNL Intelligent Services] を使用してインサイトを生成できます。 開始するには、次のドキュメントを参照してください。

* [Attribution AI の概要](./attribution-ai/overview.md)
* [顧客 AI の概要](./customer-ai/overview.md)
