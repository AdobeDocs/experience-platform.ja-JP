---
title: Adobe Experience Platform リリースノート 2019年9月
description: Adobe Experience Platform の 2019年9月のリリースノート。
doc-type: release notes
last-update: September 13, 2019
author: ens28527
exl-id: 7f503046-a3b4-4fdb-833c-4205b6e9fa04
source-git-commit: b48c24ac032cbf785a26a86b50a669d7fcae5d97
workflow-type: tm+mt
source-wordcount: '531'
ht-degree: 45%

---

# Adobe Experience Platform リリースノート

**リリース日：2019年9月10日（PT）**

Adobe Experience Platform の既存の機能に対するアップデート：

* [[!DNL Data Ingestion]](#ingestion)
* [[!DNL Data Science Workspace]](#dsw)
* [[!DNL Query Service]](#query)

## [!DNL Data Ingestion] {#ingestion}

Adobe Experience Platform には、あらゆる種類のデータやデータのレイテンシを取り込むための豊富な機能が用意されています。Adobe Experience Platform [!DNL Data Ingestion] は、データ取り込みに複数の代替手段を提供します。これには、バッチ API、ストリーミング API、ネイティブのAdobe コネクタ、データ統合パートナー、Adobe Experience Platform UI などがあります。

**新機能**

| 機能 | 説明 |
| ----------- | ---------- |
| ストリーミング取得用の新しいドメイン | `dcs.data.adobe.net` ドメインは、新しい共通データ収集ドメイン `dcs.adobedc.net` に移動しました。ユーザーは、Adobe Experience Platform の改訂されたストリーミング取得ドキュメントに従って、実装を更新する必要があります。Adobe Experience Platform のストリーミング取得に関連するすべてのドキュメントは、新しいドメインを使用するように更新されています。 |

詳しくは、[データ取得ドキュメント](../../ingestion/home.md)を参照してください。

## [!DNL Data Science Workspace] {#dsw}

Adobe Experience Platform [!DNL Data Science Workspace] は、機械学習モデルを構築して運用することで、データサイエンティストは、Adobe ソリューションおよびサードパーティシステムをまたいでデータやコンテンツから洞察をシームレスに生み出すことができる、[!DNL Experience Platform] 内の完全に管理されたサービスです。 [!DNL Data Science Workspace] は [!DNL Experience Platform] と緊密に統合されており、XDM データの調査と準備、その後、機械学習のインサイトを使用してデータを自動的に強化するモデルの開発と運用など、エンドツーエンドの [!DNL Real-Time Customer Profile] ータサイエンスライフサイクルを強化します。

**新機能**

| 機能 | 説明 |
| -----------| ---------- |
| UI を介したサービスのスケジュール | [!DNL Experience Platform] Orchestration サービスと統合して、UI を使用するユーザー定義のスケジュールでモデルのトレーニングとスコアリングを自動化します。 |
| [!DNL Service Gallery] | 再設計された [!DNL Service Gallery] ークフロー内で、自動トレーニングおよびスコアリングジョブをスケジュール設定でき、機械学習サービスの参照、監視、アクセスができます。 |
| [!DNL JupyterLab] 5.0.0 | [!DNL JupyterLab] UI の改善。 |

**既知の問題**

* 現在、[!DNL Service Gallery] には、既存のサービスを削除するためのアクセス可能な方法はありません。 当面は、[Senesi の機械学習 API リファレンスを](https://developer.adobe.com/experience-platform-apis/references/sensei-machine-learning/)参照して、API 呼び出しを通じて既存のサービスを削除してください。
* [!DNL Service Gallery] には、サービスのトレーニング実行とスコアリング実行をフィルタリングするページネーションのサポートがありません。
* スケジュールされたトレーニングまたはスコアリングが [!DNL Service Gallery] を通じて実行されるように設定する場合、頻度を 1 時間ごとに設定すると、スケジュールが適用されなくなります。

詳しくは、「[Data Science Workspace の概要](../../data-science-workspace/home.md)」を参照してください。

## [!DNL Query Service] {#query}

[!DNL Query Service] では、標準の SQL を使用してAdobe Experience Platformのデータに対してクエリを実行し、分析およびデータ管理の様々なユースケースに対応できます。 これは、[!DNL Data Lake] のデータセットを結合し、クエリ結果を新しいデータセットとして取得してレポートや [!DNL Data Science Workspace] で使用したり、[!DNL Real-Time Customer Profile] に取り込んだりできる、サーバーレスのツールです。

[!DNL Query Service] を使用してデータ分析のエコシステムを構築し、様々なインタラクションチャネルをまたいだ顧客の全体像を把握できます。 これらのチャネルには、POS（販売時点管理システム）、web、モバイル、CRM システムなどが含まれます。

**新機能**

| 機能 | 説明 |
| -----------| ---------- |
| [!DNL Query Editor] の改善点 | 保存関数が追加され、クエリを保存して後で使用できるようになりました。Adobe Experience Platformの [!DNL Query Service] ユーザーインターフェイスに、組織内のユーザーによって保存されたクエリを表示する「参照」タブが追加されました。 表示中のクエリに関する有用なメタデータを表示する「クエリの詳細」パネルが実装されました。 |
| 新しい属性関数 | 有効期限パラメーターを含むチャネルアトリビューションをクエリするた [!DNL Query Service] の、Adobe定義関数。 |
| SQL 構文の強化 | iLike 構文のサポート。 |
| 定義済みの XDM スキーマを使用してデータセットを生成 | Create Table as Select（CTAS）クエリの新しい句が追加され、ターゲットスキーマを指定できるようになりました。 |

詳しくは、[クエリサービスのドキュメント](../../query-service/home.md)を参照してください。
