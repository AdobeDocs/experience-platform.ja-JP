---
keywords: カスタムパーソナライゼーション;宛先;Experience Platform カスタムの宛先;
title: カスタムパーソナライゼーション接続
description: この宛先は、Adobe Experience Platformからオーディエンス情報を取得する方法として、サイトで実行されている外部のパーソナライゼーション、コンテンツ管理システム、広告サーバーおよびその他のアプリケーションを提供します。 この宛先は、ユーザープロファイルオーディエンスメンバーシップに基づいて、リアルタイムのパーソナライゼーションを提供します。
exl-id: 2382cc6d-095f-4389-8076-b890b0b900e3
source-git-commit: 1b507e9846a74b7ac2d046c89fd7c27a818035ba
workflow-type: tm+mt
source-wordcount: '923'
ht-degree: 49%

---


# カスタムパーソナライゼーション接続 {#custom-personalization-connection}

## 宛先の変更ログ {#changelog}

| リリース月 | 更新タイプ | 説明 |
|---|---|---|
| 2023年5月 | 機能とドキュメントの更新 | 2023 年 5 月の時点で、**[!UICONTROL Custom personalization]** 接続は [&#x200B; 属性ベースのパーソナライゼーション &#x200B;](../../ui/activate-edge-personalization-destinations.md#map-attributes) をサポートしており、すべてのお客様が一般に利用できます。 |

{style="table-layout:auto"}

>[!IMPORTANT]
>
>プロファイル属性には、機密データが含まれている場合があります。 このデータを保護するには、属性ベースのパーソナライゼーション用に [&#x200B; の宛先を設定する際に &#x200B;](https://developer.adobe.com/data-collection-apis/docs/)Edge Network API&rbrace; を使用する必要があります。 **[!UICONTROL Custom Personalization]**&#x200B;すべてのEdge Network API 呼び出しは、[&#x200B; 認証済みコンテキスト &#x200B;](https://developer.adobe.com/data-collection-apis/docs/getting-started/authentication) で行う必要があります。
>
><br>1&rbrace;Edge Network API を使用してプロファイル属性を取得できます [&#x200B; サーバーサイド統合を追加して、web またはモバイル SDKの実装に既に使用しているものと同じデータストリームを使用します。](https://developer.adobe.com/data-collection-apis/docs/)
>
><br> 上記の要件に従わない場合、パーソナライゼーションはオーディエンスメンバーシップのみに基づきます。

## 概要 {#overview}

この宛先の設定により、お客様の web サイトで実行されている外部のパーソナライゼーションプラットフォーム、コンテンツ管理システム、広告サーバーおよびその他のアプリケーションが、Adobe Experience Platformからオーディエンス情報を取得できるようになります。

## 前提条件 {#prerequisites}

この宛先では、実装に応じて、次のいずれかのデータ収集方法を使用する必要があります。

* Web サイトからデータを収集する場合は [&#128279;](/help/web-sdk/home.md)Adobe Experience Platform Web SDK&rbrace; を使用します。
* モバイルアプリケーションからデータを収集する場合は [&#128279;](https://developer.adobe.com/client-sdks/documentation/)Adobe Experience Platform Mobile SDK&rbrace; を使用します。
* [Web SDK](https://developer.adobe.com/data-collection-apis/docs/) または [Mobile SDK](/help/web-sdk/home.md) を使用していない場合や、プロファイル属性に基づいてユーザーエクスペリエンスをパーソナライズする場合は [0&rbrace;Edge Network API&rbrace; を使用します。](https://developer.adobe.com/client-sdks/documentation/)

>[!IMPORTANT]
>
>カスタムパーソナライゼーション接続を作成する前に、[&#x200B; エッジパーソナライゼーションの宛先に対してオーディエンスデータをアクティブ化する &#x200B;](../../ui/activate-edge-personalization-destinations.md) 方法についてのガイドをお読みください。 このガイドでは、複数の Experience Platform コンポーネントをまたいで、同じページおよび次のページのパーソナライゼーションの使用例に必要な設定手順を説明します。

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスオリジン | サポートあり | 説明 |
|---------|----------|----------|
| [!DNL Segmentation Service] | ✓ | Experience Platform [&#x200B; セグメント化サービス &#x200B;](../../../segmentation/home.md) を通じて生成されたオーディエンス。 |
| カスタムアップロード | ✓ | CSV ファイルから Experience Platform に[読み込まれた](../../../segmentation/ui/audience-portal.md#import-audience)オーディエンス。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

| 項目 | タイプ | メモ |
|---------|----------|---------|
| 書き出しタイプ | **[!DNL Profile request]** | 単一のプロファイルのカスタムパーソナライゼーションの宛先にマッピングされているすべてのオーディエンスを要求しています。 様々なカスタムパーソナライゼーションの宛先を様々な [アドビのデータ収集データストリーム](../../../datastreams/overview.md)に対して設定できます。 |
| 書き出し頻度 | **[!UICONTROL Streaming]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。詳しくは、[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)を参照してください。 |

## 宛先への接続 {#connect}

>[!CONTEXTUALHELP]
>id="platform_destinations_custom_personalization_datastream"
>title="データストリームについて"
>abstract="このオプションは、ページへの応答にオーディエンスを含めるデータ収集データストリームを決定します。ドロップダウンメニューには、宛先設定が有効になっているデータストリームのみが表示されます。宛先を設定する前に、データストリームを設定する必要があります。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/datastreams/configure.html?lang=ja" text="データストリームの設定方法を学ぶ"

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL View Destinations]** および **[!UICONTROL Manage Destinations]**&#x200B;[&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。

### 接続パラメーター {#parameters}

この宛先を[設定](../../ui/connect-destination.md)するとき、次の情報を指定する必要があります。

* **[!UICONTROL Name]**：この宛先に希望する名前を入力します。
* **[!UICONTROL Description]**：宛先の説明を入力します。 例えば、この宛先を使用しているキャンペーンを指定できます。このフィールドはオプションです。
* **[!UICONTROL Integration alias]**：この値は、JSON オブジェクト名としてExperience Platform Web SDKに送信されます。
* **[!UICONTROL Datastream]**：これにより、ページへの応答にオーディエンスを含めるデータ収集データストリームが決定されます。 ドロップダウンメニューには、宛先設定が有効になっているデータストリームのみが表示されます。詳しくは、[データストリームの設定](../../../datastreams/overview.md)を参照してください。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続への詳細の入力を終えたら「**[!UICONTROL Next]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>データをアクティブ化するには、**[!UICONTROL View Destinations]**、**[!UICONTROL Activate Destinations]**、**[!UICONTROL View Profiles]**、**[!UICONTROL View Segments]** [&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

この宛先にオーディエンスをアクティブ化する手順については、[&#x200B; プロファイルとオーディエンスのエッジパーソナライゼーションの宛先のアクティブ化 &#x200B;](../../ui/activate-edge-personalization-destinations.md) をお読みください。

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

[Tags](/help/tags/home.md) を使用せずにExperience Platform web SDKをデプロイする場合は、[&#x200B; コマンド応答 &#x200B;](/help/web-sdk/commands/command-responses.md) を使用して、書き出したデータを確認します。

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

### [!UICONTROL Custom Personalization With Attributes] への応答の例

**[!UICONTROL Custom Personalization With Attributes]** を使用する場合、API 応答は次の例のようになります。

**[!UICONTROL Custom Personalization With Attributes]** と **[!UICONTROL Custom Personalization]** の違いは、API 応答に `attributes` セクションが含まれることです。

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
