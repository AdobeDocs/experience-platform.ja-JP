---
keywords: Experience Platform;home;popular topics;segmentation;Segmentation;segment service;segments;Segments;multi-entity;multi-entity segmentation;multi-entity segments;
solution: Experience Platform
title: マルチエンティティのセグメント化
topic: overview
description: 複数エンティティのセグメント化は、製品、店舗または他の非プロファイルクラスに基づいて、プロファイルデータを拡張する機能です。接続されると、追加のクラスのデータは、プロファイルスキーマにネイティブであるかのように使用できるようになります。
translation-type: tm+mt
source-git-commit: 17ef6c1c6ce58db2b65f1769edf719b98d260fc6
workflow-type: tm+mt
source-wordcount: '418'
ht-degree: 38%

---


# マルチエンティティのセグメント化

Multi-entity segmentation is the ability to extend [!DNL Profile] data with additional data based on products, stores, or other non-profile classes. Once connected, data from additional classes becomes available as if they were native to the [!DNL Profile] schema.

マルチエンティティのセグメント化の詳細については、以下のビデオを見て、ドキュメントを読み、学習を補完してください。また、 [セグメント化の概要を調べてください](./home.md)。

>[!VIDEO](https://video.tv.adobe.com/v/28947?quality=12&learn=on)

## はじめに

このチュートリアルでは、セグメント化の使用に関わる様々な Adobe Experience Platform サービスに関する十分な知識が必要です。このチュートリアルを開始する前に、次のサービスのドキュメントを確認してください。

- [[!DNLリアルタイム顧客プロファイル]](../profile/home.md):複数のソースからの集計データに基づいて、リアルタイムで統一された消費者プロファイルを提供します。
- [Adobe Experience Platform セグメント化サービス](./home.md)：リアルタイム顧客プロファイルからセグメントを作成できます、
- [[!DNL Experience Data Model (XDM)]](../xdm/home.md):顧客体験データを [!DNL Platform] 整理する際に使用される標準化されたフレームワーク。

## XDM の関係の定義方法

Defining relationships with the structure of your [!DNL Experience Data Model] (XDM) schemas is an important and integral part of segment creation.

このプロセスは、 [!DNL Schema Registry][!DNL Schema Editor]APIまたは API を使用した 2 つのスキーマ間の関係の定義についての詳細なガイドは、[API を使用した 2 つのスキーマ間の関係の定義についてのチュートリアル](../xdm/tutorials/relationship-api.md)をお読みください。For a detailed guide on using the [!DNL Schema Editor] to define a relationship between two schemas, please read [the tutorial on defining a relationship between two schemas using the Schema Editor](../xdm/tutorials/relationship-ui.md).

## XDM関係を使用するセグメントの作成方法

Once you have defined your XDM relationships, you can use the [!DNL Segmentation Service] API to build a segment.

このプロセスは、 [!DNL Segmentation] APIまたは [!DNL Segment Builder] ユーザーインターフェイスを使用して実行できます。 For a detailed guide on using the API to build a segment, please read [the tutorial on creating a segment using the Segmentation API](./tutorials/create-a-segment.md). セグメントビルダーを使用したセグメント作成についての詳しいガイドは、『[セグメントビルダーユーザガイド](./ui/overview.md)』を参照してください。

## 複数エンティティセグメントの評価方法とアクセス方法

After creating a segment, you can evaluate and access the segment results using the [!DNL Segmentation Service] API. 複数エンティティセグメントの評価は、通常のセグメントの評価と非常に似ています。

This process can only be done using the [!DNL Segmentation Service] API. For a detailed guide on using the API to evaluate and access segments, please read the tutorial on [evaluating and accessing segments](./tutorials/evaluate-a-segment.md).