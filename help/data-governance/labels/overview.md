---
keywords: Experience Platform;ホーム;人気のピック;データガバナンス;Data Usage Label API;Policy Service API;データ使用ラベルの概要
solution: Experience Platform
title: データ使用ラベルの概要
topic-legacy: labels
description: Adobe Experience Platform データガバナンスを使用すると、データセットとフィールドにデータ使用ラベルを適用し、関連するデータ使用ポリシーに従って各データを分類できます。このドキュメントでは、Experience Platform でのデータ使用ラベルの概要を説明します。
exl-id: 4f113000-b9a1-4dfb-9502-6a5d08f0b26f
source-git-commit: 937225ff08e2e02c5840f86d6ed50644e05bdfe5
workflow-type: tm+mt
source-wordcount: '611'
ht-degree: 98%

---

# データ使用ラベルの概要

Adobe Experience Platform [!DNL Data Governance]を使用すると、データセットとフィールドにデータ使用ラベルを付けて、関連するデータ使用ポリシーに従って各データを分類できます。

このドキュメントでは、[!DNL Experience Platform] のデータ使用ラベルの概要を説明します。データガバナンスフレームワークのより詳しい説明については、このガイドを読む前に、[データガバナンスの概要](../home.md)を参照してください。

## データ使用ラベルについて

データ使用状況ラベルを使用すると、データに適用される使用ポリシーに従ってデータセットとフィールドを分類できます。ラベルはいつでも適用でき、データの管理方法を柔軟に選択できます。ベストプラクティスでは、データが [!DNL Experience Platform] に取得されるとすぐに、またはデータが [!DNL Platform] で使用できるようになるとすぐに、データのラベル付けが推奨されます。

データ使用ラベルがデータセットレベルで適用されると、そのデータセット内のすべてのフィールドにラベルが伝播されます。ラベルは、伝播ではなく、データセット内の個々のフィールド（列ヘッダー）に直接適用することもできます。

[!DNL Platform] は、「コア」データ使用ラベルを標準搭載で提供しています。これは、データガバナンスに適用される様々な一般的な制限をカバーしています。これらのラベルと使用ポリシーについて詳しくは、[コアデータ使用ラベル](reference.md)のガイドを参照してください。

アドビが提供するラベルに加えて、組織に独自のカスタムラベルを定義することもできます。詳しくは、[ラベルの管理](#manage-labels)のセクションを参照してください。

## オーディエンスセグメントのラベルの継承

[Adobe Experience Platform セグメント化サービス](../../segmentation/home.md)で作成されたすべてのオーディエンスセグメントは、対応するデータセットの使用ラベルを継承します。これにより、Experience Platform は、宛先に対してセグメントをアクティブ化する際に、データ使用ポリシーを自動で実行できます。

データセットレベルのラベルを継承する以外に、デフォルトでは、セグメントは関連するデータセットからフィールドレベルのラベルをすべて継承します。したがって、セグメントから除外する属性をより容易に識別し、除外するフィールドのラベルを継承しないようにすることができます。

Platform での自動適用の動作方法について詳しくは、[自動ポリシーの適用](../enforcement/auto-enforcement.md)の概要を参照してください。

### Adobe Audience Manager データ書き出しのコントロールからの継承

[!DNL Experience Platform] には、Adobe Audience Manager とセグメントを共有する機能があります。Audience Manager セグメントに適用されたデータエクスポートのコントロールは、[!DNL Experience Platform] [!DNL Data Governance] が認識する同等のラベルおよびマーケティングアクションに変換されます。

特定のデータ書き出しのコントロールが [!DNL Platform] のデータ使用ラベルにどのようにマッピングされるかについては、[Audience Manager ドキュメント](https://experienceleague.adobe.com/docs/audience-manager/user-guide/implementation-integration-guides/integration-experience-platform/aam-aep-audience-sharing.html?lang=ja#aam-data-export-control-in-aep)を参照してください。

## でのデータ使用ラベルの管理 [!DNL Experience Platform] {#manage-labels}

[!DNL Experience Platform] API またはユーザーインターフェイスを使用して、データ使用ラベルを管理できます。それぞれの詳細については、以下のサブセクションを参照してください。

### UI の使用

[!DNL Experience Platform] UI の&#x200B;**[!UICONTROL ポリシー]**&#x200B;ワークスペースでは、組織のコアとカスタムラベルの表示と管理が可能です。**[!DNL Datasets]** ワークスペースでは、データセットとフィールドにラベルを適用できます。詳しくは、『[ラベルのユーザガイド](user-guide.md)』を参照してください。

### API の使用

[Policy Service API](https://www.adobe.io/experience-platform-apis/references/policy-service/) の `/labels` エンドポイントを使用すると、カスタムラベルの作成など、データ使用ラベルをプログラムに従って管理できます。詳しくは、『[ラベルのエンドポイントガイド](../api/labels.md)』を参照してください。

[データセットサービス API ](https://www.adobe.io/experience-platform-apis/references/dataset-service/)は、データセットとフィールドのラベルの管理に使用します。詳しくは、[データセットラベルの管理](./dataset-api.md)のガイドを参照してください。

## 次の手順

このドキュメントでは、データ使用ラベルと、データガバナンスフレームワーク内でのラベルの役割について説明しました。[!DNL Experience Platform] でのラベルの管理方法の詳細については、このガイドを通じてリンクされているドキュメントを参照してください。
