---
keywords: Experience Platform;home;popular topics;data location;Data Location;Data management;data management;Lineage;lineage;data type;data types;Data types;Data type
solution: Experience Platform
title: データセットの概要
topic: datasets
description: このドキュメントでは、Experience Platform のデータセットのおおまかな概要を説明します。
translation-type: tm+mt
source-git-commit: 1c00456ee06c1fc09c8e4ce070c90255f51811e1
workflow-type: tm+mt
source-wordcount: '683'
ht-degree: 41%

---


# データセットの概要

All data that is successfully ingested into Adobe Experience Platform is persisted within the [!DNL Data Lake] as datasets. データセットは、通常、スキーマ（列）とフィールド（行）を含むテーブルであるデータコレクションのストレージと管理をおこなう構成体です。データセットには、保存するデータの様々な側面を記述したメタデータも含まれます。

This document provides a high-level overview of datasets in [!DNL Experience Platform].

## データセットの作成とメタデータの追跡

[!DNL Catalog Service] は、内のデータの場所と系列の記録システムで [!DNL Experience Platform]、データセットの作成と管理に使用されます。 [!DNL Catalog] 各データセットのメタデータを追跡します。このメタデータには、データセットが準拠する [!DNL Experience Data Model] (XDM)スキーマへの参照（次の節で説明）と、そのデータセットに取り込まれるレコード数が含まれます。

詳しくは、「[カタログサービスの概要](../home.md)」を参照してください。

## データセットデータに対する制限の適用

[!DNL Experience Data Model] (XDM)は、顧客体験データを [!DNL Platform] 編成する標準化されたフレームワークです。 All data that is ingested into [!DNL Platform] must conform to a pre-defined XDM schema before it can be persisted in the [!DNL Data Lake] as a dataset.

すべてのデータセットには、保存できるデータの形式と構造を制限する XDM スキーマへの参照が含まれています。データセットの XDM スキーマに準拠していないデータセットにデータをアップロードしようとすると、データの取得に失敗します。

XDM について詳しくは、「[XDM システムの概要](../../xdm/home.md)」を参照してください。

## データセットへのデータの取り込み

Adobe Experience Platform Data Ingestion represents the multiple methods by which [!DNL Platform] ingests data from various sources. 取得方法に関係なく、正常に取り込まれたデータはすべてバッチファイルに変換されます。バッチとは、1 つの単位として取り込まれる 1 つ以上のファイルで構成されるデータの単位です。These batch files are then added to dedicated datasets and persisted within the [!DNL Data Lake].

詳しくは、「[データ取得の概要](../../ingestion/home.md)」を参照してください。

## データセットへの使用状況ラベルの適用

Adobe Experience Platform [!DNL Data Governance] allows you to manage customer data in order to ensure compliance with regulations, restrictions, and policies applicable to data use. フレーム [!DNL Data Governance] ワークを使用すると、使用状況ラベルを適用して、そのデータに適用される使用状況ポリシーに従ってデータを分類できます。

データ使用状況ラベルは、データセット全体または個々のデータセットフィールドに適用できます。データセットレベルで追加されたラベルは、そのデータセット内のすべてのフィールドに継承されます。

このサービスについて詳しくは、「[データガバナンスの概要](../../data-governance/home.md)」を参照してください。で使用状況ラベルを使用する手順については、次のガイドを参照し [!DNL Platform]てください。

* [UIでのラベルの管理](../../data-governance/labels/user-guide.md)
* [APIでのデータセットラベルの管理](../../data-governance/labels/dataset-api.md)

## Datasets in downstream [!DNL Platform] services

Once datasets have been used to store ingested data, those datasets are then used by downstream [!DNL Platform] services to update customer profiles, gain insights through machine learning, and more.

様々な操作にデータセットを使用するダウンストリームサービスのリストを次に示します。詳しくは、各サービスのドキュメントを参照してください。

* [[!DNL Data Access API]](../../data-access/home.md):データセットに保存されたファイルの内容にアクセスし、ダウンロードできます。
* [Adobe Experience Platform ID サービス](../../identity-service/home.md)：デバイスやシステム間で ID をブリッジし、準拠する XDM スキーマで定義された ID フィールドに基づいてデータセットをリンクします。
* [[!DNL Real-time Customer Profile]](../../profile/home.md):を活用 [!DNL Identity Service] して、データセットから詳細な顧客プロファイルをリアルタイムで作成します。 [!DNL Real-time Customer Profile] 顧客プロファイルを個別のデータストアから取り込 [!DNL Data Lake] み、引き続き顧客データを保持します。
* [Adobe Experience Platform分類サービス](../../segmentation/home.md):セグメントを作成し、 [!DNL Real-time Customer Profile] データからオーディエンスを生成できます。 These audiences can then be exported to their own datasets within the [!DNL Data Lake].
* [Adobe Experience Platform Data Science Workspace](../../data-science-workspace/home.md)：機械学習と人工知能を使用して、大規模なデータセットからインサイトを引き出します。
* [Adobe Experience Platformクエリサービス](../../query-service/home.md):で標準のSQLからクエリへのデータ送信を使用でき [!DNL Experience Platform]ます。クエリ内の任意のデータセットを結合し、レポート、 [!DNL Data Lake] またはで使用する新しいデータセットとして結果を取得でき [!DNL Data Science Workspace][!DNL Real-time Customer Profile]ます。

## 次の手順

By reading this document, you have been introduced to the core uses of datasets in [!DNL Experience Platform], as well as the various [!DNL Platform] services that utilize datasets. For more details on the many ways datasets are used in [!DNL Platform], please review the service documentation linked throughout this overview.

For steps on how to interact with datasets within the [!DNL Experience Platform] UI, see the [datasets user guide](user-guide.md).