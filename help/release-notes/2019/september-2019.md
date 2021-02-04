---
title: Adobe Experience Platform リリースノート
description: Experience Platform リリースノート 2019 年 9 月 10 日
doc-type: release notes
last-update: September 13, 2019
author: ens28527
translation-type: tm+mt
source-git-commit: 1b398e479137a12bcfc3208d37472aae3d6721e1
workflow-type: tm+mt
source-wordcount: '532'
ht-degree: 46%

---


# Adobe Experience Platform リリースノート

**リリース日：2019 年 10 月 10 日**

Adobe Experience Platform の既存の機能のアップデート：

* [[!DNL Data Ingestion]](#ingestion)
* [[!DNL Data Science Workspace]](#dsw)
* [[!DNL Query Service]](#query)

## [!DNL Data Ingestion] {#ingestion}

Adobe Experience Platform には、あらゆる種類のデータやデータのレイテンシを取り込むための豊富な機能が用意されています。Adobe Experience Platform[!DNL Data Ingestion]は、Batch API、Streaming API、ネイティブAdobeコネクタ、Data Integrationパートナー、Adobe Experience PlatformUIなど、データを取り込むための複数の代替手段を提供します。

**新機能**

| 機能 | 説明 |
| ----------- | ---------- |
| ストリーミング取得用の新しいドメイン | `dcs.data.adobe.net` ドメインは、新しい共通データ収集ドメイン `dcs.adobedc.net` に移動しました。ユーザーは、Adobe Experience Platform の改訂されたストリーミング取得ドキュメントに従って、実装を更新する必要があります。Adobe Experience Platform のストリーミング取得に関連するすべてのドキュメントは、新しいドメインを使用するように更新されています。 |

詳しくは、[データ取得ドキュメント](../../ingestion/home.md)を参照してください。

## [!DNL Data Science Workspace] {#dsw}

Adobe Experience Platform[!DNL Data Science Workspace]は[!DNL Experience Platform]内で完全に管理されたサービスで、データ科学者は機械学習モデルの構築と運用を通じて、Adobeソリューションやサードパーティ製システム全体のデータやコンテンツからの洞察をシームレスに生み出すことができます。 [!DNL Data Science Workspace] は、XDMデータの調査と準備、モデルの開発と運用を含む、エンドツーエンドのデータ科学ライフサイクルと緊密に統合され、エンドツーエンドのデータ科学ライフサイクル [!DNL Platform]  [!DNL Real-time Customer Profile] を強化しています。

**新機能**

| 機能 | 説明 |
| -----------| ---------- |
| UI を介したサービスのスケジュール | [!DNL Platform] Orchestration Serviceと統合され、UIを使用してユーザー定義のスケジュールを使用して、モデルのトレーニングとスコアリングを自動化します。 |
| [!DNL Service Gallery] | 機械学習サービスを参照、監視、アクセスし、自動トレーニングとスコアリングジョブをスケジュールできます。これらはすべて再設計された[!DNL Service Gallery]内にあります。 |
| [!DNL JupyterLab] 5.0.0 | [!DNL JupyterLab] UIの改善。 |

**既知の問題**

* 現在、[!DNL Service Gallery]では、既存のサービスを削除するアクセス方法はありません。 当面は、[Senesi の機械学習 API リファレンスを](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/sensei-ml-api.yaml)参照して、API 呼び出しを通じて既存のサービスを削除してください。
* [!DNL Service Gallery]には、サービスのトレーニングとスコアの実行をフィルターするためのページネーションのサポートはありません。
* [!DNL Service Gallery]を通じてスケジュールされたトレーニングまたはスコアリングを実行する場合、頻度を1時間ごとに設定すると、スケジュールが適用されなくなります。

詳しくは、「[Data Science Workspace の概要](../../data-science-workspace/home.md)」を参照してください。

## [!DNL Query Service] {#query}

[!DNL Query Service] は、Adobe Experience Platformの標準的なSQLからクエリデータへの使用を可能にし、様々な分析およびデータ管理の使用例をサポートします。これは、[!DNL Data Lake]のデータセットを結合し、クエリ結果を新しいデータセットとして取り込み、レポート、[!DNL Data Science Workspace]で使用したり、[!DNL Real-time Customer Profile]に取り込んだりするためのサーバレスツールです。

[!DNL Query Service]を使ってデータ分析のエコシステムを構築し、様々なインタラクションチャネルにわたる顧客の画像を作成できます。 これらのチャネルには、POS（販売時点管理システム）、web、モバイル、CRM システムなどが含まれます。

**新機能**

| 機能 | 説明 |
| -----------| ---------- |
| [!DNL Query Editor]の改善点 | 保存関数が追加され、クエリを保存して後で使用できるようになりました。Adobe Experience Platformの[!DNL Query Service]ユーザーインターフェイスに「参照」タブが追加され、組織内のユーザーが保存したクエリを表示できるようになりました。 表示中のクエリに関する有用なメタデータを表示する「クエリの詳細」パネルが実装されました。 |
| 新しい属性関数 | 有効期限パラメーターを使用したチャネル属性のクエリに対する[!DNL Query Service]内のAdobe定義関数。 |
| SQL 構文の強化 | iLike 構文のサポート。 |
| 定義済みの XDM スキーマを使用してデータセットを生成 | Create Table as Select（CTAS）クエリの新しい句が追加され、ターゲットスキーマを指定できるようになりました。 |

詳しくは、[クエリサービスのドキュメント](../../query-service/home.md)を参照してください。