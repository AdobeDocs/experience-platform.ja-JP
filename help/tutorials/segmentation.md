---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: セグメントのチュートリアル
topic: tutorial
translation-type: tm+mt
source-git-commit: ae244711ed89f4c7d6f87fd38bf7f8324e9b64be
workflow-type: tm+mt
source-wordcount: '540'
ht-degree: 0%

---


# セグメントのチュートリアル

Adobe Experience Platform [!DNL Segmentation Service][!DNL Real-time Customer Profile] には、セグメントを作成してデータからオーディエンスを生成するためのユーザーインターフェイスとRESTful APIが用意されています。 これらのセグメントは一元的に設定され、管理され [!DNL Platform]、どのアドビソリューションでも簡単にアクセスできます。 セグメント化の詳細については、まず [Segmentation Serviceの概要を参照してください](../segmentation/home.md)。

## セグメント定義の作成

セグメント定義とは、ターゲットオーディエンスの主な特性や動作を記述するために使用されるルールセットです。 概念化された後は、セグメント定義で説明されているルールを使用して、セグメントの適格なオーディエンスメンバーが決定されます。 セグメント定義の開発、テスト、プレビュー、保存は、ユー [!DNL Platform] ザーインターフェイスまたはAPIを使用して行うことができます。 セグメント定義を作成するには、「セグメントAPIの [作成](../segmentation/tutorials/create-a-segment.md) 」チュートリアル [、または『](../segmentation/ui/overview.md)セグメントビルダーのUIユーザーガイド』に従います。

## セグメントの評価とアクセス結果

セグメント定義を開発、テストおよび保存したら、予定された評価またはオンデマンド評価を通じてセグメントを評価できます。 スケジュールされた評価（「スケジュールされたセグメント化」とも呼ばれます）では、特定の時間にエクスポートジョブを実行するための定期的なスケジュールを作成できます。オンデマンド評価では、セグメントオーディエンスを作成します。 詳しくは、セグメント結果の [評価とアクセスに関するチュートリアルを参照してください](../segmentation/tutorials/evaluate-a-segment.md)。

## セグメントデータのエクスポート

データを含むセグメントを書き出すには、まず [!DNL Profile] データの書き出し先となるデータセットを [作成し](../segmentation/tutorials/create-dataset-export-segment.md)、新しい書き出しジョブを開始する必要があります。 書き出しジョブの生成手順は、セグメントの [評価のチュートリアルで確認できます](../segmentation/tutorials/evaluate-a-segment.md)。

## 結合ポリシーの設定

Adobe Experience Platformを使用すると、複数のソースからデータを統合し、それを組み合わせて、個々の顧客の完全な表示を確認できます。 When bringing this data together, merge policies are the rules that [!DNL Platform] uses to determine how data will be prioritized and what data will be combined to create that unified view. RESTful APIまたはユーザーインターフェイスを使用して、新しい結合ポリシーを作成し、既存のポリシーを管理し、組織のデフォルトの結合ポリシーを設定できます。 UIで結合ポリシーを使用するには、 [!DNL Platform][結合ポリシーユーザーガイドを参照してください](../profile/ui/merge-policies.md)。 APIを使用して結合ポリシーを操作するには、 [!DNL Real-time Customer Profile][結合ポリシーの開発ガイドを参照してください](../profile/api/merge-policies.md)。

## セグメントのデータ使用コンプライアンスの実施

で使用できるセグメントは、セグメント定義内にマージポリシーIDを [!DNL Real-time Customer Profile] 含みます。 このマージポリシーには、セグメントに含めるデータセットに関する情報が含まれ、そのデータセットには該当するデータ使用ラベルが含まれます。 オーディエンスセグメントに対するデータ使用量の準拠の適用に関する具体的な手順については、セグメントの [データ使用量準拠の実施チュートリアルに従ってください](../segmentation/tutorials/governance.md)。

## ストリーミングセグメント

ストリーミングセグメント化機能は、イベントが特定のセグメントグループに入った直後に顧客を即座に評価する機能です。 この機能を使用すると、ほとんどのセグメントルールをAdobe Experience Platformに渡す際に評価できるようになりました。つまり、セグメントのメンバーシップは、スケジュール済みのセグメント化ジョブを実行せずに最新の状態に維持されます。 詳しくは、 [ストリーミングセグメントの概要](../segmentation/api/streaming-segmentation.md)。

## マルチエンティティセグメント

複数エンティティのセグメント化は、プロファイル、店舗またはその他の非製品クラスに基づいて、追加のデータを使用して [!DNL Profile] データを拡張する機能です。 接続すると、追加のクラスのデータは、 [!DNL Profile] スキーマにネイティブであるかのように使用できるようになります。 移行方法については、 [複数エンティティのセグメント化に関するドキュメントを参照してください](../segmentation/multi-entity-segmentation.md)。