---
keywords: Experience Platform；ホーム；人気の高いトピック；ストリーミング；クラウドストレージコネクタ；クラウドストレージ
solution: Experience Platform
title: UI でのクラウドストレージソースのストリーミングデータフローの作成
type: Tutorial
description: データフローは、ソースから Platform データセットにデータを取得して取り込むスケジュール済みタスクです。 このチュートリアルでは、クラウドストレージベースコネクタを使用して新しいデータフローを設定する手順を説明します。
exl-id: 75deead6-ef3c-48be-aed2-c43d1f432178
source-git-commit: 983682489e2c0e70069dbf495ab90fc9555aae2d
workflow-type: tm+mt
source-wordcount: '1055'
ht-degree: 25%

---

# UI でのクラウドストレージソースのストリーミングデータフローの作成

データフローは、ソースからAdobe Experience Platformデータセットにデータを取得して取り込むスケジュール済みタスクです。 このチュートリアルでは、UI でクラウドストレージソースのストリーミングデータフローを作成する手順を説明します。

このチュートリアルを試す前に、まずクラウドストレージアカウントと Platform の間に有効な認証済みの接続を確立する必要があります。 認証済みの接続がない場合は、次のチュートリアルのいずれかを参照して、ストリーミングクラウドストレージアカウントの認証に関する情報を確認してください。

- [[!DNL Amazon Kinesis]](../../../ui/create/cloud-storage/kinesis.md)
- [[!DNL Azure Event Hubs]](../../../ui/create/cloud-storage/eventhub.md)
- [[!DNL Google PubSub]](../../../ui/create/cloud-storage/google-pubsub.md)

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

- [データフロー](../../../../../dataflows/home.md)：データフローは、Platform 間でデータを移動するデータジョブを表します。データフローは、ソースからに至るまで、様々なサービスをまたいで設定されます。 [!DNL Identity Service]、 [!DNL Profile]、および [!DNL Destinations].
- [Data Prep](../../../../../data-prep/home.md)：Data Prep を使用すると、データエンジニアはエクスペリエンスデータモデル（XDM）との間でデータのマッピング、変換、検証をおこなうことができます。Data Prep は、CSV 取得ワークフローなどのデータ取得プロセスで「マッピング」手順として表示されます。
- [[!DNL Experience Data Model (XDM)]  システム](../../../../../xdm/home.md)：[!DNL Experience Platform] が顧客体験データの整理に使用する標準化されたフレームワーク。
   - [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   - [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
- [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

## データの追加

ストリーミングクラウドストレージアカウントの認証を作成した後、 **[!UICONTROL データを選択]** 手順が表示され、Platform に取り込むデータストリームを選択するためのインターフェイスが提供されます。

- インターフェイスの左側には、アカウント内で使用可能なデータストリームを表示できるブラウザーがあります。
- インターフェイスの右側では、JSON ファイルから最大 100 行のデータをプレビューできます。

![インターフェイス](../../../../images/tutorials/dataflow/cloud-storage/streaming/interface.png)

使用するデータストリームを選択し、「 」を選択します。 **[!UICONTROL ファイルを選択]** をクリックして、サンプルスキーマをアップロードします。

>[!TIP]
>
>データが XDM に準拠している場合は、サンプルスキーマのアップロードをスキップし、「 」を選択します。 **[!UICONTROL 次へ]** をクリックして続行します。

![select-stream](../../../../images/tutorials/dataflow/cloud-storage/streaming/select-stream.png)

スキーマがアップロードされると、プレビューインターフェイスが更新され、アップロードしたスキーマのプレビューが表示されます。 プレビューインターフェイスを使用すると、ファイルの内容と構造を検査できます。 また、 [!UICONTROL 検索フィールド] スキーマ内から特定の項目にアクセスするユーティリティ。

終了したら、「**[!UICONTROL 次へ]**」を選択します。

![schema-preview](../../../../images/tutorials/dataflow/cloud-storage/streaming/schema-preview.png)

## マッピング

この **[!UICONTROL マッピング]** の手順が表示され、ソースデータを Platform データセットにマッピングするためのインターフェイスが提供されます。

取り込むインバウンドデータのデータセットを選択します。既存のデータセットを使用することも、新しいデータセットを作成することもできます。

### 新しいデータセット

データを新しいデータセットに取り込むには、「 **[!UICONTROL 新しいデータセット]** をクリックし、提供されたフィールドにデータセットの名前と説明を入力します。 スキーマを追加するには、 **[!UICONTROL スキーマを選択]** ダイアログボックス または、 **[!UICONTROL スキーマの詳細検索]** をクリックして、適切なスキーマを検索します。

![new-dataset](../../../../images/tutorials/dataflow/cloud-storage/streaming/new-dataset.png)

この [!UICONTROL スキーマを選択] ウィンドウが表示され、選択可能なスキーマのリストが表示されます。 リストからスキーマを選択して、右側のパネルを更新し、選択したスキーマに固有の詳細（スキーマが有効かどうかに関する情報など）を表示します。 [!DNL Profile].

使用するスキーマを特定して選択したら、「 」を選択します。 **[!UICONTROL 完了]**.

![select-schema](../../../../images/tutorials/dataflow/cloud-storage/streaming/select-schema.png)

この [!UICONTROL Target データセット] 選択したスキーマがデータセットの一部として表示され、ページが更新されます。 この手順の間に、 [!DNL Profile] エンティティの属性と行動を総合的に把握できます。 すべての有効なデータセットのデータは、 [!DNL Profile] および変更は、データフローを保存する際に適用されます。

切り替え **[!UICONTROL プロファイルデータセット]** ボタンを使用して [!DNL Profile].

![new-profile](../../../../images/tutorials/dataflow/cloud-storage/streaming/new-profile.png)

### 既存のデータセット

既存のデータセットにデータを取り込むには、「 」を選択します。 **[!UICONTROL 既存のデータセット]**「 」、「 」の順に選択し、データセットアイコンを選択します。

![existing-dataset](../../../../images/tutorials/dataflow/cloud-storage/streaming/existing-dataset.png)

この **[!UICONTROL データセットを選択]** ダイアログが表示され、選択可能なデータセットのリストが表示されます。 リストからデータセットを選択し、右側のパネルを更新して、選択したデータセットに固有の詳細（データセットを有効にできるかどうかに関する情報など）を表示します。 [!DNL Profile].

使用するデータセットを特定して選択したら、「 」を選択します。 **[!UICONTROL 完了]**.

![select-dataset](../../../../images/tutorials/dataflow/cloud-storage/streaming/select-dataset.png)

データセットを選択したら、 [!DNL Profile] をに切り替えて、データセットを有効にします。 [!DNL Profile].

![既存のプロファイル](../../../../images/tutorials/dataflow/cloud-storage/streaming/existing-profile.png)

### 標準フィールドをマッピング

データセットとスキーマが確立されると、 **[!UICONTROL 標準フィールドをマッピング]** インターフェイスが表示され、データのマッピングフィールドを手動で設定できます。

>[!TIP]
>
>Platform は、選択したターゲットスキーマまたはデータセットに基づいて、自動マッピングされたフィールドに対するインテリジェントなレコメンデーションを提供します。 マッピングルールは、ユースケースに合わせて手動で調整できます。

必要に応じて、フィールドを直接マッピングするか、データ準備機能を使用してソースデータを変換して計算値を導き出すかを選択できます。マッパーインターフェイスと計算フィールドを使用した包括的な手順については、 [データ準備 UI ガイド](../../../../../data-prep/ui/mapping.md).

ソースデータをマッピングしたら、 **[!UICONTROL 次へ]**.

![マッピング](../../../../images/tutorials/dataflow/cloud-storage/streaming/mapping.png)

## データフローの詳細

この **[!UICONTROL データフローの詳細]** 手順が表示され、新しいデータフローに名前を付け、簡単な説明を入力できます。

データフローの値を指定し、「 」を選択します。 **[!UICONTROL 次へ]**.

![dataflow-detail](../../../../images/tutorials/dataflow/cloud-storage/streaming/dataflow-detail.png)

### レビュー

**[!UICONTROL レビュー]**&#x200B;手順が表示され、新しいデータフローを作成する前に確認できます。詳細は、次のカテゴリに分類されます。

- **[!UICONTROL 接続]**:アカウント名、ソースのタイプ、および使用しているストリーミングクラウドストレージソースに固有のその他の情報が表示されます。
- **[!UICONTROL データセットの割り当てとフィールドのマッピング]**:データフローに使用するターゲットデータセットとスキーマが表示されます。

データフローをレビューしたら、「**[!UICONTROL 終了]**」を選択し、データフローが作成されるまでしばらく待ちます。

![レビュー](../../../../images/tutorials/dataflow/cloud-storage/streaming/review.png)

## データフローの監視と削除

ストリーミングクラウドストレージのデータフローを作成したら、そのデータフローを通じて取り込まれるデータを監視できます。 ストリーミングデータフローの監視と削除について詳しくは、 [ストリーミングデータフローの監視](../../monitor-streaming.md).

## 次の手順

このチュートリアルでは、データフローを作成し、クラウドストレージソースからデータをストリーミングしました。 受信データは、[!DNL Real-Time Customer Profile] および [!DNL Data Science Workspace] のようなダウンストリームの Platform サービスで使用できるようになりました。詳しくは、次のドキュメントを参照してください。

- [[!DNL Real-Time Customer Profile] 概要](../../../../../profile/home.md)
- [[!DNL Data Science Workspace] 概要](../../../../../data-science-workspace/home.md)