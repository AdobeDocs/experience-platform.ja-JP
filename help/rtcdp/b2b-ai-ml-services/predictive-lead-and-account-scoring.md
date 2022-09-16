---
title: リアルタイム CDP B2B での予測リードおよびアカウントスコアリング
type: Documentation
description: Experience PlatformCDP B2B の予測リードおよびアカウントスコアリング機能の概要と詳細。
exl-id: d3afbabb-005d-4537-831a-857c88043759
source-git-commit: 99b3b2d73b87a64fcaa9ba51563c0942fc21a0dc
workflow-type: tm+mt
source-wordcount: '858'
ht-degree: 9%

---

# リアルタイム CDP B2B での予測リードおよびアカウントスコアリング

B2B マーケターは、マーケティングファネルの最上部で複数の課題に直面します。 効果的な B2B マーケターは、多数の人々を自動認定し、価値の高い目標に集中できるようにする必要があります。 認定は、マーケティングコンバージョンだけでなく、最終的な販売結果と一致する必要があります。

アカウントは、B2B 製品およびサービスを購入する最終的なエンティティです。 効果的にマーケティングおよび販売するために、B2B マーケターは、個人のだけでなく、アカウントの購入の可能性を知る必要があります。

特に、アカウントベースドマーケティングでは、アカウントをマーケティングターゲットとして戦略化します。 アカウントの傾向 — 購入スコアは、B2B マーケターがアカウント間で優先順位を付け、投資収益率を最大限に高めるのに大きく役立ちます。

予測リードおよびアカウントスコアリングサービスは、オポチュニティステージのコンバージョンイベントを学習および予測し、アカウントレベルに個人アクティビティを集計してアカウントスコアを生成することで、上記の課題に対処します。 スコアは、ユーザープロファイルおよびアカウントプロファイルのカスタムフィールドとしてすぐに使用でき、セグメント条件として簡単に含めて、オーディエンスを絞り込むことができます。 また、最も影響力のある要因は、集計と単位レベルの両方で利用でき、B2B マーケターがスコアの原因となった要素をより深く理解できるようになります。

## 予測リードとアカウントスコアリングについて {#how-it-works}

>[!NOTE]
>
>[!DNL Marketo] ユーザープロファイルレベルでコンバージョンイベントを提供できるのは唯一のデータソースなので、現在、データソースが必要です。

予測リードとアカウントスコアリングは、ツリーベース（ランダムフォレスト/グラデーションブースト）の機械学習手法を使用して、予測リードスコアリングモデルを構築します。

管理者は、複数のプロファイルスコアリング目標（モデルとも呼ばれ、設定済みのコンバージョンイベントごとに 1 つの目標）を設定でき、設定済みの目標ごとに個別のスコアを生成できます。

予測リードおよびアカウントスコアリングでは、次のコンバージョン目標タイプおよびコンバージョンフィールドをサポートしています。

| 目標タイプ | フィールド |
| --- | --- |
| `leadOperation.convertLead` | <ul><li>`leadOperation.convertLead.convertedStatus`</li><li>`leadOperation.convertLead.assignTo`</li></ul> |
| `opportunityEvent.opportunityUpdated` | <ul><li>`opportunityEvent.dataValueChanges.attributeName`</li><li>`opportunityEvent.dataValueChanges.newValue`</li><li>`opportunityEvent.dataValueChanges.oldValue`</li>例： `opportunityEvent.dataValueChanges.attributeName` 次と等しい `Stage` および `opportunityEvent.dataValueChanges.newValue` 次と等しい `Contract`</ul> |

アルゴリズムでは、次の属性と入力データを考慮に入れます。

* 人物プロファイル

| XDM フィールド | 必須／オプション |
| --- | --- |
| `personComponents.sourceAccountKey.sourceKey` | 必須 |
| `workAddress.country` | オプション |
| `extSourceSystemAudit.createdDate` | 必須 |
| `extendedWorkDetails.jobTitle` | オプション |

>[!NOTE]
> 
>アルゴリズムは、 `sourceAccountKey.sourceKey` 「 」フィールド（「 Person:personComponents 」フィールドグループ内）に入力します。

* アカウントプロファイル

| XDM フィールド | 必須／オプション |
| --- | --- |
| `accountKey.sourceKey` | 必須 |
| `extSourceSystemAudit.createdDate` | 必須 |
| `accountOrganization.industry` | オプション |
| `accountOrganization.numberOfEmployees` | オプション |
| `accountOrganization.annualRevenue.amount` | オプション |

* エクスペリエンスイベント

| XDM フィールド | 必須／オプション |
| --- | --- |
| `_id` | 必須 |
| `personKey.sourceKey` | 必須 |
| `timestamp` | 必須 |
| `eventType` | 必須 |

複数のモデルがサポートされ、次のハードリミットが設定されます。

* 各実稼動サンドボックスには 5 つのモデルの権利が付与されます。
* 各開発サンドボックスは、1 つのモデルに対して使用できます。

データ品質の要件は次のとおりです。

* トレーニングを目的とした最新のデータが 2 年間存在するのが理想です。
* 必要なデータの最小長は、6 か月と予測期間を足した長さです。
* 各予測目標に対して、少なくとも 10 個の適格なコンバージョンイベントが必要です。

スコアリングジョブは毎日実行され、結果はプロファイル属性とアカウント属性として保存されます。これは、セグメント定義やパーソナライゼーションで使用できます。 標準の分析インサイトは、アカウントの概要ダッシュボードでも利用できます。

方法の詳細については、ドキュメントを参照してください。 [予測リードとアカウントスコアの管理](/help/rtcdp/b2b-ai-ml-services/manage-predictive-lead-and-account-scoring.md) サービス。

## 予測リードとアカウントスコアリング結果の表示 {#how-to-view}

ジョブの実行後、結果は、という名前で各モデルの新しいシステムデータセットに保存されます `LeadsAI.Scores` - ***スコア名***. 各スコアフィールドグループは、 `{CUSTOM_FIELD_GROUP}.LeadsAI.the_score_name`.

| 属性 | 説明 |
| --- | --- |
| Score | プロファイルが、定義された時間枠内で予測された目標を達成する相対的な可能性。 この値は、確率の割合ではなく、全体の母集団と比較したプロファイルの確率として扱われます。 このスコアは 0～100 です。 |
| 百分位 | この値は、同様にスコアリングされた他のプロファイルと比較したプロファイルのパフォーマンスに関する情報を提供します。百分位数の範囲は 1～100 です。 |
| モデルタイプ | 選択したモデルタイプは、これが人物かアカウントスコアかを示します。 |
| スコア日 | スコアリングが発生した日付 |
| 影響を与える要因 | プロファイルがコンバージョンする可能性が高い理由の予測された理由。 要素は、次の属性で構成されます。<ul><li>コード：プロファイルの予測スコアにプラスの影響を与えるプロファイルまたは行動属性。</li><li>値：プロファイルまたは行動属性の値</li><li>重要度：予測スコアに対するプロファイルまたは行動属性の重み（低、中、高）を示します。</li></ul> |

### 顧客プロファイルスコアの表示

人物プロファイルの予測スコアを表示するには、「 **[!UICONTROL プロファイル]** 左パネルの「顧客」セクションで、id 名前空間と id 値を入力します。 終了したら、「 」を選択します。 **[!UICONTROL 表示]**.

次に、リストからプロファイルを選択します。

![顧客プロファイル](/help/rtcdp/accounts/images/b2b-view-customer-profile.png)

この **[!UICONTROL 詳細]** ページに予測スコアが含まれるようになりました。 予測スコアの横にあるグラフアイコンをクリックします。

![顧客プロファイルの予測スコア](/help/rtcdp/accounts/images/b2b-view-customer-profile-predictive-score.png)

ポップアップダイアログには、スコア、全体的なスコアの配分、このスコアの影響を与える上位の要因、スコア目標の定義が表示されます。

![顧客プロファイルの予測スコアの詳細](/help/rtcdp/accounts/images/b2b-view-customer-profile-predictive-score-details.png)

## 予測リードおよびアカウントスコアリングジョブの監視 {#monitoring-jobs}

ダッシュボードを使用して、基本指標と毎日のジョブ実行ステータスを監視できます。 指標には次のものが含まれます。

* スコアリングされた担当者/アカウントプロファイルの合計
* 次のスコアリングジョブ（日付）
* 次のトレーニングジョブ（日付）

詳しくは、 [予測リードとアカウントスコアリングのジョブの監視](/help/dataflows/ui/b2b/monitor-profile-enrichment.md).
