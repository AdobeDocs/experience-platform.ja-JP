---
title: データストリーム設定の上書き
description: Web SDK を使用してデータストリームの上書きを設定する方法を説明します。
exl-id: 8e327892-9520-43f5-abf4-d65a5ca34e6d
source-git-commit: 8be502c9eea67119dc537a5d63a6c71e0bff1697
workflow-type: tm+mt
source-wordcount: '882'
ht-degree: 14%

---

# データストリームの上書きの設定

`edgeConfigOverrides` オブジェクトを使用すると、現在のページで実行されるコマンドの設定を上書きできます。 この上書きオブジェクトはコマンドではなく、ほとんどの Web SDK コマンドに含めることができるオブジェクトです。

このオブジェクトは、国ごとに異なる web サイトやサブドメインがある場合や、異なる事業部門固有のデータを保存する複数のExperience Platformサンドボックスがある場合に役立ちます。

>[!IMPORTANT]
>
>データストリームの上書きに関するエンドツーエンドの設定手順について詳しくは、[ データストリーム設定の上書き ](../../datastreams/overrides.md#configure-overrides) のドキュメントを参照してください。

データストリーム設定の上書きは、次の 2 つの手順で行います。

1. 最初に、データストリーム UI 内の [ データストリーム設定ページ ](../../datastreams/configure.md) でデータストリーム設定の上書きを定義する必要があります。 上書きの設定方法については、[ データストリーム設定の上書き ](../../datastreams/overrides.md#configure-overrides) のドキュメントを参照してください。
2. UI でデータストリームの上書きを設定したら、次のいずれかの方法で上書きをEdge Networkに送信する必要があります。
   * Web SDK 経由 [ タグ拡張機能 ](#tag-extension)。
   * `sendEvent` または `configure` Web SDK コマンドを使用する。
   * Mobile SDK `sendEvent` コマンドを使用します。

Web SDK 設定と特定のコマンド（[`sendEvent`](sendevent/overview.md) など）の両方でオーバーライドを設定した場合、特定のコマンドのオーバーライドが優先されます。

## オブジェクトのプロパティ

このオブジェクト内では次のプロパティを使用できます。

* **データストリームの上書き**：別のデータストリームへの呼び出しを送信します。 この値を設定した場合、データストリーム設定を必要とする他の上書きは、ここで設定したデータストリームで設定する必要があります。
* **サードパーティ ID 同期コンテナ**:Adobe Audience Managerの宛先サードパーティ ID 同期コンテナの ID。 このフィールドを使用する前に、データストリームの設定でサードパーティ ID コンテナの上書きを設定する必要があります。
* **Target プロパティトークン**:Adobe Targetの destination プロパティのトークン。 このフィールドを使用する前に、データストリームの設定で Target プロパティトークンの上書きを設定する必要があります。
* **レポートスイート**:Adobe Analyticsで上書きするレポートスイート ID。 このフィールドを使用する前に、データストリームの設定でレポートスイートの上書きを設定する必要があります。

## Web SDK タグ拡張機能を使用して、データストリームの上書きをEdge Networkに送信します {#tag-extension}

設定手順について詳しくは、Web SDK タグ拡張機能からの [ データストリームの上書きの設定 ](../../tags/extensions/client/web-sdk/web-sdk-extension-configuration.md#datastrea-overrides) に関するドキュメントを参照してください。

Web SDK タグ拡張機能からデータストリームの上書きを設定する場合は、[ タグ拡張機能の設定 ](/help/tags/extensions/client/web-sdk/web-sdk-extension-configuration.md) 時に、**[!UICONTROL データストリーム設定の上書き]** の下で目的の各フィールドを設定します。

1. Adobe IDの資格情報を使用して [experience.adobe.com](https://experience.adobe.com) にログインします。
1. **[!UICONTROL データ収集]**/**[!UICONTROL タグ]** に移動します。
1. 目的のタグプロパティを選択します。
1. **[!UICONTROL 拡張機能]** に移動し、[!UICONTROL Adobe Experience Platform Web SDK **[!UICONTROL カードの]** 設定 ] をクリックします。
1. **[!UICONTROL データストリーム設定の上書き]** セクションまでスクロールします。 必要な各上書き値を設定します。
1. 「**[!UICONTROL 保存]**」をクリックして、変更を公開します。

特定のコマンドに対してのみ上書きを設定する場合は、タグルールのアクション内の目的の各フィールドを設定します。

1. Adobe IDの資格情報を使用して [experience.adobe.com](https://experience.adobe.com) にログインします。
1. **[!UICONTROL データ収集]**/**[!UICONTROL タグ]** に移動します。
1. 目的のタグプロパティを選択します。
1. **[!UICONTROL ルール]** に移動し、目的のルールを選択します。
1. [!UICONTROL  アクション ] で、既存のアクションを選択するか、アクションを作成します。
1. 「[!UICONTROL  拡張機能 ]」ドロップダウンフィールドを **[!UICONTROL Adobe Experience Platform Web SDK]** に設定し、「[!UICONTROL  アクションタイプ ] を **[!UICONTROL イベントを送信]** に設定します。
1. **[!UICONTROL データストリーム設定の上書き]** というラベルの付いたセクションまでスクロールします。
1. このセクションの各フィールドを目的のオーバーライド値に設定します。
1. 「**[!UICONTROL 変更を保持]**」をクリックして、公開ワークフローを実行します。

[!UICONTROL  開発 ] 環境、[!UICONTROL  ステージング ] 環境および [!UICONTROL  実稼動 ] 環境には、別々のフィールドが用意されています。 各環境で必要な各フィールドに、必ず入力してください。

## Web SDK JavaScript ライブラリを介して上書きをEdge Networkに送信します {#library}

データ収集 UI で [ データストリームの上書きの設定 ](../../datastreams/overrides.md) を行った後、Web SDK JavaScript ライブラリを介して、上書きをEdge Networkーに送信できるようになりました。

Web SDK を使用している場合、`edgeConfigOverrides` コマンドを使用して上書きをEdge Networkに送信することは、データストリーム設定の上書きをアクティブ化する 2 番目および最後の手順です。

データストリーム設定の上書きは、`edgeConfigOverrides` Web SDK コマンドを介して Edge Network に送信されます。このコマンドは、次のコマンドで [!DNL Edge Network] に渡されるデータストリームの上書きを作成します。 `configure` コマンドを使用している場合は、リクエストごとに上書きが渡されます。

`edgeConfigOverrides` コマンドは、次のコマンドで [!DNL Edge Network] に渡されるデータストリームの上書きを作成します。

設定の上書きが `configure` コマンドで送信される場合、次の Web SDK コマンドに含まれます。

* [sendEvent](../commands/sendevent/overview.md)
* [setConsent](../commands/setconsent.md)
* [getIdentity](../commands/getidentity.md)
* [appendIdentityToUrl](../commands/appendidentitytourl.md)
* [configure](../commands/configure/overview.md)

グローバルに指定したオプションは、個々のコマンドの設定オプションで上書きできます。

### Web SDK `sendEvent` コマンドを使用して設定の上書きを送信 {#send-event}

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
| `com_adobe_analytics.reportSuites[]` | Analytics データの送信先のレポートスイートを決定する文字列の配列。 |
| `com_adobe_identity.idSyncContainerId` | Audience Managerで使用するサードパーティ ID 同期コンテナ。 |
| `com_adobe_target.propertyToken` | Adobe Target宛先プロパティのトークン。 |

### Web SDK `configure` コマンドを使用して設定の上書きを送信 {#send-configure}

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

| パラメーター | 説明 |
|---|---|
| `edgeConfigOverrides.datastreamId` | このパラメーターを使用して、`configure` コマンドで定義されたデータストリームとは異なるデータストリームに単一のリクエストを送ることができます。 |
| `com_adobe_analytics.reportSuites[]` | Analytics データの送信先のレポートスイートを決定する文字列の配列。 |
| `com_adobe_identity.idSyncContainerId` | Audience Managerで使用するサードパーティ ID 同期コンテナ。 |
| `com_adobe_target.propertyToken` | Adobe Target宛先プロパティのトークン。 |
