---
keywords: Experience Platform；ホーム；人気の高いトピック
solution: Experience Platform
title: Policy Service APIガイド
topic-legacy: developer guide
description: Policy Service APIを使用すると、開発者はExperience Platformでデータ使用ラベルとポリシーを管理できます。 このガイドに従って、APIを使用した主な操作の実行方法を学習します。
exl-id: 23c05670-7107-4b96-bc24-0a51b5d267b2
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '504'
ht-degree: 15%

---

# [!DNL Policy Service] API ガイド

Adobe Experience Platform[!DNL Data Governance]では、顧客データを管理し、データの使用に適した規制、制限、ポリシーの遵守を確保できます。 [!DNL Experience Platform]内では、カタログ化、データ系列、データ使用のラベル付け、データ使用ポリシー、マーケティング活動のデータ使用の制御など、様々なレベルで重要な役割を果たします。

[!DNL Policy Service] APIは複数のエンドポイントを提供し、データ使用ラベルとポリシーをプログラム的に管理したり、ポリシー違反のマーケティングアクションを評価したりできます。 これらのエンドポイントの概要を以下に示します。 詳しくは、個々のエンドポイントのガイドを参照し、必要なヘッダー、サンプルAPI呼び出しの読み取りなどに関する重要な情報については、[はじめに](./getting-started.md)を参照してください。

使用可能なすべてのエンドポイントとCRUD操作を表示するには、[[!DNL Policy Service] APIスウォッガー](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/dule-policy-service.yaml)を参照してください。

## ラベル

データ使用状況ラベルを使用すると、データに適用される使用ポリシーに従ってデータセットを分類できます。ラベルはいつでも適用でき、データの管理方法を柔軟に選択できます。ベストプラクティスは、データが[!DNL Experience Platform]に取り込まれるとすぐに、またはデータが[!DNL Platform]で使用可能になるとすぐに、データのラベル付けを推奨します。 `/labels`エンドポイントを使用して、ラベルの作成、表示、編集および削除を行うことができます。 このエンドポイントの使用方法については、[ラベルエンドポイントガイド](./labels.md)を参照してください。

## マーケティングアクション

マーケティングアクション （マーケティングの使用例とも呼ばれます）[!DNL Data Governance]フレームワークに関しては、[!DNL Experience Platform]データコンシューマーが行うことのできるアクションで、組織でデータの使用を制限したいと考えています。 マーケティングアクションの操作について詳しくは、[マーケティングアクションエンドポイントガイド](./marketing-actions.md)を参照してください。

## ポリシー

データ使用ポリシーとは、[!DNL Experience Platform]内のデータに対して実行を許可（制限）されるマーケティングアクションの種類を記述するルールです。 ポリシーは次の方法で定義します。

1. 特定のマーケティングアクション
1. アクションの実行が制限されているデータ使用ラベル

APIでポリシーを管理する方法については、[ポリシーエンドポイントガイド](./policies.md)を参照してください

## 評価

[!DNL Platform]データセットにデータ使用ラベルが適用され、これらのラベルに対するマーケティングアクションに対してデータ使用ポリシーが定義されると、Data Governance機能により、これらのポリシーを適用し、ポリシー違反を構成するデータ操作を防ぐことができます。

[!DNL Policy Service] APIはエンドポイントを提供します。エンドポイントを使用すると、データセットやデータ使用ラベルの任意の組み合わせに対するマーケティング操作をテストし、ポリシー違反が発生したかどうかを確認できます。 その後、API 応答に基づいて、エクスペリエンスアプリケーション内でプロトコルを設定し、データ使用ポリシーのコンプライアンスを適切に実施できます。詳しくは、[評価用エンドポイントガイド](./evaluation.md)を参照してください。

## 次の手順

[!DNL Policy Service] APIを使用して呼び出しを開始するには、[はじめに](./getting-started.md)を読み、特定のエンドポイントの使用方法を学ぶためのエンドポイントガイドの1つを選択します。 [!DNL Experience Platform] UIを使用してラベルとポリシーを操作するには、[labels user guide](../labels/user-guide.md)と[policies user guide](../policies/user-guide.md)をそれぞれ参照してください。
