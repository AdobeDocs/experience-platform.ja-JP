---
title: Adobe Experience Platform リリースノート
description: Experience Platformリリースノート（2020年10月）
doc-type: release notes
last-update: October, 2020
author: crhoades, ens28527
translation-type: tm+mt
source-git-commit: fc1174c8435c1afc3c58dd748daf89f387a19980
workflow-type: tm+mt
source-wordcount: '591'
ht-degree: 28%

---


# Adobe Experience Platform リリースノート

**リリース日：2020 年 10 月 14 日**

- [データ準備](#data-prep)
- [リアルタイム顧客プロファイル](#profile)
- [セグメント化サービス](#segmentation)
- [ソース](#sources)

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