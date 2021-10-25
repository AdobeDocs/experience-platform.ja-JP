---
keywords: Azure event hub のコピー先。 azure event hub、azure eventhub
title: (ベータ版)!DNL Azure Event Hub] 接続
description: へのリアルタイムでの送信接続を作成します。DNL Azure Event Hub] エクスペリエンスプラットフォームからデータをストリーミングするための記憶領域です。
exl-id: f98a389a-bce3-4a80-9452-6c7293d01de3
source-git-commit: 2b1cde9fc913be4d3bea71e7d56e0e5fe265a6be
workflow-type: tm+mt
source-wordcount: '476'
ht-degree: 3%

---

# (ベータ版) [!DNL Azure Event Hubs] 接続

## 概要 {#overview}

>[!IMPORTANT]
>
>[!DNL Azure Event Hubs]プラットフォームの移行先は、現在ベータ版になっています。ドキュメントと機能は変更される場合があります。

[!DNL Azure Event Hubs] は、非常に大きなデータストリーミングプラットフォームとイベント取り込みサービスです。 毎秒数百万のイベントの受信と処理ができます。 イベントハブに送信されたデータは、任意のリアルタイム分析プロバイダー、またはバッチまたはストレージアダプターを使用して、変換して保存することができます。

[!DNL Azure Event Hubs]Adobe エクスペリエンスプラットフォームからデータをストリーミングするために、ストレージへのリアルタイムでの送信接続を作成することができます。

* について詳しくは、 [!DNL Azure Event Hubs] Microsoft のマニュアルを参照してください [ ](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-about) 。
* プログラムに接続するには [!DNL Azure Event Hubs] 、 [ ストリーミング送信先 API のチュートリアルを参照してください ](../../api/streaming-destinations.md) 。
* プラットフォームのユーザーインターフェイスを使用してに接続するには、 [!DNL Azure Event Hubs] 以下の項を参照してください。

![AWS Kinesis の UI の機能について](../../assets/catalog/cloud-storage/event-hubs/catalog.png)

## ユースケース {#use-cases}

などのストリーミング出力を使用することにより、必要に応じて [!DNL Azure Event Hubs] 、高い値を持つセグメンテーションイベントおよび関連するプロファイル属性を、選択したシステムに簡単に送ることができます。

例えば、取引先によってホワイトペーパーがダウンロードされています。これにより、「propensity」セグメントに限定されます。 取引関係があるセグメントを宛先にマップすることによって [!DNL Azure Event Hubs] 、このイベントがに表示され [!DNL Azure Event Hubs] ます。 企業の IT システムに最適な機能として、開発者は独自のアプローチを採用し、イベントの上部にビジネスロジックを説明することができます。

## 書き出しタイプ {#export-type}

**プロファイルベース** -セグメントのすべてのメンバーを、出席者のアクティブ化ワークフローの「属性の選択」画面で選択したとおりに、目的のスキーマフィールド (例えば、電子メールアドレス、電話番号、姓) と共に書き出すことができ [ ](../../ui/activate-streaming-profile-destinations.md#select-attributes) ます。

## 目的の場所に接続します。 {#connect}

この送信先に接続するには、宛先の設定チュートリアルで説明されている手順に従って [ ](../../ui/connect-destination.md) ください。

### 接続パラメーター {#parameters}

このコピー先を設定する際に、 [ ](../../ui/connect-destination.md) 次の情報を入力する必要があります。

* **[!UICONTROL SAS キー名]** と **[!UICONTROL sas キー]** : sas キー名とキーを入力します。 Microsoft のマニュアルに記載されている SAS キーを使用した認証方法について説明 [!DNL Azure Event Hubs] [ ](https://docs.microsoft.com/en-us/azure/event-hubs/authenticate-shared-access-signature) します。
* **[!UICONTROL 名前空間]** : [!DNL Azure Event Hubs] 名前空間に名前を入力します。 Microsoft の [!DNL Azure Event Hubs] マニュアルに記載されている名前空間について説明 [ ](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-create#create-an-event-hubs-namespace) します。
* **[!UICONTROL 名前]** : 接続の名前を入力 [!DNL Azure Event Hubs] します。
* **[!UICONTROL 説明]** : 接続に関する説明を入力します。  例: &quot;Premium tier customers&quot;、&quot;Males kitesurfing&quot; に関心があります。
* **[!UICONTROL eventHubName]** : ストリームの名前を指定 [!DNL Azure Event Hubs] します。

## セグメントをこの宛先にアクティブにします。 {#activate}

[ ](../../ui/activate-streaming-profile-destinations.md) この宛先までの視聴ユーザーセグメントをアクティブにする方法については、「プロファイルの書き出し先のストリーミング送信先のストリーミングについて」を参照してください。

## 書き出したデータ {#exported-data}

書き出した [!DNL Experience Platform] データは [!DNL Azure Event Hubs] JSON 形式になります。 例えば、次のイベントには、特定のセグメントを対象としていて、別のセグメントを終了した対象ユーザーの電子メールアドレスプロファイル属性が含まれています。 このようなお客様の id は、d と email になります。

```json
{
  "person": {
    "email": "yourstruly@adobe.com"
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
>* [フローサービス API を使用して、Azure Event Hub に接続し、データを有効にします。](../../api/streaming-destinations.md)
>* [AWS Kinesis 宛先](./amazon-kinesis.md)
>* [宛先のタイプとカテゴリ](../../destination-types.md)

