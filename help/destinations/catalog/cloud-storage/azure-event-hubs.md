---
keywords: Azure event hub の宛先；azure event hub;azure eventhub
title: Azure Event Hubs 接続
description: Experience Platformからデータをストリーミングするために、ストレージへ  [!DNL Azure Event Hubs]  リアルタイムのアウトバウンド接続を作成します。
badgeUltimate: label="Ultimate" type="Positive"
exl-id: f98a389a-bce3-4a80-9452-6c7293d01de3
source-git-commit: d0ee4b30716734b8fce3509a6f3661dfa572cc9f
workflow-type: tm+mt
source-wordcount: '2212'
ht-degree: 45%

---

# [!DNL Azure Event Hubs] 接続

## 概要 {#overview}

>[!IMPORTANT]
>
> この宛先を使用できるのは [Adobe Real-time Customer Data Platform Ultimate](https://helpx.adobe.com/jp/legal/product-descriptions/real-time-customer-data-platform.html) の顧客のみです。

[!DNL Azure Event Hubs] は、ビッグデータストリーミングプラットフォームとイベント取り込みサービスです。 1 秒あたり数百万のイベントを受信し、処理できます。 イベントハブに送信されたデータは、任意のリアルタイム分析プロバイダーまたはバッチ/ストレージアダプターを使用して変換および保存できます。

[!DNL Azure Event Hubs] ストレージへのリアルタイムアウトバウンド接続を作成して、Adobe Experience Platformからデータをストリーミングできます。

* [!DNL Azure Event Hubs] について詳しくは、[Microsoft ドキュメント ](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-about) を参照してください。
* プログラムによってに接続する [!DNL Azure Event Hubs] は、[ ストリーミング宛先 API チュートリアル ](../../api/streaming-destinations.md) を参照してください。
* Experience Platform ユーザーインターフェイスを使用して [!DNL Azure Event Hubs] に接続するには、以下の節を参照してください。

![UI でのAWS Kinesis](../../assets/catalog/cloud-storage/event-hubs/catalog.png)

## ユースケース {#use-cases}

[!DNL Azure Event Hubs] などのストリーミング宛先を使用すると、価値の高いセグメント化イベントや関連するプロファイル属性を、選択したシステムに簡単にフィードできます。

例えば、ある見込み客がダウンロードしたホワイトペーパーによって、「コンバージョンする傾向の高い」セグメントに見込み客を選定するとします。 見込み客が含まれるオーディエンスを [!DNL Azure Event Hubs] の宛先にマッピングすると、[!DNL Azure Event Hubs] でこのイベントを受け取ります。 そこで、エンタープライズの IT システムで最も効果が高いと思われる通り、日曜大工のアプローチを採用し、イベントの上にビジネスロジックを記述できます。

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスオリジン | サポートあり | 説明 |
|---------|----------|----------|
| [!DNL Segmentation Service] | ✓ | Experience Platform [ セグメント化サービス ](../../../segmentation/home.md) を通じて生成されたオーディエンス。 |
| カスタムアップロード | ✓ | CSV ファイルから Experience Platform に[読み込まれた](../../../segmentation/ui/audience-portal.md#import-audience)オーディエンス。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL プロファイルベース]** | [宛先のアクティベーションワークフロー](../../ui/activate-batch-profile-destinations.md#select-attributes)のプロファイル属性選択画面で選択した目的のスキーマフィールド（例：メールアドレス、電話番号、姓）と共に、セグメントのすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)の詳細についてはこちらを参照してください。 |

{style="table-layout:auto"}

## IP アドレスの許可リスト {#ip-address-allowlist}

お客様のセキュリティおよびコンプライアンスの要件を満たすために、Experience Platformは [!DNL Azure Event Hubs] しい宛先に許可リストできる静的 IP のリストを提供します。 許可リストに使用できる IP のリストについて詳しくは、[ストリーミング宛先用 IP アドレスの許可リスト](/help/destinations/catalog/streaming/ip-address-allow-list.md)を参照してください。

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL 宛先の表示]** および **[!UICONTROL 宛先の管理]**&#x200B;[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。この宛先に接続する際は、次の情報を指定する必要があります。

### 認証情報 {#authentication-information}

#### 標準認証 {#standard-authentication}

![Azure Event Hubs 標準認証の詳細に関する入力済みフィールドを示す UI 画面の画像 ](../../assets/catalog/cloud-storage/event-hubs/event-hubs-standard-authentication.png)

**[!UICONTROL 標準認証]** タイプを選択して HTTP エンドポイントに接続する場合は、以下のフィールドを入力し、「**[!UICONTROL 宛先に接続]**」を選択します。

* **[!UICONTROL SAS キー名]**：認証ルールの名前。SAS キー名とも呼ばれます。
* **[!UICONTROL SAS キー]**: Event Hubs 名前空間のプライマリキー。 Event Hubs リストを入力するには、`sasPolicy` が対応する `sasKey` に **管理** 権限が設定されている必要があります。 SAS キーを使用した [!DNL Azure Event Hubs] への認証については、[Microsoft ドキュメント ](https://docs.microsoft.com/en-us/azure/event-hubs/authenticate-shared-access-signature) を参照してください。
* **[!UICONTROL 名前空間]**:[!DNL Azure Event Hubs] 名前空間を入力します。 [!DNL Azure Event Hubs] 名前空間について詳しくは、[Microsoft ドキュメント ](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-create#create-an-event-hubs-namespace) を参照してください。

#### 共有アクセス署名（SAS）認証 {#sas-authentication}

![Azure Event Hubs 標準認証の詳細に関する入力済みフィールドを示す UI 画面の画像 ](../../assets/catalog/cloud-storage/event-hubs/event-hubs-sas-authentication.png)

**[!UICONTROL 標準認証]** タイプを選択して HTTP エンドポイントに接続する場合は、以下のフィールドを入力し、「**[!UICONTROL 宛先に接続]**」を選択します。

* **[!UICONTROL SAS キー名]**：認証ルールの名前。SAS キー名とも呼ばれます。
* **[!UICONTROL SAS キー]**: Event Hubs 名前空間のプライマリキー。 Event Hubs リストを入力するには、`sasPolicy` が対応する `sasKey` に **管理** 権限が設定されている必要があります。 SAS キーを使用した [!DNL Azure Event Hubs] への認証については、[Microsoft ドキュメント ](https://docs.microsoft.com/en-us/azure/event-hubs/authenticate-shared-access-signature) を参照してください。
* **[!UICONTROL 名前空間]**:[!DNL Azure Event Hubs] 名前空間を入力します。 [!DNL Azure Event Hubs] 名前空間について詳しくは、[Microsoft ドキュメント ](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-create#create-an-event-hubs-namespace) を参照してください。
* **[!UICONTROL イベントハブ名]**:[!DNL Azure Event Hub] ーザー名を入力します。 [!DNL Azure Event Hubs] の名前について詳しくは、[Microsoft ドキュメント ](https://learn.microsoft.com/en-us/azure/event-hubs/event-hubs-create#create-an-event-hub) を参照してください。

### 宛先の詳細の入力 {#destination-details}

>[!CONTEXTUALHELP]
>id="platform_destinations_connect_eventhubs_includesegmentnames"
>title="セグメント名を含める"
>abstract="書き出すオーディエンスの名前をデータの書き出しに含めるかどうかを切り替えます。このオプションを選択したデータの書き出しの例に関するドキュメントを表示します。"

>[!CONTEXTUALHELP]
>id="platform_destinations_connect_eventhubs_includesegmenttimestamps"
>title="セグメントのタイムスタンプを含める"
>abstract="オーディエンスが作成および更新された際の UNIX タイムスタンプと、アクティブ化のためにオーディエンスが宛先にマップされた際の UNIX タイムスタンプをデータの書き出しに含めるかどうかを切り替えます。このオプションを選択したデータの書き出しの例に関するドキュメントを表示します。"

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

![Azure Event Hubs 宛先の詳細に関する入力済みフィールドを示す UI 画面の画像 ](../../assets/catalog/cloud-storage/event-hubs/event-hubs-destination-details.png)

* **[!UICONTROL 名前]**：接続の名前を入力し [!DNL Azure Event Hubs] す。
* **[!UICONTROL 説明]**：接続の説明を入力します。  例：「Premium 層のお客様」、「Kitesheving に興味のあるお客様」。
* **[!UICONTROL eventHubName]**:[!DNL Azure Event Hubs] の宛先へのストリームの名前を指定します。
* **[!UICONTROL セグメント名を含める]**：書き出すオーディエンスの名前をデータの書き出しに含めるかどうかを切り替えます。 このオプションを選択したデータの書き出しの例については、[書き出されたデータ](#exported-data)の節を参照してください。
* **[!UICONTROL セグメントのタイムスタンプを含める]**：オーディエンスが作成および更新された際の UNIX タイムスタンプと、アクティブ化のためにオーディエンスが宛先にマッピングされた際の UNIX タイムスタンプをデータの書き出しに含めるかどうかを切り替えます。 このオプションを選択したデータの書き出しの例については、[書き出されたデータ](#exported-data)の節を参照してください。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>* データをアクティブ化するには、**[!UICONTROL 宛先の表示]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]** および **[!UICONTROL セグメントの表示]**&#x200B;[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* [ 同意ポリシーの評価 ](/help/data-governance/enforcement/auto-enforcement.md#consent-policy-evaluation) は現在、Azure Event Hubs 宛先への書き出しではサポートされていません。 [詳細情報](/help/destinations/ui/activate-streaming-profile-destinations.md#consent-policy-evaluation)。

この宛先にオーディエンスをアクティブ化する手順については、[ ストリーミングプロファイル書き出し宛先に対するオーディエンスデータのアクティブ化 ](../../ui/activate-streaming-profile-destinations.md) を参照してください。

## プロファイルの書き出し動作 {#profile-export-behavior}

Experience Platformは、オーディエンスの選定または他の重要なイベントに続いてプロファイルに関連する更新が発生した際に宛先へデータを書き出すためにのみ、[!DNL Azure Event Hubs] ースの宛先へのプロファイルの書き出し動作を最適化します。 プロファイルは、以下の状況で宛先に書き出されます。

* 宛先にマッピングされた 1 つ以上のオーディエンスのオーディエンスメンバーシップの変更によって、プロファイルの更新が決定された場合。 例えば、プロファイルは、宛先にマッピングされたいずれかのオーディエンスに適合しているか、宛先にマッピングされたいずれかのオーディエンスから退出しています。
* プロファイルの更新が、[ID マップ](/help/xdm/field-groups/profile/identitymap.md)の変更によって決定する場合。例えば、宛先にマッピングされたオーディエンスの 1 つに対して既に適合しているプロファイルの ID マップ属性に新しい ID が追加されたとします。
* プロファイルの更新は、宛先にマッピングされた属性のうち、少なくとも 1 つの属性が変更されたことで判断されました。例えば、マッピング手順で宛先にマッピングされた属性の 1 つがプロファイルに追加されます。

上記のすべての場合で、適切な更新が行われたプロファイルのみが宛先に書き出されます。例えば、宛先フローにマッピングされたオーディエンスに 100 人のメンバーがいて、5 つの新しいプロファイルがセグメントに適合している場合、宛先への書き出しは増分で行われ、5 つの新しいプロファイルのみが含まれます。

変更箇所に関わらず、マッピングされたすべての属性がプロファイルに対して書き出されることに注意してください。 したがって、上の例では、属性自体が変更されていない場合でも、これら 5 つの新しいプロファイルに対してマッピングされた属性がすべて書き出されます。

### データの書き出しを決定する要素と、書き出しに含まれる内容 {#what-determines-export-what-is-included}

特定のプロファイルについて書き出されるデータに関しては、*[!DNL Azure Event Hubs] ースの宛先へのデータ書き出しを決定する要因は何か* および *書き出しに含まれるデータはどれか* という 2 つの異なる概念を理解することが重要です。

| 宛先の書き出しを決定する要素 | 宛先の書き出しに含まれる内容 |
|---------|----------|
| <ul><li>マッピングされた属性とセグメントは、宛先の書き出しのキューとして機能します。つまり、プロファイルの `segmentMembership` ステータスが `realized` または `exiting` に変更されたり、マッピングされた属性が更新されたりすると、宛先の書き出しが開始されます。</li><li>現在 ID は [!DNL Azure Event Hubs] の宛先にマッピングできないので、特定のプロファイルの ID が変わると、宛先の書き出しも決まります。</li><li>属性の変更は、同じ値であるかどうかに関わらず、属性に対する更新として定義されます。つまり、値自体が変更されていない場合でも、属性の上書きは変更と見なされます。</li></ul> | <ul><li>**メモ**:Azure Event Hubs 宛先の書き出し動作は、2025 年 9 月リリースで更新されました。 以下で強調表示されている新しい動作は、現在、このリリース以降に作成された新しい Azure Event Hubs 宛先にのみ適用されます。 既存の Azure Event Hubs 宛先の場合、古い書き出し動作を引き続き使用するか、Adobeに連絡して、マッピングされたオーディエンスのみが書き出される新しい動作に移行できます。 すべての組織は、2026 年に徐々に新しい動作に移行されます。<br><br> <span class="preview"> **新しい書き出し動作**：宛先にマッピングされ、変更されたセグメントは、segmentMembership オブジェクトに含まれます。 シナリオによっては、複数の呼び出しを使用して書き出される場合があります。また、シナリオによっては、変更されていない一部のセグメントも呼び出しに含まれる場合があります。いずれの場合も、データフローでマッピングされたセグメントのみが書き出されます。</span></li><br>**古い動作**:`segmentMembership` オブジェクトには、アクティブ化データフローでマッピングされたセグメントが含まれます。このセグメントについて、プロファイルのステータスが選定またはセグメント出口イベントの後に変更されました。 これらのセグメントが、アクティブ化データフローでマッピングされたセグメントと同じ [ 結合ポリシー ](/help/profile/merge-policies/overview.md) に属する場合、プロファイルが選定された他のマッピングされていないセグメントを宛先の書き出しに含めることができます。 <li>`identityMap` オブジェクト内のすべての ID も含まれます（Experience Platformでは現在、[!DNL Azure Event Hubs] の宛先での ID マッピングをサポートしていません）。</li><li>マッピングされた属性のみが宛先の書き出しに含まれます。</li></ul> |

{style="table-layout:fixed"}

>[!BEGINSHADEBOX]

例えば、[!DNL Azure Event Hubs] の宛先に対するこのデータフローについて考えてみましょう。ここでは、3 つのオーディエンスがデータフローで選択され、4 つの属性が宛先にマッピングされます。

![Amazon Kinesis 宛先のデータフロー ](/help/destinations/assets/catalog/http/profile-export-example-dataflow.png)

宛先へのプロファイルの書き出しは、*3 つのマッピングされたセグメント*&#x200B;のいずれかに適合またはいずれかを離脱するプロファイルによって決定されます。データの書き出しでは、`segmentMembership` オブジェクト（以下の [ 書き出されたデータ ](#exported-data) の節を参照）に、その特定のプロファイルがメンバーであり、書き出しをトリガーしたオーディエンスと同じ結合ポリシーを共有している場合、他のマッピングされたオーディエンスが表示されることがあります。 プロファイルが **デロリアンを保有する顧客** セグメントの対象となり、かつ **基本サイトアクティブおよび市区町村 – ダラス** セグメントのメンバーでもある場合、他の 2 つのオーディエンスもデータ書き出しの `segmentMembership` オブジェクトに存在します。これは、これらが **デロリアンを保有する顧客** セグメントと同じ結合ポリシーを共有する場合、データフローでマッピングされるためです。

プロファイル属性の観点から、上記でマッピングした 4 つの属性に対する変更によって、書き出しの宛先が決定し、プロファイルに存在する 4 つのマッピング済み属性のいずれかがデータ書き出しに表示されます。

>[!ENDSHADEBOX]

## 履歴データのバックフィル {#historical-data-backfill}

既存の宛先に新しいオーディエンスを追加する場合、または新しい宛先を作成し、それにオーディエンスをマッピングする場合、Experience Platformは、履歴オーディエンス選定データを宛先に書き出します。 オーディエンス *以前* に適合し、オーディエンスが宛先に追加されたプロファイルは、約 1 時間以内に宛先に書き出されます。

## 書き出したデータ {#exported-data}

書き出された [!DNL Experience Platform] データは、JSON 形式で [!DNL Azure Event Hubs] の宛先に格納されます。例えば、以下の書き出しには、特定のセグメントに適合し、別の 2 つのセグメントのメンバーであり、別のセグメントから離脱したプロファイルが含まれています。 書き出しには、プロファイル属性の名、姓、生年月日、個人メールアドレスも含まれます。 このプロファイルの ID は、ECID とメールです。

```json
{
  "person": {
    "birthDate": "YYYY-MM-DD",
    "name": {
      "firstName": "John",
      "lastName": "Doe"
    }
  },
  "personalEmail": {
    "address": "john.doe@acme.com"
  },
  "segmentMembership": {
   "ups":{
      "7841ba61-23c1-4bb3-a495-00d3g5fe1e93":{
         "lastQualificationTime":"2022-01-11T21:24:39Z",
         "status":"exited"
      },
      "59bd2fkd-3c48-4b18-bf56-4f5c5e6967ae":{
         "lastQualificationTime":"2022-01-02T23:37:33Z",
         "status":"realized"
      },
      "947c1c46-008d-40b0-92ec-3af86eaf41c1":{
         "lastQualificationTime":"2021-08-25T23:37:33Z",
         "status":"realized"
      },
      "5114d758-ce71-43ba-b53e-e2a91d67b67f":{
         "lastQualificationTime":"2022-01-11T23:37:33Z",
         "status":"realized"
      }
   }
},
  "identityMap": {
    "ecid": [
      {
        "id": "14575006536349286404619648085736425115"
      },
      {
        "id": "66478888669296734530114754794777368480"
      }
    ],
    "email_lc_sha256": [
      {
        "id": "655332b5fa2aea4498bf7a290cff017cb4"
      },
      {
        "id": "66baf76ef9de8b42df8903f00e0e3dc0b7"
      }
    ]
  }
}
```

書き出されたデータのその他の例を以下に示します。これらは「**[!UICONTROL セグメント名を含める]**」および「**[!UICONTROL セグメントのタイムスタンプを含める]**」オプションに対して接続データフローで選択した UI 設定によって異なります。

+++ 以下のデータの書き出しの例では、`segmentMembership` セクションにオーディエンス名が含まれています

```json
"segmentMembership": {
        "ups": {
          "5b998cb9-9488-4ec3-8d95-fa8338ced490": {
            "lastQualificationTime": "2019-04-15T02:41:50+0000",
            "status": "realized",
            "createdAt": 1648553325000,
            "updatedAt": 1648553330000,
            "mappingCreatedAt": 1649856570000,
            "mappingUpdatedAt": 1649856570000,
            "name": "First name equals John"
          }
        }
      }
```

+++

+++ 以下のデータの書き出しの例では、`segmentMembership` セクションにオーディエンスのタイムスタンプが含まれています

```json
"segmentMembership": {
        "ups": {
          "5b998cb9-9488-4ec3-8d95-fa8338ced490": {
            "lastQualificationTime": "2019-04-15T02:41:50+0000",
            "status": "realized",
            "createdAt": 1648553325000,
            "updatedAt": 1648553330000,
            "mappingCreatedAt": 1649856570000,
            "mappingUpdatedAt": 1649856570000,
          }
        }
      }
```

+++

## 制限と再試行ポリシー {#limits-retry-policy}

Experience Platform は 95％ の確率で、HTTP 宛先の各データフローにおいて、送信に成功したメッセージのスループット待ち時間を 10 分未満、リクエスト数を 1 秒あたり 10,000 件未満で提供しようと試みます。

HTTP API 宛先へのリクエストが失敗した場合、Experience Platform は失敗したリクエストを保存し、リクエストをエンドポイントに送信するために 2 回再試行します。

>[!MORELIKETHIS]
>
>* [Azure Event Hubs に接続し、Flow Service API を使用してデータをアクティブ化する ](../../api/streaming-destinations.md)
>* [AWS Kinesis の宛先 ](./amazon-kinesis.md)
>* [宛先のタイプとカテゴリ](../../destination-types.md)
