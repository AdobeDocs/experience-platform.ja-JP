---
title: Adobe Experience Platform リリースノート
description: Experience Platformリリースノート（2020年6月11日）
doc-type: release notes
last-update: June 10, 2020
author: crhoades, ens28527
translation-type: tm+mt
source-git-commit: b6cfdf56c20065bdc3e8a9fedf6007ddd74eaeaa
workflow-type: tm+mt
source-wordcount: '306'
ht-degree: 6%

---


# Adobe Experience Platform リリースノート

**リリース日：2020 年 6 月 10 日**

Adobe Experience Platformの既存の機能の更新：

- [Data Science Workspace](#dsw)
- [ソース](#sources)

## Data Science Workspace {#dsw}

Data Science Workspaceは、機械学習と人工知能を使用して、データから洞察を引き出します。 Data Science WorkspaceはAdobe Experience Platformに統合されており、アドビのソリューション全体でコンテンツやデータアセットを使用して予測を行うのに役立ちます。

Data Science Workspaceは、リアルタイム機械学習を通じて、より良いエクスペリエンスと予測を可能にする新しい方法に取り組んでいます。 Real-time Machine Learningは、APIエンドポイントを介して、カスタムまたはインポートしたプレトレーニング済みの機械学習モデルを、業界標準の相互運用可能なモデル形式で作成、テスト、導入する機能を提供します。

リアルタイム機械学習はアルファベット順で、現在開発中です。

| 機能 | 説明 |
|--- | ---|
| JupterLabランチャーリアルタイムMLスターター | JupyterLabランチャーに、Real-time Machine Learning (Alpha)用のPythonノートブックスターターが含まれるようになりました。 |

リアルタイム機械学習のアルファについて詳しくは、 [リアルタイム機械学習の概要を参照してください](../../data-science-workspace/real-time-machine-learning/home.md)。

## ソース {#sources}

Adobe Experience Platformでは、外部ソースからデータを取り込みながら、Platform Servicesを使用してデータの構造、ラベル付け、拡張を行うことができます。 アドビのアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRMシステムなど、様々なソースからデータを取り込むことができます。

エクスペリエンスプラットフォームは、RESTful APIとインタラクティブUIを備えており、様々なデータプロバイダーのソース接続を簡単に設定できます。 これらのソース接続を使用すると、外部のストレージシステムやCRMサービスの認証と接続、取り込みの実行時間の設定、データ取り込みスループットの管理を行うことができます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| クラウドストレージシステム向けの追加のAPIとUIのサポート | Apache HDFS用の新しいソースコネクタ |
| データベース用の追加のAPIとUIのサポート | Couchbase用の新しいソースコネクタ。 |

ソースについて詳しくは、 [ソースの概要を参照してください](../../sources/home.md)。
