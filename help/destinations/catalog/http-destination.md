---
keywords: ストリーミング；
title: HTTP 接続
description: Adobe Experience Platformの HTTP 宛先を使用すると、プロファイルデータをサードパーティの HTTP エンドポイントに送信できます。
exl-id: 165a8085-c8e6-4c9f-8033-f203522bb288
source-git-commit: 3aac1e7c7fe838201368379da8504efc8e316e1c
workflow-type: tm+mt
source-wordcount: '357'
ht-degree: 9%

---

# （アルファ） [!DNL HTTP] 接続

>[!IMPORTANT]
>
>Platform の [!DNL HTTP] の宛先は現在アルファです。 ドキュメントと機能は変更される場合があります。

## 概要 {#overview}

[!DNL HTTP] の宛先は、プロファイルデータをサードパーティの [!DNL HTTP] エンドポイントに送信するのに役立つ [!DNL Adobe Experience Platform] ストリーミングの宛先です。

プロファイルデータを [!DNL HTTP] エンドポイントに送信するには、まず [[!DNL Adobe Experience Platform]](#connect-destination) で宛先に接続する必要があります。

## ユースケース {#use-cases}

[!DNL HTTP] の宛先は、XDM プロファイルデータとオーディエンスセグメントを汎用の [!DNL HTTP] エンドポイントに書き出す必要があるお客様をターゲットにしています。

[!DNL HTTP] エンドポイントは、お客様独自のシステムまたはサードパーティのソリューションのいずれかです。

## 宛先に接続 {#connect}

この宛先に接続するには、[ 宛先の設定に関するチュートリアル ](../ui/connect-destination.md) で説明されている手順に従います。

### 接続パラメーター {#parameters}

[ この宛先を設定 ](../ui/connect-destination.md) する際は、次の情報を指定する必要があります。

* **[!UICONTROL httpEndpoint]**:プロ [!DNL URL] ファイルデータの送信先の HTTP エンドポイントの。
   * 必要に応じて、[!UICONTROL httpEndpoint] [!DNL URL] にクエリパラメーターを追加できます。
* **[!UICONTROL authEndpoint]**:認証に [!DNL URL] 使用する HTTP エンドポイント [!DNL OAuth2] の。
* **[!UICONTROL クライアント ID]**:クライア [!DNL clientID] ント資格情報で使用され [!DNL OAuth2] るパラメーター。
* **[!UICONTROL クライアント秘密鍵]**:クライア [!DNL clientSecret] ント資格情報で使用され [!DNL OAuth2] るパラメーター。

   >[!NOTE]
   >
   >現在、[!DNL OAuth2] クライアント資格情報のみがサポートされています。

* **[!UICONTROL 名前]**:この宛先が将来認識される名前を入力します。
* **[!UICONTROL 説明]**:今後この宛先を識別するのに役立つ説明を入力します。
* **[!UICONTROL カスタムヘッダー]**:宛先の呼び出しに含めるカスタムヘッダーを、次の形式で入力します。 `header1:value1,header2:value2,...headerN:valueN`.

   >[!IMPORTANT]
   >
   >現在の実装には、少なくとも 1 つのカスタムヘッダーが必要です。 この制限は、今後のアップデートで解決されます。

## この宛先へのセグメントのアクティブ化 {#activate}

この宛先に対してオーディエンスセグメントをアクティブ化する手順については、[ ストリーミングプロファイルの書き出し先へのオーディエンスデータのアクティブ化 ](../ui/activate-streaming-profile-destinations.md) を参照してください。

### 宛先属性 {#attributes}

[[!UICONTROL  属性を選択 ]](../ui/activate-streaming-profile-destinations.md#select-attributes) 手順では、[ 和集合スキーマ ](../../profile/home.md#profile-fragments-and-union-schemas) から一意の識別子を選択することをAdobeにお勧めします。 宛先に書き出す一意の ID およびその他の XDM フィールドを選択します。

## エクスポートされたデータ {#exported-data}

書き出された [!DNL Experience Platform] データは、JSON 形式の [!DNL HTTP] 宛先に格納されます。 例えば、以下のイベントは、特定のセグメントに適合し、別のセグメントから離脱したオーディエンスの電子メールアドレスプロファイル属性を含みます。 この見込み客の ID は [!DNL ECID] と電子メールです。

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
