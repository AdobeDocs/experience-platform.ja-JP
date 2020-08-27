---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Policy Service API開発者ガイド
topic: developer guide
translation-type: tm+mt
source-git-commit: 71678b10c9e137016ea404305b272508b9c8cabe
workflow-type: tm+mt
source-wordcount: '472'
ht-degree: 15%

---


# [!DNL Policy Service] API開発者ガイド

Adobe Experience Platform [!DNL Data Governance] allows you to manage customer data and ensure compliance with regulations, restrictions, and policies applicable to data use. It plays a key role within [!DNL Experience Platform] at various levels, including cataloging, data lineage, data usage labeling, data usage policies, and controlling usage of data for marketing actions.

この [!DNL Policy Service] APIは複数のエンドポイントを提供し、データ使用ラベルとポリシーをプログラム的に管理したり、ポリシー違反のマーケティングアクションを評価したりできます。 これらのエンドポイントの概要を以下に示します。 詳しくは、個々のエンドポイントのガイドを参照してください。必要なヘッダーに関する重要な情報、サンプルAPI呼び出しの読み方などについては、 [はじめに](./getting-started.md) 「はじめに」のガイドを参照してください。

使用可能なすべてのエンドポイントとCRUD操作を表示するには、 [[!DNL Policy Service] APIスウォッガーにアクセスします](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/dule-policy-service.yaml)。

## ラベル

データ使用状況ラベルを使用すると、データに適用される使用ポリシーに従ってデータセットを分類できます。ラベルはいつでも適用でき、データの管理方法を柔軟に選択できます。Best practices encourage labeling data as soon as it is ingested into [!DNL Experience Platform], or as soon as data becomes available for use in [!DNL Platform]. 端点を使用して、ラベルの作成、表示、編集および削除を行うことができ `/labels` ます。 このエンドポイントの使用方法については、『 [ラベルエンドポイントガイド](./labels.md)』を参照してください。

## マーケティングアクション

マーケティングアクション （マーケティングの使用例とも呼ばれます） [!DNL Data Governance] フレームワークの観点から見ると、 [!DNL Experience Platform] データ消費者が行うことのできるアクションで、組織がデータの使用を制限したいと考えています。 マーケティングアクションの操作について詳しくは、『マー [ケティングアクションエンドポイントガイド](./marketing-actions.md)』を参照してください。

## ポリシー

Data usage policies are rules that describe the kinds of marketing actions that you are allowed to, or restricted from, performing on data within [!DNL Experience Platform]. ポリシーは次の方法で定義します。

1. 特定のマーケティングアクション
1. アクションの実行が制限されているデータ使用ラベル

APIでポリシーを管理する方法については、ポリシーエンドポイントガイドを参照して [ください](./policies.md)

## 評価

Once data usage labels have been applied to [!DNL Platform] datasets, and data usage policies have been defined for marketing actions against those labels, Data Governance capabilities allow you to enforce those policies and prevent data operations that constitute policy violations.

The [!DNL Policy Service] API provides endpoints that allow you to test marketing actions against datasets or arbitrary combinations of data usage labels in order to check if any policy violations occur. その後、API 応答に基づいて、エクスペリエンスアプリケーション内でプロトコルを設定し、データ使用ポリシーのコンプライアンスを適切に実施できます。See the [evaluation endpoints guide](./evaluation.md) for more information.

## 次の手順

APIを使用した呼び出しの実行を開始するには、「 [!DNL Policy Service] はじめに [](./getting-started.md) 」ガイドを読み、特定のエンドポイントの使用方法を学ぶためのエンドポイントガイドの1つを選択します。 UIを使用してラベルとポリシーを操作するには、『 [!DNL Experience Platform] labels user guide [』と『](../labels/user-guide.md) policies user guide [](../policies/user-guide.md)』をそれぞれ参照してください。