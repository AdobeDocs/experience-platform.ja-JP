---
keywords: Azureイベントハブの宛先；azureイベントハブ；azure eventhub
title: （ベータ版）Azureイベントハブ接続
description: Azureイベントハブストレージへのリアルタイムアウトバウンド接続を作成して、Experience Platformからデータをストリーミングします。
translation-type: tm+mt
source-git-commit: 709908196bb5df665c7e7df10dc58ee9f3b0edbf
workflow-type: tm+mt
source-wordcount: '543'
ht-degree: 6%

---


# （ベータ版） [!DNL Azure Event Hubs]接続

## 概要 {#overview}

>[!IMPORTANT]
>
>プラットフォームの[!DNL Azure Event Hubs]宛先は現在ベータ版です。 ドキュメントと機能は変更される場合があります。

[!DNL Azure Event Hubs] は、大規模なデータストリーミングプラットフォームおよびイベント取り込みサービスです。1秒あたり数百万個のイベントを受信し、処理できます。 イベントハブに送信されたデータは、任意のリアルタイム分析プロバイダーまたはバッチ/ストレージアダプターを使用して、変換および保存できます。

[!DNL Azure Event Hubs]ストレージへのリアルタイムの送信接続を作成して、Adobe Experience Platformからデータをストリーミングできます。

* [!DNL Azure Event Hubs]の詳細については、[Microsoftのドキュメント](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-about)を参照してください。
* プログラム的に[!DNL Azure Event Hubs]に接続するには、[ストリーミング送信先APIのチュートリアル](../../api/streaming-destinations.md)を参照してください。
* プラットフォームユーザーインターフェイスを使用して[!DNL Azure Event Hubs]に接続するには、以下の節を参照してください。

![AWSKinesisのUI](../../assets/catalog/cloud-storage/event-hubs/catalog.png)

## 使用例 {#use-cases}

[!DNL Azure Event Hubs]などのストリーミング送信先を使用すると、高価値のセグメントイベントや関連するプロファイル属性を、簡単に任意のシステムにフィードできます。

例えば、見込み客が、「コンバージョンする傾向が高い」セグメントに該当するホワイトペーパーをダウンロードしたとします。 見込み客が属するセグメントを[!DNL Azure Event Hubs]宛先にマッピングすると、[!DNL Azure Event Hubs]にこのイベントが表示されます。 企業のITシステムで最も効果的に機能すると考えられるように、Do-It-Yoursenアプローチを採用し、イベントの上にビジネスロジックを記述することができます。

## エクスポートの種類{#export-type}

**プロファイルベース**  — セグメントのすべてのメンバーを、必要なスキーマフィールド(例：電子メールアドレス、電話番号、姓)。 [宛先アクティベーションワークフローの属性を選択画面で選択](../../ui/activate-destinations.md#select-attributes)。

## 宛先の接続 {#connect-destination}

[を含む、クラウドストレージの接続先への接続方法については、[!DNL Azure Event Hubs]クラウドストレージの接続先ワークフロー](./workflow.md)を参照してください。

[!DNL Azure Event Hubs]宛先に対して、宛先を作成ワークフローで次の情報を入力します。

## 認証手順{#authentication-step}

* **[!UICONTROL SASキー]** 名と **[!UICONTROL SASキー]**:SASキーの名前とキーを入力します。[Microsoftのドキュメント](https://docs.microsoft.com/en-us/azure/event-hubs/authenticate-shared-access-signature)で、SASキーを使用した[!DNL Azure Event Hubs]への認証について説明します。
* **[!UICONTROL 名前空間]**: [!DNL Azure Event Hubs] 名前空間を入力します。[Microsoftドキュメント](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-create#create-an-event-hubs-namespace)の[!DNL Azure Event Hubs]名前空間について説明します。

![認証手順で必要な入力](../../assets/catalog/cloud-storage/event-hubs/authentication.png)

## 設定手順{#setup-step}

* **[!UICONTROL 名前]**:接続先の名前を入力し [!DNL Azure Event Hubs]ます。
* **[!UICONTROL 説明]**:接続の説明を入力します。例：「Premium tier customers」、「Osins in kitesurfing」
* **[!UICONTROL eventHubName]**:目的のストリームの名前を指定し [!DNL Azure Event Hubs] ます。
* **[!UICONTROL マーケティングアクション]**:マーケティングアクションは、データをエクスポート先にエクスポートする意図を示します。Adobe定義のマーケティングアクションから選択するか、独自のマーケティングアクションを作成することができます。 マーケティング活動の詳細については、「[Adobe Experience Platform](../../../data-governance/policies/overview.md)のデータガバナンス」ページを参照してください。 Adobe定義の個々のマーケティングアクションについて詳しくは、[データ使用ポリシーの概要](../../../data-governance/policies/overview.md)を参照してください。

![設定手順で必要なデータ](../../assets/catalog/cloud-storage/event-hubs/setup.png)

## セグメントのアクティブ化 {#activate-segments}

セグメントのアクティベーションワークフローについて詳しくは、「[宛先へのプロファイルとセグメントのアクティブ化](../../ui/activate-destinations.md)」を参照してください。

## エクスポートされたデータ{#exported-data}

書き出した[!DNL Experience Platform]データは、JSON形式で[!DNL Azure Event Hubs]に格納されます。 例えば、次のイベントには、特定のセグメントに該当し、別のセグメントから離脱したオーディエンスの電子メールアドレスプロファイル属性が含まれています。 この見込み客のIDは、ECIDと電子メールです。

```json
{
  "person": {
    "email": "yourstruly@adobe.con"
  },
  "segmentMembership": {
    "ups": {
      "7841ba61-23c1-4bb3-a495-00d3g5fe1e93": {
        "lastQualificationTime": "2020-05-25T21:24:39Z",
        "status": "exited"
      },
      "59bd2fkd-3c48-4b18-bf56-4f5c5e6967ae": {
        "lastQualificationTime": "2020-05-25T23:37:33Z",
        "status": "existing"
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


>[!MORELIKETHIS]
>
>* [Azureイベントハブに接続し、Flow Service APIを使用してデータをアクティブにする](../../api/streaming-destinations.md)
>* [AWSKinesisの宛先](./amazon-kinesis.md)
>* [宛先のタイプとカテゴリ](../../destination-types.md)