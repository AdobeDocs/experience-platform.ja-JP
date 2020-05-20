---
title: Adobe Experience Platform リリースノート
description: Experience Platformリリースノート（2019年9月11日）
doc-type: release notes
last-update: September 13, 2019
author: ens28527
translation-type: tm+mt
source-git-commit: e5fa12b92f7006f2c5c428b25f81dade57733498
workflow-type: tm+mt
source-wordcount: '590'
ht-degree: 4%

---


# Adobe Experience Platform リリースノート

**リリース日：2019 年 9 月 10 日**

Adobe Experience Platformの既存の機能の更新：

* [データ収集](#ingestion)
* [Data Science Workspace](#dsw)
* [クエリサービス](#query)

## データ収集 {#ingestion}

Adobe Experience Platformは、あらゆる種類のデータや遅延を取り込むための豊富な機能セットを提供します。 Adobe Experience Platform Data Ingestは、Batch API、Streaming API、ネイティブAdobe Connectors、Data Integrationパートナー、Adobe Experience Platform UIなど、データを取り込むための複数の代替手段を提供します。

**新機能**

| 機能 | 説明 |
| ----------- | ---------- |
| ストリーミング取り込み用の新しいドメイン | ドメインは、新しい共通データ収集ドメインに移動され `dcs.data.adobe.net``dcs.adobedc.net`ました。 ユーザーは、改訂されたAdobe Experience Platform Streaming Ingestionドキュメントに従って実装を更新する必要があります。 Adobe Experience Platformストリーミングの取り込みに関連するすべてのドキュメントが、新しいドメインを使用するように更新されました。 |

詳しくは、 [データ収集ドキュメントを参照してください](../../ingestion/home.md)。

## Data Science Workspace {#dsw}

Adobe Experience Platform Data Science Workspaceは、Experience Platform内の完全に管理されたサービスです。Experience Platform Data Science Workspaceを使用すると、機械学習モデルの作成と運用により、データ科学者はアドビのソリューションやサードパーティ製システム全体のデータやコンテンツからの洞察をシステムにわたってシームレスに生成できます。 Data Science Workspaceはプラットフォームと緊密に統合され、XDMデータの調査と準備を含むエンドツーエンドのデータ科学のライフサイクルを強化します。その後、モデルの開発と運用を行い、リアルタイム顧客プロファイルを機械学習インサイトと自動的に強化します。

**新機能**

| 機能 | 説明 |
| -----------| ---------- |
| UIを介したサービスのスケジュール | Platform Orchestration Serviceと統合され、UIを使用して、モデルのトレーニングとスコアリングをユーザー定義のスケジュールで自動化します。 |
| サービスギャラリー | 機械学習サービスを参照、監視、アクセスし、自動トレーニングとスコアリングジョブのスケジュールを設計し直したサービスギャラリー内にあります。 |
| JupterLab 5.0.0 | JupterLabのUIの改善。 |

**既知の問題**

* 現在、サービスギャラリーには、既存のサービスを削除するアクセス方法はありません。 それまでの間、API呼び出しを使用して既存のサービスを削除するには、 [Senesi Machine Learning APIリファレンスを参照してください](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/sensei-ml-api.yaml) 。
* サービスギャラリーには、サービスのトレーニングとスコアリングの実行をフィルターするページネーションのサポートはありません。
* サービスギャラリーで予定されたトレーニングまたはスコアリングの実行を設定する場合、頻度を1時間ごとに設定すると、スケジュールが適用されなくなります。

詳しくは、 [Data Science Workspaceの概要を参照してください](../../data-science-workspace/home.md)。

## クエリサービス {#query}

クエリサービスは、Adobe Experience Platformの標準のSQLからクエリデータへの変換機能を提供し、様々な分析およびデータ管理の使用例をサポートします。 これは、Data Lakeのデータセットを結合し、クエリ結果を新しいデータセットとして取り込み、レポート、Data Science Workspaceで使用したり、リアルタイム顧客プロファイルに取り込んだりするための、サーバーレスのツールです。

クエリサービスを使用してデータ分析のエコシステムを構築し、様々なインタラクションチャネルにわたる顧客の画像を作成できます。 これらのチャネルには、POS（販売時点管理システム）、Web、モバイル、CRMなどが含まれます。

**新機能**

| 機能 | 説明 |
| -----------| ---------- |
| クエリエディターの機能強化 | クエリを保存して後で操作できる保存関数が追加されました。 Adobe Experience Platformのクエリサービスユーザーインターフェイスに「参照」タブが追加され、組織内のユーザーが保存したクエリを表示できます。 表示中のクエリに関する有用なメタデータを表示する「クエリの詳細」パネルを実装しました。 |
| 新しいアトリビューション関数 | 有効期限パラメーターを使用したチャネル属性のための、クエリサービスからクエリへのアドビ定義の関数。 |
| SQL構文の改良 | iLike構文のサポート。 |
| 定義済みのXDMスキーマを使用したデータセットの生成 | ターゲットスキーマを指定できる新しい句を、選択としてテーブルを作成(CTAS)クエリに追加しました。 |

For more information, refer to the [Query Service documentation](../../query-service/home.md).