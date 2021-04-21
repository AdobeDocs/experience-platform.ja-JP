---
keywords: Experience Platform；ホーム；人気の高いトピック；ポリシーの適用；自動適用；APIベースの適用；データガバナンス
solution: Experience Platform
title: ポリシー適用の概要
topic-legacy: guide
description: データ使用ラベルがAdobe Experience Platformのデータセットに適用され、これらのラベルに対するマーケティング活動に対してデータ使用ポリシーが定義されると、データ管理機能により、これらのポリシーを適用し、ポリシー違反を構成するデータ操作を防ぐことができます。 Data Governance機能によって提供されるプラットフォームのポリシー適用には、APIベースの適用と自動適用の2つの方法があります。
exl-id: d19d8060-85a1-405c-856d-f59041947a33
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '237'
ht-degree: 10%

---

# ポリシー適用の概要

データ使用ラベルがデータセットに適用され、データ使用ポリシーがラベルに対するマーケティングアクションに定義されると、Adobe Experience Platformデータガバナンス機能により、これらのポリシーを適用し、ポリシー違反を構成するデータ操作を防ぐことができます。

[!DNL Platform]上の[!DNL Data Governance]機能によって提供されるポリシー適用には、次の2つの方法があります。APIベースの強制および自動強制。

## APIベースの強制

[!DNL Policy Service] APIはエンドポイントを提供します。エンドポイントを使用すると、データセットやデータ使用ラベルの任意の組み合わせに対するマーケティング操作をテストし、ポリシー違反が発生したかどうかを確認できます。 その後、API 応答に基づいて、エクスペリエンスアプリケーション内でプロトコルを設定し、データ使用ポリシーのコンプライアンスを適切に実施できます。

APIを使用してポリシーを評価する手順については、[APIベースの強制](./api-enforcement.md)のチュートリアルを参照してください。

## 自動強制

Experience Platformでは、データ・ライング、データ・クラス分け、ポリシー管理機能を活用して、ポリシー違反を自動的に評価し、表面化します。 詳しくは、[自動ポリシー適用](./auto-enforcement.md)の概要を参照してください。
