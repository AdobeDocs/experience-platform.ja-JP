---
keywords: Experience Platform;ホーム;人気のトピック;データの場所;データ場所;データ管理;データの管理;系列;系列;データ型;データの型;データのタイプ;データタイプ
solution: Experience Platform
title: データセットの概要
description: このドキュメントでは、Experience Platform のデータセットのおおまかな概要を説明します。
exl-id: 51ecefb0-a699-4b1a-80f1-26c6ba92fcbf
source-git-commit: 74867f56ee13430cbfd9083a916b7167a9a24c01
workflow-type: tm+mt
source-wordcount: '784'
ht-degree: 100%

---

# データセットの概要

Adobe Experience Platform に正常に取り込まれたすべてのデータは、[!DNL Data Lake] 内にデータセットとして保持されます。データセットは、通常、スキーマ（列）とフィールド（行）を含むテーブルであるデータコレクションのストレージと管理をおこなう構成体です。データセットには、保存するデータの様々な側面を記述したメタデータも含まれます。

このドキュメントでは、[!DNL Experience Platform] のデータセットのおおまかな概要を説明します。

## データセットの作成とメタデータの追跡

[!DNL Catalog Service] は、[!DNL Experience Platform] 内のデータの場所と系列のレコードシステムであり、データセットの作成と管理に使用されます。[!DNL Catalog] は各データセットのメタデータを追跡します。このメタデータには、データセットが準拠する [!DNL Experience Data Model]（XDM）スキーマへの参照（次の節で説明）と、そのデータセットに取り込まれるレコード数が含まれます。

詳しくは、「[カタログサービスの概要](../home.md)」を参照してください。

## データセットデータに対する制限の適用

[!DNL Experience Data Model]（XDM）は、[!DNL Platform] が顧客体験データを整理する際に使用する標準化されたフレームワークです。[!DNL Platform] に取り込まれるすべてのデータは、[!DNL Data Lake] にデータセットとして保持する前に、事前定義済みの XDM スキーマに準拠している必要があります。

すべてのデータセットには、保存できるデータの形式と構造を制限する XDM スキーマへの参照が含まれています。データセットの XDM スキーマに準拠していないデータセットにデータをアップロードしようとすると、データの取得に失敗します。

XDM について詳しくは、「[XDM システムの概要](../../xdm/home.md)」を参照してください。

## データセットへのデータの取り込み

Adobe Experience Platform のデータ取得は、[!DNL Platform] が様々なソースからデータを取り込む複数の方法を表します。取得方法に関係なく、正常に取り込まれたデータはすべてバッチファイルに変換されます。バッチとは、1 つの単位として取り込まれる 1 つ以上のファイルで構成されるデータの単位です。その後、これらのバッチファイルは、専用のデータセットに追加され、[!DNL Data Lake] 内で保持されます。

詳しくは、「[データ取得の概要](../../ingestion/home.md)」を参照してください。

## データセットへの使用状況ラベルの適用

Adobe Experience Platform データガバナンスを使用すると、データの使用に適した規制、制限、ポリシーへのコンプライアンスを確保するために、顧客データを管理できます。データガバナンスフレームワークを使用すると、使用状況ラベルを適用し、そのデータに適用される使用ポリシーに従ってデータを分類できます。

>[!IMPORTANT]
>
>データセットレベルでのラベルの適用は、データガバナンスのユースケースでのみサポートされています。データのアクセスポリシーを作成しようとしている場合は、データセットが基づいている[スキーマにラベルを適用](../../xdm/tutorials/labels.md)する必要があります。詳しくは、[属性ベースのアクセス制御](../../access-control/abac/overview.md)の概要を参照してください。

データ使用状況ラベルは、データセット全体または個々のデータセットフィールドに適用できます。データセットレベルで追加されたラベルは、そのデータセット内のすべてのフィールドに継承されます。

このサービスについて詳しくは、「[データガバナンスの概要](../../data-governance/home.md)」を参照してください。[!DNL Platform] で使用状況ラベルを使用する手順については、次のガイドを参照してください。

* [UI でのラベルの管理](../../data-governance/labels/user-guide.md)
* [API でのデータセットラベルの管理](../../data-governance/labels/dataset-api.md)

## ダウンストリーム [!DNL Platform] サービスのデータセット

データセットを使用して、取り込んだデータを保存すると、ダウンストリーム [!DNL Platform] サービスでこれらのデータセットが使用され、顧客プロファイルの更新や機械学習を通じたインサイトの取得などが行われます。

様々な操作にデータセットを使用するダウンストリームサービスのリストを次に示します。詳しくは、各サービスのドキュメントを参照してください。

* [[!DNL Data Access API]](../../data-access/home.md)：データセット内に保存されたファイルの内容にアクセスしてダウンロードできます。
* [Adobe Experience Platform ID サービス](../../identity-service/home.md)：デバイスやシステム間で ID をブリッジし、準拠する XDM スキーマで定義された ID フィールドに基づいてデータセットをリンクします。
* [[!DNL Real-Time Customer Profile]](../../profile/home.md)：[!DNL Identity Service] を使用して、データセットから詳細な顧客プロファイルをリアルタイムで作成できます。[!DNL Real-Time Customer Profile] は [!DNL Data Lake] からデータを取り込み、顧客プロファイルを独立したデータストアに保持します。
* [Adobe Experience Platform セグメント化サービス](../../segmentation/home.md)：[!DNL Real-Time Customer Profile] データからセグメントを作成し、オーディエンスを生成できるようにします。これらのオーディエンスは、[!DNL Data Lake] 内の独自のデータセットに書き出すことができます。
* [Adobe Experience Platform Data Science Workspace](../../data-science-workspace/home.md)：機械学習と人工知能を使用して、大規模なデータセットからインサイトを引き出します。
* [Adobe Experience Platform クエリサービス](../../query-service/home.md)：[!DNL Experience Platform] で標準の SQL を使用してデータをクエリし、[!DNL Data Lake] 内のデータセットと結合したり、[!DNL Data Science Workspace]、または [!DNL Real-Time Customer Profile] で使用する新しいデータセットとしてクエリ結果を取得したりできます。
* [Adobe Experience Platform Destinations Service](../../destinations/home.md)：レポーティングやデータサイエンスアクティビティ用に、[データセット](/help/destinations/ui/export-datasets.md)を希望のクラウドストレージやメールマーケティング宛先に書き出すことができます。

## 次の手順

このドキュメントでは、[!DNL Experience Platform] でデータセットを使用する主要な方法と、データセットを活用した様々な [!DNL Platform] サービスについて説明しました。[!DNL Platform] でデータセットを使用する様々な方法について詳しくは、この概要でリンクされているサービスドキュメントを参照してください。

[!DNL Experience Platform] UI でデータセットを操作する手順については、[データセットのユーザーガイド](user-guide.md)を参照してください。
