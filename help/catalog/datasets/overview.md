---
keywords: Experience Platform；ホーム；人気のトピック；データの場所；データの場所；データの場所；データ管理；データ管理；系列；系列；データ型；データ型；データ型
solution: Experience Platform
title: データセットの概要
topic-legacy: datasets
description: このドキュメントでは、Experience Platform のデータセットのおおまかな概要を説明します。
exl-id: 51ecefb0-a699-4b1a-80f1-26c6ba92fcbf
source-git-commit: 03e7863f38b882a2fbf6ba0de1755e1924e8e228
workflow-type: tm+mt
source-wordcount: '711'
ht-degree: 43%

---

# データセットの概要

Adobe Experience Platformに正常に取り込まれたすべてのデータは、 [!DNL Data Lake] データセットとして。 データセットは、通常、スキーマ（列）とフィールド（行）を含むテーブルであるデータコレクションのストレージと管理をおこなう構成体です。データセットには、保存するデータの様々な側面を記述したメタデータも含まれます。

このドキュメントでは、 [!DNL Experience Platform].

## データセットの作成とメタデータの追跡

[!DNL Catalog Service] は、内のデータの場所と系列のレコードシステムです [!DNL Experience Platform]、およびは、データセットの作成と管理に使用されます。 [!DNL Catalog] は、各データセットのメタデータを追跡します。このメタデータには、 [!DNL Experience Data Model] (XDM) データセットが準拠するスキーマ（次の節で説明）と、そのデータセットに取り込まれるレコードの数。

詳しくは、「[カタログサービスの概要](../home.md)」を参照してください。

## データセットデータに対する制限の適用

[!DNL Experience Data Model] (XDM) は標準化されたフレームワークで、 [!DNL Platform] は顧客体験データを整理します。 に取り込まれるすべてのデータ [!DNL Platform] で保持するには、事前定義済みの XDM スキーマに準拠している必要があります。 [!DNL Data Lake] データセットとして。

すべてのデータセットには、保存できるデータの形式と構造を制限する XDM スキーマへの参照が含まれています。データセットの XDM スキーマに準拠していないデータセットにデータをアップロードしようとすると、データの取得に失敗します。

XDM について詳しくは、「[XDM システムの概要](../../xdm/home.md)」を参照してください。

## データセットへのデータの取り込み

Adobe Experience Platform Data Ingestion は、複数の方法を表します。 [!DNL Platform] 様々なソースからデータを取り込みます。 取得方法に関係なく、正常に取り込まれたデータはすべてバッチファイルに変換されます。バッチとは、1 つの単位として取り込まれる 1 つ以上のファイルで構成されるデータの単位です。その後、これらのバッチファイルは専用のデータセットに追加され、 [!DNL Data Lake].

詳しくは、「[データ取得の概要](../../ingestion/home.md)」を参照してください。

## データセットへの使用状況ラベルの適用

Adobe Experience Platform データガバナンスを使用すると、データの使用に適した規制、制限、ポリシーへのコンプライアンスを確保するために、顧客データを管理できます。データガバナンスフレームワークを使用すると、使用状況ラベルを適用し、そのデータに適用される使用ポリシーに従ってデータを分類できます。

データ使用状況ラベルは、データセット全体または個々のデータセットフィールドに適用できます。データセットレベルで追加されたラベルは、そのデータセット内のすべてのフィールドに継承されます。

このサービスについて詳しくは、「[データガバナンスの概要](../../data-governance/home.md)」を参照してください。で使用状況ラベルを使用する手順については、 [!DNL Platform]次のガイドを参照してください。

* [UI でのラベルの管理](../../data-governance/labels/user-guide.md)
* [API でのデータセットラベルの管理](../../data-governance/labels/dataset-api.md)

## ダウンストリームのデータセット [!DNL Platform] サービス

データセットを使用して取り込んだデータを保存すると、それらのデータセットがダウンストリームで使用されます [!DNL Platform] 顧客プロファイルを更新し、機械学習を通じてインサイトを得るためのサービスなど。

様々な操作にデータセットを使用するダウンストリームサービスのリストを次に示します。詳しくは、各サービスのドキュメントを参照してください。

* [[!DNL Data Access API]](../../data-access/home.md):データセット内に保存されたファイルの内容にアクセスしてダウンロードできます。
* [Adobe Experience Platform ID サービス](../../identity-service/home.md)：デバイスやシステム間で ID をブリッジし、準拠する XDM スキーマで定義された ID フィールドに基づいてデータセットをリンクします。
* [[!DNL Real-time Customer Profile]](../../profile/home.md):活用 [!DNL Identity Service] を使用して、データセットから詳細な顧客プロファイルをリアルタイムで作成できます。 [!DNL Real-time Customer Profile] からデータを取り込む [!DNL Data Lake] とは、顧客プロファイルを独立したデータストアに保持します。
* [Adobe Experience Platform Segmentation Service](../../segmentation/home.md):セグメントを構築し、 [!DNL Real-time Customer Profile] データ。 これらのオーディエンスは、 [!DNL Data Lake].
* [Adobe Experience Platform Data Science Workspace](../../data-science-workspace/home.md)：機械学習と人工知能を使用して、大規模なデータセットからインサイトを引き出します。
* [Adobe Experience Platform Query Service](../../query-service/home.md):標準の SQL を使用して、でデータをクエリできます。 [!DNL Experience Platform]（内の任意のデータセットを結合） [!DNL Data Lake] クエリ結果を新しいデータセットとして取得し、レポートで使用します。 [!DNL Data Science Workspace]または [!DNL Real-time Customer Profile].

## 次の手順

このドキュメントでは、でのデータセットの主な使用方法を紹介しました。 [!DNL Experience Platform]、 [!DNL Platform] データセットを利用するサービス。 でのデータセットの多くの使用方法の詳細 [!DNL Platform]の場合は、この概要でリンクされているサービスドキュメントを確認してください。

内でデータセットを操作する手順については、 [!DNL Experience Platform] UI( [データセットユーザーガイド](user-guide.md).
