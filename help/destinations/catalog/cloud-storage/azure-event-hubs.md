---
keywords: Azure イベントハブの宛先；Azure イベントハブ；Azure EventHub
title: （ベータ版） [!DNL Azure Event Hubs] 接続
description: へのリアルタイムアウトバウンド接続を作成する [!DNL Azure Event Hubs] ストレージからExperience Platformからデータをストリーミングします。
exl-id: f98a389a-bce3-4a80-9452-6c7293d01de3
source-git-commit: b0c2c8313e05d1316f23dc15d99893e1887f8dcf
workflow-type: tm+mt
source-wordcount: '741'
ht-degree: 2%

---

# （ベータ版） [!DNL Azure Event Hubs] 接続

## 概要 {#overview}

>[!IMPORTANT]
>
>この [!DNL Azure Event Hubs] の宛先は、現在ベータ版です。 ドキュメントと機能は変更される場合があります。

[!DNL Azure Event Hubs] は、ビッグデータストリーミングプラットフォームおよびイベント取り込みサービスです。 1 秒あたりに数百万件のイベントを受信して処理できます。 イベントハブに送信されるデータは、任意のリアルタイム分析プロバイダーまたはバッチ/ストレージアダプターを使用して、変換および保存できます。

へのリアルタイムアウトバウンド接続を作成できます [!DNL Azure Event Hubs] Adobe Experience Platformからデータをストリーミングするためのストレージ。

* 詳しくは、 [!DNL Azure Event Hubs]を参照し、 [Microsoftドキュメント](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-about).
* 接続するには [!DNL Azure Event Hubs] プログラムを使用して、 [ストリーミング宛先 API のチュートリアル](../../api/streaming-destinations.md).
* 接続するには [!DNL Azure Event Hubs] Platform ユーザーインターフェイスを使用する場合は、以下の節を参照してください。

![UI でのAWS Kinesis](../../assets/catalog/cloud-storage/event-hubs/catalog.png)

## ユースケース {#use-cases}

次のようなストリーミング宛先を使用する [!DNL Azure Event Hubs]を使用すると、高価値のセグメントイベントや関連するプロファイル属性を、選択したシステムに簡単にフィードできます。

例えば、見込み客がホワイトペーパーをダウンロードし、それを「コンバージョン傾向が高い」セグメントに認定したとします。 見込み客が属するセグメントを [!DNL Azure Event Hubs] の宛先に指定した場合、このイベントは [!DNL Azure Event Hubs]. 企業の IT システムに最適に対応できると思われるように、このイベントの上に、自らのアプローチを使用し、ビジネスロジックを説明することができます。

## 書き出しタイプ {#export-type}

**プロファイルベース**  — セグメントのすべてのメンバーを、目的のスキーマフィールド ( 例：電子メールアドレス、電話番号、姓 )。 [オーディエンスのアクティベーションワークフロー](../../ui/activate-streaming-profile-destinations.md#select-attributes).

## 宛先に接続 {#connect}

この宛先に接続するには、 [宛先設定のチュートリアル](../../ui/connect-destination.md).

### 接続パラメーター {#parameters}

While [設定](../../ui/connect-destination.md) この宛先には、次の情報を指定する必要があります。

* **[!UICONTROL SAS キー名]** および **[!UICONTROL SAS キー]**:SAS キー名とキーを入力します。 認証の詳細 [!DNL Azure Event Hubs] に SAS キーを設定 [Microsoftドキュメント](https://docs.microsoft.com/en-us/azure/event-hubs/authenticate-shared-access-signature).
* **[!UICONTROL 名前空間]**:次の項目を入力します。 [!DNL Azure Event Hubs] 名前空間。 詳細 [!DNL Azure Event Hubs] 名前空間 [Microsoftドキュメント](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-create#create-an-event-hubs-namespace).
* **[!UICONTROL 名前]**:接続先の名前を入力 [!DNL Azure Event Hubs].
* **[!UICONTROL 説明]**:接続の説明を入力します。  例：&quot;プレミアム層顧客&quot;、&quot;キテサーフィンに興味を持つ男性&quot;。
* **[!UICONTROL eventHubName]**:ストリームの名前を [!DNL Azure Event Hubs] 宛先。

## この宛先へのセグメントのアクティブ化 {#activate}

詳しくは、 [ストリーミングプロファイルの書き出し先に対するオーディエンスデータのアクティブ化](../../ui/activate-streaming-profile-destinations.md) を参照してください。

## プロファイルの書き出し動作 {#profile-export-behavior}

Experience Platformは、セグメント認定または他の重要なイベントの後にプロファイルに関連する更新が発生した場合にのみ、プロファイルの書き出し動作を Azure Event Hubs の宛先に最適化します。 プロファイルは、次の状況で宛先に書き出されます。

* プロファイルの更新は、宛先にマッピングされた少なくとも 1 つのセグメントのセグメントメンバーシップの変更によってトリガーされました。 例えば、プロファイルは、宛先にマッピングされたいずれかのセグメントに適合しているか、宛先にマッピングされたいずれかのセグメントから退出しています。
* プロファイルの更新は、 [id マップ](/help/xdm/field-groups/profile/identitymap.md). 例えば、宛先にマッピングされたセグメントの 1 つに対して既に適合しているプロファイルの ID マップ属性に新しい ID が追加されたとします。
* プロファイルの更新は、宛先にマッピングされた少なくとも 1 つの属性の属性の変更によってトリガーされました。 例えば、マッピング手順で宛先にマッピングされた属性の 1 つがプロファイルに追加されます。

上記のすべての場合、関連する更新がおこなわれたプロファイルのみが宛先にエクスポートされます。 例えば、宛先フローにマッピングされたセグメントのメンバーが 100 人で、5 つの新しいプロファイルがセグメントの対象として認定されている場合、宛先への書き出しは増分で、5 つの新しいプロファイルのみが含まれます。

変更内容がどこにあっても、プロファイルに対してマッピングされたすべての属性が書き出されることに注意してください。 したがって、上の例では、属性自体が変更されていない場合でも、これら 5 つの新しいプロファイルに対してマッピングされたすべての属性が書き出されます。

## 書き出されたデータ {#exported-data}

エクスポート済み [!DNL Experience Platform] データは次の場所に配置されます： [!DNL Azure Event Hubs] JSON 形式で書き出します。 例えば、以下のイベントには、特定のセグメントに適合し、別のセグメントから離脱したオーディエンスの電子メールアドレスプロファイル属性が含まれます。 この見込み客の ID は、ECID と電子メールです。

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
>* [Azure Event Hubs に接続し、フローサービス API を使用してデータをアクティブ化する](../../api/streaming-destinations.md)
>* [AWS Kinesisの宛先](./amazon-kinesis.md)
>* [宛先のタイプとカテゴリ](../../destination-types.md)

