---
title: edgeConfigOverrides
description: 実装に合わせてデータストリームの上書きを設定します。
source-git-commit: f4a2778c71ad6a48621212f3ece1776d1b3ac643
workflow-type: tm+mt
source-wordcount: '402'
ht-degree: 0%

---

# `edgeConfigOverrides` （`configure` コマンド）

`edgeConfigOverrides` オブジェクトを使用すると、現在のページで実行されるコマンドの設定を上書きできます。 このオブジェクトは、国ごとに異なる web サイトやサブドメインがある場合や、異なる事業部門固有のデータを保存する複数のExperience Platform サンドボックスがある場合に役立ちます。 ページ上の 1 つのコマンドのみの設定を上書きする場合は、[`edgeConfigOverrides` コマンドで `sendEvent` オブジェクトを使用することを検討してください &#x200B;](../sendevent/edgeconfigoverrides.md)

データストリーム設定の上書きプロセスは、次の 2 つの主な手順で構成されます。

1. まず、データストリーム UI で [&#x200B; データストリームの設定 &#x200B;](/help/datastreams/configure.md) を行う際に、データストリーム設定の上書きを定義する必要があります。 上書きの設定方法の手順については、データストリームに関するドキュメントの [&#x200B; データストリーム設定の上書き &#x200B;](/help/datastreams/overrides.md) を参照してください。
1. データストリーム UI でデータストリームの上書きを設定したら、`edgeConfigOverrides` オブジェクトを設定できます。

`edgeConfigOverrides` コマンドで `configure` オブジェクトを設定すると、Adobeに送信されるすべてのデータに適用されます。 次のコマンドは _また_`edgeConfigOverrides` オブジェクトをサポートします。

* [&#39;sendEvent&#39;](../sendevent/edgeconfigoverrides.md)
* [&#39;setConsent&#39;](../setconsent.md)
* [&#39;getIdentity&#39;](../getidentity.md)
* [&#39;appendIdentityToUrl&#39;](../appendidentitytourl.md)

上記のいずれかのコマンドで `edgeConfigOverrides` を設定すると、両方が設定されている場合、`edgeConfigOverrides` コマンドの `configure` オブジェクトよりも優先されます。 これらのコマンドに `edgeConfigOverrides` オブジェクトが含まれていない場合は、`edgeConfigOverrides` コマンドの `configure` オブジェクトが使用されます。

## 例

データストリーム設定でサポートされるすべてのサービスが有効になっている場合、以下のサンプルはこの設定を上書きし、すべてのサービスを無効にします。

```js
alloy("configure", {
  orgId: "97D1F3F459CE0AD80A495CBE@AdobeOrg",
  datastreamId: "db9c70a1-6f11-4563-b0e9-b5964ab3a858",
  edgeConfigOverrides: {
    com_adobe_experience_platform: {
      enabled: false,
      datasets: {
        event: {
          datasetId: "64b6f930753dd41ca8d4fd77"
        }
      },
      com_adobe_edge_ode: {
        enabled: false
      },
      com_adobe_edge_segmentation: {
        enabled: false
      },
      com_adobe_edge_destinations: {
        enabled: false
      },
      com_adobe_edge_ajo: {
        enabled: false
      },
    },
    com_adobe_analytics: {
      enabled: false,
      reportSuites: ["exampleoverridersid","exampleoverridersid2"]
    },
    com_adobe_identity: {
      idSyncContainerId: 34373
    },
    com_adobe_target: {
      enabled: false,
      propertyToken: "01dbc634-07c1-d8f9-ca69-b489a5ac5e94"
    },
    com_adobe_audience_manager: {
      enabled: false
    },
    com_adobe_launch_ssf: {
      enabled: false
    }
  }
});
```

| パラメーター | タイプ | 説明 |
| --- | --- | --- |
| **`orgId`** | `string` | 会社の IMS 組織 ID。 |
| **`datastreamId`** | `string` | データの送信先のデータストリーム ID。 |
| **`com_adobe_experience_platform`** | `object` | Adobe Experience Platform サービスの動的データストリーム設定を定義します。 |
| **`com_adobe_experience_platform.enabled`** | `boolean` | イベントがAdobe Experience Platformに送信されるかどうかを指定します。 |
| **`com_adobe_experience_platform.datasets`** | `object` | イベントに使用するデータセットを定義します。 |
| **`com_adobe_experience_platform.com_adobe_edge_ode.enabled`** | `boolean` | イベントがOffer Decisioningに送信されるかどうかを指定します。 |
| **`com_adobe_experience_platform.com_adobe_edge_segmentation.enabled`** | `boolean` | イベントがEdge セグメント化に送信されるかどうかを指定します。 |
| **`com_adobe_experience_platform.com_adobe_edge_destinations.enabled`** | `boolean` | イベントがEdge Destinations に送信されるかどうかを指定します。 |
| **`com_adobe_experience_platform.com_adobe_edge_ajo.enabled`** | `boolean` | イベントがAdobe Journey Optimizerに送信されるかどうかを指定します。 |
| **`com_adobe_analytics.enabled`** | `boolean` | イベントがAdobe Analyticsに送信されるかどうかを指定します。 |
| **`com_adobe_analytics.reportSuites[]`** | `string[]` | Analytics データの送信先のレポートスイートを決定する文字列の配列。 |
| **`com_adobe_audience_manager.enabled`** | `boolean` | イベントがAdobe Audience Managerに送信されるかどうかを指定します。 |
| **`com_adobe_identity.idSyncContainerId`** | `integer` | Audience Managerで使用するサードパーティ ID 同期コンテナ。 `com_adobe_audience_manager.enabled` を `true` に設定する必要があります。 そうでない場合、Audience Manager サービスは無効になります。 |
| **`com_adobe_target.enabled`** | `boolean` | イベントがAdobe Targetに送信されるかどうかを指定します。 |
| **`com_adobe_target.propertyToken`** | `string` | Adobe Target宛先プロパティのトークン。 |
| **`com_adobe_launch_ssf`** | `boolean` | イベントがサーバーサイド転送に送信されるかどうかを決定します。 |

## Web SDK タグ拡張機能を使用した設定のオーバーライド

このフィールドと同等の Web SDK タグ拡張機能は、タグ拡張機能を設定する際に [&#x200B; 設定の上書き &#x200B;](/help/tags/extensions/client/web-sdk/configure/configuration-overrides.md) の下にあります。
