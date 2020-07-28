---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: データ使用ラベルの概要
topic: labels
translation-type: tm+mt
source-git-commit: 0534fe8dcc11741ddc74749d231e732163adf5b0
workflow-type: tm+mt
source-wordcount: '485'
ht-degree: 30%

---


# データ使用ラベルの概要

Data Usage Labeling and Enforcement (DULE) is the core mechanism of Adobe Experience Platform [!DNL Data Governance]. DULE 機能を使用すると、データセットとフィールドにデータ使用ラベルを付けて、関連するデータ使用ポリシーに従って各データを分類できます。

This document provides an overview of data usage labels (also known as DULE labels) in [!DNL Experience Platform]. DULE フレームワークのより詳しい紹介については、このガイドを読む前に、[データガバナンスの概要](../home.md)を参照してください。

## データ使用ラベルについて

データ使用状況ラベルを使用すると、データに適用される使用ポリシーに従ってデータセットとフィールドを分類できます。ラベルはいつでも適用でき、データの管理方法を柔軟に選択できます。Best practices encourage labeling data as soon as it is ingested into [!DNL Experience Platform], or as soon as data becomes available for use in [!DNL Platform].

データ使用ラベルがデータセットレベルで適用されると、そのデータセット内のすべてのフィールドにラベルが伝播されます。ラベルは、伝播ではなく、データセット内の個々のフィールド（列ヘッダー）に直接適用することもできます。

[!DNL Platform] は、「コア」データ使用ラベルを標準搭載で提供しています。これは、データ管理に適用される様々な一般的な制限をカバーしています。 これらのラベルとそれらが表す使用ポリシーについて詳しくは、 [コアデータ使用ラベルのガイドを参照してください](reference.md)。

Adobeが提供するラベルに加えて、独自のカスタムラベルを定義することもできます。 For steps on how to do this in the UI, see the [data usage labels user guide](./user-guide.md). API呼び出しを使用してこれを実行する手順については、『 [data usage labels API』ガイドを参照してください](./api.md)。

## オーディエンスセグメントのラベルの継承

[Adobe Experience Platformセグメントサービスで作成されたすべてのオーディエンスセグメントは](../../segmentation/home.md) 、対応するデータセットの使用ラベルを継承します。 これにより、 [!DNL Experience Platform] (など [!DNL Real-time Customer Data Platform])の上に構築されたアプリケーションで、宛先に対するセグメントをアクティブ化する際に、自動的にデータ使用ポリシーが適用されます。

データセットレベルのラベルを継承する以外に、デフォルトでは、セグメントは関連するデータセットからフィールドレベルのラベルをすべて継承します。 ベースのアプリケーションがセグメントをどのように消費するかに応じて、使用するフィールドを指定できるので、除外されたフィールドのラベルをセグメントが継承できない場合があります。 [!DNL Platform]

Real-time CDPでの自動強制の動作方法の詳細については、 [AdobeReal-time CDP Data Governanceの概要を参照してください](../../rtcdp/privacy/data-governance-overview.md#enforce-data-usage-compliance)。

### Adobe Audience Managerデータエクスポートコントロールからの継承

[!DNL Experience Platform] には、Adobe Audience Managerとセグメントを共有する機能があります。 Audience Managerセグメントに適用されたデータエクスポートコントロールは、で認識される同等のラベルおよびマーケティングアクションに変換され [!DNL Experience Platform][!DNL Data Governance]ます。

特定のデータエクスポートコントロールとのデータ使用ラベルとの対応付けにつ [!DNL Platform]いては、 [Audience Managerのドキュメントを参照してください](https://docs.adobe.com/content/help/en/audience-manager/user-guide/implementation-integration-guides/integration-experience-platform/aam-aep-audience-sharing.html#aam-data-export-control-in-aep)。


## 次の手順

これで、データ使用ラベルの概要がわかったので、引き続き[ユーザーガイド](user-guide.md)を読んで、 UI でラベルを管理する方法を学ぶことができます。[!DNL Experience Platform]APIを使用してラベルを管理する手順については、『 [使用ラベルAPIガイド](./api.md)』を参照してください。