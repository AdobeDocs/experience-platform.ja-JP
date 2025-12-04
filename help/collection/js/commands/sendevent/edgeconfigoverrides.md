---
title: edgeConfigOverrides
description: 単に sendEvent コマンドのデータストリームの上書きを設定します。
exl-id: 8e327892-9520-43f5-abf4-d65a5ca34e6d
source-git-commit: db7e6df1b1a0eb19518d9c6ccd6e6bb9131d5a3e
workflow-type: tm+mt
source-wordcount: '227'
ht-degree: 0%

---

# `edgeConfigOverrides` （`sendEvent` コマンド）

`edgeConfigOverrides` オブジェクトを使用すると、現在の `sendEvent` コマンドの構成設定のみを上書きできます。 このオブジェクトは、同じページに、他の Web SDK実装とは異なる設定で実行する特定のコマンドがある場合に役立ちます。 特定のページ上のすべてのコマンドの設定を上書きする場合は、[`edgeConfigOverrides` コマンドの `configure` オブジェクトを使用することを検討してください ](../configure/edgeconfigoverrides.md)

包括的なデータストリーム設定の上書きプロセスは、次の 2 つの主な手順で構成されます。

1. まず、データストリーム UI で [ データストリームの設定 ](/help/datastreams/configure.md) を行う際に、データストリーム設定の上書きを定義する必要があります。 上書きの設定方法の手順については、データストリームに関するドキュメントの [ データストリーム設定の上書き ](/help/datastreams/overrides.md) を参照してください。
1. データストリーム UI でデータストリームの上書きを設定したら、`edgeConfigOverrides` オブジェクトを設定できます。

`configure` コマンドは `edgeConfigOverrides` オブジェクトもサポートします。[`edgeConfigOverrides`](../configure/edgeconfigoverrides.md) コマンドの `configure` を参照してください。 `edgeConfigOverrides` コマンドの `sendEvent` オブジェクトが `edgeConfigOverrides` コマンドの `configure` オブジェクトよりも優先されます（両方が設定されている場合）。

## 例

データストリーム設定でサポートされるすべてのサービスが有効になっている場合、以下のサンプルはこの設定を上書きし、すべてのサービスを無効にします（各サービスの `enabled: false` 設定を参照してください）。 このオブジェクトは、[`edgeConfigOverrides`](../configure/edgeconfigoverrides.md) コマンドの `configure` オブジェクトと同じプロパティをサポートします。

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
      reportSuites: ["examplersid3"],
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

## Web SDK タグ拡張機能を使用したデータストリーム設定の上書き

このオブジェクトと同等の Web SDK タグ拡張機能は、「[」アクションを設定する際の ](/help/tags/extensions/client/web-sdk/actions/send-event.md#datastream-configuration-overrides) データストリーム設定の上書き [!UICONTROL Send event] セクションです。
