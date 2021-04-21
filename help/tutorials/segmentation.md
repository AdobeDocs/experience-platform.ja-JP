---
keywords: Experience Platform；ホーム；人気の高いトピック
solution: Experience Platform
title: セグメント化のチュートリアル
topic-legacy: tutorial
type: Tutorial
description: Adobe Experience Platform セグメント化サービスは、セグメントを作成し、リアルタイム顧客プロファイルデータからオーディエンスを生成できるユーザーインターフェイスおよび RESTful API を提供します。これらのセグメントは、プラットフォーム上で一元的に設定および管理され、アドビのどのソリューションからでも簡単にアクセスできます。
exl-id: e45de6b5-ff71-4908-ad79-898084763704
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '589'
ht-degree: 55%

---

# セグメント化のチュートリアル

Adobe Experience Platform[!DNL Segmentation Service]は、セグメントを作成して[!DNL Real-time Customer Profile]データからオーディエンスを生成するためのユーザーインターフェイスとRESTful APIを提供します。 これらのセグメントは[!DNL Platform]上で一元的に構成および管理され、どのAdobeソリューションでも容易にアクセスできます。 セグメント化の詳細について確認するには、最初に「[Segmentation Service overview](../segmentation/home.md)」をお読みください。

## セグメント定義の作成

セグメント定義は、ターゲットオーディエンスの重要な特徴やビヘイビアーの説明に使用されるルールセットです。概念化が完了したら、セグメント定義で説明されているルールを使用して、セグメントに適したオーディエンスメンバーを決定します。セグメント定義の開発、テスト、プレビュー、保存は、[!DNL Platform]ユーザーインターフェイスまたはAPIを使用して行うことができます。 セグメント定義を作成するには、[セグメント API の作成に関するチュートリアル](../segmentation/tutorials/create-a-segment.md)、または[セグメントビルダー UI のユーザガイド](../segmentation/ui/overview.md)に従います。

## セグメントの評価とアクセス結果

セグメント定義を開発、テスト、保存したら、スケジュールに沿った評価またはオンデマンド評価を通じてセグメントを評価できます。スケジュールに沿った評価（「スケジュールに沿ったセグメント化」とも呼ばれる）では、特定の時間に書き出しジョブを実行する反復スケジュールを作成できます。一方、オンデマンド評価では、セグメントジョブを作成してオーディエンスを即座に作成します。詳しくは、[セグメント結果の評価とアクセス](../segmentation/tutorials/evaluate-a-segment.md)に関するチュートリアルを参照してください。

## セグメントデータの書き出し

[!DNL Profile]データを含むセグメントの書き出しを行うには、まず[データの書き出し先となるデータセット](../segmentation/tutorials/create-dataset-export-segment.md)を作成し、新しい書き出しジョブを開始する必要があります。 エクスポートジョブの生成手順は、[セグメント](../segmentation/tutorials/evaluate-a-segment.md)の評価のチュートリアルで確認できます。

## 結合ポリシーの設定

Adobe Experience Platform では、複数のソースのデータを統合して、個々の顧客の全体像を把握できます。このデータを統合する際、マージポリシーは、[!DNL Platform]がデータの優先順位付け方法と、どのデータを組み合わせて統合表示を作成するかを決定するために使用するルールです。 RESTful API またはユーザーインターフェイスを介すると、新しい結合ポリシーの作成、既存のポリシーの管理、組織のデフォルトの結合ポリシーの設定をおこなえます。[!DNL Platform] UIの結合ポリシーを使用するには、[merge policiesユーザーガイド](../profile/ui/merge-policies.md)を参照してください。 [!DNL Real-time Customer Profile] APIを使用して結合ポリシーを操作するには、[merge policies開発者ガイド](../profile/api/merge-policies.md)を参照してください。

## セグメントでデータ使用のコンプライアンスを徹底する

[!DNL Real-time Customer Profile]で使用できるセグメントは、セグメント定義内にマージポリシーIDを含みます。 この結合ポリシーには、セグメントに含めるデータセットに関する情報があります。さらに、データセットには適用可能なデータ使用ラベルが含まれています。オーディエンスセグメントでデータ使用のコンプライアンスを徹底するための具体的な手順については、[セグメントでデータ使用のコンプライアンスを徹底するためのチュートリアル](../segmentation/tutorials/governance.md)を参照してください。

## ストリーミングセグメント化

ストリーミングセグメント化機能は、イベントが特定のセグメントグループに入った直後に顧客を即座に評価する機能です。 この機能を使用すると、ほとんどのセグメントルールを、データがAdobe Experience Platformに渡される際に評価できるようになりました。つまり、セグメントのメンバーシップは、スケジュール済みのセグメント化ジョブを実行せずに最新の状態に維持されます。 詳しくは、[ストリーミングによるセグメント化の概 要](../segmentation/api/streaming-segmentation.md)を参照してください。

## マルチエンティティのセグメント化

マルチエンティティセグメント化機能は、プロファイル、店舗、または他の非製品クラスに基づいて、追加のデータを使用して[!DNL Profile]データを拡張する機能です。 接続すると、追加のクラスのデータは、[!DNL Profile]スキーマにネイティブであるかのように使用できるようになります。 詳しくは、[マルチエンティティのセグメント化に関するドキュメント](../segmentation/multi-entity-segmentation.md)を参照してください。
