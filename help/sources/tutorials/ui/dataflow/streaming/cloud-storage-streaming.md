---
keywords: Experience Platform；ホーム；人気のトピック；ストリーミング；cloud storage コネクタ；cloud storage
solution: Experience Platform
title: UI でクラウドストレージソースのストリーミングデータフローを作成
type: Tutorial
description: データフローは、ソースからExperience Platform データセットにデータを取得して取り込む、スケジュールされたタスクです。 このチュートリアルでは、クラウドストレージベースコネクタを使用して新しいデータフローを設定する手順を説明します。
exl-id: 75deead6-ef3c-48be-aed2-c43d1f432178
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '1078'
ht-degree: 20%

---

# UI でクラウドストレージソースのストリーミングデータフローを作成

データフローは、ソースからAdobe Experience Platform データセットにデータを取得して取り込む、スケジュールされたタスクです。 このチュートリアルでは、クラウドストレージソースのストリーミングデータフローを UI で作成する手順について説明します。

このチュートリアルを試す前に、まず、クラウドストレージアカウントとExperience Platformの間に、有効で認証済みの接続を確立する必要があります。 認証済みの接続がまだない場合、ストリーミングクラウドストレージアカウントの認証について詳しくは、次のいずれかのチュートリアルを参照してください。

- [[!DNL Amazon Kinesis]](../../../ui/create/cloud-storage/kinesis.md)
- [[!DNL Azure Event Hubs]](../../../ui/create/cloud-storage/eventhub.md)
- [[!DNL Google PubSub]](../../../ui/create/cloud-storage/google-pubsub.md)

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

- [ データフロー ](../../../../../dataflows/home.md)：データフローは、Experience Platform間でデータを移動するデータジョブを表します。 データフローは、ソース、[!DNL Identity Service]、[!DNL Profile]、[!DNL Destinations] など、様々なサービスをまたいで設定されます。
- [Data Prep](../../../../../data-prep/home.md): Data Prep を使用すると、データエンジニアはエクスペリエンスデータモデル（XDM）との間でデータのマッピング、変換、検証をおこなうことができます。 Data Prep は、CSV 取得ワークフローなどのデータ取得プロセスで「マッピング」手順として表示されます。
- [[!DNL Experience Data Model (XDM)]  システム](../../../../../xdm/home.md)：[!DNL Experience Platform] が顧客体験データの整理に使用する標準化されたフレームワーク。
   - [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   - [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
- [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

## データの追加

>[!NOTE]
>
>特定のイベントハブについて、コンシューマーグループごとに 1 つのソースデータフローのみを作成できます。

ストリーミングクラウドストレージアカウントの認証を行う **[!UICONTROL データを選択]** 手順が表示され、Experience Platformに取り込むデータストリームを選択するためのインターフェイスが表示されます。

- インターフェイスの左側は、アカウント内で利用可能なデータストリームを表示できるブラウザーです。
- インターフェイスの右側の部分では、JSON ファイルから最大 100 行のデータをプレビューできます。

![ インターフェイス ](../../../../images/tutorials/dataflow/cloud-storage/streaming/interface.png)

使用するデータストリームを選択し、「**[!UICONTROL ファイルを選択]** を選択して、サンプルスキーマをアップロードします。

>[!TIP]
>
>データが XDM に準拠している場合、サンプルスキーマのアップロードをスキップし、「**[!UICONTROL 次へ]**」を選択して続行できます。

![select-stream](../../../../images/tutorials/dataflow/cloud-storage/streaming/select-stream.png)

スキーマがアップロードされると、プレビューインターフェイスが更新され、アップロードしたスキーマのプレビューが表示されます。 プレビューインターフェイスを使用すると、ファイルの内容と構造を検査できます。 また、[!UICONTROL  フィールドを検索 ] ユーティリティを使用して、スキーマ内から特定の項目にアクセスすることもできます。

終了したら、「**[!UICONTROL 次へ]**」を選択します。

![schema-preview](../../../../images/tutorials/dataflow/cloud-storage/streaming/schema-preview.png)

## マッピング

**[!UICONTROL マッピング]** 手順が表示され、ソースデータをExperience Platform データセットにマッピングするためのインターフェイスが表示されます。

取り込むインバウンドデータのデータセットを選択します。既存のデータセットを使用することも、新しいデータセットを作成することもできます。

### 新しいデータセット

データを新しいデータセットに取り込むには、「**[!UICONTROL 新しいデータセット]**」を選択し、表示されたフィールドにデータセットの名前と説明を入力します。 スキーマを追加するには、**[!UICONTROL スキーマを選択]** ダイアログボックスで既存のスキーマ名を入力します。 または、**[!UICONTROL スキーマの詳細検索]** を選択して、適切なスキーマを検索することもできます。

![new-dataset](../../../../images/tutorials/dataflow/cloud-storage/streaming/new-dataset.png)

[!UICONTROL  スキーマを選択 ] ウィンドウが開き、使用可能なスキーマのリストが表示されます。 リストからスキーマを選択して右側のパネルを更新し、選択したスキーマに固有の詳細（スキーマが [!DNL Profile] に対して有効になっているかどうかについての情報など）を表示します。

使用するスキーマを特定して選択したら、「**[!UICONTROL 完了]**」を選択します。

![select-schema](../../../../images/tutorials/dataflow/cloud-storage/streaming/select-schema.png)

[!UICONTROL  ターゲットデータセット ] ページが、データセットの一部として表示された、選択したスキーマで更新されます。 この手順では、データセットの [!DNL Profile] を有効にして、エンティティの属性と動作の全体像を作成できます。 有効なすべてのデータセットのデータは [!DNL Profile] に含まれ、変更はデータフローを保存するときに適用されます。

**[!UICONTROL プロファイルデータセット]** ボタンを切り替えて、[!DNL Profile] のターゲットデータセットを有効にします。

![new-profile](../../../../images/tutorials/dataflow/cloud-storage/streaming/new-profile.png)

### 既存のデータセット

データを既存のデータセットに取り込むには、**[!UICONTROL 既存のデータセット]** を選択してから、データセットアイコンを選択します。

![existing-dataset](../../../../images/tutorials/dataflow/cloud-storage/streaming/existing-dataset.png)

**[!UICONTROL データセットを選択]** ダイアログが表示され、使用可能なデータセットのリストから選択できます。 リストからデータセットを選択して右側のパネルを更新し、選択したデータセットに固有の詳細（データセットを [!DNL Profile] に対して有効にできるかどうかに関する情報など）を表示します。

使用するデータセットを特定して選択したら、「**[!UICONTROL 完了]**」を選択します。

![select-dataset](../../../../images/tutorials/dataflow/cloud-storage/streaming/select-dataset.png)

データセットを選択したら、「[!DNL Profile]」切り替えスイッチを選択して、[!DNL Profile] 用のデータセットを有効にします。

![existing-profile](../../../../images/tutorials/dataflow/cloud-storage/streaming/existing-profile.png)

### 標準フィールドをマッピング

データセットとスキーマが確立されると、**[!UICONTROL 標準フィールドをマッピング]** インターフェイスが表示され、データのマッピングフィールドを手動で設定できます。

>[!TIP]
>
>Experience Platformは、選択したターゲットスキーマまたはデータセットに基づいて、自動マッピングされたフィールドに対してインテリジェントなレコメンデーションを提供します。 マッピングルールは、ユースケースに合わせて手動で調整できます。

必要に応じて、フィールドを直接マッピングするか、データ準備機能を使用してソースデータを変換して計算値を導き出すかを選択できます。マッパーインターフェイスと計算フィールドの使用に関する包括的な手順については、[ データ準備 UI ガイド ](../../../../../data-prep/ui/mapping.md) を参照してください。

ソースデータがマッピングされたら、「**[!UICONTROL 次へ]**」を選択します。

![マッピング](../../../../images/tutorials/dataflow/cloud-storage/streaming/mapping.png)

## データフローの詳細

**[!UICONTROL データフローの詳細]** 手順が表示され、新しいデータフローに名前を付けて簡単な説明を入力できます。

データフローの値を指定して「**[!UICONTROL 次へ]**」を選択します。

![dataflow-detail](../../../../images/tutorials/dataflow/cloud-storage/streaming/dataflow-detail.png)

### レビュー

**[!UICONTROL レビュー]**&#x200B;手順が表示され、新しいデータフローを作成する前に確認できます。詳細は、次のカテゴリに分類されます。

- **[!UICONTROL 接続]**：アカウント名、ソースのタイプ、および使用しているストリーミングクラウドストレージソースに固有のその他の情報を表示します。
- **[!UICONTROL データセットの割り当てとフィールドのマッピング]**：データフローに使用するターゲットデータセットとスキーマを表示します。

データフローをレビューしたら、「**[!UICONTROL 終了]**」を選択し、データフローが作成されるまでしばらく待ちます。

![レビュー](../../../../images/tutorials/dataflow/cloud-storage/streaming/review.png)

## データフローの監視と削除

ストリーミングクラウドストレージのデータフローを作成したら、それを通じて取り込まれるデータを監視できます。 ストリーミングデータフローの監視と削除について詳しくは、[ ストリーミングデータフローの監視 ](../../monitor-streaming.md) に関するチュートリアルを参照してください。

## 次の手順

このチュートリアルでは、クラウドストレージソースからデータをストリーミングするデータフローを正常に作成しました。 これで、[!DNL Real-Time Customer Profile] や [!DNL Data Science Workspace] などのダウンストリームのExperience Platform サービスで受信データを使用できるようになりました。 詳しくは、次のドキュメントを参照してください。

- [[!DNL Real-Time Customer Profile] 概要](../../../../../profile/home.md)
- [[!DNL Data Science Workspace] 概要](../../../../../data-science-workspace/home.md)