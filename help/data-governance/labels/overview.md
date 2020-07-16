---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: データ使用量ラベルの概要
topic: labels
translation-type: tm+mt
source-git-commit: 0534fe8dcc11741ddc74749d231e732163adf5b0
workflow-type: tm+mt
source-wordcount: '485'
ht-degree: 0%

---


# データ使用量ラベルの概要

Data Usage Labeling and Enforcement(DULE)は、Adobe Experience Platformの中核的なメカニズム [!DNL Data Governance]です。 DULE機能を使用すると、データセットとフィールドにデータ使用ラベルを適用し、関連するデータ使用ポリシーに従ってそれぞれを分類できます。

このドキュメントでは、のデータ使用ラベル（DULEラベルとも呼ばれます）の概要を説明 [!DNL Experience Platform]します。 このガイドを読む前に、DULEフレームワークのより強固な概要について、 [データ・ガバナンスの概要](../home.md) （英語）を参照してください。

## データ使用量ラベルについて

データ使用ラベルを使用すると、データに適用される使用ポリシーに従ってデータセットとフィールドを分類できます。 ラベルはいつでも適用でき、データの管理方法を柔軟に選択できます。 ベストプラクティスでは、データがに取り込まれるとすぐに、 [!DNL Experience Platform]またはデータがで使用可能になるとすぐに、データのラベル付けを推奨 [!DNL Platform]します。

データセットレベルで適用されるデータ使用量ラベルは、データセット内のすべてのフィールドに反映されます。 ラベルは、データセット内の個々のフィールド（列ヘッダー）に、伝播せずに直接適用することもできます。

[!DNL Platform] は、「コア」データ使用ラベルを標準搭載で提供しています。これは、データ管理に適用される様々な一般的な制限をカバーしています。 これらのラベルとそれらが表す使用ポリシーについて詳しくは、 [コアデータ使用ラベルのガイドを参照してください](reference.md)。

アドビが提供するラベルに加えて、独自のカスタムラベルを定義することもできます。 UIでこの操作を行う手順については、『 [data usage labels user guide](./user-guide.md)』を参照してください。 API呼び出しを使用してこれを実行する手順については、『 [data usage labels API』ガイドを参照してください](./api.md)。

## オーディエンスセグメントのラベルの継承

[Adobe Experience Platformセグメントサービスで作成されたすべてのオーディエンスセグメントは](../../segmentation/home.md) 、対応するデータセットの使用ラベルを継承します。 これにより、 [!DNL Experience Platform] (など [!DNL Real-time Customer Data Platform])の上に構築されたアプリケーションで、宛先に対するセグメントをアクティブ化する際に、自動的にデータ使用ポリシーが適用されます。

データセットレベルのラベルを継承する以外に、デフォルトでは、セグメントは関連するデータセットからフィールドレベルのラベルをすべて継承します。 ベースのアプリケーションがセグメントをどのように消費するかに応じて、使用するフィールドを指定できるので、除外されたフィールドのラベルをセグメントが継承できない場合があります。 [!DNL Platform]

Real-time CDPでの自動強制の動作方法について詳しくは、 [Adobe Real-time CDP Data Governanceの概要を参照してください](../../rtcdp/privacy/data-governance-overview.md#enforce-data-usage-compliance)。

### Adobe Audience Managerデータエクスポートコントロールからの継承

[!DNL Experience Platform] には、Adobe Audience Managerとセグメントを共有する機能があります。 Audience Managerセグメントに適用されたデータエクスポートコントロールは、で認識される同等のラベルおよびマーケティングアクションに変換され [!DNL Experience Platform][!DNL Data Governance]ます。

特定のデータエクスポートコントロールとのデータ使用ラベルとの対応付けにつ [!DNL Platform]いては、 [Audience Managerのドキュメントを参照してください](https://docs.adobe.com/content/help/en/audience-manager/user-guide/implementation-integration-guides/integration-experience-platform/aam-aep-audience-sharing.html#aam-data-export-control-in-aep)。


## 次の手順

データ使用ラベルが導入されたので、 [ユーザガイドを読み、](user-guide.md)[!DNL Experience Platform] UIでラベルを管理する方法を学び続けることができます。 APIを使用してラベルを管理する手順については、『 [使用ラベルAPIガイド](./api.md)』を参照してください。