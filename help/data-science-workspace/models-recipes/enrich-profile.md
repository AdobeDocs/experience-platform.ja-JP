---
keywords: Experience Platform;機械学習モデル;データ科学ワークスペース;リアル時間顧客プロフィール人気のあるトピック;機械学習のインサイト
solution: Experience Platform
title: 機械学習のインサイトによるリアルタイム顧客プロファイルのエンリッチメント
type: Tutorial
description: このドキュメントでは、機械学習のインサイトを使用してリアルタイム顧客プロファイルを強化する方法に関するガイドを提供します。
exl-id: 397023c9-383d-4a21-b58a-0f920631ac56
source-git-commit: 923c6f2deb4d1199cfc5dc9dc4ca7b4da154aaaa
workflow-type: tm+mt
source-wordcount: '653'
ht-degree: 13%

---

# 機械学習のインサイトによる [!DNL Real-Time Customer Profile] の強化

>[!NOTE]
>
>Data Science Workspaceは購入できなくなりました。
>
>このドキュメントは、以前に データ Science ワークスペース の利用資格を持つ既存のお客様を対象としています。

Adobe Experience Platform [!DNL Data Science Workspace] は、機械学習モデルを作成、評価、利用してデータ予測と分析情報を生成するためのツールとリソースを提供します。 機械学習の分析情報が [!DNL Profile]対応データセットに取り込まれると、その同じデータも [!DNL Profile] レコードとして取り込まれ、 [!DNL Adobe Experience Platform Segmentation Service]を使用してセグメント化できます。

セグメント化プロセスは、オーディエンスの評価方法によって異なります。 オーディエンスが **ストリーミング**&#x200B;として構成されている場合、モデルによってプロファイルに書き込まれたすべての新しい更新がリアルタイムで処理されます。 ただし、オーディエンスが **バッチ** 評価用に構成されている場合、新しい値は次のバッチで評価されます。

このドキュメントでは、機械学習の分析情報で [!DNL Real-Time Customer Profile] を強化するためのチュートリアルへのリンクを提供します。

## はじめに

以下のチュートリアルを完了するには、 [!DNL Profile] データの取り込みとオーディエンスの作成について十分に理解している必要があります。 このチュートリアルを開始する前に、次のサービスのドキュメントを確認してください。

- [[!DNL Real-Time Customer Profile]](../../profile/home.md)：複数のソースから集計したデータに基づいて、個々の顧客の完全で統一された表現を提供します。
- [[!DNL Identity Service]](../../identity-service/home.md):Platform に取り込まれる様々なデータソースの ID を結合することで、[!DNL Real-Time Customer Profile] を有効にします。
- [[!DNL Experience Data Model (XDM)]](../../xdm/home.md): Platform が顧客体験データを整理する際に使用する標準化されたフレームワーク。

上記のドキュメントに加え、スキーマおよびスキーマエディターに関する次のガイドも確認することを強くお勧めします。

- [スキーマ構成の基本](../../xdm/schema/composition.md): [!DNL Experience Platform]で使用するスキーマを構成するためのXDMスキーマ、構成要素、原則、およびベスト プラクティスについて説明します。
- [スキーマ エディター チュートリアル](../../xdm/tutorials/create-schema-ui.md): [!DNL Experience Platform] 内のスキーマエディターを使用してスキーマを作成する詳細な手順を示します。

## 出力スキーマとデータセット作成と設定 {#create-an-output-schema-and-dataset}

スコアリング分析情報で [!DNL Real-Time Customer Profile] を充実させるための最初のステップは、データが定義する現実世界のオブジェクト (人など) を知ることです。 データを理解することで、リレーショナル データベースを設計するのと同じように、意味を追加する構造を記述および設計できます。

クラスを割り当てることで、スキーマの構成が開始されます。クラスは、スキーマに含まれるデータ（レコードまたは時系列）の行動面を定義します。独自のスキーマの作成開始には、チュートリアルの [スキーマエディターを使用したスキーマの作成](../../xdm/tutorials/create-schema-ui.md)の手順フォローするます。 データセットの [!DNL Profile] を有効にする前に、データセットのスキーマをプライマリ ID フィールドを持つように設定してから、スキーマの [!DNL Profile] を有効にする必要があります。 データが [!DNL Profile] 対応データセットに取り込まれると、その同じデータも [!DNL Profile] レコードとして取り込まれます。

[!DNL Schema Registry] API を使用してスキーマを作成する場合は、[[!DNL Schema Registry] 開発者ガイド](../../xdm/api/getting-started.md)を参照してから、[API を使用したスキーマの作成](../../xdm/tutorials/create-schema-api.md)に関するチュートリアルを試してください。

スキーマとデータセットを準備したら、適切なモデルを使用してスコアリング実行を実行することで、スコアリングデータを生成してデータセットに取り込むことができます。

## オーディエンス作成 [!DNL Segment Builder] {#create-audiences-using-the-segment-builder}

スコアリングデータの分析情報を生成して [!DNL Profile] 対応データセットに取り込んだら、 [!DNL Segment Builder]を使用して動的なオーディエンスを作成できます。

[!DNL Segment Builder]には、[!DNL Profile]データ要素を操作できる豊富なワークスペースが用意されています。ワークスペースは、データプロパティを表すために使用されるドラッグ＆ドロップタイルなど、ルールを作成および編集するための直感的なコントロールを提供します。 [[!DNL Segment Builder] ユーザー ガイド](../../segmentation/ui/segment-builder.md)に従って、以下について学習してください。

- 属性、イベント、既存のオーディエンスを組み合わせて構築ブロックとして使用してセグメント定義を作成します。
- ルールビルダーのキャンバスとコンテナを使用して、オーディエンスルールの実行順序を制御します。
- 見込みオーディエンスの見積もりを表示し、必要に応じてセグメント定義を調整できます。
- スケジュールされたセグメント化のすべてのセグメント定義を有効にする。
- ストリーミングセグメント化に対して指定したセグメント定義を有効にします。

## 次の手順 {#next-steps}

オーディエンスと [!DNL Segment Builder] ーディエンスについて詳しくは、[ セグメント化サービスの概要 ](../../segmentation/home.md) を参照してください。

[!DNL Real-Time Customer Profile]の詳細については、「[Real-時間 カスタマー プロフィールの概要」を参照してください。](../../profile/home.md)
