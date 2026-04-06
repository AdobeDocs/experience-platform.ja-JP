---
keywords: カスタムパーソナライゼーション;宛先;Experience Platform カスタムの宛先;
title: カスタムパーソナライゼーション接続
description: この宛先は、Adobe Experience Platformからオーディエンス情報を取得する方法として、サイト上で実行されている外部パーソナライゼーション、コンテンツ管理システム、およびサーバー、およびその他のアプリケーションを提供します。 この宛先は、ユーザープロファイルのオーディエンスメンバーシップに基づいて、リアルタイムのパーソナライゼーションを提供します。
exl-id: 2382cc6d-095f-4389-8076-b890b0b900e3
source-git-commit: 58f69a78fb3c622c8741d7a1618f15509c160a5b
workflow-type: tm+mt
source-wordcount: '1111'
ht-degree: 38%

---


# カスタムパーソナライゼーション接続 {#custom-personalization-connection}

## 宛先の変更ログ {#changelog}

| リリース月 | 更新タイプ | 説明 |
|---|---|---|
| 2023年5月 | 機能とドキュメントの更新 | 2023年5月の時点で、**[!UICONTROL Custom personalization]**&#x200B;接続は[属性ベースのパーソナライゼーション &#x200B;](../../ui/activate-edge-personalization-destinations.md#map-attributes)をサポートしており、すべての顧客が一般に利用できます。 |

{style="table-layout:auto"}

>[!IMPORTANT]
>
>プロファイル属性には、機密データが含まれる場合があります。 このデータを保護するには、属性ベースのパーソナライゼーション用に[宛先を設定する際に、](https://developer.adobe.com/data-collection-apis/docs/)Edge Network API **[!UICONTROL Custom Personalization]**&#x200B;を使用する必要があります。 すべてのEdge Network API呼び出しは、[認証済みコンテキスト &#x200B;](https://developer.adobe.com/data-collection-apis/docs/getting-started/authentication)で行う必要があります。
>
><br>WebまたはMobile SDKの実装に既に使用しているのと同じデータストリームを使用するサーバーサイド統合を追加することで、[Edge Network API](https://developer.adobe.com/data-collection-apis/docs/)を介してプロファイル属性を取得できます。
>
><br>上記の要件に従わない場合、パーソナライゼーションはオーディエンスメンバーシップのみに基づいて行われます。

## 概要 {#overview}

この宛先を設定して、外部パーソナライゼーションプラットフォーム、コンテンツ管理システム、および顧客web サイトで実行されているその他のアプリケーションが[!DNL Adobe Experience Platform]からオーディエンス情報を取得できるようにします。

## 前提条件 {#prerequisites}

この宛先では、実装に応じて、次のいずれかのデータ収集方法を使用する必要があります。

* Web サイトからデータを収集する場合は、[Adobe Experience Platform Web SDK](/help/collection/js/js-overview.md)を使用します。
* モバイルアプリケーションからデータを収集する場合は、[Adobe Experience Platform モバイルSDK](https://developer.adobe.com/client-sdks/documentation/)を使用します。
* Web SDKまたはモバイル SDKを使用していない場合、またはプロファイル属性に基づいてユーザーエクスペリエンスをパーソナライズする場合は、[Edge Network API](https://developer.adobe.com/data-collection-apis/docs/)を使用します。

>[!IMPORTANT]
>
>**属性ベースのパーソナライゼーション要件：** プロファイル属性（オーディエンスメンバーシップだけでなく）に基づいてパーソナライズする場合、**は、Web SDKまたはMobile SDKをデータ収集に使用しているかどうかに関係なく、認証済みのサーバーサイド統合で** Edge Network API[を](https://developer.adobe.com/data-collection-apis/docs/)使用する必要があります。
>
>Web SDKとモバイルSDKだけでも、オーディエンスメンバーシップにもとづくパーソナライゼーションのみがサポートされます。 Edge Network APIは、パーソナライゼーション用のプロファイル属性を安全に取得するために&#x200B;**必須**&#x200B;です。

>[!IMPORTANT]
>
>カスタムパーソナライゼーション接続を作成する前に、「[&#x200B; オーディエンスデータをエッジパーソナライゼーション宛先にアクティベートする方法](../../ui/activate-edge-personalization-destinations.md)」に関するガイドをお読みください。 このガイドでは、複数の Experience Platform コンポーネントをまたいで、同じページおよび次のページのパーソナライゼーションの使用例に必要な設定手順を説明します。

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスの由来 | サポートあり | 説明 |
|---------|----------|----------|
| [!DNL Segmentation Service] | ○ | Experience Platform [&#x200B; セグメント化サービス &#x200B;](../../../segmentation/home.md)を通じて生成されたオーディエンス。 |
| その他すべてのオーディエンスの生成元 | ○ | このカテゴリには、[!DNL Segmentation Service]を通じて生成されたオーディエンス以外のすべてのオーディエンスのオリジンが含まれます。 [様々なオーディエンスの起源](/help/segmentation/ui/audience-portal.md#customize)について読みます。 次に例を示します。 <ul><li> カスタムアップロードオーディエンス [がCSV ファイルからExperience Platformに](../../../segmentation/ui/audience-portal.md#import-audience)をインポートしました。</li><li> 類似オーディエンス， </li><li> 連合オーディエンス， </li><li> [!DNL Adobe Journey Optimizer]などの他のExperience Platform アプリで生成されたオーディエンス </li><li> その他。 </li></ul> |

{style="table-layout:auto"}



オーディエンスのデータタイプ別にサポートされるオーディエンス：

| オーディエンスのデータタイプ | サポートあり | 説明 | ユースケース |
|--------------------|-----------|-------------|-----------|
| [人物オーディエンス &#x200B;](/help/segmentation/types/people-audiences.md) | ○ | 顧客プロファイルにもとづいて、マーケティング施策の特定のグループをターゲットにすることができます。 | 買い物客やカートの放棄が多い |
| [&#x200B; アカウントオーディエンス &#x200B;](/help/segmentation/types/account-audiences.md) | × | アカウントベースドマーケティング戦略のために、特定の組織内の個人をターゲットにします。 | B2B マーケティング |
| [見込みオーディエンス &#x200B;](/help/segmentation/types/prospect-audiences.md) | × | まだ顧客ではないが、ターゲットオーディエンスと特徴を共有する個人をターゲットにします。 | サードパーティデータによる見込み顧客の開拓 |
| [&#x200B; データセットの書き出し](/help/catalog/datasets/overview.md) | × | [!DNL Adobe Experience Platform] データ レイクに保存されている構造化データのコレクション。 | レポート，データサイエンスワークフロー |

{style="table-layout:auto"}


## 書き出しのタイプと頻度 {#export-type-frequency}

| 項目 | タイプ | メモ |
|---------|----------|---------|
| 書き出しタイプ | **[!DNL Profile request]** | 1つのプロファイルのカスタムパーソナライゼーション宛先にマッピングされているすべてのオーディエンスを要求しています。 様々なカスタムパーソナライゼーションの宛先を様々な [アドビのデータ収集データストリーム](../../../datastreams/overview.md)に対して設定できます。 |
| 書き出し頻度 | **[!UICONTROL Streaming]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。詳しくは、[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)を参照してください。 |

## 宛先への接続 {#connect}

>[!CONTEXTUALHELP]
>id="platform_destinations_custom_personalization_datastream"
>title="データストリームについて"
>abstract="このオプションは、ページへの応答にオーディエンスを含めるデータ収集データストリームを決定します。ドロップダウンメニューには、宛先設定が有効になっているデータストリームのみが表示されます。宛先を設定する前に、データストリームを設定する必要があります。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/datastreams/configure.html?lang=ja" text="データストリームの設定方法を学ぶ"

>[!IMPORTANT]
>
>宛先に接続するには、**[!UICONTROL View Destinations]**&#x200B;および&#x200B;**[!UICONTROL Manage Destinations]** [&#x200B; アクセス制御権限](/help/access-control/home.md#permissions)が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。

### 接続パラメーター {#parameters}

この宛先を[設定](../../ui/connect-destination.md)するとき、次の情報を指定する必要があります。

* **[!UICONTROL Name]**：この宛先の優先名を入力します。
* **[!UICONTROL Description]**：宛先の説明を入力します。 例えば、この宛先を使用しているキャンペーンを指定できます。このフィールドはオプションです。
* **[!UICONTROL Integration alias]**：この値は、JSON オブジェクト名としてExperience Platform Web SDKに送信されます。
* **[!UICONTROL Datastream]**：これにより、ページへの応答にオーディエンスを含めるデータ収集データストリームが決まります。 ドロップダウンメニューには、宛先設定が有効になっているデータストリームのみが表示されます。詳しくは、[データストリームの設定](../../../datastreams/overview.md)を参照してください。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の提供が完了したら、**[!UICONTROL Next]**&#x200B;を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
>
>データをアクティブ化するには、**[!UICONTROL View Destinations]**、**[!UICONTROL Activate Destinations]**、**[!UICONTROL View Profiles]**&#x200B;および&#x200B;**[!UICONTROL View Segments]** [&#x200B; アクセス制御権限](/help/access-control/home.md#permissions)が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

この宛先に対してオーディエンスをアクティブ化する手順については、[&#x200B; プロファイルとオーディエンスをアクティブ化するエッジパーソナライゼーションの宛先](../../ui/activate-edge-personalization-destinations.md)を参照してください。

## 書き出したデータ {#exported-data}

[Adobe Experience Platform のタグ](/help/tags/home.md)を使用して Experience Platform Web SDK をデプロイする場合、[イベント完了の送信](/help/tags/extensions/client/web-sdk/event-types.md)機能を使用すると、カスタムコードアクションには `event.destinations` 変数が追加され、書き出したデータを確認できます。

`event.destinations` 変数のサンプル値は次のようになります。

```json
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

[&#x200B; タグ &#x200B;](/help/tags/home.md)を使用してExperience Platform Web SDKをデプロイしていない場合は、[&#x200B; コマンド応答](/help/collection/js/commands/command-responses.md)を使用して、書き出されたデータを確認します。

[!DNL Adobe Experience Platform]からのJSON応答を解析して、[!DNL Adobe Experience Platform]と統合しているアプリケーションの対応する統合エイリアスを見つけることができます。 オーディエンス IDは、ターゲティングパラメーターとしてアプリケーションのコードに渡すことができます。 次に、これが宛先の応答に特有なサンプルを示します。

```js
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

### [!UICONTROL Custom Personalization With Attributes]の応答例 {#example-response-custom-personalization-with-attributes}

**[!UICONTROL Custom Personalization With Attributes]**&#x200B;を使用する場合、API応答は次の例のようになります。

**[!UICONTROL Custom Personalization With Attributes]**&#x200B;と&#x200B;**[!UICONTROL Custom Personalization]**&#x200B;の違いは、API応答に`attributes` セクションが含まれていることです。

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
