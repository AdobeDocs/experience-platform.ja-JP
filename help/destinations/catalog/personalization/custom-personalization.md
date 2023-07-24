---
keywords: カスタムパーソナライゼーション;宛先;Experience Platform カスタムの宛先;
title: カスタムパーソナライゼーション接続
description: この宛先は、Adobe Experience Platformからオーディエンス情報を取得する方法として、サイトで実行されている外部のパーソナライゼーション、コンテンツ管理システム、広告サーバーおよびその他のアプリケーションを提供します。 この宛先は、ユーザープロファイルのオーディエンスメンバーシップに基づいて、リアルタイムのパーソナライゼーションを提供します。
exl-id: 2382cc6d-095f-4389-8076-b890b0b900e3
source-git-commit: d8c1e41b90b5b81dd8475bd697f31ba27551e7fa
workflow-type: tm+mt
source-wordcount: '933'
ht-degree: 58%

---


# カスタムパーソナライゼーション接続 {#custom-personalization-connection}

## 宛先の変更ログ {#changelog}

| リリース月 | 更新タイプ | 説明 |
|---|---|---|
| 2023年5月 | 機能とドキュメントの更新 | 2023 年 5 月現在、 **[!UICONTROL カスタムパーソナライゼーション]** 接続サポート [属性ベースのパーソナライゼーション](../../ui/activate-edge-personalization-destinations.md#map-attributes) およびは、すべてのお客様が一般に利用できます。 |

{style="table-layout:auto"}

>[!IMPORTANT]
>
> プロファイル属性には、機密データが含まれている場合があります。 このデータを保護するには、 **[!UICONTROL カスタムパーソナライゼーション]** の宛先では、 [Edge Network Server API](/help/server-api/overview.md) 属性ベースのパーソナライゼーションの宛先を設定する際に使用します。 すべてのサーバー API 呼び出しは、 [認証コンテキスト](../../../server-api/authentication.md).
>
><br>既に統合に Web SDK または Mobile SDK を使用している場合は、サーバー側の統合を追加して、Server API を介して属性を取得できます。
>
><br>上記の要件に従わない場合、パーソナライゼーションはオーディエンスのメンバーシップのみに基づきます。

## 概要 {#overview}

この宛先を使用すると、Adobe Experience Platformから外部のパーソナライゼーションプラットフォーム、コンテンツ管理システム、広告サーバー、および顧客の Web サイト上で実行されているその他のアプリケーションに対してオーディエンス情報を取得できます。

## 前提条件 {#prerequisites}

この統合は、[Adobe Experience Platform Web SDK](../../../edge/home.md) または [Adobe Experience Platform Mobile SDK](https://aep-sdks.gitbook.io/docs/) によって実現されます。この宛先を使用するには、次の SDK のいずれかを使用する必要があります。

>[!IMPORTANT]
>
>カスタムパーソナライゼーション接続を作成する前に、次の手順に関するガイドをお読みください。 [エッジパーソナライゼーションの宛先に対するオーディエンスデータのアクティブ化](../../ui/activate-edge-personalization-destinations.md). このガイドでは、複数の Experience Platform コンポーネントをまたいで、同じページおよび次のページのパーソナライゼーションの使用例に必要な設定手順を説明します。

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるすべてのオーディエンスについて説明します。

すべての宛先は、Experience Platformを通じて生成されたオーディエンスのアクティブ化をサポートします [セグメント化サービス](../../../segmentation/home.md).

また、この宛先では、以下の表で説明するオーディエンスのアクティブ化もサポートされます。

| オーディエンスタイプ | 説明 |
---------|----------|
| カスタムアップロード | CSV ファイルからExperience Platformに取り込まれたオーディエンス。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!DNL Profile request]** | 単一のプロファイルのカスタムパーソナライゼーションの宛先にマッピングされるすべてのオーディエンスを要求しています。 様々なカスタムパーソナライゼーションの宛先を様々な [アドビのデータ収集データストリーム](../../../datastreams/overview.md)に対して設定できます。 |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいてExperience Platform内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。 [ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)の詳細についてはこちらを参照してください。 |

## 宛先への接続 {#connect}

>[!CONTEXTUALHELP]
>id="platform_destinations_custom_personalization_datastream"
>title="データストリーム ID について"
>abstract="このオプションは、ページへの応答にオーディエンスを含めるデータ収集データストリームを決定します。ドロップダウンメニューには、宛先設定が有効になっているデータストリームのみが表示されます。宛先を設定する前に、データストリームを設定する必要があります。"
>additional-url="https://experienceleague.adobe.com/docs/platform-learn/implement-mobile-sdk/initial-configuration/create-datastream.html" text="データストリームの設定方法の説明"

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。

### 接続パラメーター {#parameters}

この宛先を[設定](../../ui/connect-destination.md)するとき、次の情報を指定する必要があります。

* **[!UICONTROL 名前]**：この宛先に希望する名前を入力します。
* **[!UICONTROL 説明]**：宛先についての説明を入力します。例えば、この宛先を使用しているキャンペーンを指定できます。このフィールドはオプションです。
* **[!UICONTROL 統合エイリアス]**：この値は、JSON オブジェクト名として Experience Platform Web SDK に送信されます。
* **[!UICONTROL データストリーム ID]**:これにより、どのデータ収集データストリームでオーディエンスがページへの応答に含まれるかが決まります。 ドロップダウンメニューには、宛先設定が有効になっているデータストリームのみが表示されます。詳しくは、[データストリームの設定](../../../datastreams/overview.md)を参照してください。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続への詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対するオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>データをアクティブ化するには、**[!UICONTROL 宛先の管理]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]**&#x200B;および&#x200B;**[!UICONTROL セグメントの表示]** [に対するアクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

読み取り [プロファイルおよびオーディエンスのエッジパーソナライゼーションの宛先をアクティブ化する](../../ui/activate-edge-personalization-destinations.md) を参照してください。

## 書き出したデータ {#exported-data}

[Adobe Experience Platform のタグ](../../../tags/home.md)を使用して Experience Platform Web SDK をデプロイする場合、[イベント完了の送信](../../../tags/extensions/client/web-sdk/event-types.md)機能を使用すると、カスタムコードアクションには `event.destinations` 変数が追加され、書き出したデータを確認できます。

`event.destinations` 変数のサンプル値は次のようになります。

```
[
   {
      "type":"profileLookup",
      "destinationId":"7bb4cb8d-8c2e-4450-871d-b7824f547111",
      "alias":"personalizationAlias",
      "segments":[
         {
            "id":"399eb3e7-3d50-47d3-ad30-a5ad99e8ab77"
         },
         {
            "id":"499eb3e7-3d50-47d3-ad30-a5ad99e8ab77"
         }
      ]
   }
]
```

[タグ](../../../tags/home.md)を使用せずに Experience Platform Web SDK をデプロイする場合、[イベントからの応答の処理](../../../edge/fundamentals/tracking-events.md#handling-responses-from-events)機能を使用して、書き出したデータを確認します。

Adobe Experience Platform からの JSON 応答は、Adobe Experience Platform と統合しているアプリケーションの対応する統合エイリアスを見つけるために解析できます。オーディエンス ID は、ターゲティングパラメーターとしてアプリケーションのコードに渡すことができます。 次に、これが宛先の応答に特有なサンプルを示します。

```
alloy("sendEvent", {
  "renderDecisions": true,
  "xdm": {
    "commerce": {
      "order": {
        "purchaseID": "a8g784hjq1mnp3",
        "purchaseOrderNumber": "VAU3123",
        "currencyCode": "USD",
        "priceTotal": 999.98
      }
    }
  }
}).then(function(result) {
    if(result.destinations) { // Looking to see if the destination results are there
 
        // Get the destination with a particular alias
        var personalizationDestinations = result.destinations.filter(x => x.alias == "personalizationAlias")
        if(personalizationDestinations.length > 0) {
             // Code to pass the audience IDs into the system that corresponds to personalizationAlias
        }
        var adServerDestinations = result.destinations.filter(x => x.alias == "adServerAlias")
        if(adServerDestinations.length > 0) {
            // Code to pass the audience IDs into the system that corresponds to adServerAlias
        }
     }
   })
  .catch(function(error) {
    // Tracking the event failed.
  });
```

### 応答の例： [!UICONTROL 属性を含むカスタムパーソナライゼーション]

を使用する場合 **[!UICONTROL 属性を含むカスタムパーソナライゼーション]**&#x200B;に設定した場合、API 応答は以下の例のようになります。

違いは **[!UICONTROL 属性を含むカスタムパーソナライゼーション]** および **[!UICONTROL カスタムパーソナライゼーション]** は `attributes` 」の節を参照してください。

```json
[
    {
        "type": "profileLookup",
        "destinationId": "7bb4cb8d-8c2e-4450-871d-b7824f547130",
        "alias": "personalizationAlias",
        "attributes": {
             "countryCode": {
                   "value" : "DE"
              },
             "membershipStatus": {
                   "value" : "PREMIUM"
              }
         },         
        "segments": [
            {
                "id": "399eb3e7-3d50-47d3-ad30-a5ad99e8ab77"
            },
            {
                "id": "499eb3e7-3d50-47d3-ad30-a5ad99e8ab77"
            }
        ]
    }
]
```

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのように データガバナンスを実施するかについて詳しくは、[データガバナンスの概要](../../../data-governance/home.md)を参照してください。
