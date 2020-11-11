---
title: Adobe Experience Platform リリースノート
description: Experience Platformリリースノート（2020年10月）
doc-type: release notes
last-update: October, 2020
author: crhoades, ens28527
translation-type: tm+mt
source-git-commit: e2b0048703816dc481eb9486310d86a8f2483af2
workflow-type: tm+mt
source-wordcount: '1028'
ht-degree: 16%

---


# Adobe Experience Platform リリースノート

**リリース日：2020 年 10 月 14 日**

- [データ準備](#data-prep)
- [リアルタイム顧客プロファイル](#profile)
- [セグメント化サービス](#segmentation)
- [ソース](#sources)
- [Time to Value](#time-to-value)

## データ準備 {#data-prep}

データ準備を使用すると、データエンジニアはエクスペリエンスデータモデル(XDM)との間でデータのマッピング、変換、検証を行うことができます。

**主な特長**

| 機能 | 説明 |
| ------- | ----------- |
| `is_set` 関数 | この `is_set` 関数を使用すると、ソースデータ内に属性が存在するかどうかを確認できます。 `is_set` と組み合わせて使用して、属性の存在と属性内の値の存在の両方 `is_empty` を確認できます。 |
| `get_values` 関数 | この `get_values` 関数を使用すると、任意のキーの入力マップから値を取得できます。 |

For more information, please read the [Data Prep overview](../../data-prep/home.md).

## リアルタイム顧客プロファイル {#profile}

Adobe Experience Platform を使用すると、いつでもどこでもブランドとのやり取りが顧客に対して調整され、一貫性と関連性のあるエクスペリエンスを提供できます。With [!DNL Real-time Customer Profile], you can see a holistic view of each individual customer that combines data from multiple channels, including online, offline, CRM, and third party data. [!DNL Profile] 個別の顧客データを統合表示に統合し、各顧客の操作に関する実用的でタイムスタンプのあるアカウントを提供できます。

| 機能 | 説明 |
| ------- | ----------- |
| プロファイルプレビューAPIの追加 | プロファイルプレビューAPI(`/previewsamplestatus`)には、IMS組織全体のプロファイルフラグメントの合計の内訳を表示する機能と、ID名前空間全体のプロファイルフラグメントの表示機能が含まれるようになりました。 |
| 和集合スキーマ表示の更新 | Experience PlatformUIでは、和集合スキーマに貢献するすべてのスキーマとデータセットに関する情報、およびIDや関係フィールドなどの表面キー属性を、より簡単に見つけることができます。 これらの更新により、トラブルシューティングおよび検証機能が向上し、プロファイルが正しく設定され、IDが正しく関連付けられ、データが正常に取り込まれたことを確認できます。 |

For more information on [!DNL Real-time Customer Profile], including tutorials and best practices for working with [!DNL Profile] data, please read the [Real-time Customer Profile overview](../../profile/home.md).

## セグメント化サービス {#segmentation}

Adobe Experience Platform Segmentation Service provides a user interface and RESTful API that allows you to build segments and generate audiences from your [!DNL Real-time Customer Profile] data. These segments are centrally configured and maintained on [!DNL Platform], making them readily accessible by any Adobe application.

[!DNL Segmentation Service] 顧客ベース内のマーケティング可能な人々のグループを区別する基準を説明することで、特定のプロファイルのサブセットを定義します。 セグメントは、レコードデータ（人口統計情報など）や、顧客によるブランドとのやり取りを表す時系列イベントに基づいて作成できます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| ストリーミングセグメントの制限の削除 | ルックバック期間の7日間の制限は解除されました。 |

For more information on [!DNL Segmentation Service], please see the [Segmentation overview](../../segmentation/home.md)

## ソース {#sources}

Adobe Experience Platform can ingest data from external sources while allowing you to structure, label, and enhance that data using [!DNL Platform] services. アドビアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM など、様々なソースからデータを取得することができます。

[!DNL Experience Platform] は、様々なデータプロバイダーのソース接続を簡単に設定できるようにする RESTful API とインタラクティブな UI を提供します。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理をおこなうことができます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| 階層マッピング | データ取り込みプロセス中に、JSONやParketなどの階層的なソースファイルをプレビューできます。 |
| SFTP用SSH認証のサポート | RSA/DSA Open SSHキーを [!DNL Platform] 使用して、SFTPアカウントをに接続できます。 See the [SFTP overview](../../sources/connectors/cloud-storage/ftp-sftp.md) for more information. |
| UXの強化 | データセットは、データ取り込みプロセス [!DNL Profile] 中に有効にすることができます。 詳しくは、「 [クラウドストレージのdataflowワークフローのチュートリアル](../../sources/tutorials/ui/dataflow/batch/cloud-storage.md) 」を参照してください。 |

ソースについて詳しくは、[ソースの概要](../../sources/home.md)を参照してください。

## Time to Value {#time-to-value}

Adobe Experience Platformは、マーケティング・オペレーションズ・チームが、広範なデータ・エンジニアリングの専門知識を必要とせずに、顧客の360度の表示を完全に構築できるようにします。 目標は、データ速度を通じてチームや価値を加速させることです。

「タイム・トゥ・バリュー」は、個人を横断的に削減します。 データエンジニアは、データアクティビティの透明性を高め、効率的かつ迅速にタスクを完了できるので、堅牢で拡張性の高いリアルタイムの顧客プロファイルを迅速に利用できます。 その後、マーケターは堅牢な完全な顧客プロファイルを使用して、セグメント化とアクティベーションを行うことができます。

### 機能のハイライト

#### スキーマ

ユーザビリティとワークフローがアップグレードされ、スキーマ構成内の主要フィールドの標準化および透明性をすぐに把握できるインサイトを提供します。 「和集合スキーマ」として表される個々のデータモデルの組み合わせに対してデータ系列を公開し、構造と材料に関するインサイトをリアルタイムの顧客プロファイルに提供します。

- スキーマワークフローのアップグレード
   - 最も一般的なタイプのXDMスキーマに対してショートカットを使用し、スキーマエディターの自動設定と目的に基づくミックスインレコメンデーションを行います。
   - 複数のミックスイン選択機能とプレビュー機能により、ワークフローの効率を向上
   - ID、関係、必須フィールド、非推奨フィールドなど、スキーマ構成の主要な属性を透明にする
- 和集合スキーマのデータ系列と主要属性の透明度

#### データの取り込みと収集

自動マッピング、マッピングプレビュー、操作性のアップグレードにより、プロファイル、ダウンストリームセグメント化、アクティベーションで使用するプラットフォームやソースのデータが取り込まれます。 このシステムは、ITの外部の人々でも、このプロセスを使いやすくする効率と知性を備えています。

- カタログページカードとデータテーブルのインラインアクションパターンのアップグレードにより、データソースへの容易なアクセスが可能
- データ取り込み用に計算されたフィールド/式
- データマッピングの推奨事項により、取り込みプロセスが迅速になります。
- プレビューと検証のマッピング

#### プロファイル設定

マーケティング担当者向けのカスタマイズ機能を備えたプロファイルビューアを使用すると、セグメント化、計画およびアクティベーションの場合に使用するプロファイルの構成を理解できます。 統合ワークフローは、統合ポリシーに対して段階的なワークフローを提供することにより、制御された効率的な方法でプロファイルを統合します。

- 個々のプロファイルを拡張プロファイルビューアに表示します。このビューアは、完全なカスタマイズを含むダッシュボードを表示し、マーケティング担当者のビジネス目標に基づいてチャネル間のデータをグループ化して表示します。
- ビジネスのニーズに応じて、基本情報ウィジェットで標準の属性とカスタマイズした属性を編集します。
- 和集合スキーマセレクターを使用して、リアルタイム顧客プロファイルの属性でウィジェットをカスタマイズします。 和集合スキーマは、プロファイルデータの取り込みで使用される基礎となるデータモデルから派生します。


#### 監視

データフローの透明性を確保し、ソースコネクタからシステムへのデータトラフィックの正常性に関する洞察を提供し、トラブルシューティングのためのセルフサービスの向上と迅速な実行性を提供します。

- すべてのフローの実行を監視し、完了ステータス、実行期間、処理されたファイルのリスト、エラー、実行可能な診断など、各実行の詳細表示を確認します