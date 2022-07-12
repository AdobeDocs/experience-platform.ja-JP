---
keywords: Azure イベントハブの宛先；Azure イベントハブ；Azure EventHub
title: Azure Event Hubs 接続
description: へのリアルタイムアウトバウンド接続を作成する [!DNL Azure Event Hubs] ストレージからExperience Platformからデータをストリーミングします。
exl-id: f98a389a-bce3-4a80-9452-6c7293d01de3
source-git-commit: dd18350387aa6bdeb61612f0ccf9d8d2223a8a5d
workflow-type: tm+mt
source-wordcount: '2002'
ht-degree: 3%

---

# [!DNL Azure Event Hubs] 接続

## 概要 {#overview}

>[!IMPORTANT]
>
> この宛先は次の場所でのみ使用できます： [Real-time Customer Data Platform Ultimate](https://helpx.adobe.com/jp/legal/product-descriptions/real-time-customer-data-platform.html) 顧客。

[!DNL Azure Event Hubs] は、ビッグデータストリーミングプラットフォームおよびイベント取り込みサービスです。 1 秒あたりに数百万件のイベントを受信して処理できます。 イベントハブに送信されるデータは、任意のリアルタイム分析プロバイダーまたはバッチ/ストレージアダプターを使用して、変換および保存できます。

へのリアルタイムアウトバウンド接続を作成できます [!DNL Azure Event Hubs] Adobe Experience Platformからデータをストリーミングするためのストレージ。

* 詳しくは、 [!DNL Azure Event Hubs]を参照し、 [Microsoftドキュメント](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-about).
* 接続するには [!DNL Azure Event Hubs] プログラムを使用して、 [ストリーミング宛先 API のチュートリアル](../../api/streaming-destinations.md).
* 接続するには [!DNL Azure Event Hubs] Platform ユーザーインターフェイスを使用する場合は、以下の節を参照してください。

![UI でのAWS Kinesis](../../assets/catalog/cloud-storage/event-hubs/catalog.png)

## ユースケース {#use-cases}

次のようなストリーミング宛先を使用する [!DNL Azure Event Hubs]を使用すると、高価値のセグメントイベントや関連するプロファイル属性を、選択したシステムに簡単にフィードできます。

例えば、見込み客がホワイトペーパーをダウンロードし、それを「コンバージョン傾向が高い」セグメントに認定したとします。 見込み客が属するセグメントを [!DNL Azure Event Hubs] の宛先に指定した場合、このイベントは [!DNL Azure Event Hubs]. 企業の IT システムに最適に対応できると思われるように、このイベントの上に、自らのアプローチを使用し、ビジネスロジックを説明することができます。

## エクスポートのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、次の表を参照してください。

| 項目 | タイプ | 備考 |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL プロファイルベース]** | セグメントのすべてのメンバーを、目的のスキーマフィールド ( 例：（電子メールアドレス、電話番号、姓）。「プロファイル属性を選択」画面で選択します。 [宛先のアクティベーションワークフロー](../../ui/activate-batch-profile-destinations.md#select-attributes). |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は、API ベースの接続です。 セグメント評価に基づいてExperience Platform内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。 詳細を表示 [ストリーミング先](/help/destinations/destination-types.md#streaming-destinations). |

{style=&quot;table-layout:auto&quot;}

## IP アドレスの許可リスト {#ip-address-allowlist}

お客様のセキュリティおよびコンプライアンス要件を満たすために、Experience Platformは、 [!DNL Azure Event Hubs] 宛先。 参照： [ストリーミング先の IP アドレス許可リスト](/help/destinations/catalog/streaming/ip-address-allow-list.md) ：する IP の完全なリストを表示しま許可リストす。

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、 **[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions). 詳しくは、 [アクセス制御の概要](/help/access-control/ui/overview.md) または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。この宛先に接続する際は、次の情報を指定する必要があります。

### 認証情報 {#authentication-information}

#### 標準認証 {#standard-authentication}

![Azure Event Hubs 標準認証の詳細に関する入力済みフィールドを示す UI 画面の画像](../../assets/catalog/cloud-storage/event-hubs/event-hubs-standard-authentication.png)

次を選択した場合、 **[!UICONTROL 標準認証]** HTTP エンドポイントに接続するには、以下のフィールドに入力し、「 」を選択します。 **[!UICONTROL 宛先に接続]**:

* **[!UICONTROL SAS キー名]**:認証規則の名前。SAS キー名とも呼ばれます。
* **[!UICONTROL SAS キー]**:Event Hubs 名前空間のプライマリキー。 この `sasPolicy` この `sasKey` 必ず～に対応する **管理** [Event Hubs] リストに値が入力されるように設定された権限。 認証の詳細 [!DNL Azure Event Hubs] に SAS キーを設定 [Microsoftドキュメント](https://docs.microsoft.com/en-us/azure/event-hubs/authenticate-shared-access-signature).
* **[!UICONTROL 名前空間]**:次の項目を入力します。 [!DNL Azure Event Hubs] 名前空間。 詳細 [!DNL Azure Event Hubs] 名前空間 [Microsoftドキュメント](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-create#create-an-event-hubs-namespace).

#### 共有アクセス署名 (SAS) 認証 {#sas-authentication}

![Azure Event Hubs 標準認証の詳細に関する入力済みフィールドを示す UI 画面の画像](../../assets/catalog/cloud-storage/event-hubs/event-hubs-sas-authentication.png)

次を選択した場合、 **[!UICONTROL 標準認証]** HTTP エンドポイントに接続するには、以下のフィールドに入力し、「 」を選択します。 **[!UICONTROL 宛先に接続]**:

* **[!UICONTROL SAS キー名]**:認証規則の名前。SAS キー名とも呼ばれます。
* **[!UICONTROL SAS キー]**:Event Hubs 名前空間のプライマリキー。 この `sasPolicy` この `sasKey` 必ず～に対応する **管理** [Event Hubs] リストに値が入力されるように設定された権限。 認証の詳細 [!DNL Azure Event Hubs] に SAS キーを設定 [Microsoftドキュメント](https://docs.microsoft.com/en-us/azure/event-hubs/authenticate-shared-access-signature).
* **[!UICONTROL 名前空間]**:次の項目を入力します。 [!DNL Azure Event Hubs] 名前空間。 詳細 [!DNL Azure Event Hubs] 名前空間 [Microsoftドキュメント](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-create#create-an-event-hubs-namespace).
* **[!UICONTROL 名前空間]**:次の項目を入力します。 [!DNL Azure Event Hubs] 名前空間。 詳細 [!DNL Azure Event Hubs] 名前空間 [Microsoftドキュメント](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-create#create-an-event-hubs-namespace).

### 宛先の詳細を入力 {#destination-details}

>[!CONTEXTUALHELP]
>id="platform_destinations_connect_eventhubs_includesegmentnames"
>title="セグメント名を含める"
>abstract="データの書き出しで、書き出すセグメントの名前を含めるかどうかを切り替えます。 このオプションを選択したデータエクスポートの例に関するドキュメントを表示します。"

>[!CONTEXTUALHELP]
>id="platform_destinations_connect_eventhubs_includesegmenttimestamps"
>title="セグメントのタイムスタンプを含める"
>abstract="セグメントが作成および更新された際の UNIX タイムスタンプと、セグメントがアクティベーションのために宛先にマッピングされた際の UNIX タイムスタンプをデータエクスポートに含めるかどうかを切り替えます。 このオプションを選択したデータエクスポートの例に関するドキュメントを表示します。"

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。 UI でフィールドの横にアスタリスクが表示される場合は、そのフィールドが必須であることを示します。

![Azure Event Hubs の宛先の詳細に関する入力済みフィールドを示す UI 画面の画像](../../assets/catalog/cloud-storage/event-hubs/event-hubs-destination-details.png)

* **[!UICONTROL 名前]**:接続先の名前を入力 [!DNL Azure Event Hubs].
* **[!UICONTROL 説明]**:接続の説明を入力します。  例：&quot;プレミアム層のお客様&quot;、&quot;Customers in terested in kitesurfing&quot;。
* **[!UICONTROL eventHubName]**:ストリームの名前を [!DNL Azure Event Hubs] 宛先。
* **[!UICONTROL セグメント名を含める]**:データの書き出しで、書き出すセグメントの名前を含めるかどうかを切り替えます。 このオプションを選択した場合のデータエクスポートの例については、 [書き出されたデータ](#exported-data) の節を参照してください。
* **[!UICONTROL セグメントのタイムスタンプを含める]**:セグメントが作成および更新された際の UNIX タイムスタンプと、セグメントがアクティベーションのために宛先にマッピングされた際の UNIX タイムスタンプをデータエクスポートに含めるかどうかを切り替えます。 このオプションを選択した場合のデータエクスポートの例については、 [書き出されたデータ](#exported-data) の節を参照してください。

### アラートの有効化 {#enable-alerts}

アラートを有効にして、宛先へのデータフローのステータスに関する通知を受け取ることができます。 リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートの詳細については、 [UI を使用した宛先アラートの購読](../../ui/alerts.md).

宛先接続の詳細の指定が完了したら、 **[!UICONTROL 次へ]**.

## この宛先に対してセグメントをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>データをアクティブ化するには、 **[!UICONTROL 宛先の管理]**, **[!UICONTROL 宛先のアクティブ化]**, **[!UICONTROL プロファイルの表示]**、および **[!UICONTROL セグメントを表示]** [アクセス制御権限](/help/access-control/home.md#permissions). 詳しくは、 [アクセス制御の概要](/help/access-control/ui/overview.md) または製品管理者に問い合わせて、必要な権限を取得してください。

詳しくは、 [ストリーミングプロファイルの書き出し先に対するオーディエンスデータのアクティブ化](../../ui/activate-streaming-profile-destinations.md) を参照してください。

## プロファイルの書き出し動作 {#profile-export-behavior}

Experience Platformにより、 [!DNL Azure Event Hubs] 宛先：セグメントの選定やその他の重要なイベントに従ってプロファイルに関連する更新が発生した場合にのみ、宛先にデータを書き出します。 プロファイルは、次の状況で宛先に書き出されます。

* プロファイルの更新は、宛先にマッピングされた少なくとも 1 つのセグメントのセグメントメンバーシップの変更によって決定されました。 例えば、プロファイルは、宛先にマッピングされたいずれかのセグメントに適合しているか、宛先にマッピングされたいずれかのセグメントから退出しています。
* プロファイルの更新は、 [id マップ](/help/xdm/field-groups/profile/identitymap.md). 例えば、宛先にマッピングされたセグメントの 1 つに対して既に適合しているプロファイルの ID マップ属性に新しい ID が追加されたとします。
* プロファイルの更新は、宛先にマッピングされた属性の少なくとも 1 つに対する属性の変更によって決定されました。 例えば、マッピング手順で宛先にマッピングされた属性の 1 つがプロファイルに追加されます。

上記のすべての場合、関連する更新がおこなわれたプロファイルのみが宛先にエクスポートされます。 例えば、宛先フローにマッピングされたセグメントのメンバーが 100 人で、5 つの新しいプロファイルがセグメントの対象として認定されている場合、宛先への書き出しは増分で、5 つの新しいプロファイルのみが含まれます。

変更内容がどこにあっても、プロファイルに対してマッピングされたすべての属性が書き出されることに注意してください。 したがって、上の例では、属性自体が変更されていない場合でも、これら 5 つの新しいプロファイルに対してマッピングされたすべての属性が書き出されます。

### 何がデータエクスポートを決定し、何がエクスポートに含まれるか {#what-determines-export-what-is-included}

特定のプロファイルに対して書き出されるデータに関しては、 *が、 [!DNL Azure Event Hubs] 宛先* および *エクスポートに含まれるデータ*.

| 宛先の書き出しを決定する要素 | 宛先の書き出しに含まれる内容 |
|---------|----------|
| <ul><li>マッピングされた属性とセグメントは、宛先の書き出しのキューとして機能します。 つまり、マッピングされたセグメントの状態（null から適合済み、適合済み/既存から既存へ）が変更されたり、マッピングされた属性が更新された場合、宛先の書き出しがキックオフされます。</li><li>ID は現在にマッピングできないので [!DNL Azure Event Hubs] 宛先、特定のプロファイル上の ID の変更によって、宛先の書き出しも決まります。</li><li>属性に対する変更は、同じ値であるかどうかに関わらず、属性に対する更新として定義されます。 つまり、値自体が変更されていない場合でも、属性の上書きは変更と見なされます。</li></ul> | <ul><li>（最新のメンバーシップステータスを持つ）すべてのセグメントは、データフローにマッピングされているかどうかに関係なく、 `segmentMembership` オブジェクト。</li><li>内のすべての ID `identityMap` オブジェクトも含まれます (Experience Platformは現在、 [!DNL Azure Event Hubs] 宛先 )。</li><li>マッピングされた属性のみが宛先エクスポートに含まれます。</li></ul> |

{style=&quot;table-layout:fixed&quot;}

例えば、このデータフローを [!DNL Azure Event Hubs] 宛先。この宛先では、3 つのセグメントがデータフローで選択され、4 つの属性が宛先にマッピングされます。

![Amazon Kinesis宛先のデータフロー](/help/destinations/assets/catalog/http/profile-export-example-dataflow.png)

宛先へのプロファイルエクスポートは、いずれかの *3 つのマッピングされたセグメント*. ただし、データエクスポートでは、 `segmentMembership` オブジェクト ( [書き出されたデータ](#exported-data) の節を参照 )、その特定のプロファイルがそのメンバーの場合は、その他のマッピングされていないセグメントが表示されることがあります。 プロファイルが DeLorean Cars セグメントで顧客の資格を得ている一方で、「Back to the Future」映画や SF ファンセグメントのメンバーでもある場合、他の 2 つのセグメントも `segmentMembership` データエクスポートのオブジェクト（データフローでマッピングされていない場合）。

プロファイル属性の観点から、上でマッピングした 4 つの属性に対する変更によって、書き出し先が決まり、プロファイルに存在する 4 つのマッピング済み属性のいずれかがデータ書き出しに表示されます。

## 履歴データのバックフィル {#historical-data-backfill}

新しいセグメントを既存の宛先に追加する場合、または新しい宛先を作成してセグメントをマッピングする場合、Experience Platformは宛先にセグメントの資格情報の履歴データをエクスポートします。 セグメントに適合するプロファイル *前* セグメントが宛先に追加され、約 1 時間以内に宛先に書き出されます。

## 書き出したデータ {#exported-data}

エクスポート済み [!DNL Experience Platform] データは、 [!DNL Azure Event Hubs] の宛先を JSON 形式で指定します。 例えば、以下のエクスポートには、特定のセグメントに適合し、別の 2 つのセグメントのメンバーであり、別のセグメントから離脱したプロファイルが含まれています。 書き出しには、プロファイル属性の名、姓、生年月日、個人の電子メールアドレスも含まれます。 このプロファイルの ID は、ECID と電子メールです。

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
         "status":"existing"
      },
      "947c1c46-008d-40b0-92ec-3af86eaf41c1":{
         "lastQualificationTime":"2021-08-25T23:37:33Z",
         "status":"existing"
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

次に、 **[!UICONTROL セグメント名を含める]** および **[!UICONTROL セグメントのタイムスタンプを含める]** options:

+++ 以下のデータエクスポートのサンプルでは、 `segmentMembership` セクション

```json
"segmentMembership": {
        "ups": {
          "5b998cb9-9488-4ec3-8d95-fa8338ced490": {
            "lastQualificationTime": "2019-04-15T02:41:50+0000",
            "status": "existing",
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

+++ 以下のデータエクスポートの例では、 `segmentMembership` セクション

```json
"segmentMembership": {
        "ups": {
          "5b998cb9-9488-4ec3-8d95-fa8338ced490": {
            "lastQualificationTime": "2019-04-15T02:41:50+0000",
            "status": "existing",
            "createdAt": 1648553325000,
            "updatedAt": 1648553330000,
            "mappingCreatedAt": 1649856570000,
            "mappingUpdatedAt": 1649856570000,
          }
        }
      }
```

+++

## 制限および再試行ポリシー {#limits-retry-policy}

95%の確率で、Experience Platformは、各データフローの HTTP 宛先への 1 秒あたり 10.000 リクエスト未満の割合で正常に送信されたメッセージに対して、10 分未満のスループット遅延を提供しようとします。

HTTP API 宛先へのリクエストが失敗した場合、Experience Platformは失敗したリクエストを保存し、リクエストをエンドポイントに送信するために 2 回再試行します。

>[!MORELIKETHIS]
>
>* [Azure Event Hubs に接続し、フローサービス API を使用してデータをアクティブ化する](../../api/streaming-destinations.md)
>* [AWS Kinesisの宛先](./amazon-kinesis.md)
>* [宛先のタイプとカテゴリ](../../destination-types.md)

