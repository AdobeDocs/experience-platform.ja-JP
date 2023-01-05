---
solution: Experience Platform
title: 業界データモデルの概要
topic-legacy: overview
description: 標準のエクスペリエンスデータモデル (XDM) コンポーネントを使用して構築できる、様々な業種向けの標準化されたデータモデルについて説明します。
exl-id: 8fa9a610-36b5-470f-ad63-f2a4a060e0f1
source-git-commit: d3f914cb4bcd18980e433c6fd17a663ad0fb5a84
workflow-type: tm+mt
source-wordcount: '355'
ht-degree: 2%

---

# 業界データモデルの概要

エクスペリエンスデータモデル (XDM) を使用すると、高度にカスタマイズ可能なスキーマを作成して、ビジネスに関連する主な顧客体験データを取り込むことができます。 Adobe Experience Platformは、XDM に準拠するようにデータをモデリングするプロセスを合理化するために、汎用の標準 XDM コンポーネントスイートを提供し、複数の業界で一般的に使用される概念を取り込みます。

>[!NOTE]
>
>消費者のニーズに最適な新しい標準 XDM コンポーネントが継続的にリリースされています。 最新のコンポーネントのリストについては、次の操作を実行できます。 [UI での既存のリソースの参照](../../ui/explore.md) または [公式 XDM リポジトリ](https://github.com/adobe/xdm/tree/master/components) GitHub で

ビジネスが運用する業界に応じて、一部の XDM コンポーネントは、他の XDM コンポーネントよりもニーズに関連します。 さらに、XDM スキーマ間で確立する関係は、業界によって異なります。

特定の業界に基づくデータモデリング戦略を導くのに役立つように、このガイドでは、サポートされる複数の業界向けに、エンティティ関係図 (ERD) を参照します。

## 前提条件

このガイドで参照される ERD を読むには、XDM コンポーネントがフォームスキーマとどのようにやり取りするか、および XDM スキーマ全体のExperience Platformでの動作方法に関する十分な知識が必要です。 先に進む前に、次の概要ドキュメントを必ずお読みください。

* [XDM システムの概要](../../home.md):Platform エコシステムでの XDM の動作について説明します。
* [スキーマ構成の基本](../../schema/composition.md):XDM コンポーネント（スキーマフィールドグループ、クラス、データ型など）がスキーマの構造にどのように貢献するか、および ID フィールドの役割について説明します。

また、 [データモデリングのベストプラクティスガイド](../../schema/best-practices.md) を参照してください。

## 業界データモデル ERD {#erds}

ERD は次の業種向けに提供されています。

* [[!UICONTROL 小売]](./retail.md)
* [[!UICONTROL 金融機関]](./financial.md)
* [[!UICONTROL 医療]](./healthcare.md)
* [[!UICONTROL 通信業]](./telecom.md)
* [[!UICONTROL 旅行およびホスピタリティ]](./travel-hospitality.md)

## 次の手順

このドキュメントでは、業界データモデル ERD の概要と、その解釈方法を示しました。 ERD を表示するには、上のリストから ERD を選択します。
