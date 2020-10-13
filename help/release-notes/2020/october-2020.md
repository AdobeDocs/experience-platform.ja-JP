---
title: Adobe Experience Platform リリースノート
description: Experience Platformリリースノート（2020年10月）
doc-type: release notes
last-update: October, 2020
author: crhoades, ens28527
translation-type: tm+mt
source-git-commit: ab87cac94ae69acde3be75ae95b11cf003a274e9
workflow-type: tm+mt
source-wordcount: '281'
ht-degree: 37%

---


# Adobe Experience Platform リリースノート

**リリース日：2020年10月**

- [データ準備](#data-prep)
- [ソース](#sources)

## データ準備 {#data-prep}

データ準備を使用すると、データエンジニアはエクスペリエンスデータモデル(XDM)との間でデータのマッピング、変換、検証を行うことができます。

**主な特長**

| 機能 | 説明 |
| ------- | ----------- |
| `is_set` 関数 | この `is_set` 関数を使用すると、ソースデータ内に属性が存在するかどうかを確認できます。 `is_set` と組み合わせて使用して、属性の存在と属性内の値の存在の両方 `is_empty` を確認できます。 |
| `get_values` 関数 | この `get_values` 関数を使用すると、任意のキーの入力マップから値を取得できます。 |

For more information, please read the [Data Prep overview](../../data-prep/home.md).

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