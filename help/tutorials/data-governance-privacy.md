---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: データガバナンスとプライバシーのチュートリアル
topic: tutorial
translation-type: tm+mt
source-git-commit: ee08f43400fa72abce95ed52aff879f954f4b4d6
workflow-type: tm+mt
source-wordcount: '502'
ht-degree: 0%

---


# データガバナンスとプライバシーのチュートリアル

Data Usage Labeling and Enforcement(DULE)は、Adobe Experience Platform Data Governanceの中核的なメカニズムです。 DULE機能を使用すると、データセットとフィールドにデータ使用ラベルを適用し、関連するデータ使用ポリシーに従ってそれぞれを分類できます。 ラベルの使用を開始する前に、 [データ・ガバナンスの概要](../data-governance/home.md) （Data Governanceの概要）を参照して、プラットフォーム内のDULEフレームワークのより強力な紹介を確認してください。

Adobe Experience Platform Privacy Serviceは、RESTful APIとユーザーインターフェイスを備えており、これにより、様々なソリューション間でプライバシーとコンプライアンスの要求を調整できます。 詳しくは、まず [プライバシーサービスの概要を参照してください](../privacy-service/home.md)。

## データ使用ラベル

データ使用ラベルを使用すると、データに適用される使用ポリシーに従ってデータセットとフィールドを分類できます。 ラベルはいつでも適用でき、データの管理方法を柔軟に選択できます。 ベストプラクティスは、データのラベル付けがエクスペリエンスプラットフォームに取り込まれるとすぐに、またはデータがプラットフォームで使用できるようになるとすぐに行うことを推奨します。 データセットレベルで適用されるデータ使用量ラベルは、データセット内のすべてのフィールドに反映されます。 ラベルは、データセット内の個々のフィールド（列ヘッダー）に、伝播せずに直接適用することもできます。 データ使用ラベルをデータに適用する方法については、 [データ使用ラベルの概要を参照してください](../data-governance/labels/overview.md)。

## データ使用ポリシーの作成

DULE Policy Service APIを使用すると、DULEポリシーを作成および管理して、特定のDULEラベルを含むデータに対して実行できるマーケティングアクションを決定できます。 使用を開始するには、 [データ使用ポリシーの概要を読みます](../data-governance/policies/overview.md)。

## データ使用ポリシーの適用

データ用にData Usage Labeling and Enforcement(DULE)ラベルを作成し、これらのラベルに対するマーケティングアクション用のDULEポリシーを作成した後は、DULE Policy Service APIを使用して、マーケティングアクションがデータセットに対して実行されたか、またはDULEラベルの任意のグループを評価できます。 その後、独自の内部プロトコルを設定し、API応答に基づくポリシー違反を処理できます。 開始するには、 [ポリシー実施の概要](../data-governance/enforcement/overview.md)。

## オーディエンスセグメントのデータ使用コンプライアンスの実施

リアルタイム顧客プロファイルで使用できるセグメントは、セグメント定義内にマージポリシーIDを含みます。 このマージポリシーには、セグメントに含めるデータセットに関する情報が含まれ、そのデータセットには該当するデータ使用ラベルが含まれます。 オーディエンスセグメントに対するデータ使用量の準拠の適用に関する具体的な手順については、セグメントの [データ使用量準拠の実施チュートリアルに従ってください](../segmentation/tutorials/governance.md)。

## プライバシーサービスの概要

プライバシーサービスは、RESTful APIとユーザーインターフェイスを提供し、Adobe Experience Cloudアプリケーション全体でデータサブジェクト（顧客）の個人データを管理できます。 また、プライバシーサービスは、Experience Cloudアプリケーションに関連するジョブのステータスと結果にアクセスできる、中央の監査とログのメカニズムも提供します。 プライバシーサービスジョブの作成および監視方法の手順については、 [プライバシーサービス開発者ガイド](../privacy-service/api/getting-started.md) または [プライバシーサービスユーザーガイドに記載されている手順に従ってください](../privacy-service/ui/overview.md)。