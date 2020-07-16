---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: XDMスキーマと記述子
topic: tutorial
translation-type: tm+mt
source-git-commit: 5c5f6c4868e195aef76bacc0a1e5df3857647bde
workflow-type: tm+mt
source-wordcount: '407'
ht-degree: 0%

---


# (XDM) [!DNL Experience Data Model] スキーマと関係記述子を使用する

標準化と相互運用性は、Adobe Experience Platformの背後にある重要な概念です。 [!DNL Experience Data Model] (XDM)は、アドビが推進する機能で、カスタマーエクスペリエンスデータを標準化し、カスタマーエクスペリエンス管理のスキーマを定義するための取り組みです。 スキーマはのデータを記述する標準的な方法で [!DNL Experience Platform]あり、スキーマに準拠するすべてのデータを組織間で競合なく再利用可能にし、複数の組織間で共有可能にすることができます。 XDMスキーマの詳細は、 [XDMシステムの概要を読んで開始してください](../xdm/home.md)。

## スキーマレジストリを使用したスキーマの作成

スキーマレジストリには、Adobe Experience Platformスキーマライブラリ内のすべてのリソースを表示および管理できるユーザーインターフェイスとRESTful APIが用意されています。 スキーマライブラリには、アドビ、パートナー、および使用するベンダーが提供するリソースと、定義してスキーマレジストリに保存するリソースが含まれています。 [!DNL Experience Platform] 組織のスキーマを作成する方法については、スキーマレジストリAPIを使用したスキーマの [作成](../xdm/tutorials/create-schema-api.md) 、またはスキーマエディターユーザーインターフェイスを使用したスキーマの [作成のチュートリアルに従ってください](../xdm/tutorials/create-schema-ui.md)。

## 2つのスキーマ間の関係の定義

様々なチャネルにわたる顧客とブランドとの関係を理解する能力は、Adobe Experience Platformの重要な部分です。 これらの関係を [!DNL Experience Data Model] (XDM)スキーマの構造内で定義すると、顧客データに対する複雑な洞察を得ることができます。 これらの関係記述子は、スキーマレジストリAPIとスキーマエディタUIを使用して定義できます。 詳しくは、API [を使用して2つのスキーマ間の関係を定義するためのチュートリアル](../xdm/tutorials/relationship-api.md) 、またはUI [を使用する方法を参照してください](../xdm/tutorials/relationship-ui.md)。

## アドホックスキーマの作成

特定の状況では、1つのデータセットでのみ使用できるように名前が付けられたフィールドを持つ [!DNL Experience Data Model] (XDM)スキーマを作成する必要が生じる場合があります。 これは「アドホック」スキーマと呼ばれます。 CSVファイルの取り込みや特定の種類の [ソース接続の作成](../ingestion/home.md) など、アドホックスキーマは様々な [!DNL Experience Platform]データ取り込み [ワークフローで使用され](../sources/home.md)ます。 アドホックスキーマの作成は、スキーマレジストリAPIを使用して行います。このワークフローの一部としてアドホックスキーマを作成する必要がある他の [!DNL Experience Platform] チュートリアルとの併用を意図しています。 アドホックスキーマの作成を開始するには、APIを使用したアドホックスキーマの [作成に関するチュートリアルを参照してください](../xdm/tutorials/ad-hoc.md)。

## 次の手順

組織のスキーマを定義したら、データを取り込むデータセットの作成を開始できます。 開始するには、次のドキュメントを参照してください。

* [データセットの概要](../catalog/datasets/overview.md)
* [データ取り込みの概要](../ingestion/home.md)