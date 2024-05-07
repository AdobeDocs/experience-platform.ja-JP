---
title: データストリームの上書きの設定
description: Web SDK を介して、データストリームの UI でデータストリームの上書きを設定し、データストリームの上書きをアクティベートする方法について説明します。
exl-id: 3f17a83a-dbea-467b-ac67-5462c07c884c
source-git-commit: b9b6320b15ee93807ebf8b48f31be7386a6f4a19
workflow-type: tm+mt
source-wordcount: '1067'
ht-degree: 68%

---

# データストリームの上書きの設定

データストリームの上書きを使用すると、Web SDK を介して Edge Network に渡されるデータストリームの追加設定を定義できます。

これにより、データストリームを作成したり、既存のトリガーを変更したりせずに、デフォルトとは異なるデータストリームの動作を設定できます。

データストリーム設定の上書きは、次の 2 つの手順で行います。

1. 最初に、データストリーム設定の上書きを [データストリーム設定ページ](configure.md).
2. 次に、上書きを次のいずれかの方法でEdge Networkに送信する必要があります。
   * 経由 `sendEvent` または `configure` [Web SDK](#send-overrides) コマンド。
   * Web SDK を使用 [タグ拡張機能](../tags/extensions/client/web-sdk/web-sdk-extension-configuration.md).
   * Mobile SDK を使用 [sendEvent](#send-overrides) API または次を使用 [ルール](#send-overrides).

この記事では、サポートされているすべてのタイプの上書きに対するエンドツーエンドのデータストリーム設定の上書きプロセスについて説明します。

>[!IMPORTANT]
>
>データストリームの上書きは、に対してのみサポートされます [Web SDK](../web-sdk/home.md) および [Mobile SDK](https://developer.adobe.com/client-sdks/home/) 統合。 [サーバー API](../server-api/overview.md) 統合では、現在、データストリームの上書きをサポートしていません。
><br>
>異なるデータストリームに異なるデータを送信する必要がある場合は、データストリームの上書きを使用する必要があります。パーソナライゼーションのユースケースや同意データに対してデータストリームの上書きを使用しない。

## ユースケース {#use-cases}

データストリームの上書きを使用する方法および使用するタイミングをより深く理解するために、Adobe Experience Platform のお客様がこの機能を使用して解決できるユースケースを次に示します。

**複数地域のデータ収集**

会社は、事業を展開する国ごとに異なる web サイトまたはサブドメインを持っています。[設定済み](configure.md)の個別のデータストリームには、対応する分析別のレポートスイート、国別の Adobe Target プロパティトークン、国別のスキーマ、データセット、Journey Optimizer 設定などがあります。また、会社は、国別のすべてのデータを集計するグローバルな設定も用意されています。

データストリームの上書きを使用すると、会社は、1 つのデータストリームにデータを送信するデフォルトの動作ではなく、異なるデータストリームにデータのフローを動的に切り替えることができます。

一般的なユースケースは、国固有のデータストリームや、顧客が注文やユーザープロファイルの更新などの重要なアクションを実行するグローバルデータストリームにデータを送信する場合もあります。

**様々なビジネスユニットに対するプロファイルと ID の区別**

複数のビジネスユニットを持つ会社が、複数のExperience Platformサンドボックスを使用して、各ビジネスユニットに固有のデータを保存したいと考えています。

会社は、デフォルトのデータストリームにデータを送信する代わりに、データストリームの上書きを使用して、各ビジネスユニットがデータを受け取るデータストリームを独自に持つようにすることができます。

## データストリーム UI でのデータストリームの上書きの設定 {#configure-overrides}

データストリーム設定の上書きを使用して、次のデータストリーム設定を変更できます。

* Experience Platform イベントデータセット
* Adobe Target プロパティトークン
* Audience Manager ID 同期コンテナ
* Adobe Analytics レポートスイート

### Adobe Target のデータストリームの上書き {#target-overrides}

Adobe Target データストリームのデータストリーム上書きを設定するには、まず Adobe Target のデータストリームを作成する必要があります。手順に従って、[Adobe Target](configure.md#target) サービスで[データストリームを設定](configure.md)します。

データストリームを作成したら、 [Adobe Target](configure.md#target) を追加して使用しているサービス **[!UICONTROL プロパティトークンの上書き]** セクションを追加して、以下の画像に示すように、目的のデータストリーム上書きを追加します。 1 行につき 1 つのプロパティトークンを追加します。

![プロパティトークンの上書きがハイライト表示された、Adobe Target サービス設定を示すデータストリーム UI のスクリーンショット。](assets/overrides/override-target.png)

必要な上書きを追加したら、データストリーム設定を保存します。

これで、Adobe Target のデータストリームの上書きが設定されました。これで、[Web SDK を介して、上書きを Edge Network に送信できるようになりました](#send-overrides)。

### Adobe Analytics のデータストリームの上書き {#analytics-overrides}

Adobe Analytics のデータストリームの上書きを設定するには、まず、[Adobe Analytics](configure.md#analytics) データストリームを作成する必要があります。手順に従って、[Adobe Analytics](configure.md#analytics) サービスで[データストリームを設定](configure.md)します。

データストリームを作成したら、 [Adobe Analytics](configure.md#target) を追加して使用しているサービス **[!UICONTROL レポートスイートの上書き]** セクションを追加して、以下の画像に示すように、目的のデータストリーム上書きを追加します。

「**[!UICONTROL バッチモードを表示]**」を選択して、レポートスイートの上書きのバッチ編集を有効にします。1 行に 1 つのレポートスイートを入力して、レポートスイートの上書きのリストをコピー＆ペーストできます。

![レポートスイートの上書きがハイライト表示された Adobe Analytics サービス設定を示すデータストリーム UI のスクリーンショット。](assets/overrides/override-analytics.png)

必要な上書きを追加したら、データストリーム設定を保存します。

これで、Adobe Analytics データストリームの上書きが設定されました。これで、[Web SDK を介して、上書きを Edge Network に送信できるようになりました](#send-overrides)。

### Experience Platform イベントデータセットのデータストリームの上書き {#event-dataset-overrides}

Experience Platform イベントデータセットのデータストリームの上書きを設定するには、まず [Adobe Experience Platform](configure.md#aep) データストリームを作成する必要があります。手順に従って、[Adobe Experience Platform](configure.md#aep) サービスで[データストリームを設定](configure.md)します。

データストリームを作成したら、 [Adobe Experience Platform](configure.md#aep) 追加したサービスと **[!UICONTROL イベントデータセットを追加]** 次の画像に示すように、1 つ以上のオーバーライドイベントデータセットを追加するオプション。

![イベントデータセットの上書きがハイライト表示された Adobe Experience Platform サービス設定を示すデータストリーム UI のスクリーンショット。](assets/overrides/override-aep.png)

必要な上書きを追加したら、データストリーム設定を保存します。

これで、Adobe Experience Platform データストリームの上書きが設定されました。これで、[Web SDK を介して、上書きを Edge Network に送信できるようになりました](#send-overrides)。

### サードパーティ ID 同期コンテナのデータストリームの上書き {#container-overrides}

サードパーティの ID 同期コンテナに対してデータストリームの上書きを設定するには、まずデータストリームを作成する必要があります。手順に従って、[データストリームを設定](configure.md)して作成します。

データストリームを作成したら、**[!UICONTROL 詳細オプション]**&#x200B;に移動し、「**[!UICONTROL サードパーティ ID の同期]**」オプションを有効にします。

次に、以下の画像に示すように、「**[!UICONTROL コンテナ ID の上書き]**」セクションを使用して、デフォルト設定を上書きするコンテナ ID を追加します。

>[!IMPORTANT]
>
>コンテナ ID は、`"1234567"` などの文字列ではなく、`1234567` のような数値である必要があります。Web SDK を介してコンテナ ID の上書きとして文字列値を送信した場合、エラーが表示されます。

![サードパーティの ID 同期コンテナがハイライト表示されたデータストリーム設定を示すデータストリーム UI のスクリーンショット。](assets/overrides/override-container.png)

必要な上書きを追加したら、データストリーム設定を保存します。

これで、ID 同期コンテナの上書きが設定されました。これで、[Web SDK を介して、上書きを Edge Network に送信](#send-overrides)できるようになりました。

## Web SDK を介して、上書きを Edge Network に送信する {#send-overrides}

データ収集 UI でデータストリームの上書きを設定した後、Web SDK または Mobile SDK を通じてEdge Networkに上書きを送信できます。

* **Web SDK**：を参照 [データストリーム設定の上書き](../web-sdk/commands/datastream-overrides.md#library) タグ拡張手順および JavaScript ライブラリコードの例については、を参照してください。
* **Mobile SDK**：データストリーム ID の上書きは、次のいずれかを使用して送信できます [sendEvent API](https://developer.adobe.com/client-sdks/edge/edge-network/tutorials/send-overrides-sendevent/) または次を使用 [ルール](https://developer.adobe.com/client-sdks/edge/edge-network/tutorials/send-overrides-rules/).

## ペイロードの例 {#payload-example}

上記の例では、が生成されます。 [!DNL Edge Network] 以下に類似したペイロード。

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
