---
keywords: Experience Platform；ホーム；人気の高いトピック
solution: Experience Platform
title: XDM スキーマおよび記述子
topic-legacy: tutorial
type: Tutorial
description: 標準化と相互運用性は、Adobe Experience Platform を支える重要な概念です。アドビが推進するエクスペリエンスデータモデル（XDM）は、カスタマーエクスペリエンスデータを標準化し、カスタマーエクスペリエンス管理のスキーマを定義する取り組みです。スキーマは Experience Platform でデータを記述する標準的な方法であり、スキーマに準拠するすべてのデータを、組織全体で競合することなく再利用でき、複数の組織間で共有することもできます。
exl-id: 1cdc45d7-57ca-4a2d-99a4-9a8cd885a511
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '478'
ht-degree: 59%

---

# [!DNL Experience Data Model] (XDM)スキーマと関係記述子を扱う

標準化と相互運用性は、Adobe Experience Platform の背後にある重要な概念です。[!DNL Experience Data Model] (XDM)は、Adobeに基づいて、顧客体験データを標準化し、顧客体験管理のスキーマを定義する取り組みです。スキーマは[!DNL Experience Platform]のデータを記述する標準的な方法で、スキーマに準拠するすべてのデータを組織間で競合なく再利用可能にし、複数の組織間で共有することも可能です。 XDM スキーマについて詳しくは、まず、[XDM 体系の概要](../xdm/home.md)を参照してください。

## スキーマレジストリを使用したスキーマの作成

スキーマレジストリは、Adobe Experience Platform スキーマライブラリのすべてのリソースを表示および管理できるユーザーインターフェイスと RESTful API を提供します。スキーマライブラリには、Adobe、[!DNL Experience Platform]パートナー、および使用するアプリケーションを持つベンダーが提供するリソース、および定義してスキーマレジストリに保存するリソースが含まれます。 組織のスキーマの作成方法については、[スキーマレジストリ API を使用してスキーマを作成する方法](../xdm/tutorials/create-schema-api.md)と、[スキーマエディターのユーザーインターフェイスを使用してスキーマを作成する方法](../xdm/tutorials/create-schema-ui.md)のチュートリアルに従ってください。

## 2 つのスキーマ間の関係の定義

様々なチャネルでの顧客とブランドとのやり取りと顧客との関係を把握する機能は、Adobe Experience Platform の重要な要素です。[!DNL Experience Data Model] (XDM)スキーマの構造内でこれらの関係を定義すると、顧客データに対する複雑な洞察を得ることができます。 これらの関係記述子は、スキーマレジストリ API とスキーマエディター UI を使用して定義できます。詳しくは、[API](../xdm/tutorials/relationship-api.md) または [UI](../xdm/tutorials/relationship-ui.md) を使用して 2 つのスキーマ間の関係を定義する方法に関するチュートリアルを参照してください。

## アドホックスキーマの作成

特定の状況では、1つのデータセットでのみ使用するために名前が付けられたフィールドを持つ[!DNL Experience Data Model] (XDM)スキーマを作成する必要が生じる場合があります。 これは「アドホック」スキーマと呼ばれます。アドホックスキーマは、CSVファイルの取り込みや特定の種類の[ソース接続](../sources/home.md)の作成など、[!DNL Experience Platform]の様々な[データ取り込み](../ingestion/home.md)ワークフローで使用されます。 アドホックスキーマの作成は、スキーマレジストリAPIを使用して行います。また、アドホックスキーマを作成する必要がある他の[!DNL Experience Platform]チュートリアルと組み合わせて使用することを目的としています。 アドホックスキーマの作成を開始するには、[API を使用してアドホックスキーマを作成する方法](../xdm/tutorials/ad-hoc.md)に関するチュートリアルを参照してください。

## 次の手順

組織のスキーマを定義したら、データを取り込むデータセットの作成を開始できます。作業を開始する場合は、次のドキュメントを参照してください。

* [データセットの概要](../catalog/datasets/overview.md)
* [データ取得の概要](../ingestion/home.md)
