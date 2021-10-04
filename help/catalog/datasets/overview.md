---
keywords: Experience Platform；ホーム；人気のあるトピック；データの場所；データの場所；データ管理；データ管理；系列；系列；データ型；データ型；データ型
solution: Experience Platform
title: データセットの概要
topic-legacy: datasets
description: このドキュメントでは、Experience Platform のデータセットのおおまかな概要を説明します。
exl-id: 51ecefb0-a699-4b1a-80f1-26c6ba92fcbf
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '707'
ht-degree: 40%

---

# データセットの概要

Adobe Experience Platformに正常に取り込まれたすべてのデータは、データセットとして [!DNL Data Lake] 内に保持されます。 データセットは、通常、スキーマ（列）とフィールド（行）を含むテーブルであるデータコレクションのストレージと管理をおこなう構成体です。データセットには、保存するデータの様々な側面を記述したメタデータも含まれます。

このドキュメントでは、[!DNL Experience Platform] のデータセットの概要を説明します。

## データセットの作成とメタデータの追跡

[!DNL Catalog Service] は、内のデータの場所と系列のレコードのシステムで、 [!DNL Experience Platform]データセットの作成と管理に使用されます。[!DNL Catalog] は、データセットが準拠する ( [!DNL Experience Data Model] XDM) スキーマへの参照（次の節で説明）と、そのデータセットに取り込まれるレコードの数を含む、各データセットのメタデータを追跡します。

詳しくは、「[カタログサービスの概要](../home.md)」を参照してください。

## データセットデータに対する制限の適用

[!DNL Experience Data Model] (XDM) は、顧客体験データを整理する際に使用する [!DNL Platform] 標準化されたフレームワークです。[!DNL Platform] に取り込まれるすべてのデータは、事前定義済みの XDM スキーマに準拠してから、データセットとして [!DNL Data Lake] に保持する必要があります。

すべてのデータセットには、保存できるデータの形式と構造を制限する XDM スキーマへの参照が含まれています。データセットの XDM スキーマに準拠していないデータセットにデータをアップロードしようとすると、データの取得に失敗します。

XDM について詳しくは、「[XDM システムの概要](../../xdm/home.md)」を参照してください。

## データセットへのデータの取り込み

Adobe Experience Platformデータ取り込みは、[!DNL Platform] が様々なソースからデータを取り込む複数の方法を表します。 取得方法に関係なく、正常に取り込まれたデータはすべてバッチファイルに変換されます。バッチとは、1 つの単位として取り込まれる 1 つ以上のファイルで構成されるデータの単位です。次に、これらのバッチファイルは専用のデータセットに追加され、[!DNL Data Lake] 内で保持されます。

詳しくは、「[データ取得の概要](../../ingestion/home.md)」を参照してください。

## データセットへの使用状況ラベルの適用

Adobe Experience Platform [!DNL Data Governance] では、データの使用に適した規制、制限、ポリシーへのコンプライアンスを確保するために、顧客データを管理できます。 [!DNL Data Governance] フレームワークを使用すると、使用状況ラベルを適用して、そのデータに適用される使用状況ポリシーに従ってデータを分類できます。

データ使用状況ラベルは、データセット全体または個々のデータセットフィールドに適用できます。データセットレベルで追加されたラベルは、そのデータセット内のすべてのフィールドに継承されます。

このサービスについて詳しくは、「[データガバナンスの概要](../../data-governance/home.md)」を参照してください。[!DNL Platform] で使用状況ラベルを使用する手順については、次のガイドを参照してください。

* [UI でのラベルの管理](../../data-governance/labels/user-guide.md)
* [API でのデータセットラベルの管理](../../data-governance/labels/dataset-api.md)

## ダウンストリーム [!DNL Platform] サービスのデータセット

データセットを使用して取り込んだデータを保存すると、ダウンストリーム [!DNL Platform] サービスでそれらのデータセットが使用され、顧客プロファイルの更新や機械学習を通じたインサイトの取得などがおこなわれます。

様々な操作にデータセットを使用するダウンストリームサービスのリストを次に示します。詳しくは、各サービスのドキュメントを参照してください。

* [[!DNL Data Access API]](../../data-access/home.md):データセット内に保存されたファイルの内容にアクセスしてダウンロードできます。
* [Adobe Experience Platform ID サービス](../../identity-service/home.md)：デバイスやシステム間で ID をブリッジし、準拠する XDM スキーマで定義された ID フィールドに基づいてデータセットをリンクします。
* [[!DNL Real-time Customer Profile]](../../profile/home.md):を活用し [!DNL Identity Service] て、データセットから詳細な顧客プロファイルをリアルタイムで作成します。[!DNL Real-time Customer Profile] からデータを取り込み、顧 [!DNL Data Lake] 客プロファイルを独自の別々のデータストアに保持します。
* [Adobe Experience Platform Segmentation Service](../../segmentation/home.md):セグメントを作成し、データからオーディエンスを生成で [!DNL Real-time Customer Profile] きます。その後、これらのオーディエンスは、[!DNL Data Lake] 内の独自のデータセットにエクスポートできます。
* [Adobe Experience Platform Data Science Workspace](../../data-science-workspace/home.md)：機械学習と人工知能を使用して、大規模なデータセットからインサイトを引き出します。
* [Adobe Experience Platformクエリサービス](../../query-service/home.md):標準の SQL を使用して、でデータをクエリし、内の任意のデータセットを結 [!DNL Experience Platform]合 [!DNL Data Lake] し、クエリ結果を新しいデータセットとして取得して、レポート、またはで使用で [!DNL Data Science Workspace]きま [!DNL Real-time Customer Profile]す。

## 次の手順

このドキュメントでは、[!DNL Experience Platform] のデータセットの主な使用方法と、データセットを利用する様々な [!DNL Platform] サービスについて説明しました。 [!DNL Platform] でデータセットを使用する多くの方法の詳細については、この概要でリンクされているサービスドキュメントを参照してください。

[!DNL Experience Platform] UI 内でデータセットを操作する手順については、『[ データセットユーザガイド ](user-guide.md)』を参照してください。
