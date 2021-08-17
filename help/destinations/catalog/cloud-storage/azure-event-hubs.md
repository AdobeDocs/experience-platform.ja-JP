---
keywords: Azureイベントハブの宛先；azureイベントハブ；azure eventhub
title: （ベータ版）!DNL Azure Event Hubs]接続
description: '!DNL Azure Event Hubs]ストレージへのリアルタイムアウトバウンド接続を作成し、Experience Platformからデータをストリーミングします。'
exl-id: f98a389a-bce3-4a80-9452-6c7293d01de3
source-git-commit: 3aac1e7c7fe838201368379da8504efc8e316e1c
workflow-type: tm+mt
source-wordcount: '476'
ht-degree: 3%

---

# （ベータ版） [!DNL Azure Event Hubs]接続

## 概要 {#overview}

>[!IMPORTANT]
>
>Platformの[!DNL Azure Event Hubs]宛先は現在ベータ版です。 ドキュメントと機能は変更される場合があります。

[!DNL Azure Event Hubs] は、ビッグデータストリーミングプラットフォームおよびイベント取り込みサービスです。1秒あたり何百万ものイベントを受信し、処理できます。 イベントハブに送信されたデータは、任意のリアルタイム分析プロバイダーまたはバッチ/ストレージアダプターを使用して、変換および保存できます。

[!DNL Azure Event Hubs]ストレージへのリアルタイム送信接続を作成して、Adobe Experience Platformからデータをストリーミングできます。

* [!DNL Azure Event Hubs]の詳細については、[Microsoftのドキュメント](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-about)を参照してください。
* プログラムで[!DNL Azure Event Hubs]に接続するには、『[ストリーミング宛先APIのチュートリアル](../../api/streaming-destinations.md)』を参照してください。
* Platformユーザーインターフェイスを使用して[!DNL Azure Event Hubs]に接続するには、以下の節を参照してください。

![UIでのAWS Kinesis](../../assets/catalog/cloud-storage/event-hubs/catalog.png)

## ユースケース {#use-cases}

[!DNL Azure Event Hubs]などのストリーミング宛先を使用すると、価値の高いセグメント化イベントや関連するプロファイル属性を、選択したシステムに簡単にフィードできます。

例えば、見込み客がホワイトペーパーをダウンロードし、それを「コンバージョン傾向が高い」セグメントに認定したとします。 見込み客が属するセグメントを[!DNL Azure Event Hubs]の宛先にマッピングすると、このイベントは[!DNL Azure Event Hubs]で受け取ります。 企業のITシステムと最適に連携すると考えられるように、Do-It-Yoursellアプローチを採用し、イベントの上にビジネスロジックを記述することができます。

## 書き出しタイプ {#export-type}

**プロファイルベースの**  — セグメントのすべてのメンバーを、目的のスキーマフィールド(例：電子メールアドレス、電話番号、姓)。オーディエンスアクティベーションワークフローの属性を選択画面 [から選択します](../../ui/activate-streaming-profile-destinations.md#select-attributes)。

## 宛先に接続 {#connect}

この宛先に接続するには、[宛先の設定に関するチュートリアル](../../ui/connect-destination.md)で説明されている手順に従います。

### 接続パラメーター {#parameters}

[この宛先を設定](../../ui/connect-destination.md)する際に、次の情報を指定する必要があります。

* **[!UICONTROL SASキー名]** とSAS **[!UICONTROL キー]**:SASキー名とキーを入力します。[Microsoftのドキュメント](https://docs.microsoft.com/en-us/azure/event-hubs/authenticate-shared-access-signature)で、SASキーを使用した[!DNL Azure Event Hubs]の認証について説明します。
* **[!UICONTROL 名前空間]**:名前空間に入力 [!DNL Azure Event Hubs] します。[!DNL Azure Event Hubs]名前空間については、[Microsoftのドキュメント](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-create#create-an-event-hubs-namespace)を参照してください。
* **[!UICONTROL 名前]**:への接続の名前を入力しま [!DNL Azure Event Hubs]す。
* **[!UICONTROL 説明]**:接続の説明を入力します。例：「プレミアム層のお客様」、「キテサーフィンに興味を持つ男性」。
* **[!UICONTROL eventHubName]**:宛先へのストリームの名前を指定し [!DNL Azure Event Hubs] ます。

## この宛先へのセグメントのアクティブ化 {#activate}

この宛先に対してオーディエンスセグメントをアクティブ化する手順については、[ストリーミングプロファイルの書き出し先へのオーディエンスデータのアクティブ化](../../ui/activate-streaming-profile-destinations.md)を参照してください。

## エクスポートされたデータ {#exported-data}

書き出された[!DNL Experience Platform]データは、JSON形式で[!DNL Azure Event Hubs]に格納されます。 例えば、以下のイベントは、特定のセグメントに適合し、別のセグメントから離脱したオーディエンスの電子メールアドレスプロファイル属性を含みます。 この見込み客のIDはECIDと電子メールです。

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
>* [Azure Event Hubsに接続し、フローサービスAPIを使用してデータをアクティブ化する](../../api/streaming-destinations.md)
* [AWS Kinesis宛先](./amazon-kinesis.md)
* [宛先のタイプとカテゴリ](../../destination-types.md)

