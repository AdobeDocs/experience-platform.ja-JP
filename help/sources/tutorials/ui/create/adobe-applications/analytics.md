---
title: UI での Adobe Analytics ソースコネクタの作成
description: UI でAdobe Analytics ソース接続を作成して、消費者データを Adobe Experience Platform に取り込む方法を説明します。
exl-id: 5ddbaf63-feaa-44f5-b2f2-2d5ae507f423
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '2676'
ht-degree: 38%

---

# UI での Adobe Analytics ソースコネクタの作成

このチュートリアルでは、Adobe Analytics レポートスイートデータをAdobe Experience Platformに取り込むために、UI でAdobe Analytics ソース接続を作成する手順について説明します。

## はじめに

このチュートリアルは、 Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [エクスペリエンスデータモデルl（XDM）システム](../../../../../xdm/home.md)：Experience Platform が顧客体験データを整理するための標準化されたフレームワークです。
* [リアルタイム顧客プロファイル](../../../../../profile/home.md)：複数のソースから集計したデータに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。
* [ サンドボックス ](../../../../../sandboxes/home.md): Experience Platformには、1 つのExperience Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

### 主な用語

このドキュメントで使用される以下の主な用語を理解することが重要です。

* **標準属性**：標準属性は、アドビで事前定義された任意の属性です。 これらはすべての顧客に対して同じ意味を持ち、[!DNL Analytics] ソースデータと [!DNL Analytics] スキーマフィールドグループで利用可能です。
* **カスタム属性**：カスタム属性とは、[!DNL Analytics] のカスタム変数階層にある任意の属性のことです。カスタム属性は、Adobe Analyticsの実装内で特定の情報をレポートスイートに取り込むために使用され、レポートスイートごとに使用方法が異なる場合があります。 カスタム属性には、eVar、prop およびリストが含まれます。eVars の詳細については、以下の[[!DNL Analytics] コンバージョン変数に関するドキュメント](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/conversion-variables/conversion-var-admin.html)を参照してください。
* **カスタムフィールドグループ内の任意の属性**：顧客が作成したフィールドグループから派生する属性はすべてユーザー定義であり、標準属性でもカスタム属性でもないとみなされます。
* **フレンドリ名**：フレンドリ名は、[!DNL Analytics] 実装のカスタム変数用に、人間がつけたラベルです。フレンドリ名の詳細については、以下の[[!DNL Analytics] コンバージョン変数に関するドキュメント](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/conversion-variables/conversion-var-admin.html)を参照してください。

## Adobe Analytics でのソース接続の作成

>[!NOTE]
>
>実稼動サンドボックスで Analytics ソースデータフローを作成すると、次の 2 つのデータフローが作成されます。
>
>* データレイクへの履歴レポートスイートデータの 13 か月のバックフィルを行うデータフロー。 このデータフローは、バックフィルが完了すると終了します。
>* ライブデータをデータレイクと [!DNL Real-Time Customer Profile] に送信するデータフローフロー。 このデータフローは継続的に実行されます。

Experience Platformの UI で、左側のナビゲーションから **[!UICONTROL Sources]** を選択し、[!UICONTROL Sources] ワークスペースにアクセスします。 [!UICONTROL カタログ]画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。また、検索バーを使用して、表示されるソースを絞り込むこともできます。

**[!UICONTROL Adobe アプリケーション]**&#x200B;カテゴリから、**[!UICONTROL Adobe Analytics]**、「**[!UICONTROL データの追加]**」の順に選択します。

![カタログ](../../../../images/tutorials/create/analytics/catalog.png)

### データの選択

>[!IMPORTANT]
>
>画面に表示されるレポートスイートは、様々な地域のレポートスイートである可能性があります。 お客様は、お客様のデータの制限事項や義務およびAdobe Experience Platformのクロスリージョンにおけるデータの使用方法を理解する責任を負います。 会社で許可されていることを確認してください。

**[!UICONTROL Analytics ソースのデータの追加]** 手順には、ソース接続を作成す [!DNL Analytics] レポートスイートデータのリストが表示されます。

レポートスイートは、レポートの基礎を形成するデータ [!DNL Analytics] コンテナです。 組織は、それぞれに異なるデータセットを含む、多数のレポートスイートを持つことができます。

ソース接続が作成されているExperience Platform サンドボックスインスタンスと同じ組織にマッピングされている限り、任意の地域（米国、英国またはシンガポール）からレポートスイートを取り込むことができます。 レポートスイートは、1 つのアクティブなデータフローのみを使用して取り込むことができます。 選択できないレポートスイートは、使用しているサンドボックスまたは別のサンドボックスで既に取り込まれています。

複数のインバウンド接続を使用して、複数のレポートスイートを同じサンドボックスに取り込むことができます。 変数（eVar やイベントなど）のスキーマが異なるレポートスイートの場合は、カスタムフィールドグループの特定のフィールドにマッピングし、[ データ準備 ](../../../../../data-prep/ui/mapping.md) を使用してデータの競合を回避する必要があります。 レポートスイートは、1 つのサンドボックスにのみ追加できます。

![](../../../../images/tutorials/create/analytics/report-suite.png)

>[!NOTE]
>
>複数のレポートスイートのデータをリアルタイム顧客プロファイルに対して有効にできるのは、異なる意味を持つ 2 つのカスタムプロパティ（eVar、リスト、prop）など、データの競合がない場合のみです。

[!DNL Analytics] ソース接続を作成するには、レポートスイートを選択してから「**[!UICONTROL 次へ]** を選択して続行します。

![](../../../../images/tutorials/create/analytics/add-data.png)

&lt;!—Analytics レポートスイートは、一度に 1 つのサンドボックスに対して設定できます。 同じレポートスイートを別のサンドボックスに読み込むには、データセットフローを削除し、別のサンドボックスの設定を使用して再度インスタンス化する必要があります。—>

### マッピング

>[!IMPORTANT]
>
>データ準備変換により、データフロー全体に待ち時間が追加される場合があります。 追加される追加の待ち時間は、変換ロジックの複雑さに応じて異なります。

[!DNL Analytics] データをターゲット XDM スキーマをマッピングする前に、まずデフォルトのスキーマとカスタムのスキーマのどちらを使用するかを選択する必要があります。

デフォルトのスキーマは、[!DNL Adobe Analytics ExperienceEvent Template] フィールドグループを含む新しいスキーマをユーザーに代わって作成します。デフォルトのスキーマを使用するには、**[!UICONTROL デフォルトのスキーマ]**&#x200B;を選択してください。

![default-schema](../../../../images/tutorials/create/analytics/default-schema.png)

カスタムスキーマを使用すると、[!DNL Analytics] データに対して、[!DNL Adobe Analytics ExperienceEvent Template] フィールドグループを持つスキーマであれば、利用可能な任意のスキーマを選択することができます。カスタムスキーマを使用するには、「**[!UICONTROL カスタムスキーマ]**」を選択してください。

![custom-schema](../../../../images/tutorials/create/analytics/custom-schema.png)

[!UICONTROL マッピング]ページには、ソースフィールドを適切なターゲットスキーマフィールドにマッピングするためのインターフェイスが用意されています。 ここから、カスタム変数を新しいスキーマフィールドグループにマッピングし、データ準備でサポートされている計算を適用できます。 ターゲットスキーマを選択してマッピングプロセスを開始します。

>[!TIP]
>
>[!DNL Adobe Analytics ExperienceEvent Template] フィールドグループを持つスキーマのみがスキーマ選択メニューに表示されます。 その他のスキーマは省略されます。 お使いのレポートスイートデータに適したスキーマがない場合は、新しいスキーマを作成する必要があります。 スキーマの作成手順について詳しくは、[UI でのスキーマの作成と編集](../../../../../xdm/ui/resources/schemas.md)ガイドを参照してください。

![select-schema](../../../../images/tutorials/create/analytics/select-schema.png)

[!UICONTROL 標準フィールドをマッピング]セクションには、[!UICONTROL 適用された標準マッピング]、[!UICONTROL 一致しない標準マッピング]および[!UICONTROL カスタムマッピング]のパネルが表示されます。各カテゴリに関する詳細は、次の表を参照してください。

| 標準フィールドをマッピング | 説明 |
| --- | --- |
| [!UICONTROL 適用された標準マッピング] | [!UICONTROL 適用された標準マッピング]パネルには、マッピングされた属性の総数が表示されます。標準マッピングとは、ソース [!DNL Analytics] データ内の全属性と [!DNL Analytics] フィールドグループ内の対応する属性との間のマッピングセットを指します。これらは事前にマッピングされており、編集できません。 |
| [!UICONTROL 一致しない標準マッピング] | [!UICONTROL 一致しない標準マッピング]パネルは、フレンドリ名の競合を含むマッピング済み属性の数を参照します。 これらの競合は、別のレポートスイートからフィールド記述子のセットが既に入力されているスキーマを再利用する場合に発生します。 フレンドリ名が競合していても、[!DNL Analytics] データフローを進めることができます。 |
| [!UICONTROL カスタムマッピング] | [!UICONTROL カスタムマッピング]パネルには、マッピングされたカスタム属性（eVar、prop、リストを含む）の数が表示されます。 カスタムマッピングとは、ソース [!DNL Analytics] データ内のカスタム属性と、選択したスキーマに含まれるカスタムフィールドグループの属性との間のマッピングセットを指します。 |

![map-standard-fields](../../../../images/tutorials/create/analytics/map-standard-fields.png)

[!DNL Analytics]ExperienceEvent テンプレートスキーマフィールドグループのプレビューを行うには、[!UICONTROL 適用された標準マッピング]パネルで「**[!UICONTROL 表示]**」を選択します。 

![表示](../../../../images/tutorials/create/analytics/view.png)

この [!UICONTROL Adobe Analytics ExperienceEvent テンプレートスキーマフィールドグループ]ページには、スキーマの構造を調べるためのインターフェイスが用意されています。 終了したら、「**[!UICONTROL 閉じる]**」をクリックします。

![field-group-preview](../../../../images/tutorials/create/analytics/field-group-preview.png)

Experience Platformは、マッピングセットにフレンドリ名の競合がないかを自動的に検出します。 マッピングセットと競合しない場合は、「**[!UICONTROL 次へ]**」をクリックして続行します。

![マッピング](../../../../images/tutorials/create/analytics/mapping.png)

>[!TIP]
>
>ソースレポートスイートと選択したスキーマ間でフレンドリ名の競合がある場合も、フィールド記述子は変更されないことを確認すれば、[!DNL Analytics] データフローを続行することが可能です。または、空の記述子セットで新しいスキーマを作成することもできます。

#### カスタムマッピング

データ準備関数を使用して、カスタム属性の新しいカスタムマッピングまたは計算フィールドを追加できます。 カスタムマッピングを追加するには、「**[!UICONTROL カスタム]**」を選択します。

![ カスタム ](../../../../images/tutorials/create/analytics/custom.png)

必要に応じて、「**[!UICONTROL 新しいマッピングを追加]**」または **[!UICONTROL 計算フィールドを追加]** を選択し、カスタム属性のカスタムマッピングの作成に進むことができます。 データ準備機能の使用方法に関する包括的な手順については、[ データ準備 UI ガイド ](../../../../../data-prep/ui/mapping.md) を参照してください。

次のドキュメントでは、データ準備、計算フィールド、およびマッピング機能について理解するための詳細なリソースを提供します。

* [データ準備の概要](../../../../../data-prep/home.md)
* [データ準備のマッピング機能](../../../../../data-prep/functions.md)
* [計算フィールドを追加](../../../../../data-prep/ui/mapping.md#calculated-fields)

<!-- 
To use Data Prep functions and add new mapping or calculated fields for custom attributes, select **[!UICONTROL View custom mappings]**.

![view-custom-mapping](../../../../images/tutorials/create/analytics/view-custom-mapping.png)

Next, select **[!UICONTROL Add new mapping]**.

Depending on your needs, you can select either **[!UICONTROL Add new mapping]** or **[!UICONTROL Add calculated field]** from the options that appear. 

![add-new-mapping](../../../../images/tutorials/create/analytics/add-new-mapping.png)

An empty mapping set appears. Select the mapping icon to add a source field.

![select-source-field](../../../../images/tutorials/create/analytics/select-source-field.png)

You can use the interface to navigate through the source schema structure and identify the new source field that you want to use. Once you have selected the source field that you want to map, select **[!UICONTROL Select]**.

![select-mapping](../../../../images/tutorials/create/analytics/select-mapping.png)

Next, select the mapping icon under [!UICONTROL Target Field] to map your selected source field to its appropriate target field.

![select-target-field](../../../../images/tutorials/create/analytics/select-target-field.png)

Similar to the source schema, you can use the interface to navigate through the target schema structure and select the target field you want to map to. Once you have selected the appropriate target field, select **[!UICONTROL Select]**.

![select-target-mapping](../../../../images/tutorials/create/analytics/select-target-mapping.png)

With your custom mapping set completed, select **[!UICONTROL Next]** to proceed.

![complete-custom-mapping](../../../../images/tutorials/create/analytics/complete-custom-mapping.png) -->

## リアルタイム顧客プロファイルのフィルタリング {#filtering-for-profile}

>[!CONTEXTUALHELP]
>id="platform_data_prep_analytics_filtering"
>title="フィルタールールの作成"
>abstract="リアルタイム顧客プロファイルにデータを送信する際に、行および列レベルのフィルタリングルールを定義します。行レベルのフィルタリングを使用して、条件を適用し、**プロファイルの取り込みに含める**&#x200B;データを指示します。列レベルのフィルタリングを使用して、**プロファイルの取り込みから除外する**&#x200B;データの列を選択します。フィルタリングルールは、データレイクに送信されるデータには適用されません。"

[!DNL Analytics] レポートスイートデータのマッピングが完了したら、フィルタリングルールと条件を適用して、リアルタイム顧客プロファイルへの取り込みにデータを選択的に含めるか除外することができます。 フィルタリングのサポートは、[!DNL Analytics] のデータに対してのみ使用でき、データは [!DNL Profile.] を入力する前にのみフィルタリングされます。すべてのデータは、データレイクに取り込まれます。

>[!BEGINSHADEBOX]

**リアルタイム顧客プロファイルの分析データのデータ準備およびフィルタリングに関する追加情報**

* フィルタリング機能は、プロファイルに送信されるデータには使用できますが、データレイクに送信されるデータには使用できません。
* ライブデータにはフィルターを使用できますが、バックフィルデータをフィルターすることはできません。
   * [!DNL Analytics] ソースは、プロファイルにデータをバックフィルしません。
* [!DNL Analytics] フローの初期設定中に Data Prep 設定を利用した場合、それらの変更は 13 か月の自動バックフィルにも適用されます。
   * ただし、フィルタリングはライブデータのみに予約されているので、フィルタリングには該当しません。
* データ準備は、ストリーミングとバッチの両方の取り込みパスに適用されます。 既存の Data Prep 設定を変更すると、それらの変更はストリーミングとバッチの両方の取り込み経路を通じて新しい受信データに適用されます。
   * ただし、Data Prep 設定は、ストリーミングデータかバッチデータかに関係なく、既にExperience Platformに取り込まれているデータには適用されません。
* Analytics の標準属性は、常に自動的にマッピングされます。 したがって、標準属性に変換を適用することはできません。
   * ただし、ID サービスまたはプロファイルで必要でない限り、標準属性を除外できます。
* 列レベルのフィルタリングを使用して、必須フィールドおよび ID フィールドをフィルタリングすることはできません。
* セカンダリ ID （特に AAID と AACustomID）を除外することはできますが、ECID を除外することはできません。
* 変換エラーが発生すると、対応する列は NULL になります。

>[!ENDSHADEBOX]

### 行レベルのフィルタリング

>[!IMPORTANT]
>
>行レベルのフィルタリングを使用して、条件を適用し、**プロファイルの取り込みに含める**&#x200B;データを指示します。列レベルのフィルタリングを使用して、**プロファイルの取り込みから除外** するデータの列を選択します。

取り込みのデータ [!DNL Profile]、行レベルと列レベルでフィルタリングできます。 行レベルのフィルタリングでは、文字列に「次を含む」、「次と等しい」、「次で始まる」、「次で終わる」などの条件を定義できます。 また、行レベルのフィルタリングを使用して、`AND` と `OR` を使用して条件を結合したり、`NOT` を使用して条件を否定したりできます。

行レベルで [!DNL Analytics] データをフィルタリングするには、「**[!UICONTROL 行フィルター]**」を選択します。

![ 行フィルター ](../../../../images/tutorials/create/analytics/row-filter.png)

左側のパネルを使用してスキーマ階層内を移動し、目的のスキーマ属性を選択して、特定のスキーマをさらにドリルダウンします。

![ 左レール ](../../../../images/tutorials/create/analytics/left-rail.png)

設定する属性を特定したら、その属性を選択して、左側のパネルからフィルタリングパネルにドラッグします。

![filtering-panel](../../../../images/tutorials/create/analytics/filtering-panel.png)

様々な条件を設定するには、「**[!UICONTROL に等しい]**」を選択し、表示されるドロップダウンウィンドウから条件を選択します。

設定可能な条件のリストを以下に示します。

* [!UICONTROL equals]
* [!UICONTROL  次と等しくない ]
* [!UICONTROL  次で始まる ]
* [!UICONTROL  次で終わる ]
* [!UICONTROL  次で終わらない ]
* [!UICONTROL contains]
* [!UICONTROL  次を含まない ]
* [!UICONTROL exists]
* [!UICONTROL  存在しません ]

![ 条件 ](../../../../images/tutorials/create/analytics/conditions.png)

次に、選択した属性に基づいて、含める値を入力します。 次の例では、[!DNL Apple] と [!DNL Google] が **[!UICONTROL Manufacturer]** 属性の一部として取り込み用に選択されています。

![include-manufacturer](../../../../images/tutorials/create/analytics/include-manufacturer.png)

フィルター条件をさらに指定するには、スキーマから別の属性を追加してから、その属性に基づいて値を追加します。 次の例では、**[!UICONTROL モデル]** 属性が追加され、[!DNL iPhone 13] や [!DNL Google Pixel 6] などのモデルが取り込み用にフィルタリングされます。

![include-model](../../../../images/tutorials/create/analytics/include-model.png)

新しいコンテナを追加するには、フィルタリングインターフェイスの右上にある省略記号（`...`）を選択し、「**[!UICONTROL コンテナを追加]** を選択します。

![add-container](../../../../images/tutorials/create/analytics/add-container.png)

新しいコンテナを追加したら、「**[!UICONTROL 含める]**」を選択し、表示されるドロップダウンウィンドウから「**[!UICONTROL 除外]**」を選択します。

![ 除外 ](../../../../images/tutorials/create/analytics/exclude.png)

次に、同じプロセスを完了して、スキーマ属性をドラッグし、フィルタリングから除外する対応する値を追加します。 次の例では、[!DNL iPhone 12]、[!DNL iPhone 12 mini]、[!DNL Google Pixel 5] はすべて **[!UICONTROL モデル]** 属性からの除外からフィルタリングされ、横は **[!UICONTROL 画面の向き]** から除外され、モデル番号 [!DNL A1633] は **[!UICONTROL モデル番号]** から除外されています。

終了したら、「**[!UICONTROL 次へ]**」を選択します。

![exclude-example](../../../../images/tutorials/create/analytics/exclude-examples.png)

### 列レベルのフィルタリング

ヘッダーから **[!UICONTROL 列フィルター]** を選択して、列レベルのフィルタリングを適用します。

![ 列フィルター ](../../../../images/tutorials/create/analytics/column-filter.png)

ページが更新されてインタラクティブスキーマツリーになり、スキーマ属性が列レベルで表示されます。 ここから、取り込みから除外するデータの列 [!DNL Profile] 選択できます。 または、列を展開して、除外する特定の属性を選択することもできます。

デフォルトでは、すべての [!DNL Analytics] は [!DNL Profile] に移動します。このプロセスを使用すると、XDM データのブランチを取り込みから除外 [!DNL Profile] きます。

終了したら、「**[!UICONTROL 次へ]**」を選択します。

![ 列が選択されました ](../../../../images/tutorials/create/analytics/columns-selected.png)

### セカンダリ ID のフィルタリング

列フィルターを使用して、プロファイルの取り込みからセカンダリ ID を除外します。 セカンダリ ID をフィルタリングするには、「**[!UICONTROL 列フィルター]**」を選択してから、「**[!UICONTROL _identities]**」を選択します。

フィルターは、ID がセカンダリとしてマークされている場合にのみ適用されます。 ID が選択されていても、プライマリとしてマークされた ID のいずれかがイベントに届いた場合、その ID は除外されません。

![ セカンダリ ID](../../../../images/tutorials/create/analytics/secondary-identities.png)

### データフローの詳細を入力

**[!UICONTROL データフローの詳細]**&#x200B;手順が表示され、データフローの名前と説明（オプション）を入力する必要があります。 完了したら、「**[!UICONTROL 次へ]**」をクリックします。

![dataflow-detail](../../../../images/tutorials/create/analytics/dataflow-detail.png)

### レビュー

[!UICONTROL レビュー]手順が表示され、新しい Analytics データフローを作成前にレビューすることができます。 接続の詳細は、次のようなカテゴリ別にグループ化されます。

* [!UICONTROL 接続]：接続のソースプラットフォームを表示します。
* [!UICONTROL データタイプ]：選択したレポートスイートと、対応するレポートスイート ID が表示されます。

![レビュー](../../../../images/tutorials/create/analytics/review.png)

## データフローの監視 {#monitor-your-dataflow}

データフローが完了したら、ソースカタログで **[!UICONTROL データフロー]** を選択し、データのアクティビティとステータスを監視できます。

![ 「データフロー」タブが選択されたソースカタログ ](../../../../images/tutorials/create/analytics/select-dataflows.png)

組織内の既存の Analytics データフローのリストが表示されます。 ここから、ターゲットデータセットを選択して、それぞれの取り込みアクティビティを表示します。

![ 組織内の既存のAdobe Analytics データフローのリスト。](../../../../images/tutorials/create/analytics/select-target-dataset.png)

[!UICONTROL  データセットアクティビティ ] ページには、Analytics からExperience Platformに送信されるデータの進行状況に関する情報が表示されます。 インターフェイスには、先月のレコードの合計、過去 7 日間に取り込んだレコードの合計、先月のデータのサイズなどの指標が表示されます。

ソースは、2 つのデータセットフローをインスタンス化します。 1 つのフローはバックフィルデータ、もう 1 つはライブデータのフローを表します。 バックフィルデータは、リアルタイム顧客プロファイルへの取り込み用に設定されていませんが、分析およびデータサイエンスのユースケース用にデータレイクへと送信されます。

バックフィル、ライブデータおよびそれぞれのレイテンシーの詳細については、[Analytics ソースの概要 ](../../../../connectors/adobe-applications/analytics.md) を参照してください。

![Adobe Analytics データ用の特定のターゲットデータセットのデータセットアクティビティページ ](../../../../images/tutorials/create/analytics/dataset-activity.png)

>[!NOTE]
>
>Analytics ソースコネクタはAdobeによって完全に管理されるので、データセットアクティビティページにバッチに関する情報が表示されません。 取り込んだレコードの周囲の指標を確認することで、データのフローを監視できます。

## データフローの削除 {#delete-dataflow}

Analytics データフローを削除するには、ソースワークスペースの上部のヘッダーから **[!UICONTROL データフロー]** を選択します。 データフローページを使用して、削除する Analytics データフローを見つけ、その横にある省略記号（`...`）を選択します。 次に、ドロップダウンメニューを使用して「**[!UICONTROL 削除]**」を選択します。

* ライブ Analytics データフローを削除すると、基になるデータセットも削除されます。
* Analytics データフローのバックフィルを削除しても、基になるデータセットは削除されませんが、対応するレポートスイートのバックフィルプロセスは停止します。 バックフィルデータフローを削除した場合でも、取り込まれたデータはデータセットを使用して表示できます。

## 次の手順とその他のリソース

接続を作成すると、受信データを格納して選択したスキーマをデータセットに投入するデータフローが自動的に作成されます。さらに、データのバックフィルが発生し、最大 13 か月の履歴データを取り込みます。初回の取り込みが完了したら、データを取り [!DNL Analytics] み、ダウンストリームのExperience Platform サービス（[!DNL Real-Time Customer Profile] やセグメント化サービスなど）で使用できるようにします。 詳しくは、次のドキュメントを参照してください。

* [[!DNL Real-Time Customer Profile] 概要](../../../../../profile/home.md)
* [[!DNL Segmentation Service] の概要](../../../../../segmentation/home.md)
* [[!DNL Data Science Workspace] の概要](../../../../../data-science-workspace/home.md)
* [[!DNL Query Service] の概要](../../../../../query-service/home.md)

次のビデオは、Adobe Analytics Source コネクタを使用したデータの取り込みに関する理解を深めることを目的としています。

>[!WARNING]
>
> 次のビデオに示す [!DNL Experience Platform] UI は旧式のものです。最新の UI のスクリーンショットと機能については、上記のドキュメントを参照してください。

>[!VIDEO](https://video.tv.adobe.com/v/29687?quality=12&learn=on)
