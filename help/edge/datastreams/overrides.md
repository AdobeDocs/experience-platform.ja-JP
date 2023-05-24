---
title: データストリームの設定の上書き
description: Web SDK を使用して、データストリームの上書きを設定し、データストリームの上書きをアクティブにする方法について説明します。
exl-id: 7829f411-acdc-49a1-a8fe-69834bcdb014
source-git-commit: d76d596818db67c99aca0606b6b6fb1a9aa977aa
workflow-type: tm+mt
source-wordcount: '948'
ht-degree: 4%

---

# データストリームの設定の上書き

データストリームのオーバーライドを使用すると、Web SDK を介して Edge ネットワークに渡されるデータストリームの追加設定を定義できます。

これにより、新しいデータストリームを作成したり、既存の設定を変更したりすることなく、デフォルトのデータストリームとは異なるトリガーの動作を設定できます。

データストリーム設定の上書きは、次の 2 つの手順で構成されます。

1. 最初に、[データストリーム設定ページ](configure.md)でデータストリーム設定の上書きを定義する必要があります。
2. 次に、Web SDK コマンドまたは Web SDK [タグ拡張機能](../extension/web-sdk-extension-configuration.md)を使用して、上書きを Edge Network に送信する必要があります。

この記事では、サポートされているすべてのタイプの上書きに対するエンドツーエンドのデータストリーム設定の上書きプロセスについて説明します。

## データストリーム UI でのデータストリームオーバーライドの設定 {#configure-overrides}

データストリーム設定の上書きを使用して、次のデータストリーム設定を変更できます。

* Experience Platformイベントデータセット
* Adobe Targetプロパティトークン
* Audience ManagerID 同期コンテナ
* Adobe Analyticsレポートスイート

### Adobe Targetのデータストリームの上書き {#target-overrides}

Adobe Targetのデータストリームの上書きを設定するには、まずAdobe Targetのデータストリームを作成する必要があります。 手順に従って、 [データストリームの設定](configure.md) と [Adobe Target](configure.md#target) サービス。

データストリームを作成したら、 [Adobe Target](configure.md#target) 追加して使用したサービス **[!UICONTROL プロパティトークンの上書き]** 」セクションを使用して、必要なデータストリームのオーバーライドを追加します（下図を参照）。 1 行につき 1 つのプロパティトークンを追加します。

![プロパティトークンのオーバーライドが示された、Adobe Targetサービス設定を示すデータストリーム UI のスクリーンショット。](../assets/datastreams/overrides/override-target.png)

必要なオーバーライドを追加したら、データストリーム設定を保存します。

これで、Adobe Targetのデータストリームオーバーライドを設定する必要があります。 次の操作を実行できます。 [Web SDK を使用して、上書きを Edge Network に送信します。](#send-overrides).

### Adobe Analyticsのデータストリームの上書き {#analytics-overrides}

Adobe Analyticsのデータストリームの上書きを設定するには、まず、 [Adobe Analytics](configure.md#analytics) データストリームが作成されました。 手順に従って、 [データストリームの設定](configure.md) と [Adobe Analytics](configure.md#analytics) サービス。

データストリームを作成したら、 [Adobe Analytics](configure.md#target) 追加して使用したサービス **[!UICONTROL レポートスイートの上書き]** 」セクションを使用して、必要なデータストリームのオーバーライドを追加します（下図を参照）。

選択 **[!UICONTROL バッチモードを表示]** ：レポートスイートの上書きのバッチ編集を有効にします。 1 行に 1 つのレポートスイートを入力して、レポートスイートのオーバーライドのリストをコピー&amp;ペーストできます。

![Adobe Analyticsサービス設定を示すデータストリーム UI のスクリーンショット。レポートスイートのオーバーライドがハイライト表示されています。](../assets/datastreams/overrides/override-analytics.png)

必要なオーバーライドを追加したら、データストリーム設定を保存します。

これで、Adobe Analyticsのデータストリームオーバーライドを設定する必要があります。 次の操作を実行できます。 [Web SDK を使用して、上書きを Edge Network に送信します。](#send-overrides).

### Experience Platformイベントデータセットのデータストリームの上書き {#event-dataset-overrides}

Experience Platformイベントデータセットのデータストリームの上書きを設定するには、まず [Adobe Experience Platform](configure.md#aep) データストリームが作成されました。 手順に従って、 [データストリームの設定](configure.md) と [Adobe Experience Platform](configure.md#aep) サービス。

データストリームを作成したら、 [Adobe Experience Platform](configure.md#aep) 追加したサービスを選択します。 **[!UICONTROL イベントデータセットを追加]** オプションを使用して、1 つ以上のオーバーライドイベントデータセットを追加できます（下図を参照）。

![Adobe Experience Platformサービス設定を示すデータストリーム UI のスクリーンショット。イベントデータセットのオーバーライドが強調表示されています。](../assets/datastreams/overrides/override-aep.png)

必要なオーバーライドを追加したら、データストリーム設定を保存します。

これで、Adobe Experience Platformのデータストリームオーバーライドを設定する必要があります。 次の操作を実行できます。 [Web SDK を使用して、上書きを Edge Network に送信します。](#send-overrides).

### サードパーティ ID 同期コンテナのデータストリームの上書き {#container-overrides}

サードパーティの ID 同期コンテナに対してデータストリームの上書きを設定するには、まずデータストリームを作成する必要があります。 手順に従って、 [データストリームの設定](configure.md) をクリックして、1 つを作成します。

データストリームを作成したら、に移動します。 **[!UICONTROL 詳細オプション]** また、 **[!UICONTROL サードパーティ ID の同期]** オプション。

次に、 **[!UICONTROL コンテナ ID の上書き]** セクションを使用して、デフォルト設定を上書きするコンテナ ID を追加します（下図を参照）。

>[!IMPORTANT]
>
>コンテナ ID は、 `1234567`、文字列ではなく `"1234567"`. Web SDK をコンテナ ID の上書きとして使用して文字列値を送信した場合、エラーが表示されます。

![サードパーティの ID 同期コンテナがハイライト表示された状態で、データストリーム設定を示すデータストリーム UI のスクリーンショット。](../assets/datastreams/overrides/override-container.png)

必要なオーバーライドを追加したら、データストリーム設定を保存します。

これで、ID 同期コンテナを上書き設定する必要があります。 次の操作を実行できます。 [Web SDK を使用して、上書きを Edge Network に送信します。](#send-overrides).

## Web SDK を使用して、上書きを Edge Network に送信します。 {#send-overrides}

>[!NOTE]
>
>Web SDK コマンドを使用して設定のオーバーライドを送信する代わりに、設定のオーバーライドを Web SDK に追加することもできます [タグ拡張](../extension/web-sdk-extension-configuration.md).

後 [データストリームの上書きの設定](#configure-overrides) データ収集 UI で、Web SDK を使用して、上書きを Edge ネットワークに送信できるようになりました。

Web SDK を使用して Edge ネットワークに上書きを送信することは、データストリーム設定の上書きをアクティブ化する 2 番目および最後の手順です。

データストリーム設定の上書きは、 `edgeConfigOverrides` Web SDK コマンド。 このコマンドは、 [!DNL Edge Network] 次のコマンド、または `configure` コマンドを呼び出します。

この `edgeConfigOverrides` コマンドは、に渡されるデータストリームの上書きを作成します。 [!DNL Edge Network] 次のコマンドで、または `configure`、リクエストごとに

設定の上書きが `configure` コマンドを使用する場合、次のサポートされるコマンドに含まれます。

* [sendEvent](../fundamentals/tracking-events.md)
* [setConsent](../consent/iab-tcf/overview.md)
* [getIdentity](../identity/overview.md)
* [appendIdentityToUrl](../identity/id-sharing.md#cross-domain-sharing)
* [設定](../fundamentals/configuring-the-sdk.md)

グローバルに指定したオプションは、個々のコマンドの configuration オプションで上書きできます。

### を介して設定の上書きを送信する `sendEvent` command {#send-event}

次の例は、設定の上書きが `sendEvent` コマンドを使用します。

```js {line-numbers="true" highlight="5-25"}
alloy("sendEvent", {
  xdm: {
    /* ... */
  },
  edgeConfigOverrides: {
    com_adobe_experience_platform: {
      datasets: {
        event: {
          datasetId: "MyOverrideDataset"
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

### を介して設定の上書きを送信する `configure` command {#send-configure}

次の例は、設定の上書きが `configure` コマンドを使用します。

```js {line-numbers="true" highlight="8-30"}
alloy("configure", {
  defaultConsent: "in",
  edgeDomain: "etc",
  edgeBasePath: "ee",
  edgeConfigId: "etc",
  orgId: "org",
  debugEnabled: true,
  edgeConfigOverrides: {
    "com_adobe_experience_platform": {
      "datasets": {
        "event": { 
          datasetId: "MyOverrideDataset"
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

### ペイロードの例 {#payload-example}

上記の例では、 [!DNL Edge Network] 次のようなペイロード：

```json
{
  "meta": {
    "configOverrides": {
      "com_adobe_experience_platform": {
        "datasets": {
          "event": {
            "datasetId": "MyOverrideDataset"
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
  "events": [  ],
  "query": {
    "identity": {
      "fetch": [
        "ECID"
      ]
    }
  }
}
```
