---
keywords: Experience Platform；機械学習モデル；Data Science Workspace；リアルタイム顧客プロファイル；人気のあるトピック；機械学習のインサイト
solution: Experience Platform
title: リアルタイム顧客プロファイルと機械学習のインサイトの強化
topic-legacy: tutorial
type: Tutorial
description: このドキュメントでは、リアルタイム顧客プロファイルと機械学習のインサイトを強化する方法に関するガイドを提供します。
exl-id: 397023c9-383d-4a21-b58a-0f920631ac56
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '640'
ht-degree: 22%

---

# [!DNL Real-time Customer Profile] を機械学習の洞察で強化

Adobe Experience Platform [!DNL Data Science Workspace] は、機械学習モデルを作成、評価、利用してデータ予測とインサイトを生成するためのツールとリソースを提供します。 機械学習の洞察が [!DNL Profile] 対応のデータセットに取り込まれると、同じデータが [!DNL Profile] レコードとして取り込まれ、[!DNL Adobe Experience Platform Segmentation Service] を使用してセグメント化できます。 プロファイルと時系列データが取得されると、リアルタイム顧客プロファイルは、既存のデータと結合して和集合表示を更新する前に、ストリーミングセグメント化と呼ばれる継続的なプロセスを通じて、そのデータをセグメントに含めるか除外するかを自動的に決定します。その結果、顧客がブランドとやり取りする際に、瞬時に計算をおこない、顧客に対して強化された個別的なエクスペリエンスを提供する意思決定をおこなうことができます。

このドキュメントでは、[!DNL Real-time Customer Profile] と機械学習の洞察を強化するためのチュートリアルへのリンクを提供します。

## はじめに

以下のチュートリアルを完了するには、[!DNL Profile] データの取り込みとセグメントの作成に関する十分な知識が必要です。 このチュートリアルを開始する前に、次のサービスのドキュメントを確認してください。

- [[!DNL Real-time Customer Profile]](../../profile/home.md):複数のソースからの集計データに基づいて、各顧客の完全で統一された表現を提供します。
- [[!DNL Identity Service]](../../identity-service/home.md):Platform に取 [!DNL Real-time Customer Profile] り込まれる様々なデータソースの ID を関連付けることで、を有効にします。
- [[!DNL Experience Data Model (XDM)]](../../xdm/home.md):Platform が顧客体験データを整理する際に使用する標準化されたフレームワーク。

上記のドキュメントに加え、スキーマおよびスキーマエディターに関する次のガイドも確認することを強くお勧めします。

- [スキーマ構成の基本](../../xdm/schema/composition.md):XDM スキーマ、構築要素、原則およびで使用するスキーマを構成するためのベストプラクティスについて説明し [!DNL Experience Platform]ます。
- [スキーマエディターのチュートリアル](../../xdm/tutorials/create-schema-ui.md):内のスキーマエディターを使用してスキーマを作成する詳細な手順を説明しま [!DNL Experience Platform]す。

## 出力スキーマとデータセットの作成と設定 {#create-an-output-schema-and-dataset}

スコアリングの洞察で [!DNL Real-time Customer Profile] を強化する最初の手順は、データが定義する実際のオブジェクト（人など）を知ることです。 データを理解することで、リレーショナルデータベースの設計と同様に、構造を説明し、意味を追加する設計が可能になります。

クラスを割り当てることで、スキーマの構成が開始されます。クラスは、スキーマに含まれるデータ（レコードまたは時系列）の行動面を定義します。独自のスキーマの作成を開始するには、[ スキーマエディター ](../../xdm/tutorials/create-schema-ui.md) を使用したスキーマの作成に関するチュートリアルの手順に従います。 [!DNL Profile] のデータセットを有効にする前に、データセットのスキーマにプライマリ ID フィールドを設定し、[!DNL Profile] のスキーマを有効にする必要があります。 データが [!DNL Profile] 対応のデータセットに取り込まれると、同じデータが [!DNL Profile] レコードとして取り込まれます。

代わりに [!DNL Schema Registry] API を使用してスキーマを作成する場合は、まず [API](../../xdm/tutorials/create-schema-api.md) を使用したスキーマの作成に関するチュートリアルを読む前に、『[[!DNL Schema Registry]  開発者ガイド ](../../xdm/api/getting-started.md)』を読んでください。

スキーマとデータセットを準備したら、適切なモデルを使用してスコアリングの実行を実行し、スコアリングデータを生成してデータセットに取り込むことができます。

## [!DNL Segment Builder] {#create-segments-using-the-segment-builder}

スコアリングデータインサイトを生成して [!DNL Profile] 対応のデータセットに取り込んだ後、 [!DNL Segment Builder] を使用して動的セグメントを作成できます。

[!DNL Segment Builder] は、[!DNL Profile] データ要素を操作できる豊富なワークスペースを提供します。 ワークスペースには、ルールを作成および編集するための直感的なコントロールがあります。例えば、データプロパティを表示する際に使用するドラッグ＆ドロップタイルなどです。[[!DNL Segment Builder]  ユーザーガイド ](../../segmentation/ui/segment-builder.md) に従って、以下の内容を確認します。

- 属性、イベントおよび既存のオーディエンスの組み合わせを構成要素として使用して、セグメント定義を作成する。
- ルールビルダーキャンバスとコンテナを使用して、セグメントルールの実行順序を制御する。
- 見込みオーディエンスの推定を表示し、必要に応じてセグメント定義を調整できます。
- スケジュールに沿ったセグメント化に対してすべてのセグメント定義を有効にする。
- ストリーミングセグメント化に対して指定したセグメント定義を有効にする。

## 次の手順 {#next-steps}

セグメントと [!DNL Segment Builder] について詳しくは、「[ セグメント化サービスの概要 ](../../segmentation/home.md)」を参照してください。

[!DNL Real-time Customer Profile] の詳細については、[ リアルタイム顧客プロファイルの概要 ](../../profile/home.md) を参照してください。
