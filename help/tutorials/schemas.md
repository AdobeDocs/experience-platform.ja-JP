---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: XDM スキーマおよび記述子
topic: tutorial
type: Tutorial
description: 標準化と相互運用性は、Adobe Experience Platform を支える重要な概念です。アドビが推進するエクスペリエンスデータモデル（XDM）は、カスタマーエクスペリエンスデータを標準化し、カスタマーエクスペリエンス管理のスキーマを定義する取り組みです。スキーマは Experience Platform でデータを記述する標準的な方法であり、スキーマに準拠するすべてのデータを、組織全体で競合することなく再利用でき、複数の組織間で共有することもできます。
translation-type: tm+mt
source-git-commit: 8c94d3631296c1c3cc97501ccf1a3ed995ec3cab
workflow-type: tm+mt
source-wordcount: '473'
ht-degree: 60%

---


# (XDM) [!DNL Experience Data Model] スキーマと関係記述子を使用する

標準化と相互運用性は、Adobe Experience Platform の背後にある重要な概念です。[!DNL Experience Data Model] (XDM)は、Adobeに基づいて、顧客体験データを標準化し、顧客体験管理のスキーマを定義する取り組みです。 Schemas are the standard way of describing data in [!DNL Experience Platform], allowing all data that conforms to schemas to be reusable without conflicts across an organization and even to be sharable between multiple organizations. XDM スキーマについて詳しくは、まず、[XDM 体系の概要](../xdm/home.md)を参照してください。

## スキーマレジストリを使用したスキーマの作成

スキーマレジストリは、Adobe Experience Platform スキーマライブラリのすべてのリソースを表示および管理できるユーザーインターフェイスと RESTful API を提供します。The Schema Library contains resources made available to you by Adobe, [!DNL Experience Platform] partners, and vendors whose applications you use, as well as resources that you define and save to the Schema Registry. 組織のスキーマの作成方法については、[スキーマレジストリ API を使用してスキーマを作成する方法](../xdm/tutorials/create-schema-api.md)と、[スキーマエディターのユーザーインターフェイスを使用してスキーマを作成する方法](../xdm/tutorials/create-schema-ui.md)のチュートリアルに従ってください。

## 2 つのスキーマ間の関係の定義

様々なチャネルでの顧客とブランドとのやり取りと顧客との関係を把握する機能は、Adobe Experience Platform の重要な要素です。Defining these relationships within the structure of your [!DNL Experience Data Model] (XDM) schemas allows you to gain complex insights into your customer data. これらの関係記述子は、スキーマレジストリ API とスキーマエディター UI を使用して定義できます。詳しくは、[API](../xdm/tutorials/relationship-api.md) または [UI](../xdm/tutorials/relationship-ui.md) を使用して 2 つのスキーマ間の関係を定義する方法に関するチュートリアルを参照してください。

## アドホックスキーマの作成

In specific circumstances, it may be necessary to create an [!DNL Experience Data Model] (XDM) schema with fields that are namespaced for usage only by a single dataset. これは「アドホック」スキーマと呼ばれます。Ad-hoc schemas are used in various [data ingestion](../ingestion/home.md) workflows for [!DNL Experience Platform], including ingesting CSV files and creating certain kinds of [source connections](../sources/home.md). Creating an ad-hoc schema is done using the Schema Registry API and is intended to be used in conjunction with other [!DNL Experience Platform] tutorials that require creating an ad-hoc schema as part of their workflow. アドホックスキーマの作成を開始するには、[API を使用してアドホックスキーマを作成する方法](../xdm/tutorials/ad-hoc.md)に関するチュートリアルを参照してください。

## 次の手順

組織のスキーマを定義したら、データを取り込むデータセットの作成を開始できます。作業を開始する場合は、次のドキュメントを参照してください。

* [データセットの概要](../catalog/datasets/overview.md)
* [データ取得の概要](../ingestion/home.md)