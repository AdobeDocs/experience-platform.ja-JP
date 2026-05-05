---
keywords: Experience Platform；クエリサービス；Data Distiller；アクセラレータ；パラメータ化されたクエリ；SQL テンプレート
solution: Experience Platform
title: Data Distiller アクセラレータ
description: Data Distiller アクセラレーターを使用して、クエリサービス UIでAdobe承認のパラメーター化されたSQL テンプレートを実行およびスケジュールします。 アクセラレータは読み取り専用で、Adobeで管理されます。**[!UICONTROL Create custom template]**を使用して複製および編集します。
source-git-commit: 5ee579c15fc2d9954673062b08280d9060b5205a
workflow-type: tm+mt
source-wordcount: '1300'
ht-degree: 0%

---

# Data Distiller アクセラレータ {#data-distiller-accelerators}

Data Distiller アクセラレータは、一般的な分析シナリオ向けに設計された、Adobeで作成されたパラメータ化されたSQL テンプレートです。 アクセラレーターを使用して、SQLをゼロから記述することなく一般的な分析を実行できます。 アクセラレーターは読み取り専用で、Adobeによってメンテナンスされ、組織全体の一貫性を確保します。 変更が必要な場合は、カスタムテンプレートとして複製できます。

このガイドでは、[!UICONTROL Queries] ワークスペースでアクセラレータを実行、スケジュール、複製する方法について説明します。

>[!AVAILABILITY]
>
>Data Distiller アクセラレータは、Data Distiller SKUを持つ組織でのみ使用できます。 「[!UICONTROL Accelerators]」タブと関連ワークフローには、Data Distiller アドオンが必要です。 詳しくは、[Data Distillerの概要](../data-distiller/overview.md)を参照するか、Adobe担当者にお問い合わせください。

## 前提条件 {#prerequisites}

まず、次の要件を満たしていることを確認してください。

* Experience Platformの[!UICONTROL Queries] ワークスペースにアクセスできます。
* クエリ エディターの使用方法とクエリの実行方法[を理解しています。](./user-guide.md)
* [ パラメーター化されたクエリ ](./parameterized-queries.md) （実行時にSQLのプレースホルダーが置き換えられました）について詳しい方もいらっしゃると思います。

## アクセラレータの使用例 {#when-to-use}

Funnel分析、移動平均、オーディエンスの重複などの一般的な分析パターンに対応する、事前定義済みのSQLが必要な場合は、アクセラレーターを使用できます。 ユースケースに適合するアクセラレータがない場合は、[ クエリエディター](./user-guide.md#query-authoring)でカスタムクエリを作成するか、新しいアクセラレータをリクエストします（[新しいアクセラレータをリクエスト ](#request-accelerator)を参照）。

少数のアクセラレーターがダッシュボードとして開いて即座に分析したり、クエリエディターで開いてロジックを実行、スケジュール、調整したりできます。 [ ダッシュボードにリンクされたアクセラレータ ](#dashboard-accelerators) セクションを参照して、これらの事前設定済みのビジュアライゼーションがオーディエンスデータに関するインサイトをどのように提供するかをご確認ください。

アクセラレーターの使用を開始するには、**[!UICONTROL Queries]** ワークスペースに移動し、**[!UICONTROL Accelerators]** タブまたは&#x200B;**[!UICONTROL Overview]** タブを開きます。

## アクセラレーターの検出パス {#discovery-paths}

カタログ全体または推奨テンプレートのどちらを使用するかに応じて、クエリワークスペースからアクセラレータに2つの方法でアクセスできます。

### 「アクセラレータ」タブの使用

使用可能なすべてのアクセラレータを参照する場合は、このパスを使用します。 完全なアクセラレーターカタログを開くには、左側のナビゲーションで「**[!UICONTROL Queries]**」を選択し、「**[!UICONTROL Accelerators]**」タブを選択します。

ワークスペースには、名前、SQL プレビュー、およびタイムスタンプを含むアクセラレータのテーブルが表示されます。 アクセラレーター名を選択して、クエリエディターで開きます。

>[!NOTE]
>
>**[!UICONTROL Accelerators]** タブから選択したすべてのアクセラレータがクエリエディターで開きます。

![ アクセラレーターのテーブルを表示する「アクセラレーター」タブが選択されたクエリワークスペース。](../images/ui/accelerators/accelerators-tab-table.png)

### 「概要」タブの使用

このパスは、おすすめのアクセラレータにすばやくアクセスする場合に使用します。 **[!UICONTROL Queries]**&#x200B;に移動し、「**[!UICONTROL Overview]**」タブを選択します。 次に、**[!UICONTROL Recommended Data Distiller accelerators]** セクションからカードを選択します。

ほとんどのアクセラレーターはクエリエディターで開きます。 少数のアクセラレーターが、事前定義済みのビジュアライゼーションを備えたダッシュボードとして開きます。 カードがクエリエディターではなくダッシュボードを開く場合は、[ ダッシュボードにリンクされたアクセラレータ ](#dashboard-accelerators)を参照してください。

![概要タブが選択されたクエリ ワークスペースには、推奨されるData Distiller アクセラレータのリストが表示されます。](../images/ui/accelerators/queries-overview-accelerators.png)

## クエリエディターでアクセラレーターを開く {#open-accelerator}

この節では、クエリエディターでアクセラレーターを開いたときに何が起こるか、アクセラレーターの実行、スケジュール設定、カスタムテンプレートの作成など、次に実行できるアクションについて説明します。

アクセラレーターを開いた後、アクセラレーターを&#x200B;**実行**&#x200B;して結果を表示したり、**アクセラレーターを自動的に実行することを** スケジュールしたり、**カスタムテンプレートを作成して** SQLを変更したりできます。

>[!NOTE]
>
>クエリエディターでアクセラレーターを開くと、SQLは読み取り専用の状態でプリロードされ、[!UICONTROL Show results]、[!UICONTROL Undo text]、[!UICONTROL Format text]などのツールバーアクションは無効になります。

右側のパネルには、**[!UICONTROL Accelerator ID]**、**[!UICONTROL Name]**、変更の詳細などのメタデータが表示され、**[!UICONTROL Add schedule]**&#x200B;までのスケジュール設定にアクセスできます。

![ アクセラレーターを開いたクエリ エディター。SQL領域、クエリ パラメーターのタブ、右側のパネルが表示されます。](../images/ui/accelerators/accelerator-query-editor.png)

### パラメーターを指定し、アクセラレーターを実行する {#provide-parameters-execute}

アクセラレーターを実行するには、まず必要なすべてのパラメーターの値を指定する必要があります。 パラメーターは`${PARAMETER_NAME}`構文を使用し、エディターの下の&#x200B;**[!UICONTROL Query parameters]** タブに表示されます。 例えば、`${START_DATE}`には`YYYY-MM-DD`形式の日付値が必要です（例：`2024-01-01`）、`${AUDIENCE_ID}`には特定のオーディエンス IDが必要です。

アクセラレーターを実行するには：

1. **[!UICONTROL Query parameters]**&#x200B;を選択し、各パラメーターの値を入力します。
2. 再生アイコンを選択します（![再生アイコン。](../../images/icons/play.png)） ツールバーの

アクセラレーターが実行され、**[!UICONTROL Results]** タブに結果が表示されます。 これらの結果は、**[!UICONTROL Run as CTAS]**&#x200B;を使用するか、アクセラレーターをスケジュールしない限り、データセットに保持されません。

パラメーター化されたクエリについて詳しくは、[ クエリエディターでのパラメーター化されたクエリ ](./parameterized-queries.md)を参照してください。

## アクセラレーターからの結果の永続化 {#persist-results}

アクセラレーターを実行して結果を確認したら、出力をデータセットに保持できます。

結果からデータセットを作成するには、**[!UICONTROL Save]**&#x200B;を選択してアクセラレータをテンプレートとして保存し、**[!UICONTROL Run as CTAS]**&#x200B;を選択します。 **[!UICONTROL Enter output dataset details]** ダイアログが表示されます。 データセット名とオプションの説明を入力し、データセットを作成することを確認します。 このアクションは、新しいデータセットを作成し、そのデータセットに結果を書き込みます。

![ データセット名と説明が入力された[!UICONTROL Enter output dataset details] ダイアログ。](../images/ui/accelerators/output-dataset-details-dialog.png)

## アクセラレーターのスケジュール {#schedule-accelerator}

固定パラメーター値を使用してアクセラレーターを自動的に実行するようにスケジュールするには、右側のパネルで「**[!UICONTROL Add schedule]**」を選択します。

>[!TIP]
>
>スケジュールを設定する前に、必要なパラメーター値を理解していることを確認してください。 最初にアクセラレータを実行して、結果を検証します。

スケジュール設定ダイアログが表示されます。

![頻度、日付範囲、出力データセット、およびパラメーターフィールドを表示するスケジュール設定ダイアログ。](../images/ui/accelerators/schedule-details.png)

スケジュール設定ダイアログで、頻度、タイムフレーム、出力データセットおよびパラメーター値を再度指定する必要があります。 クエリエディターで入力されたパラメーター値は、スケジュール設定には含まれません。 **[!UICONTROL Dataset details]** セクションでは、**[!UICONTROL Append into existing dataset]**&#x200B;または&#x200B;**[!UICONTROL Create and append into new dataset]**&#x200B;を選択できます。 スケジュールを設定すると、設定に基づいてアクセラレータが自動的に実行され、選択したデータセットに結果が書き込まれます。

詳細な手順については、[ クエリ スケジュールの作成](./query-schedules.md#create-schedule) ガイドを参照してください。

## アクセラレーターからカスタムテンプレートを作成する {#create-custom-template}

SQLを変更したり、独自の設定でロジックを再利用したりする必要がある場合は、アクセラレーターからカスタムテンプレートを作成できます。 まず、クエリエディターでアクセラレーターを開き、**[!UICONTROL Create custom template]**&#x200B;を選択します。 必要に応じてSQLと詳細を変更し、**[!UICONTROL Save]**&#x200B;または&#x200B;**[!UICONTROL Save and close]**&#x200B;を選択してテンプレートを保存します。

保存すると、テンプレートは編集可能になり、CTASで実行、スケジュール設定、または使用できます。 テンプレートは&#x200B;**[!UICONTROL Templates]** タブに保存され、他のテンプレートと同様に管理できます。 詳しくは、[ クエリテンプレート ](./query-templates.md)を参照してください。

### カスタムテンプレートの作成時の変化 {#custom-template-differences}

複製されたテンプレートは、SQLが編集可能であり、変更を保存したり、テンプレートを削除したり、スケジュールしたりできるため、元のアクセラレーターとは異なります。 **[!UICONTROL Modified by]** フィールドに名前が表示されます。 テンプレートは、**[!UICONTROL Accelerators]**&#x200B;ではなく&#x200B;**[!UICONTROL Templates]** タブにあります。

## ダッシュボードにリンクされたアクセラレータ {#dashboard-accelerators}

**[!UICONTROL Overview]** タブの一部のアクセラレータは、SQL クエリではなくダッシュボードとして開きます。 これらのアクセラレータは、オーディエンスデータを分析するための事前構築済みのビジュアライゼーションを提供し、パラメーター入力や手作業による実行を必要としません。

次のアクセラレータが&#x200B;**[!UICONTROL Dashboards]** ワークスペースで開きます。

**[!UICONTROL Advanced Audience Overlaps]**&#x200B;は、選択したオーディエンス間または完全なオーディエンスセット間の交差を分析して、重複パターンを特定します。 これらのインサイトを活用してセグメンテーションを洗練させ、冗長なターゲティングを減らします。

**[!UICONTROL Audience Comparison]**&#x200B;は、サイズ、ID構成、経時的な変化など、2つのオーディエンス間の主要な指標を並べて比較します。 このビューを使用して、パフォーマンスの違いを評価し、ターゲティングの意思決定に役立てることができます。

**[!UICONTROL Audience Trends]**&#x200B;は、オーディエンスのサイズとID数など、オーディエンス指標が時間の経過とともにどのように変化するかを追跡します。 これらのトレンドをもとに、ビジネスの成長を追跡し、セグメンテーション戦略の成果を評価します。

**[!UICONTROL Audience Identity Overlaps]**&#x200B;は、ID タイプが選択したオーディエンス内でどのように重複するかを調べて、IDの関係を理解します。 この分析を使用して、IDの合成とセグメンテーションの精度を向上させます。

![ チャートとフィルターを使用したオーディエンス分析ビジュアライゼーションを表示するダッシュボードビュー。](../images/ui/accelerators/dashboard-accelerator-template-example.png)

ダッシュボードが開いたら、利用可能なコントロールとフィルターを使用して、オーディエンスデータを検索および比較します。 詳しくは、[ ダッシュボードテンプレート ](../../dashboards/sql-insights-query-pro-mode/templates/overview.md)を参照してください。

## 新しいアクセラレーターをリクエスト {#request-accelerator}

既存のアクセラレータでカバーされていない繰り返し使用するユースケースがある場合は、Adobe サポートチャネルを通じてリクエストを送信します。 Adobeでは、一般的な使用パターンと業界の適用性にもとづいてリクエストを評価します。

## 次の手順 {#next-steps}

アクセラレーターを使用して、一般的な分析クエリを実行および自動化できるようになりました。

ワークフローを拡張するには、[ クエリテンプレート ](./query-templates.md#browse)を作成して参照し、[ パラメーター化されたクエリ ](./parameterized-queries.md)を作成し、[ クエリ ](./query-schedules.md)をスケジュールするか、[ クエリサービスワークフロー](./user-guide.md)を探索します。
