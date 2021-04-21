---
keywords: Experience Platform；ホーム；人気の高いトピック；データガバナンス；データ使用ラベルapi；ポリシーサービスapi；データ使用ラベルの概要
solution: Experience Platform
title: データ使用量ラベルの概要
topic-legacy: labels
description: Adobe Experience Platformデータガバナンスを使用すると、データセットとフィールドにデータ使用ラベルを適用し、関連するデータ使用ポリシーに従ってそれぞれを分類できます。 このドキュメントでは、Experience Platformでのデータ使用ラベルの概要を説明します。
exl-id: 4f113000-b9a1-4dfb-9502-6a5d08f0b26f
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '619'
ht-degree: 14%

---

# データ使用ラベルの概要

Adobe Experience Platform[!DNL Data Governance]では、データセットとフィールドにデータ使用ラベルを適用し、関連するデータ使用ポリシーに従って各データを分類できます。

このドキュメントでは、[!DNL Experience Platform]のデータ使用ラベルの概要を説明します。 このガイドを読む前に、Data Governanceのフレームワークのより強力な紹介について、[Data Governance overview](../home.md)をご覧ください。

## データ使用ラベルについて

データ使用状況ラベルを使用すると、データに適用される使用ポリシーに従ってデータセットとフィールドを分類できます。ラベルはいつでも適用でき、データの管理方法を柔軟に選択できます。ベストプラクティスは、データが[!DNL Experience Platform]に取り込まれるとすぐに、またはデータが[!DNL Platform]で使用可能になるとすぐに、データのラベル付けを推奨します。

データ使用ラベルがデータセットレベルで適用されると、そのデータセット内のすべてのフィールドにラベルが伝播されます。ラベルは、伝播ではなく、データセット内の個々のフィールド（列ヘッダー）に直接適用することもできます。

[!DNL Platform] は、「コア」データ使用ラベルを標準搭載で提供しています。これは、データ管理に適用される様々な一般的な制限をカバーしています。これらのラベルと使用ポリシーについて詳しくは、[コアデータ使用ラベル](reference.md)のガイドを参照してください。

Adobeが提供するラベルに加えて、組織に独自のカスタムラベルを定義することもできます。 詳しくは、[ラベルの管理](#manage-labels)の節を参照してください。

## オーディエンスセグメントのラベルの継承

[Adobe Experience Platformセグメントサービス](../../segmentation/home.md)で作成されたすべてのオーディエンスセグメントは、対応するデータセットの使用ラベルを継承します。 これにより、Experience Platformは、宛先に対するセグメントをアクティブ化する際に、データ使用ポリシーの自動適用を提供できます。

データセットレベルのラベルを継承する以外に、デフォルトでは、セグメントは関連するデータセットからフィールドレベルのラベルをすべて継承します。 したがって、セグメントから除外する属性をより簡単に識別でき、除外するフィールドのラベルを継承しないようにすることができます。

プラットフォームでの自動強制の動作方法について詳しくは、[自動ポリシー適用](../enforcement/auto-enforcement.md)の概要を参照してください。

### Adobe Audience Managerデータエクスポート管理からの継承

[!DNL Experience Platform] には、Adobe Audience Managerとセグメントを共有する機能があります。Audience Managerセグメントに適用されたデータエクスポートコントロールは、[!DNL Experience Platform] [!DNL Data Governance]が認識する同等のラベルおよびマーケティングアクションに変換されます。

特定のデータエクスポートコントロールが[!DNL Platform]のデータ使用ラベルにどのようにマッピングされるかについては、[Audience Managerドキュメント](https://experienceleague.adobe.com/docs/audience-manager/user-guide/implementation-integration-guides/integration-experience-platform/aam-aep-audience-sharing.html#aam-data-export-control-in-aep)を参照してください。

## [!DNL Experience Platform] {#manage-labels}でのデータ使用ラベルの管理

[!DNL Experience Platform] APIまたはユーザーインターフェイスを使用して、データ使用ラベルを管理できます。 それぞれの詳細については、以下のサブセクションを参照してください。

### UI の使用

[!DNL Experience Platform] UIの&#x200B;**[!UICONTROL ポリシー]**&#x200B;ワークスペースでは、組織のコアラベルとカスタムラベルの表示と管理が可能です。 **[!DNL Datasets]**&#x200B;ワークスペースでは、データセットとフィールドにラベルを適用できます。 詳しくは、[labels user guide](user-guide.md)を参照してください。

### APIの使用

[Policy Service API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/dule-policy-service.yaml)の`/labels`エンドポイントを使用すると、カスタムラベルの作成など、データ使用ラベルをプログラムで管理できます。 詳しくは、[labels endpoint guide](../api/labels.md)を参照してください。

[データセットサービスAPI](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/dataset-service.yaml)は、データセットとフィールドのラベルの管理に使用します。 詳しくは、[データセットラベルの管理](./dataset-api.md)のガイドを参照してください。

## 次の手順

このドキュメントでは、データ使用ラベルと、データ管理フレームワーク内でのラベルの役割について説明しました。 [!DNL Experience Platform]でのラベルの管理方法の詳細については、このガイド全体のリンク先ドキュメントを参照してください。
