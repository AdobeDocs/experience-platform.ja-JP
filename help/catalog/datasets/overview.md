---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: データセットの概要
topic: datasets
translation-type: tm+mt
source-git-commit: bfbf2074a9dcadd809de043d62f7d2ddaa7c7b31
workflow-type: tm+mt
source-wordcount: '711'
ht-degree: 2%

---


# データセットの概要

Adobe Experience Platformに正常に取り込まれたすべてのデータは、データセット内に保持 [!DNL Data Lake] されます。 データセットは、スキーマ（列）とフィールド（行）を含むテーブルなど、データの集まりのストレージと管理の構成体です。 データセットには、保存するデータの様々な要素を説明するメタデータも含まれます。

このドキュメントでは、のデータセットの概要を説明し [!DNL Experience Platform]ます。

## データセットの作成とメタデータの追跡

[!DNL Catalog Service] は、内のデータの場所と系列の記録システムで [!DNL Experience Platform]、データセットの作成と管理に使用されます。 [!DNL Catalog] 各データセットのメタデータを追跡します。このメタデータには、データセットが準拠する [!DNL Experience Data Model] (XDM)スキーマへの参照（次の節で説明）と、そのデータセットに取り込まれるレコード数が含まれます。

See the [Catalog Service overview](../home.md) for more information.

## データセットデータに対する制約の適用

[!DNL Experience Data Model] (XDM)は、顧客体験データを [!DNL Platform] 編成する標準化されたフレームワークです。 に取り込まれるすべてのデータは、あらかじめ定義されたXDMスキーマに準拠していなければ、データセットとしてデータセットに保持され [!DNL Platform][!DNL Data Lake] ません。

すべてのデータセットには、格納できるデータの形式と構造を制約するXDMスキーマへの参照が含まれます。 データセットのXDMスキーマに適合しないデータセットにデータをアップロードしようとすると、取り込みに失敗します。

XDMについて詳しくは、「 [XDM System overview](../../xdm/home.md)」を参照してください。

## データセットへのデータの取り込み

Adobe Experience Platformデータ取り込みは、様々なソースからデータを [!DNL Platform] 取り込む複数の方法を表します。 取り込み方法にかかわらず、正常に取り込まれたデータはすべてバッチファイルに変換されます。 バッチとは、1 つの単位として取り込まれる 1 つ以上のファイルで構成されるデータの単位です。これらのバッチファイルは、専用のデータセットに追加され、内で保持され [!DNL Data Lake]ます。

See the [Data Ingestion overview](../../ingestion/home.md) for more information.

## 使用ラベルをデータセットに適用する

Adobe Experience Platform [!DNL Data Governance] allows you to manage customer data in order to ensure compliance with regulations, restrictions, and policies applicable to data use. Data Usage Labeling and Enforcement(DULE)を中核的なフレームワークとして使用すると、使用状況ラベルを適用して、そのデータに適用される使用状況ポリシーに従ってデータを分類できます。 [!DNL Data Governance]

データ使用量ラベルは、データセット全体または個々のデータセットフィールドに適用できます。 データセットレベルで追加されたラベルは、そのデータセット内のすべてのフィールドに継承されます。

サービスの詳細については、 [データ管理の概要](../../data-governance/home.md) （英語）を参照してください。 で使用状況ラベルを使用する手順については、次のガイドを参照し [!DNL Platform]てください。

* [UIでのラベルの管理](../../data-governance/labels/user-guide.md)
* [APIでのラベルの管理](../../data-governance/labels/api.md)

## ダウンストリーム・ [!DNL Platform] サービスのデータセット

データセットを使用して取り込んだデータを保存すると、それらのデータセットはダウンストリーム [!DNL Platform] サービスで使用され、顧客のプロファイルを更新し、機械学習を通じて洞察を得ることができます。

様々な操作にデータセットを使用するダウンストリームサービスのリストを次に示します。 詳しくは、各サービスのドキュメントを参照してください。

* [!DNL Data Access API](../../data-access/home.md): データセットに保存されたファイルの内容にアクセスし、ダウンロードできます。
* [Adobe Experience PlatformIDサービス](../../identity-service/home.md): デバイスとシステム間のIDをブリッジし、XDMスキーマが準拠するIDフィールドに基づいてデータセットをリンクします。
* [!DNL Real-time Customer Profile](../../profile/home.md): を活用 [!DNL Identity Service] して、データセットから詳細な顧客プロファイルをリアルタイムで作成します。 [!DNL Real-time Customer Profile] 顧客プロファイルを個別のデータストアから取り込 [!DNL Data Lake] み、引き続き顧客データを保持します。
* [Adobe Experience Platformセグメントサービス](../../segmentation/home.md): セグメントを作成し、 [!DNL Real-time Customer Profile] データからオーディエンスを生成できます。 これらのオーディエンスは、内の独自のデータセットにエクスポートでき [!DNL Data Lake]ます。
* [Adobe Experience Platformデータサイエンスワークスペース](../../data-science-workspace/home.md): 機械学習と人工知能を使用して、大規模なデータセットの洞察を明らかにします。
* [Adobe Experience Platformクエリサービス](../../query-service/home.md): で標準のSQLからクエリへのデータ送信を使用でき [!DNL Experience Platform]ます。クエリ内の任意のデータセットを結合し、レポート、 [!DNL Data Lake] またはで使用する新しいデータセットとして結果を取得でき [!DNL Data Science Workspace][!DNL Real-time Customer Profile]ます。
* [Adobe Experience Platform判定サービス](../../decisioning-service/home.md): 有効 [!DNL Real-time Customer Profile] なデータセットから取り込む行動データに基づいて、一連のオプションから顧客が最も可能性の高い選択肢を決定す [!DNL Profile] るために活用します。

## 次の手順

このドキュメントを読むことで、のデータセットの主な使用方法 [!DNL Experience Platform]と、データセットを利用する様々な [!DNL Platform] サービスについて説明します。 でのデータセットの多くの使用方法の詳細については、この概要全体を通してリンクされたサービスドキュメントを参照し [!DNL Platform]てください。

UI内でデータセットを操作する手順については、『デー [!DNL Experience Platform] タセットユーザガイド [](user-guide.md)』を参照してください。