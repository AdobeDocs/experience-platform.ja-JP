---
keywords: Experience Platform；機械学習モデル；Data Science Workspace；リアルタイム顧客プロファイル；人気のトピック；機械学習のインサイト
solution: Experience Platform
title: 機械学習のインサイトによるリアルタイム顧客プロファイルの強化
topic-legacy: tutorial
type: Tutorial
description: このドキュメントでは、リアルタイム顧客プロファイルと機械学習のインサイトを強化する方法に関するガイドを提供します。
exl-id: 397023c9-383d-4a21-b58a-0f920631ac56
source-git-commit: 89a9b64f2fb238c08a281f29a035ce0b24b34804
workflow-type: tm+mt
source-wordcount: '577'
ht-degree: 15%

---

# 強化 [!DNL Real-time Customer Profile] 機械学習のインサイト

Adobe Experience Platform [!DNL Data Science Workspace] は、機械学習モデルを作成、評価、利用してデータ予測とインサイトを生成するためのツールとリソースを提供します。 機械学習のインサイトが [!DNL Profile]-enabled データセット。同じデータも [!DNL Profile] 次を使用してセグメント化できるレコード： [!DNL Adobe Experience Platform Segmentation Service].

このドキュメントでは、 [!DNL Real-time Customer Profile] を機械学習のインサイトと共に使用します。

## はじめに

以下のチュートリアルを完了するには、取り込みに関する十分な知識が必要です。 [!DNL Profile] データを作成し、セグメントを作成します。 このチュートリアルを開始する前に、次のサービスのドキュメントを確認してください。

- [[!DNL Real-time Customer Profile]](../../profile/home.md):複数のソースからの集計データに基づいて、各顧客を完全かつ統一された表現で表します。
- [[!DNL Identity Service]](../../identity-service/home.md):有効 [!DNL Real-time Customer Profile] Platform に取り込まれる様々なデータソースから id を紐付けることで実現できます。
- [[!DNL Experience Data Model (XDM)]](../../xdm/home.md)： Platform が顧客体験データを整理するための標準的なフレームワーク。

上記のドキュメントに加え、スキーマおよびスキーマエディターに関する次のガイドも確認することを強くお勧めします。

- [スキーマ構成の基本](../../xdm/schema/composition.md):XDM スキーマ、構築要素、原則およびで使用するスキーマを構成するためのベストプラクティスについて説明します。 [!DNL Experience Platform].
- [スキーマエディターのチュートリアル](../../xdm/tutorials/create-schema-ui.md):内のスキーマエディターを使用してスキーマを作成する詳細な手順を説明します。 [!DNL Experience Platform].

## 出力スキーマとデータセットの作成と設定 {#create-an-output-schema-and-dataset}

富化への第一歩 [!DNL Real-time Customer Profile] スコアリングインサイトを使用すると、データが定義する実際のオブジェクト（人など）を把握できます。 データを理解することで、リレーショナルデータベースの設計と同様に、構造を説明し、意味を追加する設計をおこなうことができます。

クラスを割り当てることで、スキーマの構成が開始されます。クラスは、スキーマに含まれるデータ（レコードまたは時系列）の行動面を定義します。独自のスキーマの作成を開始するには、次のチュートリアルの手順に従います： [スキーマエディターを使用したスキーマの作成](../../xdm/tutorials/create-schema-ui.md). データセットを有効にする前に、 [!DNL Profile]の場合は、データセットのスキーマにプライマリ id フィールドを設定し、次にのスキーマを有効にする必要があります。 [!DNL Profile]. データが [!DNL Profile]-enabled データセット。同じデータも [!DNL Profile] レコード。

を使用してスキーマを作成する場合は、 [!DNL Schema Registry] API を使用する場合は、まず [[!DNL Schema Registry] 開発者ガイド](../../xdm/api/getting-started.md) ～に関するチュートリアルを試みる前に [API を使用したスキーマの作成](../../xdm/tutorials/create-schema-api.md).

スキーマとデータセットの準備が整ったら、適切なモデルを使用してスコアリングの実行を実行し、スコアリングデータを生成してデータセットに取り込むことができます。

## セグメントの作成 [!DNL Segment Builder] {#create-segments-using-the-segment-builder}

スコアリングデータインサイトを生成し、 [!DNL Profile]有効なデータセットの場合は、 [!DNL Segment Builder].

この [!DNL Segment Builder] は、操作できる豊富なワークスペースを提供します。 [!DNL Profile] データ要素。 ワークスペースには、ルールを作成および編集するための直感的なコントロールがあります。例えば、データプロパティを表示する際に使用するドラッグ＆ドロップタイルなどです。フォロー： [[!DNL Segment Builder] ユーザーガイド](../../segmentation/ui/segment-builder.md) 詳しくは、以下を参照してください。

- 属性、イベントおよび既存のオーディエンスの組み合わせを構成要素として使用して、セグメント定義を作成する。
- ルールビルダーキャンバスとコンテナを使用した、セグメントルールの実行順序の制御。
- 見込みオーディエンスの推定値を表示し、必要に応じてセグメント定義を調整できます。
- スケジュールに沿ったセグメント化に対してすべてのセグメント定義を有効にする。
- ストリーミングセグメント化に対して、指定したセグメント定義を有効にする。

## 次の手順 {#next-steps}

セグメントと [!DNL Segment Builder]、 [セグメント化サービスの概要](../../segmentation/home.md).

詳しくは、以下を参照してください。 [!DNL Real-time Customer Profile]、 [リアルタイム顧客プロファイルの概要](../../profile/home.md)
