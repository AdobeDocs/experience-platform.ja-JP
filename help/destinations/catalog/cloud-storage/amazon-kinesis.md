---
keywords: AmazonKinesis;kinesis宛先；kinesis
title: AmazonKinesis接続
description: Adobe Experience Platformからのデータをストリーミングするために、AmazonKinesisストレージへのリアルタイムの発信接続を作成します。
translation-type: tm+mt
source-git-commit: 32cb198bcf2c142b50c4b7a60282f0c923be06b1
workflow-type: tm+mt
source-wordcount: '510'
ht-degree: 6%

---


# （ベータ版） [!DNL Amazon Kinesis]接続

>[!IMPORTANT]
>
>プラットフォームの[!DNL Amazon Kinesis]宛先は現在ベータ版です。 ドキュメントと機能は変更される場合があります。

[!DNL Amazon Web Services]の[!DNL Kinesis Data Streams]サービスを使用すると、大量のデータレコードをリアルタイムで収集して処理できます。

[!DNL Amazon Kinesis]ストレージへのリアルタイムの送信接続を作成して、Adobe Experience Platformからデータをストリーミングできます。

* [!DNL Amazon Kinesis]について詳しくは、[Amazonのドキュメント](https://docs.aws.amazon.com/streams/latest/dev/introduction.html)を参照してください。
* プログラム的に[!DNL Amazon Kinesis]に接続するには、[ストリーミング送信先APIのチュートリアル](../../api/streaming-destinations.md)を参照してください。
* プラットフォームユーザーインターフェイスを使用して[!DNL Amazon Kinesis]に接続するには、以下の節を参照してください。

![AmazonKinesisUI](../../assets/catalog/cloud-storage/amazon-kinesis/catalog.png)

## 使用例 {#use-cases}

[!DNL Amazon Kinesis]などのストリーミング送信先を使用すると、高価値のセグメントイベントや関連するプロファイル属性を、簡単に任意のシステムにフィードできます。

例えば、見込み客が、「コンバージョンする傾向が高い」セグメントに該当するホワイトペーパーをダウンロードしたとします。 見込み客が属するセグメントを[!DNL Amazon Kinesis]宛先にマッピングすると、[!DNL Amazon Kinesis]にこのイベントが表示されます。 企業のITシステムで最も効果的に機能すると考えられるように、Do-It-Yoursenアプローチを採用し、イベントの上にビジネスロジックを記述することができます。

## エクスポートの種類{#export-type}

**プロファイルベース**  — セグメントのすべてのメンバーを、必要なスキーマフィールド(例：電子メールアドレス、電話番号、姓)。 [宛先アクティベーションワークフローの属性を選択画面で選択](../../ui/activate-destinations.md#select-attributes)。

## 宛先の接続 {#connect-destination}

[!DNL Amazon]でサポートされるものも含め、クラウドストレージへの接続方法については、[クラウドストレージの接続先ワークフロー](./workflow.md)を参照してください。

[!DNL Amazon Kinesis]宛先に対して、宛先を作成ワークフローで次の情報を入力します。

### 認証手順{#authentication-step}

* **[!DNL Amazon Web Services]アクセスキーと秘密鍵**:で、 [!DNL Amazon Web Services]ペアを生成して、プラットフォームに `access key - secret access key`  [!DNL Amazon Kinesis] アカウントへのアクセスを許可します。詳しくは、[AmazonWebサービスドキュメント](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html)を参照してください。
* **region**:データをストリーミングする [!DNL Amazon Web Services] 領域を指定します。

![アカウント手順の入力フィールド](../../assets/catalog/cloud-storage/amazon-kinesis/account.png)

### セットアップ手順{#setup-step}

* **名前**:接続先の名前を指定  [!DNL Amazon Kinesis]
* **説明**:への接続の説明を入力し [!DNL Amazon Kinesis]ます。
* **stream**:ア [!DNL Amazon Kinesis] カウント内の既存のデータストリームの名前を指定します。プラットフォームは、このストリームにデータをエクスポートします。
* **[!UICONTROL マーケティングアクション]**:マーケティングアクションは、データをエクスポート先にエクスポートする意図を示します。Adobe定義のマーケティングアクションから選択するか、独自のマーケティングアクションを作成することができます。 マーケティング活動の詳細については、「[Adobe Experience Platform](../../../data-governance/policies/overview.md)のデータガバナンス」ページを参照してください。 Adobe定義の個々のマーケティングアクションについて詳しくは、[データ使用ポリシーの概要](../../../data-governance/policies/overview.md)を参照してください。

![認証手順の入力フィールド](../../assets/catalog/cloud-storage/amazon-kinesis/setup.png)

<!--

>[!IMPORTANT]
>
>Platform needs `write` permissions on the bucket object where the export files will be delivered.

-->

## セグメントのアクティブ化 {#activate-segments}

セグメントのアクティベーションワークフローについて詳しくは、「[宛先へのプロファイルとセグメントのアクティブ化](../../ui/activate-destinations.md)」を参照してください。

## エクスポートされたデータ{#exported-data}

書き出した[!DNL Experience Platform]データは、JSON形式で[!DNL Amazon Kinesis]に格納されます。 例えば、次のイベントには、特定のセグメントに該当し、別のセグメントから離脱したオーディエンスの電子メールアドレスプロファイル属性が含まれています。 この見込み客のIDは、ECIDと電子メールです。

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
>* [AmazonKinesisに接続し、Flow Service APIを使用してデータをアクティブにする](../../api/streaming-destinations.md)
>* [Azureイベントハブの宛先](./azure-event-hubs.md)
>* [宛先のタイプとカテゴリ](../../destination-types.md)

