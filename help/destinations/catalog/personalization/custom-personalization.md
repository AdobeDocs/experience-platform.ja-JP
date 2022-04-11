---
keywords: カスタムパーソナライゼーション;宛先;Experience Platform カスタムの宛先;
title: カスタムパーソナライゼーション接続
description: この宛先は、Adobe Experience Platform からセグメント情報を取得する方法として、サイトで実行されている外部のパーソナライゼーション、コンテンツ管理システム、広告サーバーおよびその他のアプリケーションを提供します。この宛先は、ユーザープロファイルセグメントのメンバーシップに基づいて、リアルタイムのパーソナライゼーションを提供します。
exl-id: 2382cc6d-095f-4389-8076-b890b0b900e3
source-git-commit: 05217dead7e1365d6dcc0cc7ae4078628514d1d5
workflow-type: tm+mt
source-wordcount: '678'
ht-degree: 98%

---

# カスタムパーソナライゼーション接続 {#custom-personalization-connection}

## 概要 {#overview}

この宛先は、Adobe Experience Platform から外部のパーソナライゼーションプラットフォーム、コンテンツ管理システム、広告サーバーおよび顧客の web サイト上で実行されているその他のアプリケーションにセグメント情報を取得できます。

## 前提条件 {#prerequisites}

この統合は、[Adobe Experience Platform Web SDK](../../../edge/home.md) または [Adobe Experience Platform Mobile SDK](https://aep-sdks.gitbook.io/docs/) によって実現されます。この宛先を使用するには、次の SDK のいずれかを使用する必要があります。

>[!IMPORTANT]
>
>カスタムパーソナライゼーション接続を作成する前に、[同じページと次のページのパーソナライゼーション用にパーソナライゼーションの宛先を設定](../../ui/configure-personalization-destinations.md)する方法をお読みください。このガイドでは、複数の Experience Platform コンポーネントをまたいで、同じページおよび次のページのパーソナライゼーションの使用例に必要な設定手順を説明します。

## 書き出しタイプ および頻度 {#export-type-frequency}

**プロファイルリクエスト**：単一のプロファイルのカスタムパーソナライゼーションの宛先にマッピングされているすべてのセグメントを要求しています。様々なカスタムパーソナライゼーションの宛先を様々な [アドビのデータ収集データストリーム](../../../edge/fundamentals/datastreams.md)に対して設定できます。

## ユースケース {#use-cases}

この宛先は、オーディエンスを広告サーバーや Adobe 以外のパーソナライゼーションアプリケーションと共有し、リアルタイムで使用され、web サイトでユーザーに表示する広告を決定します。

### ユースケース 1

**ホームページのパーソナライズ**

あるホームレンタルや販売の web サイトが、Adobe Experience Platform でのセグメントの選定に基づいてホームページをパーソナライズをしたいと考えています。会社は、パーソナライズされたエクスペリエンスの対象となるオーディエンスを選択し、アドビ以外のパーソナライゼーションアプリケーション用にターゲット条件として設定した、カスタムパーソナライゼーションの宛先にこれらのオーディエンスをマッピングできます。

**ターゲットを設定したオンサイト広告**

広告サーバーに対して個別のカスタムパーソナライゼーションの宛先を使用し、同じ web サイトで、Adobe Experience Platform からターゲット条件として異なるセグメントのセットを使用して、オンサイト広告のターゲットを行うことができます。

## 宛先への接続 {#connect}

>[!CONTEXTUALHELP]
>id="platform_destinations_custom_personalization_datastream"
>title="データストリーム ID について"
>abstract="このオプションは、ページへの応答にセグメントを含めるデータ収集データストリームを決定します。ドロップダウンメニューには、宛先設定が有効になっているデータストリームのみが表示されます。宛先を設定する前に、データストリームを設定する必要があります。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/datastreams.html?lang=ja" text="データストリームの設定方法を説明します"

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。

### 接続パラメーター {#parameters}

この宛先を[設定](../../ui/connect-destination.md)するとき、次の情報を指定する必要があります。

* **[!UICONTROL 名前]**：この宛先に任意の名前を入力します。
* **[!UICONTROL 説明]**：宛先についての説明を入力します。例えば、この宛先を使用しているキャンペーンを指定できます。このフィールドはオプションです。
* **[!UICONTROL 統合エイリアス]**：この値は、JSON オブジェクト名として Experience Platform Web SDK に送信されます。
* **[!UICONTROL データストリーム ID]**：これにより、ページへの応答にセグメントを含めるデータ収集データストリームが決定されます。ドロップダウンメニューには、宛先設定が有効になっているデータストリームのみが表示されます。詳しくは、[データストリームの設定](../../../edge/fundamentals/datastreams.md)を参照してください。

## この宛先に対してセグメントをアクティブ化 {#activate}

この宛先にオーディエンスセグメントを有効化する手順については、[プロファイルリクエストの宛先へのプロファイルとセグメントの有効化](../../ui/activate-profile-request-destinations.md)を参照してください。

## 書き出したデータ {#exported-data}

[Adobe Experience Platform のタグ](../../../tags/home.md)を使用して Experience Platform Web SDK をデプロイする場合、[イベント完了の送信](../../../edge/extension/event-types.md)機能を使用すると、カスタムコードアクションには `event.destinations` 変数が追加され、書き出したデータを確認できます。

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

Adobe Experience Platform からの JSON 応答は、Adobe Experience Platform と統合しているアプリケーションの対応する統合エイリアスを見つけるために解析できます。セグメント ID は、ターゲティングパラメーターとしてアプリケーションのコードに渡すことができます。次に、これが宛先の応答に特有なサンプルを示します。

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
        var personalizationDestinations = result.destinations.filter(x => x.alias == “personalizationAlias”)
        if(personalizationDestinations.length > 0) {
             // Code to pass the segment IDs into the system that corresponds to personalizationAlias
        }
        var adServerDestinations = result.destinations.filter(x => x.alias == “adServerAlias”)
        if(adServerDestinations.length > 0) {
            // Code to pass the segment ids into the system that corresponds to adServerAlias
        }
     }
   })
  .catch(function(error) {
    // Tracking the event failed.
  });
```


## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのように データガバナンスを実施するかについて詳しくは、[データガバナンスの概要](../../../data-governance/home.md)を参照してください。
