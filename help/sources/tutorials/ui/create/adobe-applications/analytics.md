---
keywords: Experience Platform；ホーム；人気の高いトピック；Analytics ソースコネクタ；Analytics コネクタ；Analytics ソース；Analytics ソース；Analytics
solution: Experience Platform
title: UI でのAdobe Analyticsソース接続の作成
topic-legacy: overview
type: Tutorial
description: UI でAdobe Analyticsソース接続を作成して、消費者データをAdobe Experience Platformに取り込む方法を説明します。
exl-id: 5ddbaf63-feaa-44f5-b2f2-2d5ae507f423
source-git-commit: f5d341daffd7d4d77ee816cc7537b0d0c52ca636
workflow-type: tm+mt
source-wordcount: '1493'
ht-degree: 8%

---

# UI でのAdobe Analyticsソース接続の作成

このチュートリアルでは、UI でAdobe Analyticsソース接続を作成し、 [!DNL Analytics] Adobe Experience Platformへのレポートスイートデータ。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [エクスペリエンスデータモデル（XDM）システム](../../../../../xdm/home.md)：Experience Platform が顧客体験データを整理するための標準化されたフレームワーク。
* [リアルタイム顧客プロファイル](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムのプロファイルを顧客に提供します。
* [サンドボックス](../../../../../sandboxes/home.md)：Experience Platform は、単一の Platform インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

### 主要用語

このドキュメント全体で使用される以下の主要用語を理解しておくことが重要です。

* **標準属性**:標準属性は、Adobeで事前定義された任意の属性です。 すべての顧客に同じ意味を持ち、 [!DNL Analytics] ソースデータと [!DNL Analytics] スキーマフィールドグループ。
* **カスタム属性**:カスタム属性は、 [!DNL Analytics]. カスタム属性は、Adobe Analyticsの実装内で特定の情報を取り込むために使用され、レポートスイートとレポートスイートでは使用方法が異なる場合があります。 カスタム属性には、eVar、prop およびリストが含まれます。 次を参照してください。 [[!DNL Analytics] コンバージョン変数に関するドキュメント](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/conversion-variables/conversion-var-admin.html?lang=en) を参照してください。
* **カスタムフィールドグループ内の任意の属性**:顧客が作成したフィールドグループから派生する属性はすべてユーザー定義で、標準属性もカスタム属性も見なされません。
* **わかりやすい名前**:わかりやすい名前は、 [!DNL Analytics] 実装。 次を参照してください。 [[!DNL Analytics] コンバージョン変数に関するドキュメント](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/conversion-variables/conversion-var-admin.html?lang=en) わかりやすい名前の詳細については、を参照してください。

## Adobe Analyticsでのソース接続の作成

Platform UI で、「 **[!UICONTROL ソース]** 左側のナビゲーションから [!UICONTROL ソース] ワークスペース。 この [!UICONTROL カタログ] 画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 また、検索バーを使用して、表示するソースを絞り込むこともできます。

以下 **[!UICONTROL Adobe]** カテゴリ、選択 **[!UICONTROL Adobe Analytics]** 次に、 **[!UICONTROL データを追加]**.

![カタログ](../../../../images/tutorials/create/analytics/catalog.png)

### データを選択

この **[!UICONTROL Analytics ソースデータの追加]** 手順が表示されます。 選択 **[!UICONTROL レポートスイート]** をクリックして、Analytics レポートスイートデータのソース接続の作成を開始し、取り込むレポートスイートを選択します。 選択できないレポートスイートは、このサンドボックスまたは別のサンドボックスで既に取り込まれています。 選択 **[!UICONTROL 次へ]** をクリックして続行します。

>[!NOTE]
>
>複数のインバウンド接続を使用して複数のレポートスイートを取り込むことはできますが、Real-time Customer Data Platformで一度に使用できるレポートスイートは 1 つだけです。

![](../../../../images/tutorials/create/analytics/add-data.png)

<!---Analytics Report Suites can be configured for one sandbox at a time. To import the same Report Suite into a different sandbox, the dataset flow will have to be deleted and instantiated again via configuration for a different sandbox.--->

### マッピング

>[!IMPORTANT]
>
>Data Prep のサポート機能 [!DNL Analytics] ソースはベータ版です。

この [!UICONTROL マッピング] ページには、ソースフィールドを適切なターゲットスキーマフィールドにマッピングするためのインターフェイスが用意されています。 ここから、カスタム変数を新しいスキーマフィールドグループにマッピングし、Data Prep のサポートに従って計算を適用できます。 ターゲットスキーマを選択してマッピングプロセスを開始します。

>[!TIP]
>
>スキーマのみ [!DNL Analytics] テンプレートフィールドグループが、スキーマ選択メニューに表示されます。 その他のスキーマは省略されます。 お使いのレポートスイートデータに適したスキーマがない場合は、新しいスキーマを作成する必要があります。 スキーマの作成手順について詳しくは、 [UI でのスキーマの作成と編集](../../../../../xdm/ui/resources/schemas.md).

![select-schema](../../../../images/tutorials/create/analytics/select-schema.png)

この [!UICONTROL 標準フィールドをマッピング] セクションにはパネルが表示されます [!UICONTROL 適用された標準マッピング], [!UICONTROL 一致しない標準マッピング] および [!UICONTROL カスタムマッピング]. 各カテゴリに関する詳細は、次の表を参照してください。

| 標準フィールドをマッピング | 説明 |
| --- | --- |
| [!UICONTROL 適用された標準マッピング] | この [!UICONTROL 適用された標準マッピング] マッピングされた属性の総数が表示されます。 標準マッピングとは、ソース内のすべての属性間のマッピングセットを指します。 [!DNL Analytics] のデータと対応する属性 [!DNL Analytics] フィールドグループを使用します。 これらは事前にマッピングされており、編集できません。 |
| [!UICONTROL 一致しない標準マッピング] | この [!UICONTROL 一致しない標準マッピング] panel は、わかりやすい名前の競合を含むマッピング済み属性の数を参照します。 これらの競合は、別のレポートスイートのフィールド記述子のセットが既に入力されているスキーマを再利用する場合に発生します。 次の手順を実行できます： [!DNL Analytics] わかりやすい名前の競合を含むデータフロー。 |
| [!UICONTROL カスタムマッピング] | この [!UICONTROL カスタムマッピング] パネルには、eVar、prop、リストを含む、マッピングされたカスタム属性の数が表示されます。 カスタムマッピングとは、ソース内のカスタム属性間のマッピングセットを指します。 [!DNL Analytics] 選択したスキーマに含まれているカスタムフィールドグループのデータと属性。 |

![map-standard-fields](../../../../images/tutorials/create/analytics/map-standard-fields.png)

次の手順で [!DNL Analytics] ExperienceEvent テンプレートスキーマフィールドグループで、「 」を選択します。 **[!UICONTROL 表示]** 内 [!UICONTROL 適用された標準マッピング] パネル。

![view](../../../../images/tutorials/create/analytics/view.png)

この [!UICONTROL Adobe Analytics ExperienceEvent テンプレートスキーマフィールドグループ] ページには、スキーマの構造を調べるためのインターフェイスが用意されています。 終了したら、「 」を選択します。 **[!UICONTROL 閉じる]**.

![field-group-preview](../../../../images/tutorials/create/analytics/field-group-preview.png)

Platform は、わかりやすい名前の競合に対して、マッピングセットを自動的に検出します。 マッピングセットと競合しない場合は、 **[!UICONTROL 次へ]** をクリックして続行します。

![マッピング](../../../../images/tutorials/create/analytics/mapping.png)

ソースレポートスイートと選択したスキーマ間でわかりやすい名前の競合がある場合、 [!DNL Analytics] データフローで、フィールド記述子は変更されないことを確認します。 または、空の記述子セットを含む新しいスキーマを作成することもできます。

選択 **[!UICONTROL 次へ]** をクリックして続行します。

![注意](../../../../images/tutorials/create/analytics/caution.png)

#### カスタムマッピング

Data Prep 関数を使用し、カスタム属性に新しいマッピングまたは計算フィールドを追加するには、 **[!UICONTROL カスタムマッピングの表示]**.

![view-custom-mapping](../../../../images/tutorials/create/analytics/view-custom-mapping.png)

次に、 **[!UICONTROL 新しいマッピングを追加]**.

必要に応じて、次のいずれかを選択できます。 **[!UICONTROL 新しいマッピングを追加]** または **[!UICONTROL 計算フィールドを追加]** を選択します。

![add-new-mapping](../../../../images/tutorials/create/analytics/add-new-mapping.png)

空のマッピングセットが表示されます。 マッピングアイコンを選択して、ソースフィールドを追加します。

![select-source-field](../../../../images/tutorials/create/analytics/select-source-field.png)

インターフェイスを使用して、ソーススキーマ構造内を移動し、使用する新しいソースフィールドを特定できます。 マッピングするソースフィールドを選択したら、 **[!UICONTROL 選択]**.

![select-mapping](../../../../images/tutorials/create/analytics/select-mapping.png)

次に、以下のマッピングアイコンを選択します。 [!UICONTROL ターゲットフィールド] 選択したソースフィールドを適切なターゲットフィールドにマッピングするには、次の手順に従います。

![select-target-field](../../../../images/tutorials/create/analytics/select-target-field.png)

ソーススキーマと同様に、インターフェイスを使用してターゲットスキーマ構造内を移動し、マッピング先のターゲットフィールドを選択できます。 適切なターゲットフィールドを選択したら、「 **[!UICONTROL 選択]**.

![select-target-mapping](../../../../images/tutorials/create/analytics/select-target-mapping.png)

カスタムマッピングセットが完了したら、「 」を選択します。 **[!UICONTROL 次へ]** をクリックして続行します。

![complete-custom-mapping](../../../../images/tutorials/create/analytics/complete-custom-mapping.png)

次のドキュメントでは、Data Prep、計算フィールド、およびマッピング関数に関する詳細なリソースを提供します。

* [Data Prep の概要](../../../../../data-prep/home.md)
* [データ準備マッピング関数](../../../../../data-prep/functions.md)
* [計算フィールドを追加](../../../../../data-prep/ui/mapping.md#calculated-fields)

### データフローの詳細を入力

この **[!UICONTROL データフローの詳細]** 手順が表示され、データフローの名前と説明（オプション）を入力する必要があります。 完了したら、「**[!UICONTROL 次へ]**」をクリックします。

![データフローの詳細](../../../../images/tutorials/create/analytics/dataflow-detail.png)

### レビュー

この [!UICONTROL レビュー] 手順が表示され、作成前に新しい Analytics データフローを確認できます。 接続の詳細は、次のようなカテゴリ別にグループ化されます。

* [!UICONTROL 接続]:接続のソースプラットフォームを表示します。
* [!UICONTROL データタイプ]:選択したレポートスイートと、対応するレポートスイート ID が表示されます。

![レビュー](../../../../images/tutorials/create/analytics/review.png)

### データフローの監視

データフローを作成したら、それを通じて取り込まれるデータを監視できます。 次の [!UICONTROL カタログ] 画面、選択 **[!UICONTROL データフロー]** :Analytics アカウントに関連付けられている確立されたフローのリストを表示します。

![select-dataflows](../../../../images/tutorials/create/analytics/select-dataflows.png)

この **データフロー** 画面が表示されます。 このページは、名前、ソースデータ、作成時間、ステータスに関する情報を含む、データセットフローのペアです。

コネクタは、2 つのデータセットフローをインスタンス化します。 1 つのフローはバックフィルデータを表し、もう 1 つはライブデータのフローです。 バックフィルデータは、プロファイルに対して設定されていませんが、分析およびデータサイエンスの使用例のためにデータレイクに送信されます。

バックフィル、ライブデータおよびそれぞれの待ち時間について詳しくは、 [Analytics Data Connector の概要](../../../../connectors/adobe-applications/analytics.md).

表示するデータセットフローをリストから選択します。

![select-target-dataset](../../../../images/tutorials/create/analytics/select-target-dataset.png)

この **[!UICONTROL データセットアクティビティ]** ページが表示されます。 このページには、グラフの形式で消費されるメッセージの割合が表示されます。 選択 **[!UICONTROL データガバナンス]** 上部のヘッダーから、ラベル付けフィールドにアクセスします。

![dataset-activity](../../../../images/tutorials/create/analytics/dataset-activity.png)

データセットフローの継承ラベルは、 [!UICONTROL データガバナンス] 画面 Analytics からのデータにラベルを付ける方法について詳しくは、 [データ使用ラベルガイド](../../../../../data-governance/labels/user-guide.md).

![data-gov](../../../../images/tutorials/create/analytics/data-gov.png)

データフローを削除するには、次にアクセスします： [!UICONTROL データフロー] 」ページで省略記号 (`...`) をクリックし、「 」を選択します。 [!UICONTROL 削除].

![次を削除します。](../../../../images/tutorials/create/analytics/delete.png)

## 次の手順とその他のリソース

接続が作成されると、データフローが自動的に作成され、受信データが格納され、選択したスキーマでデータセットに入力されます。 さらに、データのバックフィルが発生し、最大 13 か月の履歴データが取り込まれます。最初の取り込みが完了したら、 [!DNL Analytics] データを作成し、次のようなダウンストリームの Platform サービスで使用 [!DNL Real-time Customer Profile] およびセグメント化サービス。 詳しくは、次のドキュメントを参照してください。

* [[!DNL Real-time Customer Profile] の概要](../../../../../profile/home.md)
* [[!DNL Segmentation Service] の概要](../../../../../segmentation/home.md)
* [[!DNL Data Science Workspace] の概要](../../../../../data-science-workspace/home.md)
* [[!DNL Query Service] の概要](../../../../../query-service/home.md)

次のビデオは、Adobe Analytics Source コネクタを使用したデータの取り込みに関する理解を深めることを目的としています。

>[!WARNING]
>
> 次のビデオに示す [!DNL Platform] UI は古くなっています。最新の UI のスクリーンショットと機能については、上記のドキュメントを参照してください。

>[!VIDEO](https://video.tv.adobe.com/v/29687?quality=12&learn=on)
