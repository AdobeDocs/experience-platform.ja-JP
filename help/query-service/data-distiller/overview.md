---
title: Data Distillerの概要
description: ライセンス付与に関する、クエリサービスデータの Data Distillerの使用制限の概要です。
source-git-commit: fa4fc154f57243250dec9bdf9557db13ef7768e8
workflow-type: tm+mt
source-wordcount: '553'
ht-degree: 0%

---

# Data Distillerの概要

Data Distillerは、Adobe Experience Platformの機能のサブセットを含むパッケージ提供です。 Data Distillerを使用すると、クエリサービスでバッチクエリを実行することで、リアルタイム顧客プロファイルや分析的な使用例に対して、取り込み後のデータの準備（クリーニング、整形、操作など）を実行できます。 Data Distillerの使用は、現在および継続中のライセンス（以下のうち少なくとも 1 つ）に依存します。Adobe Real-time Customer Data Platform、Customer Journey Analytics、Adobe Journey Optimizer。

## ライセンス使用状況 {#license-usage}

詳しくは、 [Data Distillerライセンス使用状況ドキュメント](./license-usage.md) を参照して、組織のクエリサービスライセンスの使用状況に関する重要な情報を確認してください。

## スコーピングパラメーター {#scoping-parameters}

スコーピングパラメーターは、ライセンス容量で定義された、提案された使用例の範囲に関連する使用制限です。 アドオンがない場合、Data Distillerのスコープパラメーターは次のようになります。

* **時間を計算**:PSQL またはクエリサービス API を使用して、任意のサンドボックスで実行されるバッチクエリ（スケジュール設定またはその他）を実行し、データのスキャンと書き込みをおこなうことができます。 使用許諾契約の範囲プロセスで決定された、年間に割り当てられた計算時間を使用します。 合計計算時間は、すべてのサンドボックスに蓄積されます。
* **取り込まれたデータ**:Data Distillerを使用してAdobe Experience Platformに取り込まれるデータに対して問い合わせをおこなうことができる制限は、現在のAdobe Real-time Customer Data Platform、Customer Journey Analytics、Adobe Journey Optimizerのライセンスで説明されている制限の影響を受けます。
* **データレイクストレージ**:Adobe Real-time Customer Data Platform、Customer Journey Analytics、Adobe Journey Optimizerに対する現在のライセンスで提供されているデータレイクストレージは、Data Distillerでも使用できます。 データレイクストレージは共有機能です。
* **クエリサービスユーザー**:Adobe Real-time Customer Data Platform、Customer Journey Analytics、Adobe Journey Optimizerに対する現在のライセンスで詳しく説明されているクエリサービスユーザーの数は、Data Distillerでも使用できます。 クエリサービスユーザーは共有機能です。

## ガードレール

詳しくは、 [クエリサービスガードレール](../guardrails.md) ライセンス付与に関するクエリサービスデータのデフォルトの使用制限に関するドキュメント。

## 静的制限

静的制限は、Adobe Experience Platform Activation の機能境界に関連する使用制限です。 [Adobe Experience Platform Activation の詳細](https://helpx.adobe.com/ca/legal/product-descriptions/adobe-experience-platform0.html) は、ヘルプドキュメントの「Adobe」にあります。 Data Distillerの静的制限の概要を以下に示します。詳しくは、クエリサービスガードレールのドキュメントを参照してください。

* **バッチクエリ**:スケジュールされたバッチクエリは、24 時間後にタイムアウトします。
* **クエリサービス**:クエリサービスは、次の目的で使用できます。
   * データ分析および取り込み後のデータ準備（クリーニング、シェーピング、操作）に SQL クエリを実行する。
   * SQL クエリを実行して、BI ツールに直接表示するロールアップ指標を作成する。
   * Adobe Experience Platform内のデータを迅速に検査する。
* **レポート API 呼び出し**:レポート API を使用して集計データに対してクエリを確実に実行するには、効率的に実行するのに十分なリソースが必要です。 これには、Real-time Customer Data Platformが提供するクエリなど、既存のデータモデルを強化するクエリが含まれます。 レポート API は、各クエリに同時実行スロットを割り当てることで、リソースの使用率を追跡します。 同時に使用できるレポート API 呼び出しは最大 4 つです。 BI ツールを通じてレポート API にアクセスし、より多くの同時実行スロットが必要な場合は、BI サーバーが必要です。


