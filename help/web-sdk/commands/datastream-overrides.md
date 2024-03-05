---
title: データストリーム設定の上書き
description: Web SDK を使用してデータストリームの上書きを設定する方法について説明します。
source-git-commit: 9cb46957a1c9755dcad6279aae5f5cc04eb6b305
workflow-type: tm+mt
source-wordcount: '882'
ht-degree: 14%

---


# データストリームの上書きの設定

The `edgeConfigOverrides` オブジェクトを使用すると、現在のページで実行されるコマンドの設定を上書きできます。 このオーバーライドオブジェクトは、コマンドではなく、ほとんどの Web SDK コマンドに含めることができるオブジェクトです。

このオブジェクトは、国ごとに異なる Web サイトやサブドメインがある場合、または異なるビジネスユニットに固有のデータを保存する複数のExperience Platformサンドボックスがある場合に役立ちます。

>[!IMPORTANT]
>
>データストリームの上書きに関するエンドツーエンドの設定手順について詳しくは、 [datastream 設定の上書き](../../datastreams/overrides.md#configure-overrides) ドキュメント。

データストリーム設定の上書きは、次の 2 つの手順で構成されます。

1. まず、 [datastream 設定ページ](../../datastreams/configure.md)（データストリーム UI 内） 詳しくは、 [datastream 設定の上書き](../../datastreams/overrides.md#configure-overrides) オーバーライドの設定方法に関するドキュメントを参照してください。
2. UI でデータストリームの上書きを設定したら、次のいずれかの方法で上書きを Edge ネットワークに送信する必要があります。
   * Web SDK を使用 [タグ拡張](#tag-extension).
   * を通じて `sendEvent` または `configure` Web SDK コマンド。
   * Mobile SDK を使用 `sendEvent` コマンドを使用します。

Web SDK 設定と特定のコマンド ( [`sendEvent`](sendevent/overview.md)) の場合は、特定のコマンドの上書きが優先されます。

## オブジェクトのプロパティ

このオブジェクト内では、次のプロパティを使用できます。

* **データストリームの上書き**：別のデータストリームに呼び出しを送信します。 この値を設定した場合、データストリーム設定が必要な他の上書きは、ここで設定したデータストリームで設定する必要があります。
* **サードパーティ ID 同期コンテナ**:Adobe Audience Managerの宛先サードパーティ ID 同期コンテナの ID。 このフィールドを使用する前に、データストリームの設定でサードパーティの ID コンテナの上書きを設定する必要があります。
* **Target プロパティトークン**:Adobe Targetの宛先プロパティのトークン。 このフィールドを使用する前に、データストリームの設定で Target プロパティトークンの上書きを設定する必要があります。
* **レポートスイート**:Adobe Analyticsで上書きするレポートスイート ID。 このフィールドを使用する前に、データストリームの設定でレポートスイートの上書きを設定する必要があります。

## Web SDK タグ拡張機能を使用した Edge Network へのデータストリームの上書きの送信 {#tag-extension}

次のドキュメントを参照してください： [datastream の上書きの設定](../../tags/extensions/client/web-sdk/web-sdk-extension-configuration.md#datastrea-overrides) Web SDK タグ拡張機能から。

Web SDK タグ拡張機能からデータストリームのオーバーライドを設定する場合は、以下の各フィールドを設定します。 **[!UICONTROL データストリーム設定の上書き]** when [タグ拡張の設定](/help/tags/extensions/client/web-sdk/web-sdk-extension-configuration.md).

1. にログインします。 [experience.adobe.com](https://experience.adobe.com) Adobe ID資格情報を使用して。
1. に移動します。 **[!UICONTROL データ収集]** > **[!UICONTROL タグ]**.
1. 目的のタグプロパティを選択します。
1. に移動します。 **[!UICONTROL 拡張機能]**&#x200B;を選択し、次に **[!UICONTROL 設定]** の [!UICONTROL Adobe Experience Platform Web SDK] カード。
1. 下にスクロールして、 **[!UICONTROL データストリーム設定の上書き]** 」セクションに入力します。 必要な各上書き値を設定します。
1. クリック **[!UICONTROL 保存]**&#x200B;をクリックし、変更を公開します。

特定のコマンドのみに上書きを設定する場合は、タグルールのアクション内で各フィールドを設定します。

1. にログインします。 [experience.adobe.com](https://experience.adobe.com) Adobe ID資格情報を使用して。
1. に移動します。 **[!UICONTROL データ収集]** > **[!UICONTROL タグ]**.
1. 目的のタグプロパティを選択します。
1. に移動します。 **[!UICONTROL ルール]**&#x200B;を選択し、目的のルールを選択します。
1. の下 [!UICONTROL アクション]、既存のアクションを選択するか、アクションを作成します。
1. を設定します。 [!UICONTROL 拡張] ドロップダウンフィールド **[!UICONTROL Adobe Experience Platform Web SDK]**&#x200B;をクリックし、 [!UICONTROL アクションタイプ] から **[!UICONTROL イベントを送信]**.
1. 下にスクロールして、ラベル付きのセクションを表示します。 **[!UICONTROL データストリーム設定の上書き]**.
1. このセクションの各フィールドに、目的の上書き値を設定します。
1. クリック **[!UICONTROL 変更を保持]**&#x200B;を開き、パブリッシュワークフローを実行します。

に別のフィールドが提供されます。 [!UICONTROL 開発], [!UICONTROL ステージング]、および [!UICONTROL 実稼動] 環境。 各環境の各目的のフィールドに必ず入力してください。

## Web SDK JavaScript ライブラリを使用した Edge ネットワークへの上書きの送信 {#library}

後 [データストリームの上書きの設定](../../datastreams/overrides.md) データ収集 UI で、Web SDK JavaScript ライブラリを使用して、上書きを Edge ネットワークに送信できるようになりました。

Web SDK を使用している場合は、 `edgeConfigOverrides` コマンドは、データストリーム設定の上書きをアクティブ化する 2 番目および最後の手順です。

データストリーム設定の上書きは、`edgeConfigOverrides` Web SDK コマンドを介して Edge Network に送信されます。このコマンドは、 [!DNL Edge Network] 次のコマンドで、 を使用している場合、 `configure` コマンドを使用する場合、リクエストごとにオーバーライドが渡されます。

The `edgeConfigOverrides` コマンドは、に渡されるデータストリームの上書きを作成します。 [!DNL Edge Network] 次のコマンドで、

設定の上書きが `configure` コマンドで送信される場合、次の Web SDK コマンドに含まれます。

* [sendEvent](../commands/sendevent/overview.md)
* [setConsent](../commands/setconsent.md)
* [getIdentity](../commands/getidentity.md)
* [appendIdentityToUrl](../commands/appendidentitytourl.md)
* [configure](../commands/configure/overview.md)

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
| `com_adobe_analytics.reportSuites[]` | Analytics データを送信するレポートスイートを決定する文字列の配列。 |
| `com_adobe_identity.idSyncContainerId` | Audience Managerで使用するサードパーティ ID 同期コンテナです。 |
| `com_adobe_target.propertyToken` | Adobe Target宛先プロパティのトークン。 |

### Web SDK を使用した設定の上書きの送信 `configure` command {#send-configure}

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
| `com_adobe_analytics.reportSuites[]` | Analytics データを送信するレポートスイートを決定する文字列の配列。 |
| `com_adobe_identity.idSyncContainerId` | Audience Managerで使用するサードパーティ ID 同期コンテナです。 |
| `com_adobe_target.propertyToken` | Adobe Target宛先プロパティのトークン。 |