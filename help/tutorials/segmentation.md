---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: セグメント化のチュートリアル
topic: tutorial
type: Tutorial
description: Adobe Experience Platform セグメント化サービスは、セグメントを作成し、リアルタイム顧客プロファイルデータからオーディエンスを生成できるユーザーインターフェイスおよび RESTful API を提供します。これらのセグメントは、プラットフォーム上で一元的に設定および管理され、アドビのどのソリューションからでも簡単にアクセスできます。
translation-type: tm+mt
source-git-commit: 97dfd3a9a66fe2ae82cec8954066bdf3b6346830
workflow-type: tm+mt
source-wordcount: '584'
ht-degree: 56%

---


# セグメント化のチュートリアル

Adobe Experience Platform [!DNL Segmentation Service] provides a user interface and RESTful API that allows you to build segments and generate audiences from your [!DNL Real-time Customer Profile] data. These segments are centrally configured and maintained on [!DNL Platform], and are readily accessible by any Adobe solution. セグメント化の詳細について確認するには、最初に「[Segmentation Service overview](../segmentation/home.md)」をお読みください。

## セグメント定義の作成

セグメント定義は、ターゲットオーディエンスの重要な特徴やビヘイビアーの説明に使用されるルールセットです。概念化が完了したら、セグメント定義で説明されているルールを使用して、セグメントに適したオーディエンスメンバーを決定します。The developing, testing, previewing, and saving of a segment definition can be done using the [!DNL Platform] user interface or APIs. セグメント定義を作成するには、[セグメント API の作成に関するチュートリアル](../segmentation/tutorials/create-a-segment.md)、または[セグメントビルダー UI のユーザガイド](../segmentation/ui/overview.md)に従います。

## セグメントの評価とアクセス結果

セグメント定義を開発、テスト、保存したら、スケジュールに沿った評価またはオンデマンド評価を通じてセグメントを評価できます。スケジュールに沿った評価（「スケジュールに沿ったセグメント化」とも呼ばれる）では、特定の時間に書き出しジョブを実行する反復スケジュールを作成できます。一方、オンデマンド評価では、セグメントジョブを作成してオーディエンスを即座に作成します。詳しくは、[セグメント結果の評価とアクセス](../segmentation/tutorials/evaluate-a-segment.md)に関するチュートリアルを参照してください。

## セグメントデータの書き出し

Exporting segments containing [!DNL Profile] data requires first [creating a dataset into which the data will be exported](../segmentation/tutorials/create-dataset-export-segment.md), then initiating a new export job. 書き出しジョブの生成手順は、セグメントの [評価のチュートリアルで確認できます](../segmentation/tutorials/evaluate-a-segment.md)。

## 結合ポリシーの設定

Adobe Experience Platform では、複数のソースのデータを統合して、個々の顧客の全体像を把握できます。When bringing this data together, merge policies are the rules that [!DNL Platform] uses to determine how data will be prioritized and what data will be combined to create that unified view. RESTful API またはユーザーインターフェイスを介すると、新しい結合ポリシーの作成、既存のポリシーの管理、組織のデフォルトの結合ポリシーの設定をおこなえます。To work with merge policies in the [!DNL Platform] UI, visit the [merge policies user guide](../profile/ui/merge-policies.md). To work with merge policies using the [!DNL Real-time Customer Profile] API, see the [merge policies developer guide](../profile/api/merge-policies.md).

## セグメントでデータ使用のコンプライアンスを徹底する

Segments that are enabled for use in [!DNL Real-time Customer Profile] contain a merge policy ID within their segment definition. この結合ポリシーには、セグメントに含めるデータセットに関する情報があります。さらに、データセットには適用可能なデータ使用ラベルが含まれています。オーディエンスセグメントでデータ使用のコンプライアンスを徹底するための具体的な手順については、[セグメントでデータ使用のコンプライアンスを徹底するためのチュートリアル](../segmentation/tutorials/governance.md)を参照してください。

## ストリーミングセグメント化

ストリーミングセグメント化機能は、イベントが特定のセグメントグループに入った直後に顧客を即座に評価する機能です。 詳しくは、[ストリーミングによるセグメント化の概 要](../segmentation/api/streaming-segmentation.md)を参照してください。

## マルチエンティティのセグメント化

Multi-entity segmentation is the ability to extend [!DNL Profile] data with additional data based on products, stores, or other non-profile classes. Once connected, data from additional classes becomes available as if they were native to the [!DNL Profile] schema. 詳しくは、[マルチエンティティのセグメント化に関するドキュメント](../segmentation/multi-entity-segmentation.md)を参照してください。