---
title: AI アシスタントの自然言語から SQL モデルへの変換の詳細
description: AI Assistant Natural Language to SQL AI モデルについて説明します。
exl-id: ca157945-5f74-45d0-9d40-c65d09a8e80d
source-git-commit: 26d05e9519491ef2e8f239cba6a2cde43cff941c
workflow-type: tm+mt
source-wordcount: '658'
ht-degree: 0%

---

# AI Assistant Operational Insights Natural Language to SQL モデルの詳細

>[!IMPORTANT]
>
>Adobeは、より多くのモデル詳細を積極的に公開しています。利用可能になり次第、Experience Leagueにドキュメントが追加されます。

## モデルの概要 {#model-overview}

* **モデル名とバージョン**:Adobe Experience Platform AI アシスタント Operational Insights Natural Language to SQL Model （[!DNL NL2SQL]）。
* **モデルリリース日**:2025 年 2 月
* **モデルの目的**：モデルは、運用インサイトに関するお客様の自然言語クエリを SQL クエリに変換するように設計されています。 これらの SQL クエリは、スキーマ、データセット、オーディエンス、宛先、ジャーニーなど、顧客のAdobe Experience Platform エンティティに関するメタデータを含むExperience Platform ナレッジグラフで実行されます。 次に、SQL クエリの結果を使用して、顧客の元の自然言語の質問に対する応答を生成します。
* **対象ユーザー**：このモデルの主要なユーザーは、マーケティング担当者、データアナリストまたはカスタマージャーニー管理者で、自然言語を使用してExperience Platform内のオペレーショナルインサイトを理解して操作することを目指しています。 SQL やデータエンジニアリングの専門家ではないかもしれませんが、十分な情報に基づいた意思決定を行い、カスタマーエクスペリエンスを最適化するには、Experience Platform エンティティに関する迅速かつ正確な回答が必要です。
* **ユースケース**：このモデルは、オーディエンス、ジャーニー、スキーマ、属性、データセット、宛先など、Experience Platform エンティティに関するメタデータにアクセスする際に役立ちます。 ユーザーは、どのオーディエンスがアクティブ化されているか、またはどのオーディエンスが特定のスキーマを使用しているかなど、自然言語で質問できます。また、モデルでは、これらをExperience Platform ナレッジグラフ上の SQL クエリに変換します。 これにより、ユーザーは、システムを手動で調査またはクエリしなくても、運用上の可視性を迅速に得て、十分な情報に基づいた意思決定を行うことができます。
* **問題点**：このモデルは、Experience Platformを使用するビジネスユーザーやアナリストが直面する主な問題点に対処します。例えば、相互接続された大量のメタデータのナビゲートの複雑さ、運用インサイトを取得するための SQL クエリを手動で記述する必要性、オーディエンス、データセット、ジャーニーなどのExperience Platform エンティティの接続やパフォーマンスの状況を把握できないことなどです。 このモデルを使用すると、自然言語によるメタデータへのアクセスが可能になり、SQL 生成が自動化されるので、技術リソースへの依存が軽減され、insightの検出時間が短縮され、ユーザーがデータに基づいた迅速な意思決定を行えるようになります。

## モデルの詳細 {#model-details}

* **モデルタイプ**：動的なコンテキスト内学習を使用した LLM プロンプト
* **入力**: ユーザーの自然言語クエリ。
* **出力**: モデルは SQL クエリを [!DNL Snowflake] 構文で出力します。これは、Experience Platform ナレッジグラフを使用して実行されます。

**入力例**

```console
How many datasets were created within the last 10 days?
```

**出力例**

```SQL
SELECT
    COUNT(*) AS datasetCount 
FROM hkg_dim_dataset 
WHERE
    createdTime >= DATEADD(day, -10, CURRENT_DATE);
```

## モデル評価 {#model-evaluation}

* **評価指標とプロシージャ**:[!DNL NL2SQL] のリクエストを確認し、正しい SQL 結果が得られるリクエストの数を評価することで、モデルが評価されます。 評価プロセスは、ルールベースの照合（SQL の標準化と直接の SQL 文字列照合）、LLM ベースの SQL ソルバー、および人間の評価の組み合わせです。
* **評価データと前処理**：回帰テストにはオープンセットを使用し、サンプルした実際の顧客トラフィックを通じてモデルのパフォーマンスを監視する週別注釈プロジェクトもあります。

## モデルの導入 {#model-deployment}

* **モデルの監視**：基本モデルは [!DNL Azure] でホストされます。
* **モデルの更新**:Adobe Experience Platform AI アシスタントの Operational Insights Natural Language to SQL Model は、クエスチョンバンクの拡張により、定期的（毎週）に更新されます。 必要に応じて、新しいプロンプト戦略と指示を使用してモデルも更新されます。

## 公平性と偏見 {#fairness-and-bias}

* **モデルの公平性**:Adobeでは、バイアスを導入したりステレオタイプを補強したりすることなく、異なるユーザーの意図や言語のバリエーションにわたって一貫してモデルがクエリを解釈および生成できるように、迅速な監査、説明可能性、偏見や非倫理的な出力が生じないように対策を講じます。
* **データバイアス**：モデル出力は、コンテキスト内学習の例や、取得者がプロンプトで選択する内容の影響を受けます。 問題銀行の例には、製品管理の観点から代表的と考えられる例が含まれています。 ユースケースに応じて、モデル出力に含まれる潜在的なバイアスが、意図したアプリケーションにどのように合致するか、または意図したアプリケーションにどのような影響を与えるかを検討する必要があります。

## 倫理的考慮事項 {#ethical-considerations}

**モデルに関連する倫理的考慮事項**:PII や機密属性の公開を避けるために、モデルは、既存のデータバイアスを強化せず、アクセス制御の境界を尊重するように設計されています。 これには、責任ある使用に関するスキーマフィールドのフィルタリング、タグ付け、監査が含まれます。
