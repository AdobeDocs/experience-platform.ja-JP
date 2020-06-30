---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: データ使用量ラベルの概要
topic: labels
translation-type: tm+mt
source-git-commit: d4964231ee957349f666eaf6b0f5729d19c408de
workflow-type: tm+mt
source-wordcount: '351'
ht-degree: 0%

---


# データ使用量ラベルの概要

Data Usage Labeling and Enforcement(DULE)は、Adobe Experience Platformデータガバナンスの中核的なメカニズムです。 DULE機能を使用すると、データセットとフィールドにデータ使用ラベルを適用し、関連するデータ使用ポリシーに従ってそれぞれを分類できます。

このドキュメントでは、のデータ使用ラベル（DULEラベルとも呼ばれます）の概要を説明 [!DNL Experience Platform]します。 このガイドを読む前に、DULEフレームワークのより強固な概要について、 [データ・ガバナンスの概要](../home.md) （英語）を参照してください。

## データ使用量ラベルについて

データ使用ラベルを使用すると、データに適用される使用ポリシーに従ってデータセットとフィールドを分類できます。 ラベルはいつでも適用でき、データの管理方法を柔軟に選択できます。 ベストプラクティスでは、データがに取り込まれるとすぐに、 [!DNL Experience Platform]またはデータがで使用可能になるとすぐに、データのラベル付けを推奨 [!DNL Platform]します。

データセットレベルで適用されるデータ使用量ラベルは、データセット内のすべてのフィールドに反映されます。 ラベルは、データセット内の個々のフィールド（列ヘッダー）に、伝播せずに直接適用することもできます。

で使用可能なデータ使用ラベル [!DNL Experience Platform] とそれらが表す使用ポリシーについて詳しくは、 [サポートされるデータ使用ラベルのガイドを参照してください](reference.md)。

## オーディエンスセグメントのラベルの継承

[Adobe Experience Platformセグメントサービスで作成されたすべてのオーディエンスセグメントは](../../segmentation/home.md) 、対応するデータセットの使用ラベルを継承します。 これにより、 [!DNL Experience Platform] (など [!DNL Real-time Customer Data Platform])の上に構築されたアプリケーションで、宛先に対するセグメントをアクティブ化する際に、自動的にデータ使用ポリシーが適用されます。

データセットレベルのラベルを継承する以外に、デフォルトでは、セグメントは関連するデータセットからフィールドレベルのラベルをすべて継承します。 ベースのアプリケーションがセグメントをどのように消費するかに応じて、使用するフィールドを指定できるので、除外されたフィールドのラベルをセグメントが継承できない場合があります。 [!DNL Platform]

Real-time CDPでの自動強制の動作方法について詳しくは、 [Adobe Real-time CDP Data Governanceの概要を参照してください](../../rtcdp/privacy/data-governance-overview.md#enforce-data-usage-compliance)。

<!-- (Add after DEC mapping reference is added to AAM docs to link out to)
### Inheritance from Adobe Audience Manager Data Export Controls

Experience Platform has the ability to share segments with Adobe Audience Manager. Any Data Export Controls that have been applied to Audience Manager segments are translated to equivalent labels and marketing actions recognized by Experience Platform Data Governance.

For a reference on how specific Data Export Controls map to data usage labels in Platform, please refer to the [Audience Manager documentation](https://docs.adobe.com/content/help/en/audience-manager/user-guide/features/data-export-controls.html).
-->

## 次の手順

データ使用ラベルが導入されたので、 [ユーザガイドを読み、](user-guide.md)[!DNL Experience Platform] UIでラベルを管理する方法を学び続けることができます。 APIを使用してラベルを管理する手順については、 [Catalog Service開発者ガイドの該当する節を参照してください](../../catalog/api/labels.md)。