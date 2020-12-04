---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: データガバナンスとプライバシーのチュートリアル
topic: tutorial
type: Tutorial
description: このドキュメントでは、Adobe Experience PlatformデータガバナンスとAdobe Experience Platform Privacy Serviceに関する各種チュートリアルの概要を説明します。
translation-type: tm+mt
source-git-commit: 97dfd3a9a66fe2ae82cec8954066bdf3b6346830
workflow-type: tm+mt
source-wordcount: '496'
ht-degree: 51%

---


# [!DNL Data Governance] および [!DNL Privacy] Tutorials

Adobe Experience Platform・データ・ガバナンスを使用すると、データセットやフィールドにデータ使用ラベルを適用し、関連するデータ使用ポリシーに従って各項目を分類し、それらのデータセットやフィールドに対して特定のアクションが実行された場合にポリシー違反を評価できます。 このドキュメントに示すチュートリアルを使い始める前に、フレームワークのより強力な概要について [[!DNL Data Governance] 概要](../data-governance/home.md) を参照してください。

Adobe Experience Platform [!DNL Privacy Service] provides a RESTful API and user interface that allow you to coordinate privacy and compliance requests across various solutions. 詳しくは、「[Privacy Service の概要](../privacy-service/home.md)」を参照してください。

## データ使用ラベルの追加

データ使用状況ラベルを使用すると、データに適用される使用ポリシーに従ってデータセットを分類できます。ラベルはいつでも適用でき、データの管理方法を柔軟に選択できます。Best practices encourage labeling data as soon as it is ingested into [!DNL Experience Platform], or as soon as data becomes available for use in [!DNL Platform]. データセットレベルで適用されるデータ使用量ラベルは、データセット内のすべてのフィールドに反映されます。ラベルは、データセット内の個々のフィールド（列ヘッダー）に、伝播せずに直接適用することもできます。データ使用ラベルをデータに適用する方法については、「[データ使用ラベルの概要](../data-governance/labels/overview.md)」を参照してください。

## データ使用ポリシーの作成

The [!DNL Policy Service] API allows you to create and manage data usage policies to determine what marketing actions can be taken against data that contains certain usage labels. 開始する前に、「[データ使用ポリシーの概要](../data-governance/policies/overview.md)」をお読みください。

## データ使用ポリシーの作成

データに使用ラベルを追加し、それらのラベルに対するマーケティングアクションに関するポリシーを作成したら、を使用して、データセットまたは使用ラベルの任意のグループに対してマーケティングアクションがポリシー違反と見なされるかを評価できます。 [!DNL Policy Service API] その後、API 応答に基づいてポリシー違反を処理する独自の内部プロトコルを設定できます。開始するには、「[ポリシー実施の概要](../data-governance/enforcement/overview.md)」にアクセスします。

## データセグメントのデータ使用コンプライアンスのオーディエンスを実施する

Segments that are enabled for use in [!DNL Real-time Customer Profile] contain a merge policy ID within their segment definition. この結合ポリシーには、セグメントに含めるデータセットに関する情報があります。さらに、データセットには適用可能なデータ使用ラベルが含まれています。オーディエンスセグメントでデータ使用のコンプライアンスを徹底するための具体的な手順については、[セグメントでデータ使用のコンプライアンスを徹底するためのチュートリアル](../segmentation/tutorials/governance.md)を参照してください。

## 使用の手引き [!DNL Privacy Service]

[!DNL Privacy Service] は、Adobe Experience Cloud アプリケーション全体でデータのサブジェクト（顧客）の個人データを管理する RESTful API とユーザーインターフェイスを提供します。[!DNL Privacy Service]また、 は、 アプリケーションに関連するジョブのステータスと結果にアクセスできる、中央監査とログのメカニズムも提供します。[!DNL Experience Cloud]For instructions showing how to create and monitor [!DNL Privacy Service] jobs, follow the steps provided in the [Privacy Service developer guide](../privacy-service/api/getting-started.md) or the [Privacy Service user guide](../privacy-service/ui/overview.md).