---
title: Adobe Experience Platform リリースノート
description: Experience Platformリリースノート（2020年8月11日）
doc-type: release notes
last-update: August 10, 2020
author: crhoades, ens28527
translation-type: tm+mt
source-git-commit: 89531ad458bd41720090ef2c429376af4460d7c0
workflow-type: tm+mt
source-wordcount: '296'
ht-degree: 33%

---


# Adobe Experience Platform リリースノート

**リリース日：2020 年 12 月 8 日**

Experience Platformに関する最新のリリースノート

- [[!DNL Data Science Workspace]](#dsw)
- [[!DNLソース]](#sources)

## [!DNL Data Science Workspace] {#dsw}

[!DNL Data Science Workspace] 機械学習と人工知能を使用して、データから洞察を引き出します。 Integrated into Adobe Experience Platform, [!DNL Data Science Workspace] helps you make predictions using your content and data assets across Adobe solutions.

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| VMの強化 [!DNL JupyterLab] | 長時間稼働する [!DNL JupyterLab notebook] 仮想マシンの安定性が向上しました。 |

詳細については、『 [!DNL JupyterLab]ユーザガイド [[!DNL JupyterLab] ](../../data-science-workspace/jupyterlab/overview.md)』を参照してください。

## ソース {#sources}

Adobe Experience Platform can ingest data from external sources while allowing you to structure, label, and enhance that data using [!DNL Platform] services. アドビアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM など、様々なソースからデータを取得することができます。

[!DNL Experience Platform] は、様々なデータプロバイダーのソース接続を簡単に設定できるようにする RESTful API とインタラクティブな UI を提供します。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理をおこなうことができます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| フロー実行の監視 | すべてのフローの実行を監視し、完了ステータス、実行期間、処理されたファイルのリスト、エラー、指標など、各実行の詳細表示を確認できます。 詳細は、 [監視データフロー](../../sources/tutorials/ui/monitor.md) ドキュメントを参照してください。 |
| アカウントの更新 | ユーザーは、既存のアカウントの資格情報、名前および説明を更新して、より意味のある情報を提供し、作成された可能性のあるエラーを修正できます。 |
| フロー実行通知 | イベントを登録し、Webフックを登録して、フローの実行に関するステータス、指標、エラーに関するリアルタイム通知を受信できます。 |
| UIカタログの改善 | ソースカタログ画面が更新され、選択したオブジェクトの主なアクションに簡単にアクセスできるようになりました。 |

ソースについて詳しくは、[ソースの概要](../../sources/home.md)を参照してください。
