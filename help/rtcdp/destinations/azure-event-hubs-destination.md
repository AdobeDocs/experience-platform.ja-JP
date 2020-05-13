---
title: （ベータ版）Azureイベントハブの宛先
seo-title: （ベータ版）Azureイベントハブの宛先
description: Azureイベントハブストレージへのリアルタイムのアウトバウンド接続を作成して、エクスペリエンスプラットフォームからデータをストリーミングします。
seo-description: Azureイベントハブストレージへのリアルタイムのアウトバウンド接続を作成して、エクスペリエンスプラットフォームからデータをストリーミングします。
translation-type: tm+mt
source-git-commit: 47e03d3f58bd31b1aec45cbf268e3285dd5921ea
workflow-type: tm+mt
source-wordcount: '470'
ht-degree: 6%

---


# （ベータ版）Azureイベントハブの宛先

>[!IMPORTANT]
>
>Adobe Real-time CDPの [!DNL Azure Event Hubs] 宛先は現在ベータ版です。 ドキュメントと機能は変更される場合があります。

## 概要 {#overview}

[!DNL Azure Event Hubs] は、大規模なデータストリーミングプラットフォームおよびイベント取り込みサービスです。 1秒あたり数百万個のイベントを受信し、処理できます。 イベントハブに送信されたデータは、任意のリアルタイム分析プロバイダーまたはバッチ/ストレージアダプターを使用して、変換および保存できます。

Adobe Experience Platformからのデータをストリーミングするために、 [!DNL Azure Event Hubs] ストレージへのリアルタイムのアウトバウンド接続を作成できます。

* 詳細については、 [!DNL Azure Event Hubs]Microsoftのマニュアルを参照してください [](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-about)。
* API呼び出しを [!DNL Azure Event Hubs] 使用して接続するには、 [ストリーミング宛先APIチュートリアルを参照してください](/help/rtcdp/destinations/streaming-destinations-api-tutorial.md)。
* Adobe Real-time CDPユーザーインターフェイス [!DNL Azure Event Hubs] を使用して接続するには、以下のセクションを参照してください。

![AWS KinesisのUI](/help/rtcdp/destinations/assets/azure-event-hubs-destination.png)

## 使用例 {#use-cases}

Azureイベントハブなどのストリーミング宛先を使用すると、高価値のセグメントイベントや関連するプロファイル属性を、お客様のシステムに簡単にフィードできます。

例えば、見込み客が、「コンバージョンする傾向が高い」セグメントに該当するホワイトペーパーをダウンロードしたとします。 見込み客が属するセグメントをAzureイベントハブの宛先にマッピングすると、このイベントがAzureイベントハブで受け取られます。 企業のITシステムで最も効果的に機能すると考えられるように、Do-It-Yoursenアプローチを採用し、イベントの上にビジネスロジックを記述することができます。

## 宛先の接続 {#connect-destination}

See [Cloud storage destinations workflow ](/help/rtcdp/destinations/cloud-storage-destinations-workflow.md)for instructions on how to connect to your cloud storage destinations, including [!DNL Azure Event Hubs].

For [!DNL Azure Event Hubs] destinations, enter the following information in the create destination workflow:

### アカウントステップ内 {#account-step}

* **[!UICONTROL SASキー名]** と **[!UICONTROL SASキー]**: SASキーの名前とキーを入力します。 SASキーを使用し [!DNL Azure Event Hubs] たときの認証については、 [Microsoftのドキュメントを参照してください](https://docs.microsoft.com/en-us/azure/event-hubs/authenticate-shared-access-signature)。
* **[!UICONTROL 名前空間]**: 名前空間を入力し [!DNL Azure Event Hubs] ます。 [!DNL Azure Event Hubs] Microsoftのドキュメントで [名前空間について説明します](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-create#create-an-event-hubs-namespace)。

![認証手順で必要な入力](/help/rtcdp/destinations/assets/event-hubs-account-step.png)

### 認証手順の {#authentication-step}

* **[!UICONTROL 名前]**: 接続先の名前を入力し [!DNL Azure Event Hubs]ます。
* **[!UICONTROL 説明]**: 接続の説明を入力します。  例： 「Premium tier customers」、「Osins in kitesurfing」
* **[!UICONTROL eventHubName]**: 目的のストリームの名前を指定し [!DNL Azure Event Hubs] ます。

![設定手順で必要なデータ](/help/rtcdp/destinations/assets/event-hubs-authentication-step.png)

## セグメントのアクティブ化 {#activate-segments}

セグメントのアクティベーションワークフローについて詳しくは、「[宛先へのプロファイルとセグメントのアクティブ化](/help/rtcdp/destinations/activate-destinations.md)」を参照してください。


## 書き出されたデータ {#exported-data}

書き出したエクスペリエンスプラットフォームのデータは、JSON形式 [!DNL Azure Event Hubs] で取得されます。 例えば、特定のセグメントから離脱したオーディエンスのハッシュ化された電子メールIDを含むイベントストリームは、次のようになります。

```
{
   "segmentMembership":{
      "ups":{
         "7841ba61-23c1-4bb3-a495-00d695fe1e93":{
            "lastQualificationTime":"2020-03-03T21:24:39Z",
            "status":"exited"
         }
      }
   }
},
"identityMap":{
   "email_lc_sha256":[
      {
         "id":"655332b5fa2aea4498bf7a290cff017cb4"
      },
      {
         "id":"66baf76ef9de8b42df8903f00e0e3dc0b7"
      }
   ]
},
```



>[!MORELIKETHIS]
>
>* [Azureイベントハブに接続し、API呼び出しを使用してデータをアクティブにする](/help/rtcdp/destinations/streaming-destinations-api-tutorial.md)
>* [AWS Kinesisの宛先](/help/rtcdp/destinations/amazon-kinesis-destination.md)
>* [宛先のタイプとカテゴリ](/help/rtcdp/destinations/destination-types.md)