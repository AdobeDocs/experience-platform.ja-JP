---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: データ使用ポリシーの概要
topic: policies
translation-type: tm+mt
source-git-commit: d08f06b3c2bdb21e0f4935bbcb6f163892ab9842
workflow-type: tm+mt
source-wordcount: '301'
ht-degree: 2%

---


# データ使用ポリシーの概要

データ使用ラベルがデータのコンプライアンスを効果的にサポートするためには、データ使用ポリシーを実装する必要があります。 データ使用ポリシーとは、エクスペリエンスプラットフォーム内のデータに対して実行を許可（制限）されるマーケティングアクションの種類を記述するルールです。

マーケティングアクションの例としては、データセットをサードパーティのサービスにエクスポートしたい場合などが考えられます。 個人識別情報(PII)などの特定の種類のデータをエクスポートできず、「I」ラベル（IDデータ）がデータセットに適用されているというポリシーが設定されている場合、Policy Serviceからデータ使用ポリシーに違反したことを伝える応答が返されます。

## データ使用ポリシーの作成および使用方法

データ使用ラベルが適用されると、データ管理者は [DULE Policy Service APIを使用してポリシーを作成できます](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/dule-policy-service.yaml)。

データ管理者は、Policy Service APIを使用して、DULEラベルを含むデータに対して行われるマーケティングアクションに関連するポリシーを管理および評価できます。 APIを使用すると、ポリシーの作成と更新、ポリシーのステータスの決定、マーケティングアクションを使用して、特定のアクションがデータ使用ポリシーに違反しているかどうかを評価できます。

Policy Service API内では、すべてのポリシーとマーケティングアクションを「または」 `core``custom` リソースと呼びます。 `core` リソースはアドビが定義および保守しますが、 `custom` リソースは個々のお客様が作成および保守します。 したがって、 `custom` リソースは一意であり、リソースを作成した組織にのみ表示されます。

DULE Policy Service APIが提供する主要な操作の実行について詳しくは、 [Policy Service開発者ガイドを参照してください](../api/getting-started.md)。 DULEポリシーの操作手順については、DULEポリシーの [作成と評価に関するチュートリアルを参照してください](create.md)。