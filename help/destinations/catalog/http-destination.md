---
keywords: ストリーミング
title: HTTP 接続
description: Adobe エクスペリエンスプラットフォームの HTTP の出力先は、サードパーティの HTTP エンドポイントにプロファイルデータを送信することができます。
exl-id: 165a8085-c8e6-4c9f-8033-f203522bb288
source-git-commit: 2b1cde9fc913be4d3bea71e7d56e0e5fe265a6be
workflow-type: tm+mt
source-wordcount: '357'
ht-degree: 9%

---

# (アルファ) [!DNL HTTP] 接続

>[!IMPORTANT]
>
>[!DNL HTTP]プラットフォームの移行先は、現在アルファで指定されています。ドキュメントと機能は変更される場合があります。

## 概要 {#overview}

[!DNL HTTP]リンク先は、 [!DNL Adobe Experience Platform] サードパーティのエンドポイントにプロファイルデータを送信するためのストリーミングの出力先になり [!DNL HTTP] ます。

プロファイルデータをエンドポイントに送信するには [!DNL HTTP] 、まず宛先に接続する必要があり [[!DNL Adobe Experience Platform]](#connect-destination) ます。

## ユースケース {#use-cases}

[!DNL HTTP]リンク先は、XDM プロファイルデータおよび参加者セグメントを汎用エンドポイントに書き出す必要があるユーザーを対象としてい [!DNL HTTP] ます。

[!DNL HTTP] エンドポイントには、お客様のシステムまたはサードパーティ製のソリューションを使用できます。

## 目的の場所に接続します。 {#connect}

この送信先に接続するには、宛先の設定チュートリアルで説明されている手順に従って [ ](../ui/connect-destination.md) ください。

### 接続パラメーター {#parameters}

このコピー先を設定する際に、 [ ](../ui/connect-destination.md) 次の情報を入力する必要があります。

* **[!UICONTROL httpEndpoint]** : [!DNL URL] プロファイルデータの送信先となる HTTP エンドポイントの情報を指定します。
   * 必要に応じて、httpEndpoint にクエリパラメーターを追加することもでき  [!DNL URL] ます。
* **[!UICONTROL authEndpoint]** : [!DNL URL] 認証に使用される HTTP エンドポイントのを示し [!DNL OAuth2] ます。
* **[!UICONTROL クライアント ID]** : [!DNL clientID] クライアントの資格情報に使用されているパラメーターを指定 [!DNL OAuth2] します。
* **[!UICONTROL クライアントシークレット]** : [!DNL clientSecret] クライアントの資格情報で使用されるパラメーター [!DNL OAuth2] です。

   >[!NOTE]
   >
   >[!DNL OAuth2]現在、クライアントの資格情報のみがサポートされています。

* **[!UICONTROL 「名前」]** : 今後その宛先に付けられる名前を入力します。
* **[!UICONTROL 説明]** : 今後この宛先を識別するために使用する説明を入力します。
* **[!UICONTROL 「カスタムヘッダー]** 」: 宛先の呼び出しに含めるカスタムヘッダーを入力します。 `header1:value1,header2:value2,...headerN:valueN` .

   >[!IMPORTANT]
   >
   >現在の実装では、カスタムヘッダーが少なくとも1つ必要です。 この制限は今後のアップデートで解決されます。

## セグメントをこの宛先にアクティブにします。 {#activate}

[ ](../ui/activate-streaming-profile-destinations.md) この宛先までの視聴ユーザーセグメントをアクティブにする方法については、「プロファイルの書き出し先のストリーミング送信先のストリーミングについて」を参照してください。

### 宛先属性 {#attributes}

「 [[!UICONTROL  属性を選択」段階では、 ]](../ui/activate-streaming-profile-destinations.md#select-attributes) ユニオンスキーマから一意の識別子を選択することをお勧め [ ](../../profile/home.md#profile-fragments-and-union-schemas) します。 宛先に書き出す一意の ID およびその他の XDM フィールドを選択します。

## 書き出したデータ {#exported-data}

書き出した [!DNL Experience Platform] データは [!DNL HTTP] JSON 形式で宛先に格納されます。 例えば、次のイベントには、特定のセグメントを対象としていて、別のセグメントを終了した対象ユーザーの電子メールアドレスプロファイル属性が含まれています。 この取引関係の id は、 [!DNL ECID] 電子メールによって送信されます。

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
