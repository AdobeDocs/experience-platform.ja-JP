---
title: Adobe Experience Platformリリースノート（2022 年 6 月）
description: Adobe Experience Platformの 2022 年 6 月のリリースノート。
source-git-commit: 71f781fafa6124237955e7979fab29e7c03f984e
workflow-type: tm+mt
source-wordcount: '693'
ht-degree: 51%

---

# Adobe Experience Platform リリースノート

**リリース日：2022 年 6 月 22 日**

Adobe Experience Platform の既存の機能に対するアップデート：

- [[!DNL Data Prep]](#data-prep)
- [[!DNL Data Science Workspace]](#dsw)
- [[!DNL Destinations]](#destinations)
- [クエリサービス](#query-service)
- [ソース](#sources)

## [!DNL Data Prep] {#data-prep}

[!DNL Data Prep] を使用すると、データエンジニアは Experience Data Model（XDM）との間でデータをマッピング、変換および検証できます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| の機能強化 [!DNL Data Prep] Recommendations | [!DNL Data Prep] Recommendationsは今、より賢く、より速くなっています。 新しい検証チェックを使用すると、最も一般的なマッピングエラーが大幅に減り、値への時間が短縮されます。 |

{style=&quot;table-layout:auto&quot;}

[!DNL Data Prep] について詳しくは、[[!DNL Data Prep] 概要](../../data-prep/home.md)を参照してください。

## [!DNL Data Science Workspace] {#dsw}

 Data Science Workspace は、機械学習と人工知能を使用して、データからインサイトを引き出します。Adobe Experience Platform に統合された Data Science Workspace は、アドビソリューションでコンテンツやデータアセットを使用して予測をおこなうことを支援します。Data Science Workspace がこれを実現する方法の 1 つは、JupyterLab を使用することです。 JupyterLab は、<a href="https://jupyter.org/" target="_blank">プロジェクト Jupyter</a> の Web ベースのユーザーインターフェイスで、Adobe Experience Platform に緊密に統合されています。これは、データ科学者が Jupyter のノートブック、コード、データを扱うための、インタラクティブな開発環境を提供します。

| 機能 | 説明 |
| --- | --- |
| JupyterLab ランチャー | JupyterLab Launcher に Spark 3.2 ノートブックのスターターが含まれるようになりました。 Spark 2.4 ノートブックスタートは、Spark 3.2 ノートブックに置き換えられ、このリリースに含まれる予定です。 |
| Spark 3.2 | 新しい Scala(Spark) と PySpark のレシピで Spark 3.2 が使用されるようになりました。 |
| カーネル | Scala(Spark) ノートブックは、Scala カーネルを介して作成されるようになりました。 PySpark ノートブックは、Python カーネルを介して作成されるようになりました。 Spark と PySpark カーネルは廃止され、今後のリリースで削除される予定です。 |
| レシピ | 新しい PySpark と Spark のレシピは、Python と R のレシピと同様の Docker ワークフローに従うようになりました。 |

{style=&quot;table-layout:auto&quot;}

Data Science Workspace の一般情報について詳しくは、 [概要ドキュメント](../../data-science-workspace/home.md).

## [!DNL Destinations] {#destinations}

[!DNL Destinations] は、Adobe Experience Platform からのデータの円滑なアクティベーションを可能にする、事前定義済みの出力先プラットフォームとの統合です。宛先を使用して、クロスチャネルマーケティングキャンペーン、電子メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

**新しい宛先**

| 宛先 | 説明 |
| ----------- | ----------- |
| [[!DNL Medallia]](/help/destinations/catalog/voice/medallia-connector.md) | ターゲットを絞った Medalia の調査とフィードバックの収集のプロファイルをアクティブ化して、顧客のニーズと期待をより深く理解します。 |

{style=&quot;table-layout:auto&quot;}

宛先の一般的な情報については、[宛先の概要](../../destinations/home.md)を参照してください。

## クエリサービス {#query-service}

クエリサービスを使用すると、標準 SQL を使用して Adobe Experience Platform [!DNL Data Lake] でデータに対してクエリを実行できます。任意のデータセットを [!DNL Data Lake] から結合し、クエリの結果を新しいデータセットとして取得することで、レポートや Data Science Workspace で使用したり、リアルタイム顧客プロファイルに取り込んだりできます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| アドホックスキーマのラベル付け | クエリサービス CTAS クエリを通じて自動的に生成されるアドホックスキーマのデータフィールドにラベルを適用して、機密データへのアクセスを管理します。 アドホックスキーマの特定のフィールド（データセット）の使用を制限して、機密性の高い個人データと個人を特定できる情報の両方へのアクセスを制御できます。 属性ベースのアクセス制御機能を使用すると、Platform UI を通じてアドホックスキーマフィールドにラベルを付けることができます。 |
| `FLATTEN` 設定 | サードパーティの BI ツールを使用してデータベースに接続する場合、 `FLATTEN` を設定すると、ネストされたデータ構造が別々の列に統合され、属性名が行値を保持する列名になります。 これにより、アドホックスキーマの使いやすさが向上し、ネストされたデータ構造をサポートしない BI ツールでデータを取得、分析、変換およびレポートするために必要なワークロードが削減されます。 |

{style=&quot;table-layout:auto&quot;}

クエリサービスの詳細については、 [クエリサービスの概要](../../query-service/home.md).

## ソース {#sources}

Adobe Experience Platform では、外部ソースからデータを取り込むときに、Platform サービスを使用して、そのデータの構造化、ラベル付け、拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM システムなど、様々なソースからデータを取り込むことができます。

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

| 機能 | 説明 |
| --- | --- |
| [!DNL Mixpanel] ソースのベータ版リリース | これで、 [!DNL Mixpanel] 分析データを取り込むソース [!DNL Mixpanel] アカウントからExperience Platformへ。 詳しくは、 [[!DNL Mixpanel] ソースドキュメント](../../sources/connectors/analytics/mixpanel.md) を参照してください。 |

{style=&quot;table-layout:auto&quot;}

ソースについて詳しくは、[ソースの概要](../../sources/home.md)を参照してください。
