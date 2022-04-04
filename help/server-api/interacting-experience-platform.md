---
title: Adobe Experience Platformの操作
description: Edge Network Server API を使用してAdobe Experience Platformとやり取りする方法を説明します
seo-description: Learn how to use the Edge Network Server API to interact with Adobe Experience Platform
keywords: データ収集；出口；分析；Adobe Experience Platform Edge Network api;aep
exl-id: c49e40b7-9653-40f1-9db5-8941b20de8a3
source-git-commit: 422f859bef8faf292fd7e5fd8b6a8d31967421c1
workflow-type: tm+mt
source-wordcount: '71'
ht-degree: 1%

---

# Adobe Experience Platformの操作

## 概要 {#overview}

Experience Platformのデータ収集を有効にするには、まず [データストリームの設定](../edge/fundamentals/datastreams.md) イベントをExperience Platformデータセットに転送する

設定が完了したら、データストリーム設定に次の設定を含める必要があります。 `com_adobe_experience_platform`を作成します。


```json
{
  // datastream config header

  "settings": {
    "com_adobe_experience_platform": {
      "sandboxName": "prod",
      "enabled": true,
      "datasets": {
        "event": {
          "xdmSchema": "https://ns.adobe.com/atag/schemas/35a31609b6d3242736751df469ade031",
          "datasetId": "5f67e6ad9501b0194b5aafb6"
        }
      }
    }

    // other settings
  }
}
```
