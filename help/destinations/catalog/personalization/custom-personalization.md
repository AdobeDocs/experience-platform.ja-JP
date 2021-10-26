---
keywords: カスタムパーソナライゼーション；宛先；experience platform カスタムの宛先；
title: カスタムパーソナライゼーション接続（ベータ版）
description: この宛先は、Adobe Experience Platformからセグメント情報を取得する方法として、サイトで実行されている外部のパーソナライゼーション、コンテンツ管理システム、広告サーバーおよびその他のアプリケーションを提供します。 この宛先では、ユーザープロファイルのセグメントメンバーシップに基づいて、リアルタイムの 1 対 1 のパーソナライゼーションを提供します。
exl-id: 2382cc6d-095f-4389-8076-b890b0b900e3
source-git-commit: 50ab34cb9147cf880e199afad88e718875fb591f
workflow-type: tm+mt
source-wordcount: '586'
ht-degree: 8%

---

# カスタムパーソナライゼーション接続（ベータ版） {#custom-personalization-connection}

## 概要 {#overview}

>[!IMPORTANT]
>
>Adobe Experience Platformのカスタムパーソナライゼーション接続は、現在ベータ版です。 ドキュメントと機能は変更される場合があります。

この宛先を使用すると、Adobe Experience Platformから外部のパーソナライゼーションプラットフォーム、コンテンツ管理システム、広告サーバー、および顧客の Web サイトで実行されているその他のアプリケーションにセグメント情報を取得できます。

## 前提条件 {#prerequisites}

この統合は、 [Adobe Experience Platform Web SDK](../../../edge/home.md). この宛先を使用するには、この SDK を使用する必要があります。

## 書き出しタイプ {#export-type}

**プロファイルリクエスト** ：単一のプロファイルに対して、カスタムパーソナライゼーションの宛先にマッピングされるすべてのセグメントを要求します。 様々なカスタムパーソナライゼーションの宛先を様々な [Adobeデータ収集データストリーム](../../../edge/fundamentals/datastreams.md).

## ユースケース {#use-cases}

この宛先は、Adobeサーバーおよび広告以外のパーソナライゼーションアプリケーションとオーディエンスを共有し、リアルタイムで使用して、Web サイトで広告ユーザーに表示する広告を決定します。

### ユースケース 1

**ホームページのパーソナライズ**

レンタルや販売の Web サイトでは、Adobe Experience Platformでのセグメント認定に基づいて自分のホームページをパーソナライズする必要があります。 会社は、パーソナライズされたエクスペリエンスを得る対象のAdobeを選択し、オーディエンス以外のパーソナライゼーションアプリケーション用に設定されたカスタムパーソナライゼーションの宛先にターゲット条件としてマッピングできます。

**ターゲットを絞ったオンサイト広告**

同じ Web サイトで、広告サーバーに対して別のカスタムパーソナライゼーションの宛先を使用し、Adobe Experience Platformとは異なるセグメントセットをターゲット条件として、オンサイト広告のターゲット設定をおこなうことができます。

## 宛先に接続 {#connect}

この宛先に接続するには、 [宛先の設定チュートリアル](../../ui/connect-destination.md).

### 接続パラメーター {#parameters}

While [設定](../../ui/connect-destination.md) この宛先には、次の情報を指定する必要があります。

* **[!UICONTROL 名前]**：この宛先の名前を入力します。
* **[!UICONTROL 説明]**：宛先の説明を入力します。例えば、この宛先を使用しているキャンペーンを指定できます。このフィールドはオプションです。
* **[!UICONTROL 統合エイリアス]**:この値は、JSON オブジェクト名としてExperience PlatformWeb SDK に送信されます。
* **[!UICONTROL Datastream ID]**:これにより、ページへの応答に含まれるデータ収集データストリームを決定します。 ドロップダウンメニューには、宛先設定が有効になっているデータストリームのみが表示されます。 詳しくは、 [データストリームの設定](../../../edge/fundamentals/datastreams.md) を参照してください。

## この宛先へのセグメントのアクティブ化 {#activate}

読み取り [プロファイルリクエストの宛先へのプロファイルとセグメントのアクティブ化](../../ui/activate-profile-request-destinations.md) を参照してください。

## エクスポートされたデータ {#exported-data}

を [Adobeタグ](../../../tags/home.md) Experience PlatformWeb SDK をデプロイするには、 [イベント完了の送信](../../../edge/extension/event-types.md) 機能とカスタムコードアクションには、 `event.destinations` 変数を使用して、書き出されたデータを確認できます。

次に、 `event.destinations` 変数：

```
[
   {
      "type":"profileLookup",
      "destinationId":"7bb4cb8d-8c2e-4450-871d-b7824f547111",
      "alias":"personalizationAlias",
      "segments":[
         {
            "id":"399eb3e7-3d50-47d3-ad30-a5ad99e8ab77",
            "mergePolicyId":"69638c01-2099-4032-8b41-84bee8ebcfa4"
         },
         {
            "id":"499eb3e7-3d50-47d3-ad30-a5ad99e8ab77",
            "mergePolicyId":"69638c01-2099-4032-8b41-84bee8ebcfa4"
         }
      ]
   }
]
```

を [Adobeタグ](../../../tags/home.md) Experience PlatformWeb SDK をデプロイするには、 [イベントからの応答の処理](../../../edge/fundamentals/tracking-events.md#handling-responses-from-events) 機能を使用して、書き出されたデータを確認できます。

Adobe Experience Platformからの JSON 応答を解析して、Adobe Experience Platformと統合しているアプリケーションの対応する統合エイリアスを見つけることができます。 セグメント ID は、ターゲティングパラメーターとしてアプリケーションのコードに渡すことができます。 次に、これが宛先の応答に特有に見えるサンプルを示します。

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
