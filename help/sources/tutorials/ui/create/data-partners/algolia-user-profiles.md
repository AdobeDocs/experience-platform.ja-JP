---
title: UI を使用した Algolia ユーザープロファイルのExperience Platformへの接続
description: アルゴリアのユーザーをExperience Platformに誘導する方法を学ぶ
exl-id: d4c936a7-4983-4a12-a813-03b672116e44
source-git-commit: 9bc7d372eba9ffcfe64f90d2d58a532411e5f1ce
workflow-type: tm+mt
source-wordcount: '1136'
ht-degree: 15%

---

# UI[!DNL Algolia User Profiles] 使用したExperience Platformへのデータの取り込み

ユーザーインターフェイスを使用して [!DNL Algolia User Profiles] アカウントからAdobe Experience Platformにデータを取り込む方法については、このチュートリアルを参照してください。

## 基本を学ぶ

>[!IMPORTANT]
>
>開始する前に、「概要 [[!DNL Algolia User Profiles]  に記載されている前提条件の手順を完了していることを確認してください ](../../../../connectors/data-partners/algolia-user-profiles.md#prerequisites)。

このチュートリアルは、 Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md)：Experience Platform が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。
* [ ソース ](../../../../home.md):Experience Platformを使用すると、データを様々なソースから取得しながら、Experience Platform サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。

### 必要な資格情報の収集

[!DNL Algolia] をExperience Platformに接続するには、次の資格情報の値を指定する必要があります。

| 資格情報 | 説明 |
| --- | --- |
| アプリケーション ID | [!DNL Algolia] アプリケーション ID は、[!DNL Algolia] アカウントに割り当てられた一意の識別子です。 |
| API キー | [!DNL Algolia] API キーは、[!DNL Algolia] の検索およびインデックス作成サービスに対する API リクエストを認証および承認するために使用される資格情報です。 |

これらの資格情報について詳しくは、[!DNL Algolia] [ 認証ドキュメント ](https://www.algolia.com/doc/tools/cli/get-started/authentication/) を参照してください。

## [!DNL Algolia] アカウントを接続

Experience Platformの UI で、左側のナビゲーションから **[!UICONTROL Sources]** を選択し、*[!UICONTROL Sources]* ワークスペースにアクセスします。 *[!UICONTROL カテゴリ]* パネルで適切なカテゴリを選択できます。 または、検索バーを使用して、使用する特定のソースに移動できます。

[!DNL Algolia] を使用するには、「**[!UICONTROL データおよび ID パートナー &#x200B;]*の下の*[!UICONTROL Algolia]** ソースカードを選択してから、「**[!UICONTROL 設定]** を選択します。

>[!TIP]
>
>ソースカタログ内のソースは、特定のソースがまだ認証済みのアカウントを持っていない場合に「**[!UICONTROL 設定]**」オプションを表示します。 認証済みアカウントが存在すると、このオプションは **[!UICONTROL データを追加]** に変わります。

![ ソースカタログと Algolia ユーザープロファイルソースが選択されています。](../../../../images/tutorials/create/algolia/user-profiles/catalog.png)

## 認証

### 既存のアカウントを使用

既存のアカウントを使用するには、「**[!UICONTROL 既存のアカウント]**」を選択して、使用する [!DNL Algolia User Profiles] アカウントを選択します。 続行するには、「**[!UICONTROL 次へ]**」を選択します。

![既存のアカウントインターフェイス。](../../../../images/tutorials/create/algolia/user-profiles/existing-account.png)

### 新しいアカウントを作成

新しいアカウントを作成する場合は、「**[!UICONTROL 新しいアカウント]**」を選択し、続けて名前、説明（オプション）、資格情報 [!DNL Algolia] 入力します。 終了したら「**[!UICONTROL ソースに接続]**」を選択し、新しい接続が確立されるまでしばらく待ちます。

![ 新しいアカウントインターフェイス ](../../../../images/tutorials/create/algolia/user-profiles/new-account.png)

## データの追加

[!DNL Algolia User Profiles] アカウントを作成すると、**[!UICONTROL データを追加]** 手順が表示され、Experience Platformに取り込みたい [!DNL Algolia] ユーザープロファイルを探索するためのインターフェイスが表示されます。

* インターフェイスの左側の部分では、オプションの **[!UICONTROL インデックス]** フィールドと **[!UICONTROL アフィニティ]** フィールドを入力できます。
* インターフェイスの右側の部分では、最大 100 行のユーザープロファイルをプレビューできます。

データの選択と取り込み用のプレビューが完了したら、「**[!UICONTROL 次へ]**」を選択します。

![ ワークフローのデータ選択ステップ ](../../../../images/tutorials/create/algolia/user-profiles/select-data.png)

## データフローの詳細を入力

既存のデータセットを使用している場合は、[!DNL Algolia Profile] フィールドグループを使用しているスキーマに関連付けられているデータセットを選択します。

![ ソースワークフローの既存のデータセットステップ ](../../../../images/tutorials/create/algolia/user-profiles/dataflow-detail-existing-dataset.png)

新しいデータセットを作成する場合は、マッピングステップで必要な [!DNL Algolia Profile] フィールドグループを使用しているスキーマを選択します。

![ ソースワークフローの新しいデータセット手順 ](../../../../images/tutorials/create/algolia/user-profiles/dataflow-detail-new-dataset.png)

## XDM スキーマへのデータフィールドのマッピング

マッピングインターフェイスを使用して、データをExperience Platformに取り込む前に、ソースデータを適切なスキーマフィールドにマッピングします。  詳しくは、UI の [ マッピングガイド ](../../../../../data-prep/ui/mapping.md) を参照してください。

![ ソースワークフローのマッピングステップ ](../../../../images/tutorials/create/algolia/user-profiles/mapping.png)

## 取り込み実行のスケジュール

次に、スケジュールインターフェイスを使用して、データフローの取り込みスケジュールを定義します。

<!-- The Scheduling step allows for configuration of the data/time to execute the [!DNL Algolia Uer Profiles] Source connector. There is configuration to backfill the data from [!DNL Algolia] which will pull all the profiles from the source system.  If the source is scheduled, then it will retrieve modified profiles from the [!DNL Algolia] based on the configured time interval. -->

![ ソースワークフローのスケジュール手順 ](../../../../images/tutorials/create/algolia/user-profiles/scheduling.png)

| スケジュール設定 | 説明 |
| --- | --- |
| 頻度 | 頻度を設定して、データフローの実行頻度を示します。 頻度は次のように設定できます。 <ul><li>**1 回**：頻度を `once` に設定して、1 回限りの取り込みを作成します。 1 回限りの取り込みデータフローを作成する場合、間隔とバックフィルの設定は使用できません。 デフォルトでは、スケジュールの頻度は 1 回に設定されています。</li><li>**分**：頻度を `minute` に設定して、1 分ごとにデータを取り込むようにデータフローをスケジュールします。</li><li>**時間**：頻度を `hour` に設定して、1 時間ごとにデータを取り込むようにデータフローをスケジュールします。</li><li>**日**：頻度を `day` に設定して、1 日にデータを取り込むようにデータフローをスケジュールします。</li><li>**週**：頻度を `week` に設定して、データフローが週ごとにデータを取り込むようにスケジュールします。</li></ul> |
| 間隔 | 頻度を選択したら、間隔設定を指定して、各取り込み間の時間枠を確立できます。 例えば、頻度を日に設定し、間隔を 15 に設定すると、データフローは 15 日ごとに実行されます。 間隔をゼロに設定することはできません。 各頻度で許容される最小のインターバル値は次のとおりです。<ul><li>**1 回**：なし</li><li>**分**: 15</li><li>**時間**: 1</li><li>**日**: 1</li><li>**週**: 1</li></ul> |
| 開始時間 | 見込み実行のタイムスタンプ（UTC タイムゾーンで表示）。 |
| バックフィル | バックフィルは、最初に取り込むデータを決定します。 バックフィルが有効になっている場合、指定されたパス内の現在のすべてのファイルが、最初にスケジュールされた取り込み時に取り込まれます。 バックフィルが無効になっている場合は、最初の取り込みの実行から開始時刻の間に読み込まれたファイルのみが取り込まれます。 開始時間より前に読み込まれたファイルは取り込まれません。 |

## データフローのレビュー

取り込み前のデータフローの概要を確認するページを使用します。 詳細は、次のカテゴリにグループ化されます。

* **接続** - ソースのタイプ、選択したソースファイルの関連パスおよびそのソースファイル内の列数を表示します。
* **データセットの割り当てとフィールドのマッピング** - ソースデータがどのデータセットに取り込まれるかを、そのデータセットが準拠するスキーマを含めて表示します。
* **スケジュール** – 取り込みスケジュールのアクティブな期間、頻度、間隔を表示します。

データフローをレビューしたら、「**[!UICONTROL 終了]**」を選択し、データフローが作成されるまでしばらく待ちます。

![ ソースワークフローのレビュー手順。](../../../../images/tutorials/create/algolia/user-profiles/review.png)

## 次の手順

このチュートリアルでは、[!DNL Algolia] ソースからExperience Platformにインテントデータを取り込むデータフローを正常に作成しました。 その他のリソースについては、以下に概要を説明するドキュメントを参照してください。

### データフローの監視

データフローを作成したら、そのデータフローを通じて取り込まれるデータを監視し、取り込み率、成功、エラーに関する情報を表示できます。 データフローのモニタリング方法について詳しくは、[UI でのアカウントとデータフローのモニタリング ](../../../../../dataflows/ui/monitor-sources.md) のチュートリアルを参照してください。

### データフローの更新

データフローのスケジュール、マッピング、一般情報の設定を更新するには、[UI でのソースデータフローの更新 ](../../update-dataflows.md) に関するチュートリアルを参照してください。

### データフローの削除

不要になったデータフローや誤って作成されたデータフローは、**[!UICONTROL データフロー]**&#x200B;ワークスペース内にある&#x200B;**[!UICONTROL 削除]**&#x200B;機能で削除できます。データフローの削除方法について詳しくは、[UI でのデータフローの削除 ](../../delete.md) のチュートリアルを参照してください。
