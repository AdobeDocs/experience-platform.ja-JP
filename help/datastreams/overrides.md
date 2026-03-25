---
title: データストリームの上書きの設定
description: データストリーム UIでデータストリームオーバーライドを設定し、Web SDKまたはモバイルSDKを使用してデータストリームオーバーライドをアクティブ化する方法について説明します。
exl-id: 3f17a83a-dbea-467b-ac67-5462c07c884c
source-git-commit: bdcea238740661b453032bbab3ec7e414efd63e3
workflow-type: tm+mt
source-wordcount: '1043'
ht-degree: 53%

---

# データストリームの上書きの設定

データストリームの上書きを使用して、データストリームの追加の設定を定義します。この設定は、Web SDKまたはモバイルSDKを介してEdge Networkに渡されます。

これにより、データストリームを作成したり、既存のトリガーを変更したりすることなく、デフォルトのデータストリームとは異なるデータストリーム動作を設定できます。

データストリーム設定の上書きは、次の2つの手順で行います。

1. まず、[ データストリーム設定ページ ](configure.md)でデータストリーム設定の上書きを定義する必要があります。
2. 次に、次のいずれかの方法でEdge Networkにオーバーライドを送信する必要があります。
   * `sendEvent`または`configure` [Web SDK](#send-overrides) コマンドを使用します。
   * Web SDK [ タグ拡張機能](../tags/extensions/client/web-sdk/configure/configuration-overrides.md)を使用します。
   * モバイル SDK [sendEvent](#send-overrides) APIを使用するか、[ ルール ](#send-overrides)を使用します。

この記事では、サポートされているすべてのタイプの上書きに対するエンドツーエンドのデータストリーム設定の上書きプロセスについて説明します。

>[!IMPORTANT]
>
>[Edge Network API](https://developer.adobe.com/data-collection-apis/docs/api/)統合では、現在、データストリームの上書きはサポートされていません。
><br>
>異なるデータストリームに異なるデータを送信する必要がある場合は、データストリームの上書きを使用する必要があります。パーソナライゼーションのユースケースや同意データにデータストリームの上書きを使用しないでください。

## ユースケース {#use-cases}

データストリームの上書きを使用する方法および使用するタイミングをより深く理解するために、Adobe Experience Platform のお客様がこの機能を使用して解決できるユースケースを次に示します。

**複数地域のデータ収集**

会社は、事業を展開する国ごとに異なる web サイトまたはサブドメインを持っています。[設定済み](configure.md)の個別のデータストリームには、対応する分析別のレポートスイート、国別の Adobe Target プロパティトークン、国別のスキーマ、データセット、Journey Optimizer 設定などがあります。また、会社は、国別のすべてのデータを集計するグローバルな設定も用意されています。

データストリームの上書きを使用すると、会社は、1 つのデータストリームにデータを送信するデフォルトの動作ではなく、異なるデータストリームにデータのフローを動的に切り替えることができます。

一般的なユースケースとしては、国別のデータストリームや、顧客が注文やユーザープロファイルの更新などの重要なアクションを実行するグローバルデータストリームにデータを送信することが考えられます。

**様々なビジネスユニットに対するプロファイルと ID の区別**

複数の事業部門を持つ企業は、複数のExperience Platform サンドボックスを使用して、各事業部門に固有のデータを保存したいと考えています。

会社は、デフォルトのデータストリームにデータを送信する代わりに、データストリームの上書きを使用して、各ビジネスユニットがデータを受け取るデータストリームを独自に持つようにすることができます。

## データストリーム UI でのデータストリームの上書きの設定 {#configure-overrides}

データストリーム設定の上書きにより、次のデータストリーム設定を変更できます。

* Experience Platform イベントデータセット
* Adobe Target プロパティトークン
* Audience Manager ID 同期コンテナ
* Adobe Analytics レポートスイート

### Adobe Target のデータストリームの上書き {#target-overrides}

Adobe Target データストリームのデータストリーム上書きを設定するには、まず Adobe Target のデータストリームを作成する必要があります。手順に従って、[Adobe Target](configure.md#target) サービスで[データストリームを設定](configure.md)します。

データストリームを作成したら、追加した[Adobe Target](configure.md#target) サービスを編集し、**[!UICONTROL Property Token Overrides]** セクションを使用して、次の図に示すように、目的のデータストリームオーバーライドを追加します。 1 行につき 1 つのプロパティトークンを追加します。

![プロパティトークンの上書きがハイライト表示された、Adobe Target サービス設定を示すデータストリーム UI のスクリーンショット。](assets/overrides/override-target.png)

必要な上書きを追加したら、データストリーム設定を保存します。

これで、Adobe Target のデータストリームの上書きが設定されました。これで、[Web SDKまたはモバイル SDK](#send-overrides)を介してEdge Networkに上書きを送信できます。

### Adobe Analytics のデータストリームの上書き {#analytics-overrides}

Adobe Analytics のデータストリームの上書きを設定するには、まず、[Adobe Analytics](configure.md#analytics) データストリームを作成する必要があります。手順に従って、[Adobe Analytics](configure.md#analytics) サービスで[データストリームを設定](configure.md)します。

データストリームを作成したら、追加した[Adobe Analytics](configure.md#analytics) サービスを編集し、**[!UICONTROL Report Suite Overrides]** セクションを使用して、次の図に示すように、目的のデータストリームオーバーライドを追加します。

レポートスイートの上書きのバッチ編集を有効にするには、**[!UICONTROL Show Batch Mode]**&#x200B;を選択します。 1 行に 1 つのレポートスイートを入力して、レポートスイートの上書きのリストをコピー＆ペーストできます。

![レポートスイートの上書きがハイライト表示された Adobe Analytics サービス設定を示すデータストリーム UI のスクリーンショット。](assets/overrides/override-analytics.png)

必要な上書きを追加したら、データストリーム設定を保存します。

これで、Adobe Analytics データストリームの上書きが設定されました。これで、[Web SDKまたはモバイル SDK](#send-overrides)を介してEdge Networkに上書きを送信できます。

### Experience Platform イベントデータセットのデータストリームの上書き {#event-dataset-overrides}

Experience Platform イベントデータセットのデータストリームの上書きを設定するには、まず [Adobe Experience Platform](configure.md#aep) データストリームを作成する必要があります。手順に従って、[Adobe Experience Platform](configure.md#aep) サービスで[データストリームを設定](configure.md)します。

データストリームを作成したら、追加した[Adobe Experience Platform](configure.md#aep) サービスを編集し、**[!UICONTROL Add Event Dataset]** オプションを選択して、次の図に示すように、1つ以上のオーバーライドイベントデータセットを追加します。

![イベントデータセットの上書きがハイライト表示された Adobe Experience Platform サービス設定を示すデータストリーム UI のスクリーンショット。](assets/overrides/override-aep.png)

必要な上書きを追加したら、データストリーム設定を保存します。

これで、Adobe Experience Platform データストリームの上書きが設定されました。これで、[Web SDKまたはモバイル SDK](#send-overrides)を介してEdge Networkに上書きを送信できます。

### サードパーティ ID 同期コンテナのデータストリームの上書き {#container-overrides}

サードパーティの ID 同期コンテナに対してデータストリームの上書きを設定するには、まずデータストリームを作成する必要があります。手順に従って、[データストリームを設定](configure.md)して作成します。

データストリームを作成したら、**[!UICONTROL Advanced Options]**&#x200B;に移動し、**[!UICONTROL Third Party ID Sync]** オプションを有効にします。

次に、**[!UICONTROL Container ID Overrides]** セクションを使用して、次の画像に示すように、デフォルト設定を上書きするコンテナ IDを追加します。

>[!IMPORTANT]
>
>コンテナ ID は、`"1234567"` などの文字列ではなく、`1234567` のような数値である必要があります。Web SDK を介してコンテナ ID の上書きとして文字列値を送信した場合、エラーが表示されます。

![サードパーティの ID 同期コンテナがハイライト表示されたデータストリーム設定を示すデータストリーム UI のスクリーンショット。](assets/overrides/override-container.png)

必要な上書きを追加したら、データストリーム設定を保存します。

これで、ID 同期コンテナの上書きが設定されました。これで、[Web SDKまたはモバイル SDK](#send-overrides)を介してEdge Networkに上書きを送信できます。

## Edge Networkにオーバーライドを送信する {#send-overrides}

データ収集UIでデータストリームのオーバーライドを設定した後、Web SDKまたはモバイルSDKを介してEdge Networkにオーバーライドを送信できます。

* **Web SDK**: JavaScript ライブラリのコード例については、[ データストリーム設定の上書き](/help/collection/js/commands/configure/edgeconfigoverrides.md)を参照してください。
* **モバイル SDK**: [sendEvent API](https://developer.adobe.com/client-sdks/edge/edge-network/tutorials/send-overrides-sendevent/)を使用するか、[ ルール ](https://developer.adobe.com/client-sdks/edge/edge-network/tutorials/send-overrides-rules/)を使用して、データストリーム IDの上書きを送信できます。

## ペイロードの例 {#payload-example}

上記の例では、次の例と同様に[!DNL Edge Network] ペイロードを生成します。

```json
{
  "meta": {
    "configOverrides": {
      "com_adobe_experience_platform": {
        "datasets": {
          "event": {
            "datasetId": "SampleProfileDatasetIdOverride"
          }
        }
      },
      "com_adobe_analytics": {
        "reportSuites": [
        "MyFirstOverrideReportSuite",
        "MySecondOverrideReportSuite",
        "MyThirdOverrideReportSuite"
        ]
      },
      "com_adobe_identity": {
        "idSyncContainerId": "1234567"
      },
      "com_adobe_target": {
        "propertyToken": "63a46bbc-26cb-7cc3-def0-9ae1b51b6c62"
      }
    },
    "state": {  }
  },
  "events": [  ]
}
```
