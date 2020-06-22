---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: データ使用量ラベルの概要
topic: labels
translation-type: tm+mt
source-git-commit: e3c69589e0d4f8224b74a663b23f67e6731ddec4
workflow-type: tm+mt
source-wordcount: '365'
ht-degree: 0%

---


# データ使用量ラベルの概要

Data Usage Labeling and Enforcement(DULE)は、Adobe Experience Platformデータガバナンスの中核的なメカニズムです。 DULE機能を使用すると、データセットとフィールドにデータ使用ラベルを適用し、関連するデータ使用ポリシーに従ってそれぞれを分類できます。

このドキュメントでは、Experience Platformでのデータ使用ラベル（DULEラベルとも呼ばれます）の概要を説明します。 このガイドを読む前に、DULEフレームワークのより強固な概要について、 [データ・ガバナンスの概要](../home.md) （英語）を参照してください。

## データ使用量ラベルについて

データ使用ラベルを使用すると、データに適用される使用ポリシーに従ってデータセットとフィールドを分類できます。 ラベルはいつでも適用でき、データの管理方法を柔軟に選択できます。 ベストプラクティスは、データがExperience Platformに取り込まれるとすぐに、またはデータがPlatformで使用できるようになるとすぐに、データのラベル付けを推奨します。

データセットレベルで適用されるデータ使用量ラベルは、データセット内のすべてのフィールドに反映されます。 ラベルは、データセット内の個々のフィールド（列ヘッダー）に、伝播せずに直接適用することもできます。

Experience Platformで使用可能なデータ使用ラベルとそれらが表す使用ポリシーについて詳しくは、 [サポートされるデータ使用ラベルのガイドを参照してください](reference.md)。

## オーディエンスセグメントのラベルの継承

[Adobe Experience Platformセグメントサービスで作成されたすべてのオーディエンスセグメントは](../../segmentation/home.md) 、対応するデータセットの使用ラベルを継承します。 これにより、Experience Platform上に構築されたアプリケーション(リアルタイム顧客データPlatformなど)で、宛先に対するセグメントをアクティブ化する際に、自動的にデータ使用ポリシーの適用を提供できます。

データセットレベルのラベルを継承する以外に、デフォルトでは、セグメントは関連するデータセットからフィールドレベルのラベルをすべて継承します。 Platformベースのアプリケーションがセグメントをどのように使用するかに応じて、使用するフィールドを指定できるので、セグメントが除外されたフィールドのラベルを継承できない場合があります。

リアルタイムCDPでの自動強制の動作方法の詳細については、「 [Real-time CDP Data Governance overview](../../rtcdp/privacy/data-governance-overview.md#enforce-data-usage-compliance)」を参照してください。

<!-- (Add after DEC mapping reference is added to AAM docs to link out to)
### Inheritance from Adobe Audience Manager Data Export Controls

Experience Platform has the ability to share segments with Adobe Audience Manager. Any Data Export Controls that have been applied to Audience Manager segments are translated to equivalent labels and marketing actions recognized by Experience Platform Data Governance.

For a reference on how specific Data Export Controls map to data usage labels in Platform, please refer to the [Audience Manager documentation](https://docs.adobe.com/content/help/en/audience-manager/user-guide/features/data-export-controls.html).
-->

## 次の手順

データ使用ラベルが導入されたので、 [Experience PlatformUIでのラベルの管理方法については、引き続き](user-guide.md) ユーザガイドを参照してください。 APIを使用してラベルを管理する手順については、 [Catalog Service開発者ガイドの該当する節を参照してください](../../catalog/api/labels.md)。