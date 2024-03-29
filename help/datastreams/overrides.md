---
title: データストリームの上書きの設定
description: Web SDK を介して、データストリームの UI でデータストリームの上書きを設定し、データストリームの上書きをアクティベートする方法について説明します。
exl-id: 3f17a83a-dbea-467b-ac67-5462c07c884c
source-git-commit: 90493d179e620604337bda96cb3b7f5401ca4a81
workflow-type: tm+mt
source-wordcount: '1180'
ht-degree: 61%

---

# データストリームの上書きの設定

データストリームの上書きを使用すると、Web SDK を介して Edge Network に渡されるデータストリームの追加設定を定義できます。

これにより、デフォルトとは異なる様々なデータストリーム動作をトリガー化できます。データストリームを作成したり、既存の設定を変更したりする必要はありません。

データストリーム設定の上書きは、次の 2 つの手順で構成されます。

1. まず、 [datastream 設定ページ](configure.md).
2. 次に、次のいずれかの方法でオーバーライドを Edge ネットワークに送信する必要があります。
   * を通じて `sendEvent` または `configure` [Web SDK](#send-overrides-web-sdk) コマンド。
   * Web SDK を使用 [タグ拡張](../tags/extensions/client/web-sdk/web-sdk-extension-configuration.md).
   * Mobile SDK を使用 [sendEvent](#send-overrides-mobile-sdk) コマンドを使用します。

この記事では、サポートされているすべてのタイプの上書きに対するエンドツーエンドのデータストリーム設定の上書きプロセスについて説明します。

>[!IMPORTANT]
>
>データストリームの上書きは、次の場合にのみサポートされます。 [Web SDK](../web-sdk/home.md) および [モバイル SDK](https://developer.adobe.com/client-sdks/home/) 統合と呼ばれます。 [サーバー API](../server-api/overview.md) 統合は、現在、データストリームの上書きをサポートしていません。
><br>
>異なるデータストリームに異なるデータを送信する必要がある場合は、データストリームの上書きを使用する必要があります。パーソナライゼーションの使用例や同意データには、データストリームの上書きを使用しないでください。

## ユースケース {#use-cases}

データストリームの上書きを使用する方法および使用するタイミングをより深く理解するために、Adobe Experience Platform のお客様がこの機能を使用して解決できるユースケースを次に示します。

**複数地域のデータ収集**

会社は、事業を展開する国ごとに異なる web サイトまたはサブドメインを持っています。[設定済み](configure.md)の個別のデータストリームには、対応する分析別のレポートスイート、国別の Adobe Target プロパティトークン、国別のスキーマ、データセット、Journey Optimizer 設定などがあります。また、会社は、国別のすべてのデータを集計するグローバルな設定も用意されています。

データストリームの上書きを使用すると、会社は、1 つのデータストリームにデータを送信するデフォルトの動作ではなく、異なるデータストリームにデータのフローを動的に切り替えることができます。

一般的な使用例としては、データを国固有のデータストリームやグローバルデータストリームに送信し、顧客が注文やユーザープロファイルの更新など重要なアクションを実行する場合があります。

**様々なビジネスユニットに対するプロファイルと ID の区別**

複数のビジネスユニットを持つ会社は、複数のExperience Platformサンドボックスを使用して、各ビジネスユニットに固有のデータを保存したいと考えています。

会社は、デフォルトのデータストリームにデータを送信する代わりに、データストリームの上書きを使用して、各ビジネスユニットがデータを受け取るデータストリームを独自に持つようにすることができます。

## データストリーム UI でのデータストリームの上書きの設定 {#configure-overrides}

データストリーム設定の上書きを使用して、次のデータストリーム設定を変更できます。

* Experience Platform イベントデータセット
* Adobe Target プロパティトークン
* Audience Manager ID 同期コンテナ
* Adobe Analytics レポートスイート

### Adobe Target のデータストリームの上書き {#target-overrides}

Adobe Target データストリームのデータストリーム上書きを設定するには、まず Adobe Target のデータストリームを作成する必要があります。手順に従って、[Adobe Target](configure.md#target) サービスで[データストリームを設定](configure.md)します。

データストリームを作成したら、 [Adobe Target](configure.md#target) 追加して使用したサービス **[!UICONTROL プロパティトークンの上書き]** 」セクションを使用して、必要なデータストリームのオーバーライドを追加できます（下図を参照）。 1 行につき 1 つのプロパティトークンを追加します。

![プロパティトークンの上書きがハイライト表示された、Adobe Target サービス設定を示すデータストリーム UI のスクリーンショット。](assets/overrides/override-target.png)

必要な上書きを追加したら、データストリーム設定を保存します。

これで、Adobe Target のデータストリームの上書きが設定されました。これで、[Web SDK を介して、上書きを Edge Network に送信できるようになりました](#send-overrides)。

### Adobe Analytics のデータストリームの上書き {#analytics-overrides}

Adobe Analytics のデータストリームの上書きを設定するには、まず、[Adobe Analytics](configure.md#analytics) データストリームを作成する必要があります。手順に従って、[Adobe Analytics](configure.md#analytics) サービスで[データストリームを設定](configure.md)します。

データストリームを作成したら、 [Adobe Analytics](configure.md#target) 追加して使用したサービス **[!UICONTROL レポートスイートの上書き]** 」セクションを使用して、必要なデータストリームのオーバーライドを追加できます（下図を参照）。

「**[!UICONTROL バッチモードを表示]**」を選択して、レポートスイートの上書きのバッチ編集を有効にします。1 行に 1 つのレポートスイートを入力して、レポートスイートの上書きのリストをコピー＆ペーストできます。

![レポートスイートの上書きがハイライト表示された Adobe Analytics サービス設定を示すデータストリーム UI のスクリーンショット。](assets/overrides/override-analytics.png)

必要な上書きを追加したら、データストリーム設定を保存します。

これで、Adobe Analytics データストリームの上書きが設定されました。これで、[Web SDK を介して、上書きを Edge Network に送信できるようになりました](#send-overrides)。

### Experience Platform イベントデータセットのデータストリームの上書き {#event-dataset-overrides}

Experience Platform イベントデータセットのデータストリームの上書きを設定するには、まず [Adobe Experience Platform](configure.md#aep) データストリームを作成する必要があります。手順に従って、[Adobe Experience Platform](configure.md#aep) サービスで[データストリームを設定](configure.md)します。

データストリームを作成したら、 [Adobe Experience Platform](configure.md#aep) 追加したサービスを選択します。 **[!UICONTROL イベントデータセットを追加]** オプションを使用して、1 つ以上のオーバーライドイベントデータセットを追加できます（下図を参照）。

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

## Web SDK を介して、上書きを Edge Network に送信する {#send-overrides-web-sdk}

データ収集 UI でデータストリームの上書きを設定した後、Web SDK または Mobile SDK を使用して、上書きを Edge ネットワークに送信できます。

* **Web SDK**：詳しくは、 [datastream 設定の上書き](../web-sdk/commands/datastream-overrides.md#library) タグ拡張の手順と JavaScript ライブラリコードの例を参照してください。
* **モバイル SDK**：以下のを参照してください。

### モバイル SDK を介したデータストリーム ID の上書き {#id-override-mobile}

以下の例は、Mobile SDK 統合でのデータストリーム ID の上書きを示しています。 以下のタブを選択して、 [!DNL iOS] および [!DNL Android] 例。

>[!BEGINTABS]

>[!TAB iOS(Swift)]

この例は、Mobile SDK でのデータストリーム ID の上書きの例を示しています [!DNL iOS] 統合とも呼ばれます。

```swift
// Create Experience event from dictionary
var xdmData: [String: Any] = [
  "eventType": "SampleXDMEvent",
  "sample": "data",
]
let experienceEvent = ExperienceEvent(xdm: xdmData, datastreamIdOverride: "SampleDatastreamId")

Edge.sendEvent(experienceEvent: experienceEvent) { (handles: [EdgeEventHandle]) in
  // Handle the Edge Network response
}
```

>[!TAB Android™ (Kotlin)]

この例は、Mobile SDK でのデータストリーム ID の上書きの例を示しています [!DNL Android] 統合とも呼ばれます。

```kotlin
// Create experience event from Map
val xdmData = mutableMapOf < String, Any > ()
xdmData["eventType"] = "SampleXDMEvent"
xdmData["sample"] = "data"

val experienceEvent = ExperienceEvent.Builder()
    .setXdmSchema(xdmData)
    .setDatastreamIdOverride("SampleDatastreamId")
    .build()

Edge.sendEvent(experienceEvent) {
    // Handle the Edge Network response
}
```

>[!ENDTABS]

### Mobile SDK を介したデータストリーム設定の上書き {#config-override-mobile}

以下の例は、Mobile SDK 統合でのデータストリーム設定の上書きを示しています。 以下のタブを選択して、 [!DNL iOS] および [!DNL Android] 例。

>[!BEGINTABS]

>[!TAB iOS(Swift)]

この例は、Mobile SDK でのデータストリーム設定の上書きの例を示します [!DNL iOS] 統合とも呼ばれます。

```swift
// Create Experience event from dictionary
var xdmData: [String: Any] = [
  "eventType": "SampleXDMEvent",
  "sample": "data",
]

let configOverrides: [String: Any] = [
  "com_adobe_experience_platform": [
    "datasets": [
      "event": [
        "datasetId": "SampleEventDatasetIdOverride"
      ]
    ]
  ],
  "com_adobe_analytics": [
  "reportSuites": [
        "MyFirstOverrideReportSuite",
          "MySecondOverrideReportSuite",
          "MyThirdOverrideReportSuite"
      ]
  ],
  "com_adobe_identity": [
    "idSyncContainerId": "1234567"
  ],
  "com_adobe_target": [
    "propertyToken": "63a46bbc-26cb-7cc3-def0-9ae1b51b6c62"
 ],
]

let experienceEvent = ExperienceEvent(xdm: xdmData, datastreamConfigOverride: configOverrides)

Edge.sendEvent(experienceEvent: experienceEvent) { (handles: [EdgeEventHandle]) in
  // Handle the Edge Network response
}
```

>[!TAB Android (Kotlin)]

この例は、Mobile SDK でのデータストリーム設定の上書きの例を示します [!DNL Android] 統合とも呼ばれます。

```kotlin
// Create experience event from Map
val xdmData = mutableMapOf < String, Any > ()
xdmData["eventType"] = "SampleXDMEvent"
xdmData["sample"] = "data"

val configOverrides = mapOf(
    "com_adobe_experience_platform"
    to mapOf(
        "datasets"
        to mapOf(
            "event"
            to mapOf("datasetId"
                to "SampleEventDatasetIdOverride")
        )
    ),
    "com_adobe_analytics"
    to mapOf(
        "reportSuites"
        to listOf(
            "MyFirstOverrideReportSuite",
            "MySecondOverrideReportSuite",
            "MyThirdOverrideReportSuite"
        )
    ),
    "com_adobe_identity"
    to mapOf(
        "idSyncContainerId"
        to "1234567"
    ),
    "com_adobe_target"
    to mapOf(
        "propertyToken"
        to "63a46bbc-26cb-7cc3-def0-9ae1b51b6c62"
    )
)

val experienceEvent = ExperienceEvent.Builder()
    .setXdmSchema(xdmData)
    .setDatastreamConfigOverride(configOverrides)
    .build()

Edge.sendEvent(experienceEvent) {
    // Handle the Edge Network response
}
```

>[!ENDTABS]

## ペイロードの例 {#payload-example}

上記の例では、 [!DNL Edge Network] 以下のペイロードに似たペイロード。

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
