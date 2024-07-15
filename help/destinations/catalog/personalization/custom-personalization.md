---
keywords: カスタムパーソナライゼーション;宛先;Experience Platform カスタムの宛先;
title: カスタムパーソナライゼーション接続
description: この宛先は、Adobe Experience Platformからオーディエンス情報を取得する方法として、サイトで実行されている外部のパーソナライゼーション、コンテンツ管理システム、広告サーバーおよびその他のアプリケーションを提供します。 この宛先は、ユーザープロファイルオーディエンスメンバーシップに基づいて、リアルタイムのパーソナライゼーションを提供します。
exl-id: 2382cc6d-095f-4389-8076-b890b0b900e3
source-git-commit: 182176aa2e588aa356d5fea23b49f17bc8a50314
workflow-type: tm+mt
source-wordcount: '912'
ht-degree: 57%

---


# カスタムパーソナライゼーション接続 {#custom-personalization-connection}

## 宛先の変更ログ {#changelog}

| リリース月 | 更新タイプ | 説明 |
|---|---|---|
| 2023年5月 | 機能とドキュメントの更新 | 2023 年 5 月の時点で、**[!UICONTROL カスタムパーソナライゼーション]** 接続は [ 属性ベースのパーソナライゼーション ](../../ui/activate-edge-personalization-destinations.md#map-attributes) をサポートしており、通常すべてのお客様が利用できます。 |

{style="table-layout:auto"}

>[!IMPORTANT]
>
>プロファイル属性には、機密データが含まれている場合があります。 このデータを保護するには、属性ベースのパーソナライゼーション用に ](/help/server-api/overview.md) カスタム Personalization **[!UICONTROL をEdge Networkする際に]** 0}Configuration Server API} を使用する必要があります。 [すべての Server API 呼び出しは、[ 認証済みコンテキスト ](../../../server-api/authentication.md) で行う必要があります。
>
>Web または Mobile SDK の実装に既に使用しているのと同じデータストリームを使用するサーバーサイドEdge Networkを追加することで ](/help/server-api/overview.md)<br>1}Integration Server API を介してプロファイル属性を取得できます。[
>
><br> 上記の要件に従わない場合、パーソナライゼーションはオーディエンスメンバーシップのみに基づきます。

## 概要 {#overview}

この宛先の設定により、お客様の web サイトで実行されている外部のパーソナライゼーションプラットフォーム、コンテンツ管理システム、広告サーバーおよびその他のアプリケーションが、Adobe Experience Platformからオーディエンス情報を取得できるようになります。

## 前提条件 {#prerequisites}

この統合は、[Adobe Experience Platform Web SDK](/help/web-sdk/home.md) または [Adobe Experience Platform Mobile SDK](https://developer.adobe.com/client-sdks/documentation/) によって実現されます。この宛先を使用するには、次の SDK のいずれかを使用する必要があります。

>[!IMPORTANT]
>
>カスタムパーソナライゼーション接続を作成する前に、[ エッジパーソナライゼーションの宛先に対してオーディエンスデータをアクティブ化する ](../../ui/activate-edge-personalization-destinations.md) 方法についてのガイドをお読みください。 このガイドでは、複数の Experience Platform コンポーネントをまたいで、同じページおよび次のページのパーソナライゼーションの使用例に必要な設定手順を説明します。

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスオリジン | サポートあり | 説明 |
|---------|----------|----------|
| [!DNL Segmentation Service] | ✓ | Experience Platform[ セグメント化サービス ](../../../segmentation/home.md) を通じて生成されたオーディエンス。 |
| カスタムアップロード | ✓ | CSV ファイルから Experience Platform に[読み込まれた](../../../segmentation/ui/audience-portal.md#import-audience)オーディエンス。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!DNL Profile request]** | 単一のプロファイルのカスタムパーソナライゼーションの宛先にマッピングされているすべてのオーディエンスを要求しています。 様々なカスタムパーソナライゼーションの宛先を様々な [アドビのデータ収集データストリーム](../../../datastreams/overview.md)に対して設定できます。 |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。詳しくは、[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)を参照してください。 |

## 宛先への接続 {#connect}

>[!CONTEXTUALHELP]
>id="platform_destinations_custom_personalization_datastream"
>title="データストリーム ID について"
>abstract="このオプションは、ページへの応答にオーディエンスを含めるデータ収集データストリームを決定します。ドロップダウンメニューには、宛先設定が有効になっているデータストリームのみが表示されます。宛先を設定する前に、データストリームを設定する必要があります。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/datastreams/configure.html?lang=ja" text="データストリームの設定方法を学ぶ"

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL 宛先の表示]** および **[!UICONTROL 宛先の管理]**[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。

### 接続パラメーター {#parameters}

この宛先を[設定](../../ui/connect-destination.md)するとき、次の情報を指定する必要があります。

* **[!UICONTROL 名前]**：この宛先に希望する名前を入力します。
* **[!UICONTROL 説明]**：宛先についての説明を入力します。例えば、この宛先を使用しているキャンペーンを指定できます。このフィールドはオプションです。
* **[!UICONTROL 統合エイリアス]**：この値は、JSON オブジェクト名として Experience Platform Web SDK に送信されます。
* **[!UICONTROL データストリーム ID]**：これにより、ページへの応答にオーディエンスを含めるデータ収集データストリームが決定されます。 ドロップダウンメニューには、宛先設定が有効になっているデータストリームのみが表示されます。詳しくは、[データストリームの設定](../../../datastreams/overview.md)を参照してください。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>データをアクティブ化するには、**[!UICONTROL 宛先の表示]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]** および **[!UICONTROL セグメントの表示]**[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

この宛先にオーディエンスをアクティブ化する手順については、[ プロファイルとオーディエンスのエッジパーソナライゼーションの宛先のアクティブ化 ](../../ui/activate-edge-personalization-destinations.md) をお読みください。

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

[Tags](/help/tags/home.md) を使用せずにExperience PlatformWeb SDK をデプロイする場合、[ コマンド応答 ](/help/web-sdk/commands/command-responses.md) を使用して、書き出したデータを確認します。

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

### [!UICONTROL  属性を含むカスタムPersonalization] の応答の例

**[!UICONTROL 属性を含むカスタムPersonalization]** を使用する場合、API 応答は次の例のようになります。

**[!UICONTROL 属性を含むカスタムPersonalization]** と **[!UICONTROL カスタムPersonalization]** の違いは、API 応答に `attributes` セクションが含まれることです。

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
