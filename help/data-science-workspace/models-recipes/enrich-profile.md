---
keywords: Experience Platform；機械学習モデル；Data Science Workspace；リアルタイム顧客プロファイル；人気のトピック；機械学習のインサイト
solution: Experience Platform
title: 機械学習のインサイトによるリアルタイム顧客プロファイルのエンリッチメント
type: Tutorial
description: このドキュメントでは、機械学習のインサイトを使用してリアルタイム顧客プロファイルを強化する方法に関するガイドを提供します。
exl-id: 397023c9-383d-4a21-b58a-0f920631ac56
source-git-commit: 86e6924078c115fb032ce39cd678f1d9c622e297
workflow-type: tm+mt
source-wordcount: '577'
ht-degree: 15%

---

# 機械学習のインサイトによる [!DNL Real-Time Customer Profile] の強化

Adobe Experience Platform [!DNL Data Science Workspace] は、機械学習モデルを作成、評価および活用してデータの予測とインサイトを生み出すためのツールとリソースを提供します。 機械学習のインサイトが [!DNL Profile] 対応データセットに取り込まれると、その同じデータが [!DNL Profile] レコードとして取り込まれ、[!DNL Adobe Experience Platform Segmentation Service] を使用してセグメント化できます。

このドキュメントでは、機械学習のインサイトを活用して [!DNL Real-Time Customer Profile] ーザーを強化できるチュートリアルへのリンクを提供します。

## はじめに

以下のチュートリアルを完了するには、[!DNL Profile] データの取り込みとセグメントの作成に関する十分な知識が必要です。 このチュートリアルを開始する前に、次のサービスのドキュメントを確認してください。

- [[!DNL Real-Time Customer Profile]](../../profile/home.md)：複数のソースから集計したデータに基づいて、個々の顧客の完全で統一された表現を提供します。
- [[!DNL Identity Service]](../../identity-service/home.md):Platform に取り込まれる様々なデータソースの ID を結合することで、[!DNL Real-Time Customer Profile] を有効にします。
- [[!DNL Experience Data Model (XDM)]](../../xdm/home.md): Platform が顧客体験データを整理する際に使用する標準化されたフレームワーク。

上記のドキュメントに加え、スキーマおよびスキーマエディターに関する次のガイドも確認することを強くお勧めします。

- [ スキーマ構成の基本 ](../../xdm/schema/composition.md):[!DNL Experience Platform] で使用するスキーマを構成するための XDM スキーマ、構成要素、原則およびベストプラクティスについて説明します。
- [ スキーマエディターのチュートリアル ](../../xdm/tutorials/create-schema-ui.md):[!DNL Experience Platform] でスキーマエディターを使用してスキーマを作成する詳細な手順を提供します。

## 出力スキーマとデータセットの作成と設定 {#create-an-output-schema-and-dataset}

スコアリングインサイトで [!DNL Real-Time Customer Profile] を充実させる最初のステップは、データが定義する現実世界のオブジェクト（人物など）を知ることです。 データを理解することで、リレーショナルデータベースを設計するのと同様に、意味を追加する構造を記述し、設計できます。

クラスを割り当てることで、スキーマの構成が開始されます。クラスは、スキーマに含まれるデータ（レコードまたは時系列）の行動面を定義します。独自のスキーマの作成を開始するには、[ スキーマエディターを使用したスキーマの作成 ](../../xdm/tutorials/create-schema-ui.md) に関するチュートリアルの手順に従います。 データセットの [!DNL Profile] を有効にする前に、データセットのスキーマをプライマリ ID フィールドを持つように設定してから、スキーマの [!DNL Profile] を有効にする必要があります。 データが [!DNL Profile] 対応データセットに取り込まれると、その同じデータも [!DNL Profile] レコードとして取り込まれます。

[!DNL Schema Registry] API を使用してスキーマを作成する場合は、[[!DNL Schema Registry] 開発者ガイド](../../xdm/api/getting-started.md)を参照してから、[API を使用したスキーマの作成](../../xdm/tutorials/create-schema-api.md)に関するチュートリアルを試してください。

スキーマとデータセットを準備したら、適切なモデルを使用してスコアリング実行を実行することで、スコアリングデータを生成してデータセットに取り込むことができます。

## [!DNL Segment Builder] を使用したセグメントの作成 {#create-segments-using-the-segment-builder}

スコアリングデータインサイトを生成して [!DNL Profile] 対応データセットに取り込んだら、[!DNL Segment Builder] を使用して動的セグメントを作成できます。

[!DNL Segment Builder] のワークスペースには、[!DNL Profile] のデータ要素を操作できる豊富な機能があります。 ワークスペースには、ルールを作成および編集するための直感的なコントロールがあります。例えば、データプロパティを表示する際に使用するドラッグ&amp;ドロップタイルなどです。 [[!DNL Segment Builder]  ユーザーガイド ](../../segmentation/ui/segment-builder.md) に従って、次の内容を学習します。

- 属性、イベント、既存オーディエンスの組み合わせを構成要素として使用して、セグメント定義を作成する。
- ルールビルダーキャンバスとコンテナを使用して、セグメントルールの実行順序を制御します。
- 見込みオーディエンスの推定を表示し、必要に応じてセグメント定義を調整できます。
- すべてのセグメント定義をスケジュールされたセグメント化で有効にする
- ストリーミングセグメント化に対して指定したセグメント定義を有効にします。

## 次の手順 {#next-steps}

セグメントとセグメントについて詳しくは、[ セグメン [!DNL Segment Builder] ョンサービスの概要 ](../../segmentation/home.md) を参照してください。

[!DNL Real-Time Customer Profile] について詳しくは、[ リアルタイム顧客プロファイルの概要 ](../../profile/home.md) を参照してください。
