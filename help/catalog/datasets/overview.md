---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: データセットの概要
topic: datasets
translation-type: tm+mt
source-git-commit: 06733eb374d1b9409102a7cf13d61ed266cedaad

---


# データセットの概要

Adobe Experience Platformに正常に取り込まれたすべてのデータは、データセットとしてData Lake内に保持されます。 データセットは、スキーマ（列）とフィールド（行）を含むテーブルなど、データの集まりのストレージと管理の構成体です。 データセットには、保存するデータの様々な側面を記述したメタデータも含まれます。

このドキュメントでは、Experience Platformのデータセットの概要を説明します。

## データセットの作成と追跡メタデータ

カタログサービスは、Experience Platform内のデータの場所と系列の記録システムで、データセットの作成と管理に使用されます。 カタログは、各データセットのメタデータを追跡します。このメタデータには、データセットが準拠するExperience Data Model(XDM)スキーマへの参照（次の節で説明）と、そのデータセットに取り込まれるレコードの数が含まれます。

See the [Catalog Service overview](../home.md) for more information.

## データセットデータに対する制約の適用

エクスペリエンスデータモデル(XDM)は、プラットフォームが顧客エクスペリエンスデータを整理する際に使用する標準化されたフレームワークです。 Platformに取り込まれるすべてのデータは、データセットとしてData Lakeに保持する前に、事前定義済みのXDMスキーマに準拠している必要があります。

すべてのデータセットには、保存できるデータの形式と構造を制約するXDMスキーマへの参照が含まれています。 データセットのXDMスキーマに準拠していないデータセットにデータをアップロードしようとすると、取り込みに失敗します。

XDMについて詳しくは、 [XDM System Overviewを参照してください](../../xdm/home.md)。

## データセットへのデータの取り込み

Adobe Experience Platform Data Ingestは、プラットフォームが様々なソースからデータを取り込む複数の方法を表します。 取り込み方法に関係なく、正常に取り込まれたデータはすべてバッチファイルに変換されます。 バッチとは、単一の単位として取り込まれる1つ以上のファイルで構成されるデータの単位です。 これらのバッチファイルは、専用のデータセットに追加され、データレーク内で保持されます。

See the [Data Ingestion overview](../../ingestion/home.md) for more information.

## 使用ラベルのデータセットへの適用

Adobe Experience Platform Data Governanceを使用すると、顧客データを管理して、データの使用に適用される規制、制限およびポリシーに確実に準拠することができます。 Data Usage Labeling and Enforcement(DULE)を中核的なフレームワークとして使用すると、Data Governanceを使用して、使用ラベルを適用し、そのデータに適用される使用ポリシーに従ってデータを分類できます。

データ使用ラベルは、データセット全体または個々のデータセットフィールドに適用できます。 データセットレベルで追加されたラベルは、そのデータセット内のすべてのフィールドに継承されます。

サービスの詳 [細については](../../data-governance/home.md) 、「Data Governance overview」を参照してください。 エクスペリエンスプラットフォームUIで使用ラベルを使用する手順については、『データ使用ラベルユーザーガ [イド』を参照してください](../../data-governance/labels/user-guide.md)。

## ダウンストリームプラットフォームサービスのデータセット

データセットを使用して取り込んだデータを保存すると、ダウンストリームプラットフォームサービスでこれらのデータセットが使用され、顧客プロファイルの更新、機械学習による洞察の取得などが行われます。

次に、様々な操作にデータセットを使用するダウンストリームサービスのリストを示します。 詳しくは、各サービスのドキュメントを参照してください。

* [データアクセスAPI](../../data-access/home.md):データセット内に保存されたファイルの内容にアクセスし、ダウンロードできます。
* [Adobe Experience Platform Identity Service](../../identity-service/home.md):デバイスやシステム間でIDをブリッジし、準拠するXDMシステムで定義されたIDフィールドに基づいてデータセットをリンクするスキーマ。
* [リアルタイム顧客プロファイル](../../profile/home.md):IDサービスを利用して、データセットから詳細なプロファイルをリアルタイムで作成します。 リアルタイム顧客はData Lakeからデータを取り込み、顧客プロファイルを独立したデータストアに保持します。
* [Adobe Experience Platform Segmentation Service](../../segmentation/home.md):セグメントを作成し、リアルタイムの顧客オーディエンスデータからプロファイルを生成できます。 これらのオーディエンスは、Data Lake内の独自のデータセットに書き出すことができます。
* [Adobe Experience Platform Data Science Workspace](../../data-science-workspace/home.md):機械学習と人工知能を使用して、大規模なデータセットの洞察を明らかにします。
* [Adobe Experience Platformクエリサービス](../../query-service/home.md):標準のSQLからクエリデータをExperience Platformで使用し、Data Lake内の任意のデータセットと組み合わせて、クエリ結果をレポート、Data Science Workspaceまたはリアルタイム顧客プロファイルで使用する新しいデータセットとして取り込むことができます。
* [Adobe Experience Platform Decisioningサービス](../../decisioning-service/home.md):リアルタイムの顧客プロファイルを活用して、有効なデータセットからプロファイルが取り込む行動データに基づいて、一連のオプションから顧客が最も可能性の高い選択を決定します。

## 次の手順

このドキュメントを読むことで、Experience Platformのデータセットの中核的な使用方法と、データセットを利用する様々なプラットフォームサービスについて説明します。 プラットフォームでのデータセットの多くの使用方法の詳細については、この概要を通じてリンクされたサービスドキュメントを参照してください。

Experience Platform UI内でデータセットを操作する手順については、データセットユーザーガイド [を参照してください](user-guide.md)。