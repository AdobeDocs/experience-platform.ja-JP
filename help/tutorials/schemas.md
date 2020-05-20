---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: XDMスキーマと記述子
topic: tutorial
translation-type: tm+mt
source-git-commit: 2f0f155beacbc6a4ba2892ae211a9c0305e969ac
workflow-type: tm+mt
source-wordcount: '427'
ht-degree: 0%

---


# Experience Data Model(XDM)スキーマと関係記述子の使用

標準化と相互運用性は、Adobe Experience Platformの背後にある重要な概念です。 アドビが推進するExperience Data Model(XDM)は、カスタマーエクスペリエンスデータを標準化し、カスタマーエクスペリエンス管理のスキーマを定義する取り組みです。 スキーマは、Experience Platformのデータを記述する標準的な方法で、スキーマに準拠するすべてのデータを組織全体で競合なく再利用可能にし、複数の組織間で共有可能にすることができます。 XDMスキーマの詳細は、 [XDMシステムの概要を読んで開始してください](../xdm/home.md)。

## スキーマレジストリを使用したスキーマの作成

スキーマレジストリは、Adobe Experience Platformスキーマライブラリのすべてのリソースを表示および管理できるユーザーインターフェイスおよびRESTful APIを提供します。 スキーマライブラリには、アドビ、Experience Platformパートナー、および使用するアプリケーションのベンダーが提供するリソースと、定義してスキーマレジストリに保存するリソースが含まれています。 組織のスキーマを作成する方法については、スキーマレジストリAPIを使用したスキーマの [作成](../xdm/tutorials/create-schema-api.md) 、またはスキーマエディターユーザーインターフェイスを使用したスキーマの [作成のチュートリアルに従ってください](../xdm/tutorials/create-schema-ui.md)。

## 2つのスキーマ間の関係の定義

様々なチャネルにわたる顧客とブランドとの関係を理解する能力は、Adobe Experience Platformの重要な部分です。 エクスペリエンスデータモデル(XDM)スキーマの構造内でこれらの関係を定義すると、顧客データに対する複雑なインサイトを得ることができます。 これらの関係記述子は、スキーマレジストリAPIとスキーマエディタUIを使用して定義できます。 詳しくは、API [を使用して2つのスキーマ間の関係を定義するためのチュートリアル](../xdm/tutorials/relationship-api.md) 、またはUI [を使用する方法を参照してください](../xdm/tutorials/relationship-ui.md)。

## アドホックスキーマの作成

特定の状況では、1つのデータセットでのみ使用するために名前が付けられたフィールドを持つExperience Data Model(XDM)スキーマを作成する必要が生じる場合があります。 これは「アドホック」スキーマと呼ばれます。 アドホックスキーマは、CSVファイルの取り込みや特定の種類の [ソース接続の作成など、Experience Platformの様々な](../ingestion/home.md) データ取り込み [ワークフローで使用されます](../sources/home.md)。 アドホックスキーマの作成は、スキーマレジストリAPIを使用して行い、そのワークフローの一部としてアドホックスキーマを作成する必要がある他のExperience Platformチュートリアルと組み合わせて使用することを目的としています。 アドホックスキーマの作成を開始するには、APIを使用したアドホックスキーマの [作成に関するチュートリアルを参照してください](../xdm/tutorials/ad-hoc.md)。

## 次の手順

組織のスキーマを定義したら、データを取り込むデータセットの作成を開始できます。 開始するには、次のドキュメントを参照してください。

* [データセットの概要](../catalog/datasets/overview.md)
* [データ取り込みの概要](../ingestion/home.md)