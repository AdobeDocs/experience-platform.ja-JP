---
keywords: カスタムパーソナライゼーション；宛先；experience platform カスタムの宛先；
title: カスタムパーソナライゼーション接続
description: この宛先は、Adobe Experience Platformからセグメント情報を取得する方法として、サイトで実行されている外部のパーソナライゼーション、コンテンツ管理システム、広告サーバーおよびその他のアプリケーションを提供します。 この宛先は、ユーザープロファイルセグメントのメンバーシップに基づいて、リアルタイムのパーソナライゼーションを提供します。
exl-id: 2382cc6d-095f-4389-8076-b890b0b900e3
source-git-commit: ee9ed1c17a566f37b4ad79df7c66f8b2ffb4b879
workflow-type: tm+mt
source-wordcount: '681'
ht-degree: 6%

---

# カスタムパーソナライゼーション接続 {#custom-personalization-connection}

## 概要 {#overview}

この宛先を使用すると、Adobe Experience Platformから外部のパーソナライゼーションプラットフォーム、コンテンツ管理システム、広告サーバー、および顧客の Web サイトで実行しているその他のアプリケーションにセグメント情報を取得できます。

## 前提条件 {#prerequisites}

この統合は、 [Adobe Experience Platform Web SDK](../../../edge/home.md) または [Adobe Experience Platform Mobile SDK](https://aep-sdks.gitbook.io/docs/). この宛先を使用するには、次の SDK のいずれかを使用する必要があります。

## 書き出しタイプ {#export-type}

**プロファイルリクエスト** ：単一のプロファイルのカスタムパーソナライゼーションの宛先にマッピングされているすべてのセグメントを要求しています。 様々なカスタムパーソナライゼーションの宛先を様々な [Adobeデータ収集データストリーム](../../../edge/fundamentals/datastreams.md).

## ユースケース {#use-cases}

この宛先は、オーディエンスをAdobeサーバーおよび非広告パーソナライゼーションアプリケーションと共有し、リアルタイムで使用して、Web サイトに表示する広告ユーザーを決定します。

### ユースケース 1

**ホームページのパーソナライズ**

レンタルや販売の Web サイトで、Adobe Experience Platformでのセグメント認定に基づいて自分のホームページをパーソナライズしたいと考えています。 会社は、パーソナライズされたエクスペリエンスを取得するオーディエンスを選択し、Adobe以外のパーソナライゼーションアプリケーション用にターゲット条件として設定されたカスタムパーソナライゼーションの宛先にマッピングできます。

**ターゲットを設定したオンサイト広告**

同じ Web サイトで、広告サーバーに対して個別のカスタムパーソナライゼーションの宛先を使用し、Adobe Experience Platformとは異なるセグメントセットをターゲット条件として、オンサイト広告のターゲット設定をおこなうことができます。

## 宛先に接続 {#connect}

>[!IMPORTANT]
>
>カスタムパーソナライゼーション接続を作成する前に、次の手順に関するガイドを読むことをお勧めします。 [同じページと次のページのパーソナライゼーション用にパーソナライゼーションの宛先を設定](../../ui/configure-personalization-destinations.md). このガイドでは、複数のパーソナライゼーションコンポーネントにわたる、同じページおよび次のページのパーソナライゼーションの使用例に必要な設定手順をExperience Platformします。

>[!CONTEXTUALHELP]
>id="platform_destinations_custom_personalization_datastream"
>title="データストリーム ID について"
>abstract="このオプションは、ページへの応答にセグメントを含めるデータ収集データストリームを決定します。 ドロップダウンメニューには、宛先設定が有効になっているデータストリームのみが表示されます。 宛先を設定する前に、データストリームを設定する必要があります。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/datastreams.html?lang=en" text="データストリームの設定方法を説明します。"

この宛先に接続するには、 [宛先設定のチュートリアル](../../ui/connect-destination.md).

### 接続パラメーター {#parameters}

While [設定](../../ui/connect-destination.md) この宛先には、次の情報を指定する必要があります。

* **[!UICONTROL 名前]**：この宛先の名前を入力します。
* **[!UICONTROL 説明]**：宛先の説明を入力します。例えば、この宛先を使用しているキャンペーンを指定できます。このフィールドはオプションです。
* **[!UICONTROL 統合エイリアス]**:この値は、JSON オブジェクト名としてExperience PlatformWeb SDK に送信されます。
* **[!UICONTROL データストリーム ID]**:これにより、ページへの応答にセグメントを含めるデータ収集データストリームが決定されます。 ドロップダウンメニューには、宛先設定が有効になっているデータストリームのみが表示されます。 詳しくは、 [データストリームの設定](../../../edge/fundamentals/datastreams.md) を参照してください。

## この宛先へのセグメントのアクティブ化 {#activate}

読み取り [プロファイルリクエストの宛先へのプロファイルとセグメントのアクティブ化](../../ui/activate-profile-request-destinations.md) を参照してください。

## 書き出されたデータ {#exported-data}

次を使用する場合： [Adobe Experience Platformのタグ](../../../tags/home.md) Experience PlatformWeb SDK をデプロイするには、 [イベント完了の送信](../../../edge/extension/event-types.md) 機能とカスタムコードアクションには `event.destinations` 変数を使用して、エクスポートされたデータを確認できます。

次に、 `event.destinations` 変数：

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

を使用しない場合、 [タグ](../../../tags/home.md) Experience PlatformWeb SDK をデプロイするには、 [イベントからの応答の処理](../../../edge/fundamentals/tracking-events.md#handling-responses-from-events) 機能を使用して、書き出されたデータを確認できます。

Adobe Experience Platformからの JSON 応答は、Adobe Experience Platformと統合しているアプリケーションの対応する統合エイリアスを見つけるために解析できます。 セグメント ID は、ターゲティングパラメーターとしてアプリケーションのコードに渡すことができます。 次に、これが宛先の応答に特有に見えるサンプルを示します。

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

すべて [!DNL Adobe Experience Platform] の宛先は、データを処理する際のデータ使用ポリシーに準拠しています。 詳しくは、 [!DNL Adobe Experience Platform] データガバナンスを強制し、 [データガバナンスの概要](../../../data-governance/home.md).
