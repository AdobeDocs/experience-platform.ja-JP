---
keywords: ストリーミング；
title: HTTP API 接続
description: Adobe Experience Platformの HTTP API の宛先を使用すると、プロファイルデータをサードパーティの HTTP エンドポイントに送信できます。
exl-id: 165a8085-c8e6-4c9f-8033-f203522bb288
source-git-commit: bf36592fe4ea7b9d9b6703f3aca8fd8344fe5c9f
workflow-type: tm+mt
source-wordcount: '1274'
ht-degree: 3%

---

# （ベータ版）HTTP API 接続

>[!IMPORTANT]
>
>Platform の HTTP API の宛先は、現在ベータ版です。 ドキュメントと機能は変更される場合があります。

## 概要 {#overview}

HTTP API の宛先は [!DNL Adobe Experience Platform] プロファイルデータをサードパーティの HTTP エンドポイントに送信する際に役立つストリーミングの宛先です。

プロファイルデータを HTTP エンドポイントに送信するには、まず [宛先に接続](#connect-destination) in [!DNL Adobe Experience Platform].

## ユースケース {#use-cases}

HTTP 宛先は、XDM プロファイルデータとオーディエンスセグメントを汎用の HTTP エンドポイントに書き出す必要があるお客様をターゲットにしています。

HTTP エンドポイントは、お客様独自のシステムまたはサードパーティのソリューションのいずれかになります。

## 前提条件 {#prerequisites}

>[!IMPORTANT]
>
>会社で HTTP API 宛先ベータ版機能を有効にする場合は、Adobe担当者またはAdobeカスタマーケアにお問い合わせください。

HTTP API の宛先を使用してExperience Platformからデータを書き出すには、次の前提条件を満たす必要があります。

* REST API をサポートする HTTP エンドポイントが必要です。
* HTTP エンドポイントは、Experience Platformプロファイルスキーマをサポートする必要があります。 HTTP API の宛先では、サードパーティのペイロードスキーマへの変換はサポートされていません。 詳しくは、 [書き出されたデータ](#exported-data) 「 」セクションに、Experience Platform出力スキーマの例を示します。
* HTTP エンドポイントはヘッダーをサポートする必要があります。
* HTTP エンドポイントはをサポートしている必要があります [OAuth 2.0 クライアント資格情報](https://www.oauth.com/oauth2-servers/access-tokens/client-credentials/) 認証。 この要件は、HTTP API の宛先がベータ段階にある間に有効です。
* 次の例に示すように、クライアント資格情報をエンドポイントへのPOSTリクエストの本文に含める必要があります。

```shell
curl --location --request POST '<YOUR_API_ENDPOINT>' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--data-urlencode 'grant_type=client_credentials' \
--data-urlencode 'client_id=<CLIENT_ID>' \
--data-urlencode 'client_secret=<CLIENT_SECRET>'
```


また、 [Adobe Experience Platform Destination SDK](/help/destinations/destination-sdk/overview.md) ：統合を設定し、HTTP エンドポイントにExperience Platformプロファイルデータを送信します。

## 宛先に接続 {#connect-destination}

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

## 製品に関する考慮事項 {#product-considerations}

Experience Platformは、固定の静的 IP セットを介して HTTP エンドポイントにデータをストリーミングアウトしません。 したがって、Adobeは HTTP API の宛先に対してできる静的 IP のリストを提供できません許可リスト。

## プロファイルの書き出し動作 {#profile-export-behavior}

Experience Platformは、セグメント認定または他の重要なイベントに続いてプロファイルに関連する更新が発生した場合にのみ、HTTP API 宛先へのプロファイルの書き出し動作を最適化します。 プロファイルは、次の状況で宛先に書き出されます。

* プロファイルの更新は、宛先にマッピングされた少なくとも 1 つのセグメントのセグメントメンバーシップの変更によって決定されました。 例えば、プロファイルは、宛先にマッピングされたいずれかのセグメントに適合しているか、宛先にマッピングされたいずれかのセグメントから退出しています。
* プロファイルの更新は、 [id マップ](/help/xdm/field-groups/profile/identitymap.md). 例えば、宛先にマッピングされたセグメントの 1 つに対して既に適合しているプロファイルの ID マップ属性に新しい ID が追加されたとします。
* プロファイルの更新は、宛先にマッピングされた属性の少なくとも 1 つに対する属性の変更によって決定されました。 例えば、マッピング手順で宛先にマッピングされた属性の 1 つがプロファイルに追加されます。

上記のすべての場合、関連する更新がおこなわれたプロファイルのみが宛先にエクスポートされます。 例えば、宛先フローにマッピングされたセグメントのメンバーが 100 人で、5 つの新しいプロファイルがセグメントの対象として認定されている場合、宛先への書き出しは増分で、5 つの新しいプロファイルのみが含まれます。

変更内容がどこにあっても、プロファイルに対してマッピングされたすべての属性が書き出されることに注意してください。 したがって、上の例では、属性自体が変更されていない場合でも、これら 5 つの新しいプロファイルに対してマッピングされたすべての属性が書き出されます。

### 何が更新を決定し、何がエクスポートに含まれるか {#what-determines-export-what-is-included}

特定のプロファイルに対して書き出されるデータに関しては、 *は、HTTP API 宛先へのデータ書き出しを決定するものです。* および *エクスポートに含まれるデータ*.

| 宛先の書き出しを決定する要素 | 宛先の書き出しに含まれる内容 |
|---------|----------|
| <ul><li>マッピングされた属性とセグメントは、宛先の更新のキューとして機能します。 つまり、マッピングされたセグメントの状態（null から適合済み、適合済み/既存から既存へ）が変更されたり、マッピングされた属性が更新された場合、宛先の書き出しがキックオフされます。</li><li>ID は現在 HTTP API の宛先にマッピングできないので、特定のプロファイルの ID の変更によって宛先の書き出しも決まります。</li><li>属性に対する変更は、同じ値であるかどうかに関わらず、属性に対する更新として定義されます。 つまり、値自体が変更されていない場合でも、属性の上書きは変更と見なされます。</li></ul> | <ul><li>（最新のメンバーシップステータスを持つ）すべてのセグメントは、データフローにマッピングされているかどうかに関係なく、 `segmentMembership` オブジェクト。</li><li>内のすべての ID `identityMap` オブジェクトも含まれます (Experience Platformは、現在、HTTP API の宛先で ID マッピングをサポートしていません )。</li><li>マッピングされた属性のみが宛先エクスポートに含まれます。</li></ul> |

{style=&quot;table-layout:fixed&quot;}

例えば、このデータフローを HTTP 宛先に対して考えてみましょう。この宛先では、3 つのセグメントがデータフローで選択され、4 つの属性が宛先にマッピングされます。

![HTTP API 宛先のデータフロー](/help/destinations/assets/catalog/http/profile-export-example-dataflow.png)

<!--

![HTTP API destination dataflow](/help/destinations/assets/catalog/http/dataflow-destination.png)

![Mapped attributes](/help/destinations/assets/catalog/http/mapped-attributes.png)

-->

宛先へのプロファイルエクスポートは、いずれかの *3 つのマッピングされたセグメント*. ただし、データエクスポートでは、 `segmentMembership` オブジェクト ( [書き出されたデータ](#exported-data) の節を参照 )、その特定のプロファイルがそのメンバーの場合は、その他のマッピングされていないセグメントが表示されることがあります。 プロファイルが DeLorean Cars セグメントで顧客の資格を得ている一方で、「Back to the Future」映画や SF ファンセグメントのメンバーでもある場合、他の 2 つのセグメントも `segmentMembership` データエクスポートのオブジェクト（データフローでマッピングされていない場合）。

プロファイル属性の観点から、上でマッピングした 4 つの属性に対する変更によって、書き出し先が決まり、プロファイルに存在する 4 つのマッピング済み属性のいずれかがデータ書き出しに表示されます。

## 書き出されたデータ {#exported-data}

エクスポート済み [!DNL Experience Platform] データは、 [!DNL HTTP] の宛先を JSON 形式で指定します。 例えば、以下のエクスポートには、特定のセグメントに適合し、別の 2 つのセグメントのメンバーであり、別のセグメントから離脱したプロファイルが含まれています。 書き出しには、プロファイル属性の名、姓、生年月日、個人の電子メールアドレスも含まれます。 このプロファイルの ID は、ECID と電子メールです。

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
