---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: データガバナンスとプライバシーのチュートリアル
topic: tutorial
translation-type: tm+mt
source-git-commit: ee08f43400fa72abce95ed52aff879f954f4b4d6

---


# データガバナンスとプライバシーのチュートリアル

Data Usage Labeling and Enforcement(DULE)は、Adobe Experience Platform Data Governanceの中核的なメカニズムです。 DULE機能を使用すると、データセットとフィールドにデータ使用ラベルを適用し、関連するデータ使用ポリシーに従って各データを分類できます。 ラベルの使用を開始する前に、「 [Data Governance overview](../data-governance/home.md) 」を参照して、プラットフォーム内のDULEフレームワークのより強力な紹介を確認してください。

Adobe Experience Platform Privacy Serviceは、様々なソリューション間でプライバシーとコンプライアンスの要求を調整できるRESTful APIおよびユーザーインターフェイスを提供します。 詳しくは、プライバシーサービスの概要を参照し [てください](../privacy-service/home.md)。

## データ追加使用ラベル

データ使用ラベルを使用すると、データに適用される使用ポリシーに従ってデータセットとフィールドを分類できます。 ラベルはいつでも適用でき、データの管理方法を柔軟に選択できます。 ベストプラクティスは、データがエクスペリエンスプラットフォームに取り込まれるとすぐに、またはデータがプラットフォームで使用できるようになるとすぐに、データのラベル付けを推奨します。 データセットレベルで適用されるデータ使用量ラベルは、データセット内のすべてのフィールドに反映されます。 ラベルは、データセット内の個々のフィールド（列ヘッダー）に、伝播せずに直接適用することもできます。 データ使用ラベルをデータに適用する方法については、データ使用ラベルの概 [要を参照してください](../data-governance/labels/overview.md)。

## データ使用ポリシーの作成

DULE Policy Service APIを使用すると、DULEポリシーを作成および管理して、特定のDULEラベルを含むデータに対して実行できるマーケティングアクションを決定できます。 開始する前に、データ使用ポリシーの概 [要をお読みください](../data-governance/policies/overview.md)。

## データ使用ポリシーの適用

データのData Usage Labeling and Enforcement(DULE)ラベルを作成し、これらのラベルに対するマーケティングアクションのDULEポリシーを作成したら、DULE Policy Service APIを使用して、マーケティングアクションがデータセットまたはDULEラベルの任意のグループのポリシー違反を構成します。 その後、API応答に基づいてポリシー違反を処理する独自の内部プロトコルを設定できます。 開始するには、ポリシー実施の概 [要にアクセスします](../data-governance/enforcement/overview.md)。

## データセグメントのデータ使用コンプライアンスのオーディエンスを実施する

リアルタイム顧客プロファイルで使用できるセグメントには、セグメント定義内にマージポリシーIDが含まれます。 このマージポリシーには、セグメントに含めるデータセットに関する情報が含まれ、さらに該当するデータ使用ラベルも含まれます。 オーディエンスセグメントのデータ使用法の遵守の実施に関する具体的な手順については、セグメントのデータ使用法 [準拠の実施チュートリアルに従ってくださ](../segmentation/tutorials/governance.md)い。

## プライバシーサービスの概要

プライバシーサービスは、Adobe Experience Cloudアプリケーション全体でデータの主題（顧客）の個人データを管理するRESTful APIとユーザーインターフェイスを提供します。 また、プライバシーサービスは、Experience Cloudアプリケーションに関連するジョブのステータスと結果にアクセスできる、中央の監査とログのメカニズムも提供します。 プライバシーサービスジョブの作成および監視方法については、プライバシーサービス開発者ガイドまたは [プライバシーサービス](../privacy-service/api/getting-started.md) (英語のみ [)の](../privacy-service/ui/overview.md)手順に従ってください。