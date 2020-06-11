---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: データセットの概要
topic: datasets
translation-type: tm+mt
source-git-commit: dcdd94a3a13a13b4104e57b74ecf613bc316b0af
workflow-type: tm+mt
source-wordcount: '776'
ht-degree: 2%

---


# データセットの概要

Adobe Experience Platformに正常に取り込まれたすべてのデータは、データセットとしてData Lake内に保持されます。 データセットは、スキーマ（列）とフィールド（行）を含むテーブルなど、データの集まりのストレージと管理の構成体です。 データセットには、保存するデータの様々な要素を説明するメタデータも含まれます。

このドキュメントでは、Experience Platformのデータセットの概要を説明します。

## データセットの作成とメタデータの追跡

Catalog Serviceは、Experience Platform内のデータの場所と系列の記録システムで、データセットの作成と管理に使用されます。 カタログでは、各データセットのメタデータを追跡します。このメタデータには、データセットが準拠するExperience Data Model(XDM)スキーマへの参照（次の節で説明）と、そのデータセットに取り込まれるレコード数が含まれます。

See the [Catalog Service overview](../home.md) for more information.

## データセットデータに対する制約の適用

エクスペリエンスデータモデル(XDM)は、プラットフォームが顧客エクスペリエンスデータを編成する際に使用する標準化されたフレームワークです。 Platformに取り込まれるすべてのデータは、データセットとしてData Lakeに保持するには、事前定義済みのXDMスキーマに準拠している必要があります。

すべてのデータセットには、格納できるデータの形式と構造を制約するXDMスキーマへの参照が含まれます。 データセットのXDMスキーマに適合しないデータセットにデータをアップロードしようとすると、取り込みに失敗します。

XDMについて詳しくは、「 [XDM System overview](../../xdm/home.md)」を参照してください。

## データセットへのデータの取り込み

Adobe Experience Platform Data Ingestは、プラットフォームが様々なソースからデータを取り込む複数の方法を表しています。 取り込み方法にかかわらず、正常に取り込まれたデータはすべてバッチファイルに変換されます。 バッチとは、1 つの単位として取り込まれる 1 つ以上のファイルで構成されるデータの単位です。これらのバッチファイルは、専用のデータセットに追加され、Data Lake内で保持されます。

See the [Data Ingestion overview](../../ingestion/home.md) for more information.

## 使用ラベルをデータセットに適用する

Adobe Experience Platform Data Governanceでは、顧客データを管理して、データの使用に適用される規制、制限およびポリシーに対するコンプライアンスを確保できます。 Data Usage Labeling and Enforcement(DULE)を中核的なフレームワークとして使用すると、Data Governanceで使用ラベルを適用し、そのデータに適用される使用ポリシーに従ってデータを分類できます。

データ使用量ラベルは、データセット全体または個々のデータセットフィールドに適用できます。 データセットレベルで追加されたラベルは、そのデータセット内のすべてのフィールドに継承されます。

サービスの詳細については、 [データ管理の概要](../../data-governance/home.md) （英語）を参照してください。 で使用状況ラベルを使用する手順については、次のガイドを参照し [!DNL Platform]てください。

* [UIでのラベルの管理](../../data-governance/labels/user-guide.md)
* [APIでのラベルの管理](../../data-governance/labels/api.md)

## ダウンストリーム・プラットフォーム・サービスのデータセット

データセットを使用して取り込んだデータを保存すると、それらのデータセットはダウンストリームプラットフォームサービスで使用され、顧客プロファイルの更新、機械学習によるインサイトの取得などが行われます。

様々な操作にデータセットを使用するダウンストリームサービスのリストを次に示します。 詳しくは、各サービスのドキュメントを参照してください。

* [データアクセスAPI](../../data-access/home.md): データセットに保存されたファイルの内容にアクセスし、ダウンロードできます。
* [Adobe Experience Platform Identity Service](../../identity-service/home.md): デバイスとシステム間のIDをブリッジし、XDMスキーマが準拠するIDフィールドに基づいてデータセットをリンクします。
* [リアルタイム顧客プロファイル](../../profile/home.md): Identity Serviceを利用して、データセットから詳細な顧客プロファイルをリアルタイムで作成します。 リアルタイム顧客はData Lakeからデータを取り込み、顧客プロファイルを個別のData Storeに維持します。
* [Adobe Experience Platform Segmentation Service](../../segmentation/home.md): セグメントを作成し、リアルタイムの顧客プロファイルデータからオーディエンスを生成できます。 これらのオーディエンスは、Data Lake内の独自のデータセットにエクスポートできます。
* [Adobe Experience Platform Data Science Workspace](../../data-science-workspace/home.md): 機械学習と人工知能を使用して、大規模なデータセットの洞察を明らかにします。
* [Adobe Experience Platformクエリサービス](../../query-service/home.md): 標準のSQLからExperience Platformへのクエリデータの使用、Data Lake内の任意のデータセットとの結合、クエリ結果の取り込みを、レポート、Data Science Workspace、またはリアルタイム顧客プロファイルで使用する新しいデータセットとして行えます。
* [Adobe Experience Platform Decisioningサービス](../../decisioning-service/home.md): リアルタイムの顧客プロファイルを活用して、有効なデータセットからプロファイルが取り込む行動データに基づいて、一連のオプションから顧客がとる最も可能性の高い選択を決定します。

## 次の手順

このドキュメントを読むことで、Experience Platformのデータセットの中核的な使用方法、およびデータセットを利用する様々なプラットフォームサービスについて説明します。 プラットフォームでのデータセットの多くの使用方法の詳細については、この概要全体を通してリンクされたサービスドキュメントを参照してください。

Experience Platform UI内でデータセットを操作する手順については、『Datasetsユーザーガイド』を参照して [ください](user-guide.md)。