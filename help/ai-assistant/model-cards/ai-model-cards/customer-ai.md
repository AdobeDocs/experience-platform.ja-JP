---
title: 顧客 AI 傾向スコアリングモデルカード
description: 顧客 AI に使用する AI モデルについて説明します。
hide: true
hidefromtoc: true
source-git-commit: 21a95bd678cf83c72a08213b647ef778cfb49cfc
workflow-type: tm+mt
source-wordcount: '1629'
ht-degree: 0%

---

# 顧客 AI 傾向スコアリングモデルカード

[Adobe Experience Platformのインテリジェントサービス ](../../../intelligent-services/home.md) の一部として、[ 顧客 AI](../../../intelligent-services/customer-ai/overview.md) を使用して、個人レベルで顧客予測と説明を生成できます。

影響力のある要因の助けを借りて、顧客 AI を使用して、顧客が何をする可能性があるかとその理由を伝えることができます。 さらに、顧客 AI の予測とインサイトを活用して、最も適切なオファーとメッセージを提供することで顧客体験をパーソナライズできます。

顧客 AI を強化するために使用される AI モデルについては、このモデルカードを参照してください。

## モデルの概要 {#model-overview}

* 顧客 AI は、AI を活用したモデルで、過去の行動とビジネスとのインタラクションに基づいて、ユーザーの傾向スコアを生成するように設計されています。 顧客 AI を使用して、顧客が購入、コンテンツとのエンゲージメント、チャーンなどの特定のアクションを実行する可能性を予測します。 このモデルはExperience Platform内にデプロイされ、様々なマーケティングワークフローや顧客分析ワークフローと統合されます。
* このモデルは、購入、サブスクリプションへの新規登録、メールキャンペーンのエンゲージメントなど、顧客が特定のアクションを実行する可能性を予測することで、マーケターや顧客エンゲージメントチームに実用的なインサイトを提供するように設計されています。 出力を使用すると、企業はオーディエンスのセグメント化を最適化し、予測された行動に基づいて顧客インタラクションをパーソナライズできます。
* これは **監視付き学習分類モデル** であり、顧客の過去のデータに基づいて、イベント（購入、チャーン、エンゲージメント）が発生する可能性を予測します。 傾向スコアをモデル化するためのロジスティック回帰を含んだグラデーションブーストのデシジョンツリー（GBDT）を使用してトレーニングします。
* このモデルの主なユーザーは、Experience Platformを活用してデータ駆動型のマーケティング戦略を推進するマーケティング専門家、データアナリストおよびカスタマーエンゲージメントチームです。
* 顧客 AI はExperience Platformの AI サービスに直接統合されるため、ユーザーは事前定義済みの API とダッシュボードを使用してモデル出力にアクセスできます。 モデルで生成された傾向スコアを [Adobe Journey Optimizer](https://experienceleague.adobe.com/en/docs/journey-optimizer/using/get-started/get-started) および [Real-Time CDP](../../../rtcdp/home.md) 内で使用して、オーディエンスのセグメント化を調整し、マーケティング戦略を調整することができます。

## 使用目的 {#intended-use}

* このモデルは主に **顧客のセグメント化、ターゲット設定されたマーケティングおよびチャーン予測** に使用されます。 企業はこのモデルを活用して **顧客の購入意図の予測、マーケティングキャンペーンの最適化、パーソナライゼーションの取り組みの強化** を行います。 例えば、e コマース会社では、このモデルを使用して、意図の高い買い物客を識別し、排他的なプロモーションを提供する場合があります。
* マーケターは、多くの場合、**ターゲットとすべき適切な顧客の特定** および **エンゲージメントの取り組みの最適化** に苦労します。 このモデルは、顧客ターゲティングに対してデータ駆動型アプローチを提供し、マーケティングリソースが効率的に割り当てられるようにすることで、**推測を減らす** ことができます。
* このモデルは、e コマース、小売、金融サービス、通信、メディアなど、複数の業界で適用できます。 Any business that relies on customer engagement and personalized marketing can benefit from this model.
* The model **should not be used for high-risk decision-making**, such as financial credit scoring, medical diagnostics, or legal assessments. Additionally, it is not intended for use in predicting personally sensitive behaviors (health conditions, political preferences) due to potential ethical concerns.

## Model inputs and outputs {#model-inputs-and-outputs}

* The model processes customer behavioral data, demographic attributes, and historical interactions. This includes data such as website visit frequency, past purchase history, engagement with marketing emails, and demographic information.
* Input data must be structured as JSON objects containing customer attributes and behavioral signals. For batch processing, the model accepts CSV files formatted according to Experience Platform&#39;s data ingestion standards.
* The model outputs a propensity score between 0 and 1, where higher values indicate a higher likelihood of the predicted event occurring. Additionally, it provides feature importance scores, allowing users to understand which factors influenced the prediction.

**Example input**

```json
{
  "customer_id": 12345,
  "past_purchases": 3,
  "last_visit_days": 7,
  "email_click_rate": 0.4
}
```

**Example output**

```json
{
  "customer_id": 12345,
  "propensity_score": 0.82
}
```

## Training data

* モデルは、Experience Platformを通じて収集された **ファーストパーティ、匿名化された顧客インタラクションデータ** に基づいてトレーニングされました。 複数の業界をまたいだ履歴の顧客 **行動データ、トランザクションレコード、メールのインタラクション** エンゲージメント指標などが含まれます。
* トレーニングデータセットは **Experience Platformの様々な顧客から得られた 1,000 万** の顧客レコードで構成されています。 これらのレコードには、小売、e コマース、通信、金融などの様々な業界からの **顧客インタラクションの履歴** トランザクションデータ、行動エンゲージメントログおよび人口統計情報）が含まれます。 データは 24 か月にわたって収集され、季節的なトレンドと長期的なエンゲージメントパターンを十分に反映していました。
* データセットは主にエンゲージメントの高いユーザーから得られるため、選択バイアスが生じる可能性があります。 この問題を軽減するために、モデルでは層別化されたサンプリング、バイアス監査技術、データ増強戦略を適用しています。
* データセットには、データの一貫性、品質、操作性を確保するために大量の前処理が行われます。
   * **Handling Missing Values**: Missing values are addressed using a combination of mean imputation (for numerical fields), mode imputation (for categorical fields), and predictive modeling (for complex missing cases).
   * **カテゴリエンコーディング**：顧客セグメントや購入カテゴリなどのカテゴリ変数は、ワンホットエンコーディングとターゲットエンコーディングの手法を使用して数値表現に変換されます。
   * **フィーチャースケーリングと正規化**:Min-Max スケーリングは境界変数（年齢、収入）に適用され、z スコア標準化は通常分布されるフィーチャーに使用されます。
   * **追加の前処理**：このパイプラインには、異常値の検出と削除、重複フィルタリング、タイムスタンプの標準化、予測モデリングを強化する機能エンジニアリングが含まれます。

## モデルのアーキテクチャとトレーニング

* モデルでは、構造化データ用に最適化された [!DNL XGBoost]**を使用して**[!DNL Gradient Boosting Decision Trees] （GBDT）を活用します。 予測行動パターンを特定するために、過去の顧客イベントシーケンスに基づいてトレーニングを行います。
* モデルは、教師あり学習アプローチを使用して構築され、GBDT と [!DNL XGBoost] を主要学習アルゴリズムとして活用します。 さらに、予測精度をベンチマークするベースラインモデルとしてロジスティック回帰を組み込んだ。
* このモデルは、**[!DNL TensorFlow]**、**[!DNL XGBoost]**、**[!DNL scikit-learn]** を使用して開発されました。 **[!DNL NVIDIA V100]GPU** を使用したAdobe AI クラウドインフラストラクチャでトレーニングを実行し、大規模なデータセットをサポートします。
* Google Cloud インフラストラクチャに関するトレーニングを受けた [!DNL NVIDIA V100 GPUs]。
* [!DNL AUC-ROC]、精度リコールおよびクロス検証。

## パフォーマンスと評価

* モデルは、80% のデータがトレーニングに使用され、20% が評価に予約されるホールドアウト検証アプローチを使用してテストされました。
* モデルの有効性は、[!DNL AUC-ROC] （0.85）、precision-recall （0.78）、および F1-score （0.80）を使用して測定されます。 これらの指標は、様々なセグメントをまたいだモデルの予測能力を評価するのに役立ちます。
* 履歴データが限られている新規顧客セグメントの精度が低下します。
* 履歴データが限られている（コールドスタートの問題）お客様の場合、モデルのパフォーマンスが低下する可能性があります。 Additionally, seasonality effects (holiday shopping trends) may require frequent retraining to maintain accuracy.

## 公平性と偏見

* このモデルでは、異なるユーザーセグメント間でのパフォーマンスの相違を検出するために、**人口統計学的パリティテスト** および **敵対的な公平性評価** を実施しました。
* 分析の結果、履歴インタラクションデータが低いユーザーのパフォーマンスが **5% 低下したことが明らかになりました** この問題に対処するために、モデルにはトレーニング中の重み付けテクニックが組み込まれています。
* データセットは、異なる顧客の人口統計の比例表現を確保するために層別されており、モデルが特定のグループに好まれないように、トレーニング中に公平性制約が導入されます。 定期的なバイアス監査は、人口統計学的パリティ分析を使用して実施され、パフォーマンスの相違が検出された場合に調整できます。

## 説明性と解釈可能性

* モデルでは、**[!DNL SHapley Additive Explanations]（SHAP）を活用して** 各入力機能が予測に与える影響を定量化し、顧客属性が傾向スコアにどのように影響するかを明確にします。 SHAP 値を使用すると、すべての予測で最も影響力のある要因を特定するグローバルな解釈可能性と、特定の顧客の個々の予測を説明するローカルな解釈可能性の両方が可能になります。
* このモデルは、**[!DNL Local Interpretable Model-Agnostic Explanations]（LIME）** および SHAP をサポートし、入力機能が予測に与える影響に関するインサイトを提供します。 LIME は、入力データの乱雑なバージョンを作成し、予測の変化を観察することによってローカル説明を生成し、SHAP は各フィーチャに貢献度値を割り当て、モデル決定のグローバルおよびローカルの両方の解釈を提供します。

## 堅牢性と一般化

* このモデルは、目に見えないデータセットでテストした場合でも 80% の [!DNL AUC-ROC] を維持し、新しい顧客レコードに対して強力な一般化を示しています。 パフォーマンスは様々な顧客セグメントにわたって安定していますが、ユーザーの行動が過去のパターンから大きく逸脱すると、パフォーマンスがわずかに低下します。
* このモデルは、データの欠落、異常値の挿入、意図的な誤ラベル付けなど、混乱した敵対的な入力に対して評価されている。 通常の条件下では性能は引き続き安定していますが、極端な敵対的変更の下では精度の小さな低下（約 3～5 %）が観察されました。

## セキュリティとプライバシーの考慮事項

* モデルは **個人を特定できる情報（PII）を処理または保持しません** で、トレーニングに使用されるすべてのデータは匿名化および集計されます。 責任あるデータ使用を確実にするために、GDPR、CCPA および内部Adobe プライバシーポリシーへの厳格なコンプライアンスに従っています。
* このモデルは差分プライバシー技術を組み込み、制御されたノイズをデータに追加し、個人の再識別を防ぎます。 さらに、モデルのトレーニングと推論の前に、**ハッシュ、匿名化、トークン化の方法を使用して** PII を削除します。

## デプロイメントと統合

* モデルはAdobe Experience Platform AI サービスでホストされ、様々なAdobe アプリケーションと統合されます。 API エンドポイントを介して利用でき、マーケティングおよび顧客エンゲージメントワークフロー全体でリアルタイムの予測とバッチ処理のためのシームレスなアクセスを可能にします。
* このモデルは **[!DNL Kubernetes]ベースのデプロイメントで実行され** 様々なワークロードを効率的に処理するための自動スケーリング機能を備えています。 コンピューティングリソースには、トレーニング用の [!DNL NVIDIA V100] GPU と、リアルタイムのスケーラビリティを実現するための最適化されたCPU ベースの推論が含まれます。

## 監視とメンテナンス

* モデルは [!DNL WatsonX]**を介して継続的に** 監視され、精度ドリフト、特徴重要度シフト、予測安定性などの主要業績評価指標を追跡します。 異常値検出とアラートのメカニズムは、予想される動作から大きく逸脱した場合にチームに通知します。
* モデルは、継続的な関連性を確保するために、更新された顧客インタラクションデータを使用して毎月再トレーニングされます。 定期的な再トレーニングは、予測精度に影響を与える可能性のあるデータドリフトと季節変動を軽減するのに役立ちます

## 倫理的な懸念と責任ある AI

* The model could potentially introduce bias in decision-making if not monitored correctly. For example, if certain demographics are overrepresented in the training data, the model might unfairly favor specific customer groups.
* Experience Platform follows responsible AI guidelines, ensuring that models undergo **bias audits, fairness testing, and human oversight** before deployment.

## 既知の制限事項

* The model may struggle to accurately predict outcomes for newly launched products or customer segments where insufficient historical data is available. Additionally, seasonal variations in customer behavior can cause fluctuations in predictive accuracy if not accounted for during retraining.
* Performance declines when customer history is sparse, such as for first-time buyers or users with minimal engagement data. Additionally, if customer behaviors **shift due to external factors like economic downturns or industry trends**, the model may require rapid adaptation to maintain accuracy.

## Future improvements

* Future iterations will include transfer learning techniques to improve performance for cold-start users and enhance adaptability to changing customer behaviors. Additionally, real-time data integration will be introduced to improve model responsiveness and accuracy in dynamic marketing environments.
