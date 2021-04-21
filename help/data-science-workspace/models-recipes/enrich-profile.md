---
keywords: Experience Platform；機械学習モデル；データ科学ワークスペース；リアルタイム顧客プロファイル；人気の高いトピック；機械学習インサイト
solution: Experience Platform
title: 機械学習インサイトによるリアルタイムの顧客プロファイルの強化
topic-legacy: tutorial
type: Tutorial
description: このドキュメントでは、リアルタイム顧客プロファイルを機械学習インサイトに強化する方法のガイドを提供します。
exl-id: 397023c9-383d-4a21-b58a-0f920631ac56
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '640'
ht-degree: 22%

---

# 機械学習のインサイトで[!DNL Real-time Customer Profile]を拡張

Adobe Experience Platform[!DNL Data Science Workspace]は、機械学習モデルを作成、評価、利用し、データの予測や洞察を生み出すためのツールとリソースを提供します。 機械学習インサイトが[!DNL Profile]対応のデータセットに取り込まれると、同じデータも[!DNL Profile]レコードとして取り込まれ、[!DNL Adobe Experience Platform Segmentation Service]を使用してセグメント化できます。 プロファイルと時系列データが取得されると、リアルタイム顧客プロファイルは、既存のデータと結合して和集合表示を更新する前に、ストリーミングセグメント化と呼ばれる継続的なプロセスを通じて、そのデータをセグメントに含めるか除外するかを自動的に決定します。その結果、顧客がブランドとやり取りする際に、瞬時に計算をおこない、顧客に対して強化された個別的なエクスペリエンスを提供する意思決定をおこなうことができます。

このドキュメントには、[!DNL Real-time Customer Profile]を機械学習のインサイトで拡張できるチュートリアルへのリンクが含まれています。

## はじめに

以下のチュートリアルを完了するには、[!DNL Profile]データの取り込みとセグメントの作成に関する作業的な理解が必要です。 このチュートリアルを開始する前に、次のサービスのドキュメントを確認してください。

- [[!DNL Real-time Customer Profile]](../../profile/home.md):複数のソースからの集計データに基づいて、各顧客の完全で統一された表現を提供します。
- [[!DNL Identity Service]](../../identity-service/home.md):プラットフォーム [!DNL Real-time Customer Profile] に取り込まれる異なるデータソースからIDをブリッジすることで有効にします。
- [[!DNL Experience Data Model (XDM)]](../../xdm/home.md):プラットフォームが顧客体験データを編成する際に使用する標準化されたフレームワーク。

上記のドキュメントに加え、スキーマおよびスキーマエディターに関する次のガイドも確認することを強くお勧めします。

- [スキーマ構成の基本](../../xdm/schema/composition.md):で使用するスキーマを構成するためのXDMスキーマ、構築ブロック、原則、ベストプラクティスについて説明 [!DNL Experience Platform]します。
- [スキーマエディタのチュートリアル](../../xdm/tutorials/create-schema-ui.md):内のスキーマエディタを使用してスキーマを作成する詳細な手順を説明 [!DNL Experience Platform]します。

## 出力スキーマとデータセットの作成と設定{#create-an-output-schema-and-dataset}

スコアリングインサイトで[!DNL Real-time Customer Profile]を豊かにする最初のステップは、データが定義する現実世界のオブジェクト（人物など）を知ることです。 データに関する知識があれば、リレーショナルデータベースの設計と同様に、構造を記述し、意味を付加するための設計が可能になります。

クラスを割り当てることで、スキーマの構成が開始されます。クラスは、スキーマに含まれるデータ（レコードまたは時系列）の行動面を定義します。独自のスキーマを作成する開始を作成するには、[スキーマエディタ](../../xdm/tutorials/create-schema-ui.md)を使用したスキーマの作成のチュートリアルの手順に従ってください。 [!DNL Profile]のデータセットを有効にする前に、データセットのスキーマにプライマリIDフィールドを設定し、[!DNL Profile]のスキーマを有効にする必要があります。 データを[!DNL Profile]対応データセットに取り込むと、そのデータも[!DNL Profile]レコードとして取り込まれます。

代わりに[!DNL Schema Registry] APIを使用してスキーマを構成する場合は、[API](../../xdm/tutorials/create-schema-api.md)を使用したスキーマの作成に関するチュートリアルを開始する前に、[[!DNL Schema Registry] 開発者ガイド](../../xdm/api/getting-started.md)を読んで、開始をお勧めします。

スキーマとデータセットを準備したら、適切なモデルを使用してスコアリングを実行し、スコアリングデータを生成してデータセットに取り込むことができます。

## [!DNL Segment Builder] {#create-segments-using-the-segment-builder}

スコアリングデータインサイトを生成し、[!DNL Profile]対応のデータセットに取り込んだ後、[!DNL Segment Builder]を使用して動的なセグメントを作成できます。

[!DNL Segment Builder]は、[!DNL Profile]データ要素とやり取りできるリッチワークスペースを提供します。 ワークスペースには、ルールを作成および編集するための直感的なコントロールがあります。例えば、データプロパティを表示する際に使用するドラッグ＆ドロップタイルなどです。[[!DNL Segment Builder] ユーザーガイド](../../segmentation/ui/segment-builder.md)に従って、次の情報を確認します。

- 属性、イベント、および既存のオーディエンスの組み合わせを構成要素として使用して、セグメント定義を作成する。
- [ルールビルダ]キャンバスとコンテナを使用して、セグメントルールの実行順序を制御する。
- 見込みオーディエンスの予測を表示し、必要に応じてセグメント定義を調整できます。
- スケジュール済みセグメントのすべてのセグメント定義を有効にする。
- ストリーミングセグメントに対して指定したセグメント定義を有効にする。

## 次の手順 {#next-steps}

セグメントと[!DNL Segment Builder]について詳しくは、[Segmentation Service overview](../../segmentation/home.md)を参照してください。

[!DNL Real-time Customer Profile]の詳細については、[リアルタイム顧客プロファイルの概要](../../profile/home.md)を参照してください
