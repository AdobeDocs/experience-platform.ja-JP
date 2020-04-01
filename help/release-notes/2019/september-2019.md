---
title: Adobe Experience Platform リリースノート
description: Experience Platformリリースノート（2019年9月11日）
doc-type: release notes
last-update: September 13, 2019
author: ens28527
translation-type: tm+mt
source-git-commit: 28a8fc496c85b334e89d0f0a130d3cc5c8956399

---


# Adobe Experience Platform リリースノート

## リリース日：2019 年 9 月 10 日

## データ収集

Adobe Experience Platformは、あらゆる種類のデータや遅延を取り込むための豊富な機能セットを提供します。 Adobe Experience Platform Data Ingestは、Batch API、Streaming API、ネイティブのAdobeコネクター、データ統合パートナー、Adobe Experience Platform UIなど、データを取り込むための複数の代替手段を提供します。

**新機能**

| 機能 | 説明 |
| ----------- | ---------- |
| ストリーミング取り込み用の新しいドメイン | ドメイン `dcs.data.adobe.net` は、新しい共通のデータ収集ドメインに移動されまし `dcs.adobedc.net`た。 ユーザーは、改訂されたAdobe Experience Platform Streaming Ingestionドキュメントに従って、実装を更新する必要があります。 Adobe Experience Platformストリーミングの取り込みに関連するすべてのドキュメントが、新しいドメインを使用するように更新されました。 |

詳しくは、データ収集のドキュメントを [参照してください](../../ingestion/home.md)。

## Data Science Workspace

Adobe Experience Platform Data Science Workspaceは、Experience Platform内の完全に管理されたサービスで、Adobeソリューションやサードパーティ製システムのデータやコンテンツから、機械学習モデルの構築と運用によって、データ科学者がシームレスに洞察を生み出すことができます。 Data Science Workspaceは、プラットフォームと緊密に統合され、XDMデータの調査と準備を含む、エンドツーエンドのデータサイエンスのライフサイクルを強化します。その後、モデルの開発と運用を行い、リアルタイムの顧客プロファイルを機械学習インサイトに自動的に強化します。

**新機能**

| 機能 | 説明 |
| -----------| ---------- |
| UIを介したサービスのスケジュール | Platform Orchestration Serviceと統合され、UIを使用して、モデルのトレーニングとスコアリングをユーザー定義のスケジュールで自動化します。 |
| サービスギャラリー | 機械学習サービスを参照、監視、アクセスする機能を備え、再設計されたサービスギャラリー内で、自動トレーニングおよびスコアリングジョブのスケジュールを設定できます。 |
| JupyterLab 5.0.0 | JupyterLabのUIの改善。 |

**既知の問題**

* 現在、サービスギャラリーで既存のサービスを削除するアクセス方法はありません。 それまでの間、API呼び出しを使用して既存のサ [ービスを削除するには](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/sensei-ml-api.yaml) 、Senesi Machine Learning APIリファレンスを参照してください。
* サービスギャラリーには、サービスのトレーニングとスコアの実行をフィルターするページネーションのサポートはありません。
* サービスギャラリーでスケジュールされたトレーニングまたはスコアリングの実行を設定する場合、頻度を1時間に設定すると、スケジュールが適用されなくなります。

詳しくは、 [Data Science Workspaceの概要を参照してください](../../data-science-workspace/home.md)。

## クエリサービス

クエリサービスは、Adobe Experience Platformの標準のSQLからクエリへのデータを使用して、様々な分析およびデータ管理の使用例をサポートする機能を提供します。 Data Lakeのデータセットを結合し、クエリ結果を新しいデータセットとして取り込み、レポート、Data Science Workspace、またはリアルタイム顧客プロファイルに取り込むためのサーバーレスツールです。

クエリサービスを使用して、データ分析エコシステムを構築し、様々なインタラクションチャネルの顧客の画像を作成できます。 これらのチャネルには、販売時点管理システム、Web、モバイル、CRMなどが含まれます。

**新機能**

| 機能 | 説明 |
| -----------| ---------- |
| クエリエディターの改善 | 保存関数が追加され、保存して後で作業できるクエリが追加されました。 組織内のユーザーが保存したクエリを表示する「参照」タブを、Adobe Experience Platformのクエリサービスユーザーインターフェイスに追加しました。 表示中のクエリに関する有用なメタデータを表示する「クエリの詳細」パネルを実装。 |
| 新しいアトリビューション関数 | 有効期限パラメーターを使用したクエリアトリビューションのクエリに対するチャネルサービスのアドビ定義関数。 |
| SQL構文の強化 | iLike構文のサポート。 |
| 定義済みのXDMデータセットを生成するスキーマ | 選択としてテーブルを作成(CTAS)クエリに、選択オプションを指定できる新しい句を追加しました。 |

For more information, refer to the [Query Service documentation](../../query-service/home.md).