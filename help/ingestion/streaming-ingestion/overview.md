---
keywords: Experience Platform;home;popular topics;data ingestion;ingested data;streaming
solution: Experience Platform
title: Adobe Experience Platform のストリーミングインジェストの概要
topic: overview
description: ユーザーは、Adobe Experience Platform のストリーミングインジェストを使用して、クライアントおよびサーバー側のデバイスから Experience Platform にリアルタイムでデータを送信することができます。
translation-type: tm+mt
source-git-commit: bf99b08a1093a815687cc06372407949e170a0b3
workflow-type: tm+mt
source-wordcount: '272'
ht-degree: 38%

---


# ストリーミングインジェストの概要

Streaming ingestion for Adobe Experience Platform provides users a method to send data from client and server-side devices to [!DNL Experience Platform] in real-time.

## ストリームインジェストの機能

Adobe Experience Platform enables you to drive coordinated, consistent, and relevant experiences by generating a [!DNL Real-time Customer Profile] for each of your individual customers. Streaming ingestion plays a key role in building these profiles by enabling you to deliver [!DNL Profile] data into the [!DNL Data Lake] with as little latency as possible.

次のビデオは、ストリーミング取り込みに関する理解を深めるために設計されており、上述の概念について概説しています。

>[!VIDEO](https://video.tv.adobe.com/v/28425?quality=12&learn=on)

### プロファイルレコードと のストリーミング[!DNL ExperienceEvents]

With streaming ingestion, users can stream profile records and [!DNL ExperienceEvents] to [!DNL Platform] in seconds to help drive real-time personalization. All data sent to streaming ingestion APIs is automatically persisted in the [!DNL Data Lake].

詳しくは、[ストリーミング接続の作成に関するガイド](../tutorials/create-streaming-connection.md)を参照してください。

### データセットへのストリーミング

Once you are confident that your data is clean, you can enable your datasets for [!DNL Real-time Customer Profile] and [!DNL Identity Service].

For more information on enabling a dataset for [!DNL Profile] and [!DNL Identity Service], please read the [configure a dataset guide](../../profile/tutorials/dataset-configuration.md).

## What is the expected latency for streaming ingestion on [!DNL Platform]?

| 宛先 | 予想遅延時間 |
| --------- | ---------------- |
| リアルタイム顧客プロファイル | &lt; 1 分 |
| データレイク | &lt; 60 分 |

## Adobe Experience Platform 拡張機能

Adobe Experience Platform 拡張機能を使用して、新しいストリーミング接続を作成できます。The [!DNL Experience Platform] extension provides actions to send beacons formatted in [!DNL Experience Data Model] (XDM) for real-time ingestion to [!DNL Experience Platform]. 詳しくは、[Experience Platform 拡張機能](https://docs.adobe.com/content/help/ja-JP/launch/using/extensions-ref/adobe-extension/adobe-experience-platform-extension.html)に関するドキュメントを参照してください。