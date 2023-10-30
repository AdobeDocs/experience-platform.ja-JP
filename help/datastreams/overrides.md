---
title: データストリームの上書きの設定
description: Web SDK を介して、データストリームの UI でデータストリームの上書きを設定し、データストリームの上書きをアクティベートする方法について説明します。
exl-id: 3f17a83a-dbea-467b-ac67-5462c07c884c
source-git-commit: 5effb8a514100c28ef138ba1fc443cf29a64319a
workflow-type: tm+mt
source-wordcount: '1464'
ht-degree: 78%

---

# データストリームの上書きの設定

データストリームの上書きを使用すると、Web SDK を介して Edge Network に渡されるデータストリームの追加設定を定義できます。

これにより、新しいデータストリームを作成したり、既存の設定を変更したりすることなく、デフォルトとは異なるデータストリームの動作をトリガーできます。

データストリーム設定の上書きは、次の 2 つの手順で構成されます。

1. 最初に、[データストリーム設定ページ](configure.md)でデータストリーム設定の上書きを定義する必要があります。
2. 次に、次のいずれかの方法でオーバーライドを Edge ネットワークに送信する必要があります。
   * を通じて `sendEvent` または `configure` [Web SDK](#send-overrides-web-sdk) コマンド。
   * Web SDK を使用 [タグ拡張](../tags/extensions/client/web-sdk/web-sdk-extension-configuration.md).
   * Mobile SDK を使用 [sendEvent API](#send-overrides-mobile-sdk) を呼び出します。

この記事では、サポートされているすべてのタイプの上書きに対するエンドツーエンドのデータストリーム設定の上書きプロセスについて説明します。

>[!IMPORTANT]
>
>データストリームの上書きは、次の場合にのみサポートされます。 [Web SDK](../edge/home.md) および [モバイル SDK](https://developer.adobe.com/client-sdks/documentation/) 統合と呼ばれます。 [サーバー API](../server-api/overview.md) 統合は、現在、データストリームの上書きをサポートしていません。
><br>
>異なるデータストリームに異なるデータを送信する必要がある場合は、データストリームの上書きを使用する必要があります。パーソナライゼーションのユースケースまたは同意データには、データストリームの上書きを使用しないでください。

## ユースケース {#use-cases}

データストリームの上書きを使用する方法および使用するタイミングをより深く理解するために、Adobe Experience Platform のお客様がこの機能を使用して解決できるユースケースを次に示します。

**複数地域のデータ収集**

会社は、事業を展開する国ごとに異なる web サイトまたはサブドメインを持っています。[設定済み](configure.md)の個別のデータストリームには、対応する分析別のレポートスイート、国別の Adobe Target プロパティトークン、国別のスキーマ、データセット、Journey Optimizer 設定などがあります。また、会社は、国別のすべてのデータを集計するグローバルな設定も用意されています。

データストリームの上書きを使用すると、会社は、1 つのデータストリームにデータを送信するデフォルトの動作ではなく、異なるデータストリームにデータのフローを動的に切り替えることができます。

一般的なユースケースとしては、国別のデータストリームにデータを送信することや顧客が注文やユーザープロファイルの更新などの重要なアクションを実行する場合にグローバルデータストリームにデータを送信することがあります。

**様々なビジネスユニットに対するプロファイルと ID の区別**

複数のビジネスユニットを持つ会社は、複数の Experience Platform サンドボックスを使用して、各ビジネスユニットに固有のデータを保存したいと考えています。

会社は、デフォルトのデータストリームにデータを送信する代わりに、データストリームの上書きを使用して、各ビジネスユニットがデータを受け取るデータストリームを独自に持つようにすることができます。

## データストリーム UI でのデータストリームの上書きの設定 {#configure-overrides}

データストリーム設定の上書きを使用して、次のデータストリーム設定を変更できます。

* Experience Platform イベントデータセット
* Adobe Target プロパティトークン
* Audience Manager ID 同期コンテナ
* Adobe Analytics レポートスイート

### Adobe Target のデータストリームの上書き {#target-overrides}

Adobe Target データストリームのデータストリーム上書きを設定するには、まず Adobe Target のデータストリームを作成する必要があります。手順に従って、[Adobe Target](configure.md#target) サービスで[データストリームを設定](configure.md)します。

データストリームを作成したら、以下の画像に示すように、追加した [Adobe Target](configure.md#target) サービスを編集し、「**[!UICONTROL プロパティトークンの上書き]**」セクションを使用して、必要なデータストリームの上書きを追加します。1 行につき 1 つのプロパティトークンを追加します。

![プロパティトークンの上書きがハイライト表示された、Adobe Target サービス設定を示すデータストリーム UI のスクリーンショット。](assets/overrides/override-target.png)

必要な上書きを追加したら、データストリーム設定を保存します。

これで、Adobe Target のデータストリームの上書きが設定されました。これで、[Web SDK を介して、上書きを Edge Network に送信できるようになりました](#send-overrides)。

### Adobe Analytics のデータストリームの上書き {#analytics-overrides}

Adobe Analytics のデータストリームの上書きを設定するには、まず、[Adobe Analytics](configure.md#analytics) データストリームを作成する必要があります。手順に従って、[Adobe Analytics](configure.md#analytics) サービスで[データストリームを設定](configure.md)します。

データストリームを作成したら、以下の画像に示すように、追加した [Adobe Analytics](configure.md#target) サービスを編集して、「**[!UICONTROL レポートスイートの上書き]**」セクションを使用して、必要なデータストリームの上書きを追加します。

「**[!UICONTROL バッチモードを表示]**」を選択して、レポートスイートの上書きのバッチ編集を有効にします。1 行に 1 つのレポートスイートを入力して、レポートスイートの上書きのリストをコピー＆ペーストできます。

![レポートスイートの上書きがハイライト表示された Adobe Analytics サービス設定を示すデータストリーム UI のスクリーンショット。](assets/overrides/override-analytics.png)

必要な上書きを追加したら、データストリーム設定を保存します。

これで、Adobe Analytics データストリームの上書きが設定されました。これで、[Web SDK を介して、上書きを Edge Network に送信できるようになりました](#send-overrides)。

### Experience Platform イベントデータセットのデータストリームの上書き {#event-dataset-overrides}

Experience Platform イベントデータセットのデータストリームの上書きを設定するには、まず [Adobe Experience Platform](configure.md#aep) データストリームを作成する必要があります。手順に従って、[Adobe Experience Platform](configure.md#aep) サービスで[データストリームを設定](configure.md)します。

データストリームを作成したら、以下の画像に示すように、追加した [Adobe Experience Platform](configure.md#aep) サービスを編集して、「**[!UICONTROL イベントデータセットを追加]**」オプションを選択し、1 つ以上の上書きイベントデータセットを追加します。

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

>[!NOTE]
>
>Web SDK コマンドを介して設定の上書きを送信する代わりに、設定の上書きを Web SDK [タグ拡張](../tags/extensions/client/web-sdk/web-sdk-extension-configuration.md)に追加できます。

データ収集 UI で[データストリームの上書きを設定](#configure-overrides)した後、Web SDK を介して、上書きを Edge Network に送信できるようになりました。

Web SDK を使用している場合は、 `edgeConfigOverrides` コマンドは、データストリーム設定の上書きをアクティブ化する 2 番目および最後の手順です。

データストリーム設定の上書きは、`edgeConfigOverrides` Web SDK コマンドを介して Edge Network に送信されます。このコマンドは、次のコマンドまたは `configure` コマンドの場合は、リクエストごとに [!DNL Edge Network] に渡されるデータストリームの上書きを作成します。

`edgeConfigOverrides` コマンドは、次のコマンドまたは `configure`の場合、リクエストごとに [!DNL Edge Network] に渡されるデータストリームの上書きを作成します。

設定の上書きが `configure` コマンドで送信される場合、次の Web SDK コマンドに含まれます。

* [sendEvent](../edge/fundamentals/tracking-events.md)
* [setConsent](../edge/consent/iab-tcf/overview.md)
* [getIdentity](../edge/identity/overview.md)
* [appendIdentityToUrl](../edge/identity/id-sharing.md#cross-domain-sharing)
* [configure](../edge/fundamentals/configuring-the-sdk.md)

グローバルに指定したオプションは、個々のコマンドの設定オプションで上書きできます。

### Web SDK を使用した設定の上書きの送信 `sendEvent` command {#send-event}

以下の例は、`sendEvent` コマンド上でどのように設定の上書きが表示されるかを示します。

```js {line-numbers="true" highlight="5-25"}
alloy("sendEvent", {
  xdm: {
    /* ... */
  },
  edgeConfigOverrides: {
    datastreamId: "{DATASTREAM_ID}"
    com_adobe_experience_platform: {
      datasets: {
        event: {
          datasetId: "SampleEventDatasetIdOverride"
        },
        profile: {
          datasetId: "www"
        }
      }
    },
    com_adobe_analytics: {
      reportSuites: [
        "MyFirstOverrideReportSuite",
        "MySecondOverrideReportSuite",
        "MyThirdOverrideReportSuite"
        ]
    },
    com_adobe_identity: {
      idSyncContainerId: "1234567"
    },
    com_adobe_target: {
      propertyToken: "63a46bbc-26cb-7cc3-def0-9ae1b51b6c62"
    }
  }
});
```

| パラメーター | 説明 |
|---|---|
| `edgeConfigOverrides.datastreamId` | このパラメーターを使用して、`configure` コマンドで定義されたデータストリームとは異なるデータストリームに単一のリクエストを送ることができます。 |

### `configure` コマンドを介した設定の上書きの送信 {#send-configure}

以下の例は、`configure` コマンド上でどのように設定の上書きが表示されるかを示します。

```js {line-numbers="true" highlight="8-30"}
alloy("configure", {
  defaultConsent: "in",
  edgeDomain: "etc",
  edgeBasePath: "ee",
  datastreamId: "{DATASTREAM_ID}",
  orgId: "org",
  debugEnabled: true,
  edgeConfigOverrides: {
    "com_adobe_experience_platform": {
      "datasets": {
        "event": { 
          datasetId: "SampleProfileDatasetIdOverride"
        },
        "profile": { 
          datasetId: "www"
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
  onBeforeEventSend: function() { /* … */ });
};
```

## Mobile SDK を使用して Edge ネットワークに上書きを送信する {#send-overrides-mobile-sdk}

後 [データストリームの上書きの設定](#configure-overrides) データ収集 UI で、Mobile SDK を使用して Edge ネットワークに上書きを送信できるようになりました。

Mobile SDK を使用している場合は、 `sendEvent` API は、データストリーム設定の上書きをアクティブ化する 2 番目および最後の手順です。

Mobile SDK について詳しくは、Experience Platformモバイル SDK に関する [Mobile SDK ドキュメント](https://developer.adobe.com/client-sdks/edge/edge-network/).

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

>[!TAB Android (Kotlin)]

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
      ],
      "profile": [
        "datasetId": "SampleProfileDatasetIdOverride"
      ],
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
                to "SampleEventDatasetIdOverride"),
            "profile"
            to mapOf("datasetId"
                to "SampleProfileDatasetIdOverride")
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
          },
          "profile": {
            "datasetId": "www"
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
