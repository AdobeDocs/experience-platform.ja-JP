---
keywords: カスタムパーソナライゼーション；宛先；experience platform カスタムの宛先；
title: カスタムパーソナライゼーション接続（ベータ版）
description: この宛先は、Adobe Experience Platformからセグメント情報を取得する方法として、サイトで実行されている外部のパーソナライゼーション、コンテンツ管理システム、広告サーバーおよびその他のアプリケーションを提供します。 この宛先では、ユーザープロファイルのセグメントメンバーシップに基づいて、リアルタイムの 1 対 1 のパーソナライゼーションを提供します。
source-git-commit: 0635828cf3f637e67d2cabda860ca452e61892d4
workflow-type: tm+mt
source-wordcount: '578'
ht-degree: 8%

---

# カスタムパーソナライゼーション接続（ベータ版） {#custom-personalization-connection}

## 概要 {#overview}

>[!IMPORTANT]
>
>Adobe Experience Platformのカスタムパーソナライゼーション接続は、現在ベータ版です。 ドキュメントと機能は変更される場合があります。

この宛先を使用すると、Adobe Experience Platformから外部のパーソナライゼーションプラットフォーム、コンテンツ管理システム、広告サーバー、および顧客の Web サイトで実行されているその他のアプリケーションにセグメント情報を取得できます。

## 前提条件 {#prerequisites}

この統合は、[Adobe Experience Platform Web SDK](../../../edge/home.md) によって実行されます。 この宛先を使用するには、この SDK を使用する必要があります。

## 書き出しタイプ {#export-type}

**プロファイルリクエスト**  — 単一のプロファイルに対して、カスタムパーソナライゼーションの宛先にマッピングされるすべてのセグメントを要求します。異なる [Adobeデータ収集データストリーム ](../../../edge/fundamentals/datastreams.md) に対して、異なるカスタムパーソナライゼーションの宛先を設定できます。

## ユースケース {#use-cases}

この宛先は、Adobeサーバーおよび広告以外のパーソナライゼーションアプリケーションとオーディエンスを共有し、リアルタイムで使用して、Web サイトで広告ユーザーに表示する広告を決定します。

### ユースケース 1

**ホームページのパーソナライズ**

レンタルや販売の Web サイトでは、Adobe Experience Platformでのセグメント認定に基づいて自分のホームページをパーソナライズする必要があります。 会社は、パーソナライズされたエクスペリエンスを得る対象のAdobeを選択し、オーディエンス以外のパーソナライゼーションアプリケーション用に設定されたカスタムパーソナライゼーションの宛先にターゲット条件としてマッピングできます。

**ターゲットを絞ったオンサイト広告**

同じ Web サイトで、広告サーバーに対して別のカスタムパーソナライゼーションの宛先を使用し、Adobe Experience Platformとは異なるセグメントセットをターゲット条件として、オンサイト広告のターゲット設定をおこなうことができます。

## 宛先に接続 {#connect}

この宛先に接続するには、[ 宛先の設定に関するチュートリアル ](../../ui/connect-destination.md) で説明されている手順に従います。

### 接続パラメーター {#parameters}

[ この宛先を設定 ](../../ui/connect-destination.md) する際は、次の情報を指定する必要があります。

* **[!UICONTROL 名前]**：この宛先の名前を入力します。
* **[!UICONTROL 説明]**：宛先の説明を入力します。例えば、この宛先を使用しているキャンペーンを指定できます。このフィールドはオプションです。
* **[!UICONTROL 統合エイリアス]**:この値は、JSON オブジェクト名としてExperience PlatformWeb SDK に送信されます。
* **[!UICONTROL データストリーム ID]**:これにより、ページへの応答に含まれるデータ収集データストリームを決定します。ドロップダウンメニューには、宛先設定が有効になっているデータストリームのみが表示されます。 詳しくは、[ データストリームの設定 ](../../../edge/fundamentals/datastreams.md) を参照してください。

## この宛先へのセグメントのアクティブ化 {#activate}

この宛先に対するオーディエンスセグメントをアクティブ化する手順については、[ プロファイルリクエストの宛先へのプロファイルとセグメントのアクティブ化 ](../../ui/activate-profile-request-destinations.md) を参照してください。

## エクスポートされたデータ {#exported-data}

[Adobeタグ ](../../../tags/home.md) を使用してExperience PlatformWeb SDK をデプロイする場合は、[send event complete](../../../edge/extension/event-types.md) 機能を使用します。カスタムコードアクションには、書き出されたデータを確認するための `event.destinations` 変数が含まれます。

[Adobeタグ ](../../../tags/home.md) を使用してExperience PlatformWeb SDK をデプロイしない場合は、[ イベントからの応答の処理 ](../../../edge/fundamentals/tracking-events.md#handling-responses-from-events) 機能を使用して、書き出されたデータを確認します。

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
}).then(function(results) {
    if(results.destinations) { // Looking to see if the destination results are there
 
        // Get the destination with a particular alias
        var personalizationDestinations = results.destinations.filter(x => x.alias == “personalizationAlias”)
        if(personalizationDestinations.length > 0) {
             // Code to pass the segment IDs into the system that corresponds to personalizationAlias
        }
        var adServerDestinations = results.destinations.filter(x => x.alias == “adServerAlias”)
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

[!DNL Adobe Experience Platform] の宛先はすべて、データを処理する際のデータ使用ポリシーに準拠しています。 [!DNL Adobe Experience Platform] によるデータガバナンスの強制方法について詳しくは、「[ データガバナンスの概要 ](../../../data-governance/home.md)」を参照してください。
