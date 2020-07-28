---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: 複数エンティティのセグメント化
topic: overview
translation-type: tm+mt
source-git-commit: bfbf2074a9dcadd809de043d62f7d2ddaa7c7b31
workflow-type: tm+mt
source-wordcount: '372'
ht-degree: 33%

---


# 複数エンティティのセグメント化

Multi-entity segmentation is the ability to extend [!DNL Profile] data with additional data based on products, stores, or other non-profile classes. Once connected, data from additional classes becomes available as if they were native to the [!DNL Profile] schema.

マルチエンティティのセグメント化の詳細については、以下のビデオを見て、ドキュメントを読み、学習を補完してください。また、 [セグメント化の概要を調べてください](./home.md)。]

>[!VIDEO](https://video.tv.adobe.com/v/28947?quality=12&learn=on)

## はじめに

このチュートリアルでは、セグメント化の使用に関わる様々な Adobe Experience Platform サービスに関する十分な知識が必要です。このチュートリアルを開始する前に、次のサービスのドキュメントを確認してください。

- [!DNL Real-time Customer Profile](../profile/home.md): 複数のソースからの集計データに基づいて、リアルタイムで統一された消費者プロファイルを提供します。
- [Adobe Experience Platform セグメント化サービス](./home.md)：リアルタイム顧客プロファイルからセグメントを作成できます、
- [!DNL Experience Data Model (XDM)](../xdm/home.md): 顧客体験データを [!DNL Platform] 整理するための標準化されたフレームワーク。

## XDM の関係の定義方法

Defining relationships with the structure of your [!DNL Experience Data Model] (XDM) schemas is an important and integral part of segment creation.

このプロセスは、 [!DNL Schema Registry][!DNL Schema Editor]APIまたは API を使用した 2 つのスキーマ間の関係の定義についての詳細なガイドは、[API を使用した 2 つのスキーマ間の関係の定義についてのチュートリアル](../xdm/tutorials/relationship-api.md)をお読みください。For a detailed guide on using the [!DNL Schema Editor] to define a relationship between two schemas, please read [the tutorial on defining a relationship between two schemas using the Schema Editor](../xdm/tutorials/relationship-ui.md).

## XDM関係を使用するセグメントの作成方法

Once you have defined your XDM relationships, you can use the [!DNL Segmentation Service] API to build a segment.

このプロセスは、 [!DNL Segmentation] APIまたは [!DNL Segment Builder] ユーザーインターフェイスを使用して実行できます。 For a detailed guide on using the API to build a segment, please read [the tutorial on creating a segment using the Segmentation API](./tutorials/create-a-segment.md). セグメントビルダーを使用したセグメント作成についての詳しいガイドは、『[セグメントビルダーユーザガイド](./ui/overview.md)』を参照してください。

## 複数エンティティセグメントの評価方法とアクセス方法

After creating a segment, you can evaluate and access the segment results using the [!DNL Segmentation Service] API. 複数エンティティセグメントの評価は、通常のセグメントの評価と非常に似ています。

This process can only be done using the [!DNL Segmentation Service] API. For a detailed guide on using the API to evaluate and access segments, please read the tutorial on [evaluating and accessing segments](./tutorials/evaluate-a-segment.md).