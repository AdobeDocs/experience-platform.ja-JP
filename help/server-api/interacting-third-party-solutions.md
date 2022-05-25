---
title: サードパーティソリューションとのやり取り
description: Edge Network Server API を使用して、イベントを非Adobeソリューションに転送する方法を説明します
seo-description: Learn how to use the Edge Network Server API to forward events to non-Adobe solutions
keywords: データ収集；出口；分析；Adobe Experience Platform Edge Network api；イベント転送
exl-id: a8902b2a-fc9c-4087-a7eb-89b6cf9a6d29
source-git-commit: fb0d8aedbb88aad8ed65592e0b706bd17840406b
workflow-type: tm+mt
source-wordcount: '81'
ht-degree: 1%

---

# サードパーティソリューションとのやり取り

## 概要 {#overview}

Edge Network Server API のイベント転送機能を使用して、収集したデータを非Adobeソリューションに送信します。

## データストリームイベント転送の設定 {#event-forwarding}

サードパーティソリューションが Server API からデータを受け取れるようにするには、次の手順を実行する必要があります [データストリームの設定](../edge/datastreams/overview.md#event-forwarding-settings) イベント転送の場合。

![Adobe Analytics Datastream 設定](assets/event-forwarding-datastream.png)
