---
title: データストリーム設定の上書き
description: Web SDK を使用してデータストリームの上書きを設定する方法を説明します。
exl-id: 8e327892-9520-43f5-abf4-d65a5ca34e6d
source-git-commit: 2b8ca4bc1d5cf896820a5de95dcdfcd15edc2392
workflow-type: tm+mt
source-wordcount: '1159'
ht-degree: 10%

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
   * [`sendEvent`](../commands/sendevent/overview.md) または [`configure`](../commands/configure/overview.md) Web SDK コマンドを使用する。
   * Mobile SDK [`sendEvent`](https://developer.adobe.com/client-sdks/home/getting-started/track-events/#send-events-to-edge-network) コマンドを使用します。

Web SDK 設定と特定のコマンド（[`sendEvent`](sendevent/overview.md) など）の両方でオーバーライドを設定した場合、特定のコマンドのオーバーライドが優先されます。

>[!NOTE]
>
>設定の上書きをExperience Cloudサービスに *無効* する場合は、そのサービスがデータストリーム設定で最初に *有効* されていることを確認する必要があります。 データストリームにサービスを追加する方法について詳しくは、[ データストリームの設定 ](../../datastreams/configure.md#add-services) 方法に関するドキュメントを参照してください。

## Web SDK タグ拡張機能を使用して、データストリームの上書きをEdge Networkに送信します {#tag-extension}

設定手順について詳しくは、Web SDK タグ拡張機能からの [ データストリームの上書きの設定 ](../../tags/extensions/client/web-sdk/web-sdk-extension-configuration.md#datastrea-overrides) に関するドキュメントを参照してください。

Web SDK タグ拡張機能からデータストリームの上書きを設定する場合は、[ タグ拡張機能の設定 ](/help/tags/extensions/client/web-sdk/web-sdk-extension-configuration.md) 時に、**[!UICONTROL データストリーム設定の上書き]** の下で目的の各フィールドを設定します。

1. Adobe IDの資格情報を使用して [experience.adobe.com](https://experience.adobe.com) にログインします。
1. **[!UICONTROL データ収集]**/**[!UICONTROL タグ]** に移動します。
1. 目的のタグプロパティを選択します。
1. **[!UICONTROL 拡張機能]** に移動し、[!UICONTROL Adobe Experience Platform Web SDK **[!UICONTROL カードの]** 設定 &#x200B;] をクリックします。
1. **[!UICONTROL データストリーム設定の上書き]** セクションまでスクロールします。 必要な各上書き値を設定します。
1. 「**[!UICONTROL 保存]**」をクリックして、変更を公開します。

特定のコマンドに対してのみ上書きを設定する場合は、タグルールのアクション内の目的の各フィールドを設定します。

1. Adobe IDの資格情報を使用して [experience.adobe.com](https://experience.adobe.com) にログインします。
1. **[!UICONTROL データ収集]**/**[!UICONTROL タグ]** に移動します。
1. 目的のタグプロパティを選択します。
1. **[!UICONTROL ルール]** に移動し、目的のルールを選択します。
1. [!UICONTROL &#x200B; アクション &#x200B;] で、既存のアクションを選択するか、アクションを作成します。
1. 「[!UICONTROL &#x200B; 拡張機能 &#x200B;]」ドロップダウンフィールドを **[!UICONTROL Adobe Experience Platform Web SDK]** に設定し、「[!UICONTROL &#x200B; アクションタイプ &#x200B;] を **[!UICONTROL イベントを送信]** に設定します。
1. **[!UICONTROL データストリーム設定の上書き]** というラベルの付いたセクションまでスクロールします。
1. このセクションの各フィールドを目的のオーバーライド値に設定します。
1. 「**[!UICONTROL 変更を保持]**」をクリックして、公開ワークフローを実行します。

[!UICONTROL &#x200B; 開発 &#x200B;] 環境、[!UICONTROL &#x200B; ステージング &#x200B;] 環境および [!UICONTROL &#x200B; 実稼動 &#x200B;] 環境には、別々のフィールドが用意されています。 各環境で必要な各フィールドに、必ず入力してください。

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

次の例に、`sendEvent` 呼び出しでサポートされるすべての動的データストリーム設定オプションを示します。

データストリーム設定でサポートされるすべてのサービスが有効になっている場合、以下のサンプルはこの設定を上書きし、すべてのサービスを無効にします（各サービスの `enabled: false` 設定を参照してください）。

```js
alloy("sendEvent", {
  renderDecisions: true,
  edgeConfigOverrides: {
    datastreamId: "bfa8fe21-6157-42d3-b47a-78310920b39d",
    com_adobe_experience_platform: {
      enabled: false,
      datasets: {
        event: {
          datasetId: "64b6f949a8a6891ca8a28911",
        },
      },
      com_adobe_edge_ode: {
        enabled: false,
      },
      com_adobe_edge_segmentation: {
        enabled: false,
      },
      com_adobe_edge_destinations: {
        enabled: false,
      },
      com_adobe_edge_ajo: {
        enabled: false,
      },
    },
    com_adobe_analytics: {
      enabled: false,
      reportSuites: ["ujslconfigoverrides3"],
    },
    com_adobe_identity: {
      idSyncContainerId: 34374,
    },
    com_adobe_target: {
      enabled: false,
      propertyToken: "f3fd55e1-a06d-8650-9aa5-c8356c6e2223",
    },
    com_adobe_audience_manager: {
      enabled: false,
    },
    com_adobe_launch_ssf: {
      enabled: false,
    },
  },
});
```

| パラメーター | 説明 |
|---|---|
| `renderDecisions` |  |
| `edgeConfigOverrides.datastreamId` | このパラメーターを使用して、`configure` コマンドで定義されたデータストリームとは異なるデータストリームに単一のリクエストを送ることができます。 |
| `edgeConfigOverrides.com_adobe_experience_platform` | Experience Platformサービスの動的データストリーム設定を定義します。 |
| `edgeConfigOverrides.com_adobe_experience_platform.enabled` | イベントがExperience Platformサービスに送信されるかどうかを指定します。 |
| `edgeConfigOverrides.com_adobe_experience_platform.datasets` | イベントに使用するデータセットを定義します。 |
| `edgeConfigOverrides.com_adobe_experience_platform.com_adobe_edge_ode.enabled` | イベントがOffer decisioningサービスに送信されるかどうかを指定します。 |
| `edgeConfigOverrides.com_adobe_experience_platform.com_adobe_edge_segmentation.enabled` | イベントがエッジセグメント化サービスに送信されるかどうかを定義します。 |
| `edgeConfigOverrides.com_adobe_experience_platform.com_adobe_edge_destinations.enabled` | イベントデータをエッジ宛先に送信するかどうかを定義します。 |
| `edgeConfigOverrides.com_adobe_experience_platform.com_adobe_edge_ajo.enabled` | イベントデータをAdobe Journey Optimizer サービスに送信するかどうかを指定します。 |
| `com_adobe_analytics.enabled` | イベントデータをAdobe Analyticsに送信するかどうかを指定します。 |
| `com_adobe_analytics.reportSuites[]` | Analytics データの送信先のレポートスイートを決定する文字列の配列。 |
| `com_adobe_identity.idSyncContainerId` | Audience Managerで使用するサードパーティ ID 同期コンテナ。 この ID 同期コンテナを機能させるには、`com_adobe_audience_manager.enabled` を `true` に設定する必要があります。 それ以外の場合、Audience Managerサービスは無効になります。 |
| `com_adobe_target.enabled` | イベントデータをAdobe Targetに送信するかどうかを定義します。 |
| `com_adobe_target.propertyToken` | Adobe Target宛先プロパティのトークン。 |
| `com_adobe_audience_manager.enabled` | Audience Managerデータをイベントサービスに送信するかどうかを指定します。 |
| `com_adobe_launch_ssf` | イベントデータをサーバーサイド転送に送信するかどうかを定義します。 |

### Web SDK `configure` コマンドを使用して設定の上書きを送信 {#send-configure}

以下の例は、`configure` コマンド上でどのように設定の上書きが表示されるかを示します。

データストリーム設定でサポートされるすべてのサービスが有効になっている場合、以下のサンプルはこの設定を上書きし、すべてのサービスを無効にします（各サービスの `enabled: false` 設定を参照してください）。

```js
alloy("configure", {
  orgId: "97D1F3F459CE0AD80A495CBE@AdobeOrg",
  datastreamId: "db9c70a1-6f11-4563-b0e9-b5964ab3a858",
  edgeConfigOverrides: {
    com_adobe_experience_platform: {
      enabled: false,
      datasets: {
        event: {
          datasetId: "64b6f930753dd41ca8d4fd77",
        },
      },
      com_adobe_edge_ode: {
        enabled: false,
      },
      com_adobe_edge_segmentation: {
        enabled: false,
      },
      com_adobe_edge_destinations: {
        enabled: false,
      },
      com_adobe_edge_ajo: {
        enabled: false,
      },
    },
    com_adobe_analytics: {
      enabled: false,
      reportSuites: ["ujslconfigoverrides2"],
    },
    com_adobe_identity: {
      idSyncContainerId: 34373,
    },
    com_adobe_target: {
      enabled: false,
      propertyToken: "01dbc634-07c1-d8f9-ca69-b489a5ac5e94",
    },
    com_adobe_audience_manager: {
      enabled: false,
    },
    com_adobe_launch_ssf: {
      enabled: false,
    },
  },
});
```

| パラメーター | 説明 |
|---|---|
| `orgId` | Adobeアカウントに対応する IMS 組織 ID。 |
| `edgeConfigOverrides.datastreamId` | このパラメーターを使用して、`configure` コマンドで定義されたデータストリームとは異なるデータストリームに単一のリクエストを送ることができます。 |
| `edgeConfigOverrides.com_adobe_experience_platform` | Experience Platformサービスの動的データストリーム設定を定義します。 |
| `edgeConfigOverrides.com_adobe_experience_platform.enabled` | イベントがExperience Platformサービスに送信されるかどうかを指定します。 |
| `edgeConfigOverrides.com_adobe_experience_platform.datasets` | イベントに使用するデータセットを定義します。 |
| `edgeConfigOverrides.com_adobe_experience_platform.com_adobe_edge_ode.enabled` | イベントがOffer decisioningサービスに送信されるかどうかを指定します。 |
| `edgeConfigOverrides.com_adobe_experience_platform.com_adobe_edge_segmentation.enabled` | イベントがエッジセグメント化サービスに送信されるかどうかを定義します。 |
| `edgeConfigOverrides.com_adobe_experience_platform.com_adobe_edge_destinations.enabled` | イベントデータをエッジ宛先に送信するかどうかを定義します。 |
| `edgeConfigOverrides.com_adobe_experience_platform.com_adobe_edge_ajo.enabled` | イベントデータをAdobe Journey Optimizer サービスに送信するかどうかを指定します。 |
| `com_adobe_analytics.enabled` | イベントデータをAdobe Analyticsに送信するかどうかを指定します。 |
| `com_adobe_analytics.reportSuites[]` | Analytics データの送信先のレポートスイートを決定する文字列の配列。 |
| `com_adobe_identity.idSyncContainerId` | Audience Managerで使用するサードパーティ ID 同期コンテナ。 この ID 同期コンテナを機能させるには、`com_adobe_audience_manager.enabled` を `true` に設定する必要があります。 それ以外の場合、Audience Managerサービスは無効になります。 |
| `com_adobe_target.enabled` | イベントデータをAdobe Targetに送信するかどうかを定義します。 |
| `com_adobe_target.propertyToken` | Adobe Target宛先プロパティのトークン。 |
| `com_adobe_audience_manager.enabled` | Audience Managerデータをイベントサービスに送信するかどうかを指定します。 |
| `com_adobe_launch_ssf` | イベントデータをサーバーサイド転送に送信するかどうかを定義します。 |

