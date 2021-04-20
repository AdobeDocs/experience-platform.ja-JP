---
keywords: Experience Platform；ホーム；人気のあるトピック；データの場所；データ管理;データ管理；系統；系列；データ型；データ型；データ型；データ型
solution: Experience Platform
title: データセットの概要
topic: datasets
description: このドキュメントでは、Experience Platform のデータセットのおおまかな概要を説明します。
translation-type: tm+mt
source-git-commit: 126b3d1cf6d47da73c6ab045825424cf6f99e5ac
workflow-type: tm+mt
source-wordcount: '708'
ht-degree: 39%

---


# データセットの概要

Adobe Experience Platformに正常に取り込まれたすべてのデータは[!DNL Data Lake]内にデータセットとして保持されます。 データセットは、通常、スキーマ（列）とフィールド（行）を含むテーブルであるデータコレクションのストレージと管理をおこなう構成体です。データセットには、保存するデータの様々な側面を記述したメタデータも含まれます。

このドキュメントでは、[!DNL Experience Platform]のデータセットの概要を説明します。

## データセットの作成とメタデータの追跡

[!DNL Catalog Service] は、内のデータの場所と系列の記録システムで [!DNL Experience Platform]、データセットの作成と管理に使用されます。[!DNL Catalog] 各データセットのメタデータを追跡します。このメタデータには、データセットが準拠する [!DNL Experience Data Model] (XDM)スキーマへの参照（次の節で説明）と、そのデータセットに取り込まれるレコード数が含まれます。

詳しくは、「[カタログサービスの概要](../home.md)」を参照してください。

## データセットデータに対する制限の適用

[!DNL Experience Data Model] (XDM)は、顧客体験データを [!DNL Platform] 整理するための標準化されたフレームワークです。[!DNL Platform]に取り込まれるすべてのデータは、事前定義済みのXDMスキーマに準拠していなければ、データセットとして[!DNL Data Lake]に保持されません。

すべてのデータセットには、保存できるデータの形式と構造を制限する XDM スキーマへの参照が含まれています。データセットの XDM スキーマに準拠していないデータセットにデータをアップロードしようとすると、データの取得に失敗します。

XDM について詳しくは、「[XDM システムの概要](../../xdm/home.md)」を参照してください。

## データセットへのデータの取り込み

Adobe Experience Platformデータインジェストは、[!DNL Platform]が様々なソースからデータを取り込む複数の方法を表します。 取得方法に関係なく、正常に取り込まれたデータはすべてバッチファイルに変換されます。バッチとは、1 つの単位として取り込まれる 1 つ以上のファイルで構成されるデータの単位です。これらのバッチファイルは、専用のデータセットに追加され、[!DNL Data Lake]内に保持されます。

詳しくは、「[データ取得の概要](../../ingestion/home.md)」を参照してください。

## データセットへの使用状況ラベルの適用

Adobe Experience Platform[!DNL Data Governance]では、顧客データを管理し、データの使用に適した規制、制限、ポリシーに対するコンプライアンスを確保できます。 [!DNL Data Governance]フレームワークを使用すると、使用状況ラベルを適用して、そのデータに適用される使用状況ポリシーに従ってデータを分類できます。

データ使用状況ラベルは、データセット全体または個々のデータセットフィールドに適用できます。データセットレベルで追加されたラベルは、そのデータセット内のすべてのフィールドに継承されます。

このサービスについて詳しくは、「[データガバナンスの概要](../../data-governance/home.md)」を参照してください。[!DNL Platform]で使用ラベルを使用する手順については、次のガイドを参照してください。

* [UIでのラベルの管理](../../data-governance/labels/user-guide.md)
* [APIでのデータセットラベルの管理](../../data-governance/labels/dataset-api.md)

## ダウンストリーム[!DNL Platform]サービスのデータセット

データセットを使用して取り込んだデータを保存すると、ダウンストリーム[!DNL Platform]サービスでこれらのデータセットが使用され、顧客プロファイルの更新、機械学習によるインサイトの取得などが行われます。

様々な操作にデータセットを使用するダウンストリームサービスのリストを次に示します。詳しくは、各サービスのドキュメントを参照してください。

* [[!DNL Data Access API]](../../data-access/home.md):データセットに保存されたファイルの内容にアクセスし、ダウンロードできます。
* [Adobe Experience Platform ID サービス](../../identity-service/home.md)：デバイスやシステム間で ID をブリッジし、準拠する XDM スキーマで定義された ID フィールドに基づいてデータセットをリンクします。
* [[!DNL Real-time Customer Profile]](../../profile/home.md):を活用 [!DNL Identity Service] して、データセットから詳細な顧客プロファイルをリアルタイムで作成します。[!DNL Real-time Customer Profile] データを抽出し、顧客プロファイル [!DNL Data Lake] を独自のデータストアに維持します。
* [Adobe Experience Platform分類サービス](../../segmentation/home.md):セグメントを作成し、 [!DNL Real-time Customer Profile] データからオーディエンスを生成できます。これらのオーディエンスは、[!DNL Data Lake]内の独自のデータセットにエクスポートできます。
* [Adobe Experience Platform Data Science Workspace](../../data-science-workspace/home.md)：機械学習と人工知能を使用して、大規模なデータセットからインサイトを引き出します。
* [Adobe Experience Platformクエリサービス](../../query-service/home.md):で標準のSQLからクエリへのデータ送信を使用でき [!DNL Experience Platform]ます。クエリ内の任意のデータセットを結合 [!DNL Data Lake] し、レポート、 [!DNL Data Science Workspace]またはで使用する新しいデータセットとして結果を取得でき [!DNL Real-time Customer Profile]ます。

## 次の手順

このドキュメントを読むことで、[!DNL Experience Platform]のデータセットの主な使用方法、およびデータセットを利用する様々な[!DNL Platform]サービスについて説明します。 [!DNL Platform]でのデータセットの多くの使用方法の詳細については、この概要全体でリンクされたサービスドキュメントを参照してください。

[!DNL Experience Platform] UI内のデータセットを操作する手順については、[datasetsユーザーガイド](user-guide.md)を参照してください。