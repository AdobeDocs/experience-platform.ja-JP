---
keywords: カスタムパーソナライゼーション;宛先;Experience Platform カスタムの宛先;
title: カスタムパーソナライゼーション接続
description: この宛先は、Adobe Experience Platform からセグメント情報を取得する方法として、サイトで実行されている外部のパーソナライゼーション、コンテンツ管理システム、広告サーバーおよびその他のアプリケーションを提供します。この宛先は、ユーザープロファイルセグメントのメンバーシップに基づいて、リアルタイムのパーソナライゼーションを提供します。
exl-id: 2382cc6d-095f-4389-8076-b890b0b900e3
source-git-commit: 09e81093c2ed2703468693160939b3b6f62bc5b6
workflow-type: tm+mt
source-wordcount: '1305'
ht-degree: 59%

---

# カスタムパーソナライゼーション接続 {#custom-personalization-connection}

## 宛先の変更ログ {#changelog}

拡張された **[!UICONTROL カスタムパーソナライゼーション]** 宛先コネクタ、2 つが表示されている可能性があります **[!UICONTROL カスタムパーソナライゼーション]** カードを含める必要があります。

この **[!UICONTROL 属性を含むカスタムパーソナライゼーション]** コネクタは現在ベータ版で、限られた数のお客様のみご利用いただけます。 **[!UICONTROL カスタムパーソナライゼーション]**&#x200B;が提供する機能に加えて、 **[!UICONTROL 属性を含むカスタムパーソナライゼーション]**&#x200B;コネクタにより、オプションの[マッピング手順](/help/destinations/ui/activate-profile-request-destinations.md#map-attributes)をアクティベーションワークフローに追加します。これにより、プロファイル属性をカスタムパーソナライゼーションの宛先にマッピングし、属性ベースの同じページおよび次のページのパーソナライゼーションを有効にできます。

>[!IMPORTANT]
>
> プロファイル属性には、機密データが含まれている場合があります。 このデータを保護するために、**[!UICONTROL 属性を含むカスタムパーソナライゼーション]**&#x200B;の宛先では、データ収集に [Edge Network Server API](/help/server-api/overview.md) を使用する必要があります。さらに、すべての Server API 呼び出しは、[認証済みコンテキスト](../../../server-api/authentication.md)で行う必要があります。
>
>既に統合に Web SDK または Mobile SDK を使用している場合は、Server API を使用して属性を取得する方法は 2 つあります。
>
> * Server API を使用して属性を取得するサーバー側の統合を追加します。
> * カスタム JavaScript コードを使用してクライアント側の設定を更新し、Server API を介して属性を取得します。
>
> 上記の要件に従わない場合、パーソナライゼーションはセグメントメンバーシップのみに基づき、 **[!UICONTROL カスタムパーソナライゼーション]** コネクタ。

![2 つのカスタムパーソナライゼーションの宛先カードを並べて表示した画像。](../../assets/catalog/personalization/custom-personalization/custom-personalization-side-by-side-view.png)

## 概要 {#overview}

この宛先は、Adobe Experience Platform から外部のパーソナライゼーションプラットフォーム、コンテンツ管理システム、広告サーバーおよび顧客の web サイト上で実行されているその他のアプリケーションにセグメント情報を取得できます。

## 前提条件 {#prerequisites}

この統合は、[Adobe Experience Platform Web SDK](../../../edge/home.md) または [Adobe Experience Platform Mobile SDK](https://aep-sdks.gitbook.io/docs/) によって実現されます。この宛先を使用するには、次の SDK のいずれかを使用する必要があります。

>[!IMPORTANT]
>
>カスタムパーソナライゼーション接続を作成する前に、[同じページと次のページのパーソナライゼーション用にパーソナライゼーションの宛先を設定](../../ui/configure-personalization-destinations.md)する方法をお読みください。このガイドでは、複数の Experience Platform コンポーネントをまたいで、同じページおよび次のページのパーソナライゼーションの使用例に必要な設定手順を説明します。

## 書き出しタイプ および頻度 {#export-type-frequency}

**プロファイルリクエスト**：単一のプロファイルのカスタムパーソナライゼーションの宛先にマッピングされているすべてのセグメントを要求しています。様々なカスタムパーソナライゼーションの宛先を様々な [アドビのデータ収集データストリーム](../../../edge/datastreams/overview.md)に対して設定できます。

## ユースケース {#use-cases}

この [!DNL Custom Personalization Connection] では、独自のパーソナライゼーションパートナープラットフォーム ( [!DNL Optimizely], [!DNL Pega]) に加えて、独自のシステム（社内 CMS など）も、Experience Platformエッジネットワークのデータ収集およびセグメント化機能を活用して、より深い顧客パーソナライゼーションエクスペリエンスを強化します。

以下に説明する使用例には、サイトのパーソナライゼーションとターゲット化されたオンサイト広告の両方が含まれます。

これらのユースケースを有効にするには、Experience Platformからセグメント情報をすばやく効率的に取得し、Experience PlatformUI でカスタムパーソナライゼーション接続として設定した指定システムにこの情報を送信する方法が必要です。

これらのシステムは、外部のパーソナライゼーションプラットフォーム、コンテンツ管理システム、広告サーバー、および顧客の Web およびモバイルプロパティで実行されるその他のアプリケーションにすることができます。

### 同じページのパーソナライズ機能 {#same-page}

ユーザーが Web サイトのページを訪問します。 顧客は、現在のページ訪問情報（参照 URL、ブラウザーの言語、埋め込まれた製品情報など）を使用して、Adobe以外のプラットフォーム（例えば、パーソナライゼーション）のカスタムパーソナライゼーション接続を使用して、次のアクション/決定（例：パーソナライズ）を選択できます [!DNL Pega], [!DNL Optimizely]など )

### 次のページのパーソナライゼーション {#next-page}

あるユーザーが Web サイトのページ A を訪問します。 このインタラクションに基づいて、ユーザーは一連のセグメントの対象として認定されます。 次に、ページ A からページ B に移動するリンクをクリックします。ページ A での前のインタラクション中にユーザーが認定したセグメントと、現在の Web サイト訪問によって決定されたプロファイルの更新は、次のアクション/決定（例えば、訪問者に表示する広告バナー、A/B テストの場合は表示するページ）に使用されます。

### 次のセッションのパーソナライズ機能 {#next-session}

ユーザーが Web サイトの複数のページを訪問します。 これらのインタラクションに基づいて、ユーザーは一連のセグメントの対象として認定されます。 次に、ユーザは現在の閲覧セッションを終了する。

次の日に、ユーザーは同じ顧客 Web サイトに戻ります。 以前のインタラクションで、現在の Web サイトの訪問によって決定されたプロファイルの更新と共に、訪問者に表示する広告バナーや、表示するページのバージョン A/B テストの場合は次のアクション/決定を選択します。

## 宛先への接続 {#connect}

>[!CONTEXTUALHELP]
>id="platform_destinations_custom_personalization_datastream"
>title="データストリーム ID について"
>abstract="このオプションは、ページへの応答にセグメントを含めるデータ収集データストリームを決定します。ドロップダウンメニューには、宛先設定が有効になっているデータストリームのみが表示されます。宛先を設定する前に、データストリームを設定する必要があります。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/datastreams.html?lang=ja" text="データストリームの設定方法の説明"

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。

### 接続パラメーター {#parameters}

この宛先を[設定](../../ui/connect-destination.md)するとき、次の情報を指定する必要があります。

* **[!UICONTROL 名前]**：この宛先に希望する名前を入力します。
* **[!UICONTROL 説明]**：宛先についての説明を入力します。例えば、この宛先を使用しているキャンペーンを指定できます。このフィールドはオプションです。
* **[!UICONTROL 統合エイリアス]**：この値は、JSON オブジェクト名として Experience Platform Web SDK に送信されます。
* **[!UICONTROL データストリーム ID]**：これにより、ページへの応答にセグメントを含めるデータ収集データストリームが決定されます。ドロップダウンメニューには、宛先設定が有効になっているデータストリームのみが表示されます。詳しくは、[データストリームの設定](../../../edge/datastreams/overview.md)を参照してください。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してセグメントをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>データをアクティブ化するには、**[!UICONTROL 宛先の管理]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]**&#x200B;および&#x200B;**[!UICONTROL セグメントの表示]**[に対するアクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

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
        var personalizationDestinations = result.destinations.filter(x => x.alias == "personalizationAlias")
        if(personalizationDestinations.length > 0) {
             // Code to pass the segment IDs into the system that corresponds to personalizationAlias
        }
        var adServerDestinations = result.destinations.filter(x => x.alias == "adServerAlias")
        if(adServerDestinations.length > 0) {
            // Code to pass the segment ids into the system that corresponds to adServerAlias
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
