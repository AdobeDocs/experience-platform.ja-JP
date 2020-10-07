---
keywords: Experience Platform;home;popular topics;DULE;dule
solution: Experience Platform
title: Adobe Experience Platform のデータガバナンス
topic: overview
description: Adobe Experience Platform データガバナンスを使用すると、顧客データを管理し、データの使用に適した規制、制限、ポリシーへのコンプライアンスを確保できます。カタログ化、データ系列、データ使用のラベル付け、データ使用ポリシー、マーケティング活動のためのデータ使用の制御など、様々なレベルでExperience Platform内の主な役割を果たします。
translation-type: tm+mt
source-git-commit: 8c94d3631296c1c3cc97501ccf1a3ed995ec3cab
workflow-type: tm+mt
source-wordcount: '1338'
ht-degree: 68%

---


# [!DNL Data Governance]概要

Adobe Experience Platform の主な機能の 1 つは、複数の企業システムのデータを統合して、マーケターが顧客を識別、理解し、惹きつけられるようにすることです。このデータは、組織または法規制によって定義された使用制限の対象となる場合があります。It is therefore important to ensure that your data operations within [!DNL Platform] are compliant with data usage policies.

Adobe Experience Platform [!DNL Data Governance] allows you to manage customer data and ensure compliance with regulations, restrictions, and policies applicable to data use. It plays a key role within [!DNL Experience Platform] at various levels, including cataloging, data lineage, data usage labeling, data usage policies, and controlling usage of data for marketing actions.

## データガバナンスの役割

概念として、データガバナンスは自動的でも、単体でおこなわれるわけでもありません。一般的に、データ管理人と認識される役割は、始めは一個人の役割に過ぎませんでしたが、データガバナンスエコシステムの拡大に伴い、大きく成長しています。今日、データガバナンスを成功させるには継続的な管理と監視が必要です。また、データへの適切なラベル付け、、使用ポリシーの作成、およびそれらのポリシーのコンプライアンス準拠を実現できるツールを持っているかどうかは、データ管理人次第です。

データガバナンスは組織内のすべてユーザーの責任であるべきですが、データガバナンスサイクル内の重要な役割には、次のようなものがあります。

![データガバナンスの役割](./images/overview/roles.png)

### データ管理人

データ管理人は、データガバナンスの中心です。この役割は、規制、契約上の制限、およびポリシーを解釈し、それらをデータに直接適用します。データ管理人の役割は、これらの規制、制限、およびポリシーを理解することです。これには次が含まれます。

* データ、データセット、データサンプルを確認し、メタデータ使用ラベルを適用および管理します。
* データポリシーを作成し、データセットとフィールドに適用する。
* 組織にデータポリシーについて伝える。

### マーケター

マーケターは、データガバナンスのエンドポイントです。彼らは、データ管理人、科学者、エンジニアが作成したデータ管理インフラストラクチャからのデータを要求します。マーケターは、マーケティングの保護下において、多様な専門分野を網羅しています。

* マーケティングアナリストは、顧客を個人としてもグループ（セグメントとも呼ばれる）としても理解できるするため、データを要求します。
* マーケティングスペシャリストとエクスペリエンスデザイナーは、データを使用して新しい顧客体験を設計します。


## [!DNL Data Governance] 枠組み

The [!DNL Data Governance] framework simplifies and streamlines the process of categorizing data and creating data usage policies. データラベルが適用され、データ使用ポリシーが設定されたら、マーケティングアクションを評価して、データが正しく使用されていることを確認できます。

There are three key elements to the [!DNL Data Governance] framework: Labels, Policies, and Enforcement.

1. **ラベル：**&#x200B;プライバシーに関する考慮事項や契約条件を反映したデータを分類し、規制や組織のポリシーに準拠するようにします。
1. **ポリシー：**&#x200B;特定のデータに対して許可されるマーケティングアクションや許可されないマーケティングアクションの種類について説明します。
1. **実施：**&#x200B;ポリシーフレームワークを使用して、様々なデータアクセスパターンをまたいだポリシーを通知し、適用します。

## データ使用ラベル

[!DNL Data Governance] 適用されるポリシーのタイプに従ってデータを分類するために、データセットおよびフィールドレベルで使用量ラベルをデータステッダーに適用できるようにします。

The [!DNL Data Governance] framework includes predefined data usage labels that can be used to categorize data in three ways:

![データ使用ラベルのカテゴリ](./images/overview/label-categories.png)

* **契約データの「C」ラベル：**&#x200B;契約上の義務を負うデータや、顧客データガバナンスポリシーに関連するデータにラベルを付け、分類します。
* **識別データの「I」ラベル：**&#x200B;個人を特定できるデータまたは個人に連絡できるデータの分類に使用されます。
* **機密データの「S」ラベル：**&#x200B;地理的データなどの機密データに関連するデータにラベルを付け、分類します。

>[!NOTE]
>
> 使用可能なラベルの完全なリストと各ラベルタイプの定義については、[サポートされるデータ使用ラベル](labels/reference.md)のガイドを参照してください。

ラベルはいつでも適用でき、柔軟にデータ管理方法を選択できます。Best practice encourages labeling data as soon as it is ingested into [!DNL Experience Platform], or as soon as data becomes available in [!DNL Platform].

詳しくは、 [データ使用ラベルの概要](./labels/overview.md) を参照してください。

## データ使用ポリシー

データ使用ラベルがデータのコンプライアンスを効果的にサポートするためには、データ使用ポリシーを実装する必要があります。Data usage policies are rules that describe the kinds of marketing actions that you are allowed to, or restricted from, performing on data within [!DNL Experience Platform].

マーケティングアクションの例としては、データセットをサードパーティのサービスにエクスポートする場合などがあります。If there is a policy in place saying that specific types of data, such as Personally Identifiable Information (PII), cannot be exported and an &quot;I&quot; label (Identity data) has been applied to the dataset, you will receive a response from the [!DNL Policy Service] telling you that a data usage policy has been violated.

Once data usage labels have been applied, data stewards can create policies using the [!DNL Policy Service] API or the [!DNL Experience Platform] user interface.

>[!IMPORTANT]
>
>すべてのデータ使用ポリシー(Adobeが提供するコアポリシーを含む)は、デフォルトで無効になっています。 個々のポリシーを適用対象と見なすには、そのポリシーを手動で有効にする必要があります。

データ使用ポリシーとマーケティングアクションについて詳しくは、「 [ポリシーの概要](./policies/overview.md)」を参照してください。

## 次の手順

This document provided a high-level introduction to [!DNL Data Governance] and the[!DNL Data Governance] framework. これで、[データ使用状況ラベルのユーザーガイド](labels/user-guide.md)に進み、エクスペリエンスデータへの使用状況ラベルの追加を開始できます。

## 付録

The following section provides additional information regarding [!DNL Data Governance].

### [!DNL Data Governance] 用語

The following table outlines key terms related to [!DNL Data Governance] and the[!DNL Data Governance] framework.

| 用語 | 定義 |
|---|---|
| **契約ラベル** | 契約「C」のラベルは、契約上の義務を負うデータや、組織のデータガバナンスポリシーに関連するデータを分類するために使用されます。 |
| **クロスサイトデータ** | クロスサイトデータは、複数のサイトのデータの組み合わせです。オンサイトデータとオフサイトデータの組み合わせ、または複数のオフサイトソースのデータの組み合わせが含まれます。 |
| **データガバナンス** | データガバナンスには、データがデータの使用に関する規制や企業ポリシーに準拠していることを保証するために使用される、戦略とテクノロジーが含まれます。 |
| **データ管理人** | データ管理者とは、組織のデータアセットの管理、監視、および実施を担当する人物です。また、データガバナンスポリシーは、政府の規制や組織のポリシーに準拠するよう、保護および管理されます。 |
| **データ使用ラベル** | データ使用ラベルを使用すると、規制や企業のポリシーに準拠するためにプライバシー関連の注意事項や契約条件を反映したデータを分類できます。 |
| **データセットラベル** | データセットにはラベルを追加できます。データセット内のすべてのフィールドは、データセットのラベルを継承します。 |
| **フィールドラベル** | フィールドラベルは、データセットから継承されるデータガバナンスラベル、またはフィールドに直接適用されるデータガバナンスラベルです。フィールドに適用したデータガバナンスラベルは、データセットに継承されません。 |
| **ジオフェンス** | ジオフェンスは、GPS または RFID テクノロジーによって定義される仮想的な地理的境界で、モバイルデバイスが特定の領域に入る、または領域から離れるときにソフトウェアが応答をトリガーできるようにします。 |
| **ID ラベル** | 識別の「I」ラベルは、個人を特定できるデータまたは個人に連絡できるデータの分類に使用されます。 |
| **興味ベースのターゲティング** | 次の3つの条件が満たされた場合、興味ベースのターゲット設定（パーソナライゼーションとも呼ばれます）が発生します。オンサイトで収集されたデータは、ユーザーの興味を引くために使用され、別のサイトやアプリ（オフサイト）などの別のコンテキストで使用され、それらの参照に基づいて提供されるコンテンツや広告を選択するために使用されます。 |
| **マーケティングアクション** | A marketing action, in the context of the data governance framework, is an action that an [!DNL Experience Platform] data consumer takes, for which there is a need to check for violations of data usage policies |
| **ポリシー** | データガバナンスのフレームワークにおいて、ポリシーとは、特定のデータに対してどのようなマーケティングアクションを実行できるかどうかを示すルールを表します。 |
| **機密ラベル** | 機密情報の「S」ラベルは、自身と組織が機密とみなすデータを分類するために使用されます。 |

## その他のリソース

次のビデオは、フレーム [!DNL Data Governance] ワークについての理解を深めることを目的としています。

>[!VIDEO](https://video.tv.adobe.com/v/29708?quality=12&enable10seconds=on&speedcontrol=on)

次のビデオでは、Experience Platformの様々な [!DNL Data Governance] 機能の紹介を行っています。

>[!VIDEO](https://video.tv.adobe.com/v/36653?quality=12&enable10seconds=on&speedcontrol=on)