---
keywords: ストリーミング；
title: HTTP 接続
description: Adobe Experience Platformの HTTP API の宛先を使用すると、プロファイルデータをサードパーティの HTTP エンドポイントに送信できます。
exl-id: 165a8085-c8e6-4c9f-8033-f203522bb288
source-git-commit: 8d2c5ef477d4707be4c0da43ba1f672fac797604
workflow-type: tm+mt
source-wordcount: '633'
ht-degree: 5%

---

# （ベータ版） [!DNL HTTP] API 接続

>[!IMPORTANT]
>
>この [!DNL HTTP] の宛先は、現在ベータ版です。 ドキュメントと機能は変更される場合があります。

## 概要 {#overview}

この [!DNL HTTP] API の宛先は [!DNL Adobe Experience Platform] プロファイルデータをサードパーティに送信するのに役立つストリーミングの宛先 [!DNL HTTP] エンドポイント。

プロファイルデータをに送信するには [!DNL HTTP] エンドポイントの場合、最初に [[!DNL Adobe Experience Platform]](#connect-destination).

## ユースケース {#use-cases}

この [!DNL HTTP] の宛先は、XDM プロファイルデータとオーディエンスセグメントを汎用のに書き出す必要があるお客様をターゲットにしています [!DNL HTTP] エンドポイント。

[!DNL HTTP] エンドポイントは、お客様独自のシステムまたはサードパーティ製ソリューションのいずれかになります。

## 宛先に接続 {#connect}

この宛先に接続するには、 [宛先設定のチュートリアル](../../ui/connect-destination.md).

### 接続パラメーター {#parameters}

While [設定](../../ui/connect-destination.md) この宛先には、次の情報を指定する必要があります。

* **[!UICONTROL httpEndpoint]**:の [!DNL URL] プロファイルデータを送信する HTTP エンドポイントのパスを指定します。
   * 必要に応じて、 [!UICONTROL httpEndpoint] [!DNL URL].
* **[!UICONTROL authEndpoint]**:の [!DNL URL] に使用される HTTP エンドポイントの [!DNL OAuth2] 認証。
* **[!UICONTROL クライアント ID]**:の [!DNL clientID] パラメーター [!DNL OAuth2] クライアント資格情報。
* **[!UICONTROL クライアント秘密鍵]**:の [!DNL clientSecret] パラメーター [!DNL OAuth2] クライアント資格情報。

   >[!NOTE]
   >
   >のみ [!DNL OAuth2] クライアント資格情報は現在サポートされています。

* **[!UICONTROL 名前]**:この宛先が将来認識される名前を入力します。
* **[!UICONTROL 説明]**:今後この宛先を識別するのに役立つ説明を入力します。
* **[!UICONTROL カスタムヘッダー]**:宛先の呼び出しに含めるカスタムヘッダーを、次の形式で入力します。 `header1:value1,header2:value2,...headerN:valueN`.

   >[!IMPORTANT]
   >
   >現在の実装には、少なくとも 1 つのカスタムヘッダーが必要です。 この制限は、今後のアップデートで解決されます。

## この宛先へのセグメントのアクティブ化 {#activate}

詳しくは、 [ストリーミングプロファイルの書き出し先に対するオーディエンスデータのアクティブ化](../../ui/activate-streaming-profile-destinations.md) を参照してください。

### 宛先属性 {#attributes}

内 [[!UICONTROL 属性を選択]](../../ui/activate-streaming-profile-destinations.md#select-attributes) 手順に従い、Adobeでは、 [和集合スキーマ](../../../profile/home.md#profile-fragments-and-union-schemas). 宛先に書き出す一意の ID およびその他の XDM フィールドを選択します。

## プロファイルの書き出し動作 {#profile-export-behavior}

Experience Platformは、セグメント認定または他の重要なイベントに続いてプロファイルに関連する更新が発生した場合にのみ、HTTP API 宛先へのプロファイルの書き出し動作を最適化します。 プロファイルは、次の状況で宛先に書き出されます。

* プロファイルの更新は、宛先にマッピングされた少なくとも 1 つのセグメントのセグメントメンバーシップの変更によってトリガーされました。 例えば、プロファイルは、宛先にマッピングされたいずれかのセグメントに適合しているか、宛先にマッピングされたいずれかのセグメントから退出しています。
* プロファイルの更新は、 [id マップ](/help/xdm/field-groups/profile/identitymap.md). 例えば、宛先にマッピングされたセグメントの 1 つに対して既に適合しているプロファイルの ID マップ属性に新しい ID が追加されたとします。
* プロファイルの更新は、宛先にマッピングされた少なくとも 1 つの属性の属性の変更によってトリガーされました。 例えば、マッピング手順で宛先にマッピングされた属性の 1 つがプロファイルに追加されます。

上記のすべての場合、関連する更新がおこなわれたプロファイルのみが宛先にエクスポートされます。 例えば、宛先フローにマッピングされたセグメントのメンバーが 100 人で、5 つの新しいプロファイルがセグメントの対象として認定されている場合、宛先への書き出しは増分で、5 つの新しいプロファイルのみが含まれます。

変更内容がどこにあっても、プロファイルに対してマッピングされたすべての属性が書き出されることに注意してください。 したがって、上の例では、属性自体が変更されていない場合でも、これら 5 つの新しいプロファイルに対してマッピングされたすべての属性が書き出されます。

## 書き出されたデータ {#exported-data}

エクスポート済み [!DNL Experience Platform] データは、 [!DNL HTTP] の宛先を JSON 形式で指定します。 例えば、以下のイベントには、特定のセグメントに適合し、別のセグメントから離脱したオーディエンスの電子メールアドレスプロファイル属性が含まれます。 この見込み客の ID は次のとおりです [!DNL ECID] 電子メール

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
