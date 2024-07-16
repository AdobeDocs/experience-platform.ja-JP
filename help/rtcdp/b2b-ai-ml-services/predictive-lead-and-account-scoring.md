---
title: Real-Time CDP B2B でのリードおよびアカウントの予測スコアリング
type: Documentation
description: Experience Platform CDP B2B のリードおよびアカウントの予測スコアリング機能の概要と詳細です。
feature: Profiles, B2B
badgeB2B: label="B2B エディション" type="Informative" url="https://helpx.adobe.com/legal/product-descriptions/real-time-customer-data-platform-b2b-edition-prime-and-ultimate-packages.html newtab=true"
exl-id: d3afbabb-005d-4537-831a-857c88043759
source-git-commit: db57fa753a3980dca671d476521f9849147880f1
workflow-type: tm+mt
source-wordcount: '859'
ht-degree: 11%

---

# Real-Time CDP B2B でのリードおよびアカウントの予測スコアリング

B2B マーケターは、マーケティングファネルの最上位で複数の課題に直面します。 効果的であるためには、B2B マーケターは、高価値のターゲットに集中できるように、多数の人物を選定するための自動化された方法を必要としています。 選定は、マーケティングコンバージョンだけでなく、最終的な販売結果と連携する必要があります。

アカウントは、B2B の製品とサービスを購入する最終的なエンティティです。 効果的にマーケティングおよび販売するために、B2B マーケターは、個人だけでなくアカウントの購入可能性も知る必要があります。

特に、アカウントベースドマーケティングでは、マーケティングターゲットとしてアカウントを戦略します。 アカウントの購入傾向スコアは、B2B マーケターがアカウント間で優先順位を付けて、投資回収率を最大化するのに非常に役立ちます。

リードおよびアカウントの予測スコアリングサービスでは、商談ステージのコンバージョンイベントから学習して予測し、アカウントレベルでユーザーのアクティビティを集計してアカウントスコアを生成することで、上記の課題に対処します。 スコアは、ユーザープロファイルとアカウントプロファイルのカスタムフィールドとしてすぐに使用でき、セグメント条件として簡単に含めてオーディエンスを絞り込むことができます。 また、最も影響力のある要因は、集計と単位レベルの両方で利用でき、B2B マーケターがスコアの原因となった要素をより深く理解できるようになります。

## リードおよびアカウントの予測スコアリングについて {#how-it-works}

>[!NOTE]
>
>ユ [!DNL Marketo] ザーのプロファイルレベルでコンバージョンイベントを提供できる唯一のデータソースなので、データソースは現在必要です。

リードおよびアカウントの予測スコアリングでは、ツリーベース（ランダムフォレスト/グラデーションブースト）の機械学習手法を使用して、リードの予測スコアリングモデルを作成します。

管理者は、設定されたコンバージョンイベントごとに 1 つ、複数のプロファイルスコアリング目標（モデルとも呼ばれます）を設定する機能を持っています。これにより、設定された目標ごとに個別のスコアを生成できます。

リードおよびアカウントの予測スコアリングでは、次のコンバージョン目標タイプおよびフィールドをサポートしています。

| 目標タイプ | フィールド |
| --- | --- |
| `leadOperation.convertLead` | <ul><li>`leadOperation.convertLead.convertedStatus`</li><li>`leadOperation.convertLead.assignTo`</li></ul> |
| `opportunityEvent.opportunityUpdated` | <ul><li>`opportunityEvent.dataValueChanges.attributeName`</li><li>`opportunityEvent.dataValueChanges.newValue`</li><li>`opportunityEvent.dataValueChanges.oldValue`</li>例：`opportunityEvent.dataValueChanges.attributeName` は `Stage` と等しく、`opportunityEvent.dataValueChanges.newValue` は `Contract` と等しい</ul> |

アルゴリズムでは、次の属性と入力データが考慮されます。

* ユーザープロファイル

| XDM フィールド | 必須/オプション |
| --- | --- |
| `personComponents.sourceAccountKey.sourceKey` | 必須 |
| `workAddress.country` | オプション |
| `extSourceSystemAudit.createdDate` | 必須 |
| `extendedWorkDetails.jobTitle` | オプション |

>[!NOTE]
> 
>アルゴリズムは、Person:personComponents フィールドグループの `sourceAccountKey.sourceKey` のフィールドのみを調べます。

* アカウントプロファイル

| XDM フィールド | 必須/オプション |
| --- | --- |
| `accountKey.sourceKey` | 必須 |
| `extSourceSystemAudit.createdDate` | 必須 |
| `accountOrganization.industry` | オプション |
| `accountOrganization.numberOfEmployees` | オプション |
| `accountOrganization.annualRevenue.amount` | オプション |

* エクスペリエンスイベント

| XDM フィールド | 必須/オプション |
| --- | --- |
| `_id` | 必須 |
| `personKey.sourceKey` | 必須 |
| `timestamp` | 必須 |
| `eventType` | 必須 |

次のハードリミットが設定された複数のモデルがサポートされています。

* 各実稼動サンドボックスには、5 つのモデルの権利が付与されます。
* 開発用サンドボックスごとに 1 つのモデルに対する権限が付与されます。

データ品質の要件は次のとおりです。

* トレーニング用に最新のデータが 2 年間存在するのが理想的です。
* 必要なデータの最小長は、6 か月と予測ウィンドウです。
* 予測目標ごとに、少なくとも 10 個の適格なコンバージョンイベントが必要です。

スコアリングジョブは毎日実行され、結果はプロファイル属性およびアカウント属性として保存され、セグメント定義およびパーソナライゼーションで使用できます。 標準の分析インサイトは、アカウントの概要ダッシュボードでも使用できます。

[ リードおよびアカウントの予測スコアリングの管理 ](/help/rtcdp/b2b-ai-ml-services/manage-predictive-lead-and-account-scoring.md) サービスの管理方法について詳しくは、ドキュメントを参照してください。

## リードおよびアカウントの予測スコアリング結果の表示 {#how-to-view}

ジョブの実行後、結果は、`LeadsAI.Scores` - ***スコア名*** という名前で、各モデルの新しいシステムデータセットに保存されます。 各スコアフィールドグループは、`{CUSTOM_FIELD_GROUP}.LeadsAI.the_score_name` に配置できます。

| 属性 | 説明 |
| --- | --- |
| Score | プロファイルが、定義された期間内に予測された目標を達成する相対的な可能性。 この値は、確率のパーセンテージではなく、母集団全体と比較したプロファイルの可能性として扱われます。 このスコアは 0～100 です。 |
| 百分位 | この値は、同様にスコアリングされた他のプロファイルと比較したプロファイルのパフォーマンスに関する情報を提供します。百分位数の範囲は 1～100 です。 |
| モデルタイプ | 選択したモデルタイプは、これが人物スコアまたはアカウントのスコアであるかどうかを示します。 |
| スコア日 | スコアリングが発生した日付 |
| 影響を与える要因 | プロファイルがコンバージョンする可能性が高い理由として予測されたもの。 要素は、次の属性で構成されます。<ul><li>コード：プロファイルの予測スコアにプラスの影響を与えるプロファイルまたは行動属性</li><li>値：プロファイルまたは行動属性の値</li><li>重要度：プロファイルまたは行動属性が予測スコア（低、中、高）に持つ重みを示します。</li></ul> |

### 顧客プロファイルスコアの表示

ユーザープロファイルの予測スコアを表示するには、左側のパネルの「顧客」セクションで **[!UICONTROL プロファイル]** を選択し、ID 名前空間と ID 値を入力します。 終了したら、「**[!UICONTROL 表示]**」を選択します。

次に、リストからプロファイルを選択します。

![ 顧客プロファイル ](/help/rtcdp/accounts/images/b2b-view-customer-profile.png)

**[!UICONTROL 詳細]** ページに、予測スコアが含まれるようになりました。 予測スコアの横にあるグラフアイコンをクリックします。

![ 顧客プロファイル予測スコア ](/help/rtcdp/accounts/images/b2b-view-customer-profile-predictive-score.png)

ポップアップダイアログには、スコア、全体的なスコア分布、このスコアに影響を与えた上位の要因およびスコアの目標の定義が表示されます。

![ 顧客プロファイル予測スコアの詳細 ](/help/rtcdp/accounts/images/b2b-view-customer-profile-predictive-score-details.png)

## リードおよびアカウントの予測スコアリングジョブの監視 {#monitoring-jobs}

ダッシュボードから、基本指標と毎日のジョブ実行ステータスを監視できます。 指標には次のものが含まれます。

* スコアリングされた人物/ アカウントプロファイルの合計
* 次のスコアリングジョブ（日付）
* 次回のトレーニング作業（日付）

詳しくは、[ リードおよびアカウントの予測スコアリングに関するジョブの監視 ](/help/dataflows/ui/b2b/monitor-profile-enrichment.md) のドキュメントを参照してください。
