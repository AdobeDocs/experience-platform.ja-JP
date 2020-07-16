---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: データガバナンスとプライバシーのチュートリアル
topic: tutorial
translation-type: tm+mt
source-git-commit: 5c5f6c4868e195aef76bacc0a1e5df3857647bde
workflow-type: tm+mt
source-wordcount: '470'
ht-degree: 0%

---


# [!DNL Data Governance] および [!DNL Privacy] チュートリアル

[!DNL Data Usage Labeling and Enforcement] (DULE)は、Adobe Experience Platform [!DNL Data Governanc]eの中核的なメカニズムです。 DULE機能を使用すると、データセットとフィールドにデータ使用ラベルを適用し、関連するデータ使用ポリシーに従ってそれぞれを分類できます。 ラベルの使用を開始する前に、「 [データ管理の概要](../data-governance/home.md) 」を参照して、のDULEフレームワークのより強力な紹介を確認してくだ [!DNL Platform]さい。

Adobe Experience Platform [!DNL Privacy Service] は、RESTful APIとユーザーインターフェイスを提供し、各種ソリューション間でプライバシーとコンプライアンスの要求を調整できます。 詳しくは、 [Privacy Serviceの概要から始めてください](../privacy-service/home.md)。

## データ使用ラベル

データ使用ラベルを使用すると、データに適用される使用ポリシーに従ってデータセットとフィールドを分類できます。 ラベルはいつでも適用でき、データの管理方法を柔軟に選択できます。 ベストプラクティスでは、データがに取り込まれるとすぐに、 [!DNL Experience Platform]またはデータがで使用可能になるとすぐに、データのラベル付けを推奨 [!DNL Platform]します。 データセットレベルで適用されるデータ使用量ラベルは、データセット内のすべてのフィールドに反映されます。 ラベルは、データセット内の個々のフィールド（列ヘッダー）に、伝播せずに直接適用することもできます。 データ使用ラベルをデータに適用する方法については、 [データ使用ラベルの概要を参照してください](../data-governance/labels/overview.md)。

## データ使用ポリシーの作成

DULE [!DNL Policy Service] APIを使用すると、DULEポリシーを作成および管理して、特定のDULEラベルを含むデータに対して実行できるマーケティングアクションを決定できます。 使用を開始するには、 [データ使用ポリシーの概要を読みます](../data-governance/policies/overview.md)。

## データ使用ポリシーの適用

データ用のData Usage Labeling and Enforcement(DULE)ラベルを作成し、これらのラベルに対するマーケティングアクション用のDULEポリシーを作成したら、DULE [!DNL Policy Service] APIを使用して、マーケティングアクションがデータセット上で実行されたか、またはDULEラベルの任意のグループがポリシー違反を構成するかを評価できます。 その後、独自の内部プロトコルを設定し、API応答に基づくポリシー違反を処理できます。 開始するには、 [ポリシー実施の概要](../data-governance/enforcement/overview.md)。

## オーディエンスセグメントのデータ使用コンプライアンスの実施

で使用できるセグメントは、セグメント定義内にマージポリシーIDを [!DNL Real-time Customer Profile] 含みます。 このマージポリシーには、セグメントに含めるデータセットに関する情報が含まれ、そのデータセットには該当するデータ使用ラベルが含まれます。 オーディエンスセグメントに対するデータ使用量の準拠の適用に関する具体的な手順については、セグメントの [データ使用量準拠の実施チュートリアルに従ってください](../segmentation/tutorials/governance.md)。

## Get started with [!DNL Privacy Service]

[!DNL Privacy Service] は、RESTful APIとユーザーインターフェイスを提供し、Adobe Experience Cloudアプリケーション全体でデータサブジェクト（お客様）の個人データを管理できます。 [!DNL Privacy Service] また、中央の監査とログのメカニズムも提供します。このメカニズムを使用すると、 [!DNL Experience Cloud] アプリケーションに関連するジョブのステータスと結果にアクセスできます。 ジョブの作成および監視方法の手順については、『 [!DNL Privacy Service] Privacy Service開発者ガイド [』または『](../privacy-service/api/getting-started.md) Privacy Serviceユーザガイド [](../privacy-service/ui/overview.md)』に記載されている手順に従ってください。