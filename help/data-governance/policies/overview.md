---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: データ使用ポリシーの概要
topic: policies
translation-type: tm+mt
source-git-commit: d08f06b3c2bdb21e0f4935bbcb6f163892ab9842

---


# データ使用ポリシーの概要

データの使用ラベルがデータのコンプライアンスを効果的にサポートするためには、データの使用ポリシーを実装する必要があります。 データ使用ポリシーは、エクスペリエンスプラットフォーム内のデータで実行を許可（制限）されるマーケティングアクションの種類を記述するルールです。

マーケティングアクションの例としては、データセットをサードパーティのサービスにエクスポートする場合などが考えられます。 個人情報(PII)など特定のタイプのデータはエクスポートできず、「I」ラベル（IDデータ）がデータセットに適用されているというポリシーが設定されている場合は、Policy Serviceからデータ使用ポリシーの違反を知らせる応答が返されます。

## データ使用ポリシーの作成方法と使用方法

データ使用ラベルが適用されると、データステワードは [DULE Policy Service APIを使用してポリシーを作成できます](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/dule-policy-service.yaml)。

データ管理者として、Policy Service APIを使用して、DULEラベルを含むデータに対して実行されるマーケティングアクションに関連するポリシーを管理および評価できます。 APIを使用すると、ポリシーの作成と更新、ポリシーのステータスの決定、およびマーケティングアクションを使用して、特定のアクションがデータ使用ポリシーに違反しているかどうかを評価できます。

Policy Service API内では、すべてのポリシーとマーケティングアクションは、「または」リソースと `core` 呼ばれ `custom` ます。 `core` リソースはアドビが定義および管理するのに対し `custom` 、リソースは個々の顧客が作成および管理するのです。 したがっ `custom` て、リソースは一意で、作成元の組織にのみ表示されます。

DULE Policy Service APIで提供される主要な操作の実行について詳しくは、「 [Policy Service Developer Guide」を参照してください](../api/getting-started.md)。 DULEポリシーの操作手順については、DULEポリシーの作成と評価に関するチュートリ [アルを参照してください](create.md)。