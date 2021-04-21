---
keywords: Experience Platform；ホーム；人気の高いトピック
solution: Experience Platform
title: データガバナンスとプライバシーのチュートリアル
topic-legacy: tutorial
type: Tutorial
description: このドキュメントでは、Adobe Experience PlatformデータガバナンスとAdobe Experience Platform Privacy Serviceに関する各種チュートリアルの概要を説明します。
exl-id: c3cef447-b343-445b-a3ed-54f873f6dfb9
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '501'
ht-degree: 50%

---

# [!DNL Data Governance] および [!DNL Privacy] Tutorials

Adobe Experience Platform・データ・ガバナンスを使用すると、データセットやフィールドにデータ使用ラベルを適用し、関連するデータ使用ポリシーに従って各項目を分類し、それらのデータセットやフィールドに対して特定のアクションが実行された場合にポリシー違反を評価できます。 このドキュメントに示すチュートリアルを使い始める前に、[[!DNL Data Governance] 概要](../data-governance/home.md)を参照して、フレームワークのより強力な紹介を確認してください。

Adobe Experience Platform[!DNL Privacy Service]は、RESTful APIとユーザーインターフェイスを提供し、各種ソリューション間でプライバシーとコンプライアンスの要求を調整できます。 詳しくは、「[Privacy Service の概要](../privacy-service/home.md)」を参照してください。

## データ使用ラベルの追加

データ使用状況ラベルを使用すると、データに適用される使用ポリシーに従ってデータセットを分類できます。ラベルはいつでも適用でき、データの管理方法を柔軟に選択できます。ベストプラクティスは、データが[!DNL Experience Platform]に取り込まれるとすぐに、またはデータが[!DNL Platform]で使用可能になるとすぐに、データのラベル付けを推奨します。 データセットレベルで適用されるデータ使用量ラベルは、データセット内のすべてのフィールドに反映されます。ラベルは、データセット内の個々のフィールド（列ヘッダー）に、伝播せずに直接適用することもできます。データ使用ラベルをデータに適用する方法については、「[データ使用ラベルの概要](../data-governance/labels/overview.md)」を参照してください。

## データ使用ポリシーの作成

[!DNL Policy Service] APIを使用すると、データ使用ポリシーを作成および管理して、特定の使用ラベルを含むデータに対して実行できるマーケティングアクションを決定できます。 開始する前に、「[データ使用ポリシーの概要](../data-governance/policies/overview.md)」をお読みください。

## データ使用ポリシーの作成

データに使用ラベルを追加し、それらのラベルに対するマーケティングアクションに関するポリシーを作成したら、[!DNL Policy Service API]を使用して、マーケティングアクションがデータセットまたは任意の使用ラベルのグループに対して実行されるポリシー違反と見なすかを評価できます。 その後、API 応答に基づいてポリシー違反を処理する独自の内部プロトコルを設定できます。開始するには、「[ポリシー実施の概要](../data-governance/enforcement/overview.md)」にアクセスします。

## データセグメントのデータ使用コンプライアンスのオーディエンスを実施する

[!DNL Real-time Customer Profile]で使用できるセグメントは、セグメント定義内にマージポリシーIDを含みます。 この結合ポリシーには、セグメントに含めるデータセットに関する情報があります。さらに、データセットには適用可能なデータ使用ラベルが含まれています。オーディエンスセグメントでデータ使用のコンプライアンスを徹底するための具体的な手順については、[セグメントでデータ使用のコンプライアンスを徹底するためのチュートリアル](../segmentation/tutorials/governance.md)を参照してください。

## 基本を学ぶ： [!DNL Privacy Service]

[!DNL Privacy Service] は、Adobe Experience Cloud アプリケーション全体でデータのサブジェクト（顧客）の個人データを管理する RESTful API とユーザーインターフェイスを提供します。[!DNL Privacy Service]また、 は、 アプリケーションに関連するジョブのステータスと結果にアクセスできる、中央監査とログのメカニズムも提供します。[!DNL Experience Cloud][!DNL Privacy Service]ジョブの作成および監視方法の手順については、[Privacy Service開発者ガイド](../privacy-service/api/getting-started.md)または[Privacy Serviceユーザーガイド](../privacy-service/ui/overview.md)に記載されている手順に従ってください。
