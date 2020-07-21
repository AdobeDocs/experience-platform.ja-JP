---
title: Adobe Experience Platform リリースノート
description: Experience Platformリリースノート（2019年9月11日）
doc-type: release notes
last-update: September 13, 2019
author: ens28527
translation-type: tm+mt
source-git-commit: bfbf2074a9dcadd809de043d62f7d2ddaa7c7b31
workflow-type: tm+mt
source-wordcount: '532'
ht-degree: 5%

---


# Adobe Experience Platform リリースノート

**リリース日：2019 年 9 月 10 日**

Adobe Experience Platform内の既存の機能の更新：

* [!DNL Data Ingestion](#ingestion)
* [!DNL Data Science Workspace](#dsw)
* [!DNL Query Service](#query)

## [!DNL Data Ingestion] {#ingestion}

Adobe Experience Platformは、あらゆる種類のデータや遅延を取り込むための豊富な機能セットを提供します。 Adobe Experience Platform [!DNL Data Ingestion] は、Batch API、Streaming API、ネイティブのAdobe Connectors、Data Integrationパートナー、Adobe Experience PlatformUIなど、データを取り込むための複数の代替手段を提供します。

**新機能**

| 機能 | 説明 |
| ----------- | ---------- |
| ストリーミング取り込み用の新しいドメイン | ドメインは、新しい共通データ収集ドメインに移動され `dcs.data.adobe.net``dcs.adobedc.net`ました。 Adobe Experience Platformストリーミングの取り込みに関するドキュメントに従って、ユーザーは実装を更新する必要があります。 Adobe Experience Platformストリーミングの取り込みに関連するすべてのドキュメントが、新しいドメインを使用するように更新されました。 |

詳しくは、 [データ収集ドキュメントを参照してください](../../ingestion/home.md)。

## [!DNL Data Science Workspace] {#dsw}

Adobe Experience Platform [!DNL Data Science Workspace][!DNL Experience Platform] は、機械学習モデルの構築と運用により、データ科学者がアドビのソリューションやサードパーティ製システムのデータやコンテンツからの洞察をシームレスに生み出せるようにする、内部で完全に管理されたサービスです。 [!DNL Data Science Workspace] は、XDMデータの調査と準備を含む、エンドツーエンドのデータ科学ライフサイクルと緊密に統合され、さらにモデルの開発と運用を強化して、機械学習インサイトを自動的に強化 [!DNL Platform][!DNL Real-time Customer Profile] します。

**新機能**

| 機能 | 説明 |
| -----------| ---------- |
| UIを介したサービスのスケジュール | Orchestration Serviceと統合され、UIを使用してユーザー定義のスケジュールによるモデルのトレーニングとスコアリングを自動化します。 [!DNL Platform] |
| [!DNL Service Gallery] | 機械学習サービスを参照、監視、アクセスし、自動トレーニングとスコアリングのジョブをスケジュールできます。これらはすべて再設計された環境で行い [!DNL Service Gallery]ます。 |
| [!DNL JupyterLab] 5.0.0 | [!DNL JupyterLab] UIの改善。 |

**既知の問題**

* 現在、既存のサービスを削除するアクセス方法 [!DNL Service Gallery] はありません。 それまでの間、API呼び出しを使用して既存のサービスを削除するには、 [Senesi Machine Learning APIリファレンスを参照してください](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/sensei-ml-api.yaml) 。
* サービスのトレーニングとスコアリングの実行をフィルターするためのページネーションはサポートされていません。 [!DNL Service Gallery]
* スケジュール済みのトレーニングまたはスコアリングの実行を設定する場合 [!DNL Service Gallery]、頻度を時間別に設定すると、スケジュールが適用されなくなります。

詳しくは、 [Data Science Workspaceの概要を参照してください](../../data-science-workspace/home.md)。

## [!DNL Query Service] {#query}

[!DNL Query Service] 標準のSQLからクエリデータへのAdobe Experience Platformを使用して、様々な分析やデータ管理の使用例をサポートする機能を提供します。 It is a serverless tool that allows you to join datasets from the [!DNL Data Lake] and capture the query results as a new dataset for use in reporting, [!DNL Data Science Workspace], or for ingestion into [!DNL Real-time Customer Profile].

You can use [!DNL Query Service] to build data analysis ecosystems, creating a picture of customers across their various interaction channels. これらのチャネルには、POS（販売時点管理システム）、Web、モバイル、CRMなどが含まれます。

**新機能**

| 機能 | 説明 |
| -----------| ---------- |
| 改善点 [!DNL Query Editor] | クエリを保存して後で操作できる保存関数が追加されました。 Adobe Experience Platform上のユーザーインターフェイスに「参照」タブが追加され、組織内のユーザーが保存したクエリを表示できるようになりました。 [!DNL Query Service] 表示中のクエリに関する有用なメタデータを表示する「クエリの詳細」パネルを実装しました。 |
| 新しいアトリビューション関数 | 有効期限パラメーターを使用したチャネル属性 [!DNL Query Service] のための、クエリへのアドビ定義関数。 |
| SQL構文の改良 | iLike構文のサポート。 |
| 定義済みのXDMスキーマを使用したデータセットの生成 | ターゲットスキーマを指定できる新しい句を、選択としてテーブルを作成(CTAS)クエリに追加しました。 |

For more information, refer to the [Query Service documentation](../../query-service/home.md).