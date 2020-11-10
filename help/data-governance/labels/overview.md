---
keywords: Experience Platform;home;popular topics;data governance;data usage label api;policy service api;data usage labels overview
solution: Experience Platform
title: データ使用ラベルの概要
topic: labels
description: Adobe Experience Platformデータガバナンスを使用すると、データセットとフィールドにデータ使用ラベルを適用し、関連するデータ使用ポリシーに従ってそれぞれを分類できます。 このドキュメントでは、Experience Platformでのデータ使用ラベルの概要を説明します。
translation-type: tm+mt
source-git-commit: f86f7483e7e78edf106ddd34dc825389dadae26a
workflow-type: tm+mt
source-wordcount: '616'
ht-degree: 15%

---


# データ使用ラベルの概要

Adobe Experience Platform [!DNL Data Governance] allows you to apply data usage labels to datasets and fields, categorizing each according to related data usage policies.

このドキュメントでは、のデータ使用ラベルの概要を説明 [!DNL Experience Platform]します。 Before reading this guide, please see the [Data Governance overview](../home.md) for a more robust introduction to the Data Governance framework.

## データ使用ラベルについて

データ使用状況ラベルを使用すると、データに適用される使用ポリシーに従ってデータセットとフィールドを分類できます。ラベルはいつでも適用でき、データの管理方法を柔軟に選択できます。Best practices encourage labeling data as soon as it is ingested into [!DNL Experience Platform], or as soon as data becomes available for use in [!DNL Platform].

データ使用ラベルがデータセットレベルで適用されると、そのデータセット内のすべてのフィールドにラベルが伝播されます。ラベルは、伝播ではなく、データセット内の個々のフィールド（列ヘッダー）に直接適用することもできます。

[!DNL Platform] は、「コア」データ使用ラベルを標準搭載で提供しています。これは、データ管理に適用される様々な一般的な制限をカバーしています。 これらのラベルとそれらが表す使用ポリシーについて詳しくは、 [コアデータ使用ラベルのガイドを参照してください](reference.md)。

Adobeが提供するラベルに加えて、組織に独自のカスタムラベルを定義することもできます。 詳細は、「ラベルの [管理](#manage-labels) 」の節を参照してください。

## オーディエンスセグメントのラベルの継承

[Adobe Experience Platformセグメントサービスで作成されたすべてのオーディエンスセグメントは](../../segmentation/home.md) 、対応するデータセットの使用ラベルを継承します。 これにより、Experience Platform上に構築されたアプリケーション(例： [!DNL Real-time Customer Data Platform])で、宛先に対するセグメントをアクティブ化する際に、データ使用ポリシーの自動適用を提供できます。

データセットレベルのラベルを継承する以外に、デフォルトでは、セグメントは関連するデータセットからフィールドレベルのラベルをすべて継承します。 ベースのアプリケーションがセグメントをどのように消費するかに応じて、使用するフィールドを指定できるので、除外されたフィールドのラベルをセグメントが継承できない場合があります。 [!DNL Platform]

リアルタイムCDPでの自動強制の動作方法の詳細については、Real-time CDPでの [Data Governanceの概要を参照してください](../../rtcdp/privacy/data-governance-overview.md#enforce-data-usage-compliance)。

### Adobe Audience Managerデータエクスポート管理からの継承

[!DNL Experience Platform] には、Adobe Audience Managerとセグメントを共有する機能があります。 Audience Managerセグメントに適用されたデータエクスポートコントロールは、で認識される同等のラベルおよびマーケティングアクションに変換され [!DNL Experience Platform][!DNL Data Governance]ます。

特定のデータエクスポートコントロールとのデータ使用ラベルとの対応付けにつ [!DNL Platform]いては、 [Audience Managerのドキュメントを参照してください](https://docs.adobe.com/content/help/en/audience-manager/user-guide/implementation-integration-guides/integration-experience-platform/aam-aep-audience-sharing.html#aam-data-export-control-in-aep)。

## データ使用ラベルの管理 [!DNL Experience Platform] {#manage-labels}

データ使用ラベルは、APIまたはユーザーインターフェイスを使用して管理でき [!DNL Experience Platform] ます。 それぞれの詳細については、以下のサブセクションを参照してください。

### UI の使用

UIの **[!UICONTROL ポリシー]**[!DNL Experience Platform] ・ワークスペースでは、組織のコア・ラベルとカスタム・ラベルの表示と管理が可能です。 ワークスペースでは、データセットとフィールドにラベルを適用できます。 **[!DNL Datasets]** For more information, refer to the [labels user guide](user-guide.md).

### APIの使用

`/labels` Policy Service APIの [エンドポイントを使用すると](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/dule-policy-service.yaml) 、カスタムラベルの作成を含むデータ使用ラベルをプログラムで管理できます。 詳しくは、『 [ラベルエンドポイントガイド](../api/labels.md) 』を参照してください。

データセッ [トとフィールドのラベルの管理には](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/dataset-service.yaml) 、Dataset Service APIを使用します。 詳しくは、データセットラベルの [管理に関するガイド](./dataset-api.md) を参照してください。

## 次の手順

このドキュメントでは、データ使用ラベルと、データ管理フレームワーク内でのラベルの役割について説明しました。 でのラベルの管理方法の詳細については、このガイド全体にリンクされたドキュメントを参照してくだ [!DNL Experience Platform]さい。