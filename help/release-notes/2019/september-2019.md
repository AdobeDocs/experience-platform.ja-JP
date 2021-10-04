---
title: Adobe Experience Platform リリースノート
description: Experience Platform リリースノート 2019 年 9 月 10 日
doc-type: release notes
last-update: September 13, 2019
author: ens28527
exl-id: 7f503046-a3b4-4fdb-833c-4205b6e9fa04
source-git-commit: 0085306a2f5172eb19590cc12bc9645278bd2b42
workflow-type: tm+mt
source-wordcount: '532'
ht-degree: 46%

---

# Adobe Experience Platform リリースノート

**リリース日：2019年9月10日（PT）**

Adobe Experience Platform の既存の機能のアップデート：

* [[!DNL Data Ingestion]](#ingestion)
* [[!DNL Data Science Workspace]](#dsw)
* [[!DNL Query Service]](#query)

## [!DNL Data Ingestion] {#ingestion}

Adobe Experience Platform には、あらゆる種類のデータやデータのレイテンシを取り込むための豊富な機能が用意されています。Adobe Experience Platform [!DNL Data Ingestion] には、バッチ API、ストリーミング API、ネイティブAdobeコネクタ、データ統合パートナー、Adobe Experience Platform UI など、データを取り込むための複数の代替手段が用意されています。

**新機能**

| 機能 | 説明 |
| ----------- | ---------- |
| ストリーミング取得用の新しいドメイン | `dcs.data.adobe.net` ドメインは、新しい共通データ収集ドメイン `dcs.adobedc.net` に移動しました。ユーザーは、Adobe Experience Platform の改訂されたストリーミング取得ドキュメントに従って、実装を更新する必要があります。Adobe Experience Platform のストリーミング取得に関連するすべてのドキュメントは、新しいドメインを使用するように更新されています。 |

詳しくは、[データ取得ドキュメント](../../ingestion/home.md)を参照してください。

## [!DNL Data Science Workspace] {#dsw}

Adobe Experience Platform [!DNL Data Science Workspace] は [!DNL Experience Platform] 内の完全に管理されたサービスで、機械学習モデルを構築し運用することで、Adobeソリューションやサードパーティシステムをまたいで、データやコンテンツから洞察をシームレスに生み出すことができます。 [!DNL Data Science Workspace] はと緊密に統合さ [!DNL Platform] れ、XDM データの調査と準備、モデルの開発と運用など、エンドツーエンドのデータサイエンスのライフサイクルを強化し、機械学習インサイトを自動的に強化しま [!DNL Real-time Customer Profile] す。

**新機能**

| 機能 | 説明 |
| -----------| ---------- |
| UI を介したサービスのスケジュール | [!DNL Platform] オーケストレーションサービスと統合され、UI を使用して、ユーザー定義のスケジュールでモデルのトレーニングとスコアリングを自動化します。 |
| [!DNL Service Gallery] | 再設計された [!DNL Service Gallery] 内で、機械学習サービスを参照、監視、アクセスし、自動トレーニングジョブとスコアリングジョブのスケジュールを設定できます。 |
| [!DNL JupyterLab] 5.0.0 | [!DNL JupyterLab] UI の改善。 |

**既知の問題**

* 現在、[!DNL Service Gallery] で既存のサービスを削除するアクセス方法はありません。 当面は、[Senesi の機械学習 API リファレンスを](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/sensei-ml-api.yaml)参照して、API 呼び出しを通じて既存のサービスを削除してください。
* [!DNL Service Gallery] には、サービスのトレーニングとスコアリングの実行をフィルタリングするためのページネーションのサポートはありません。
* [!DNL Service Gallery] を介してスケジュールされたトレーニングまたはスコアリングの実行を設定する場合、頻度を 1 時間に設定すると、スケジュールが適用されなくなります。

詳しくは、「[Data Science Workspace の概要](../../data-science-workspace/home.md)」を参照してください。

## [!DNL Query Service] {#query}

[!DNL Query Service] は、標準の SQL を使用してAdobe Experience Platformでデータをクエリし、様々な分析およびデータ管理の使用例をサポートする機能を提供します。[!DNL Data Lake] のデータセットを結合し、クエリ結果を新しいデータセットとして取り込んでレポートや [!DNL Data Science Workspace] で使用したり、[!DNL Real-time Customer Profile] に取り込んだりできる、サーバーレスのツールです。

[!DNL Query Service] を使用してデータ分析エコシステムを構築し、様々なインタラクションチャネルをまたいだ顧客の全体像を把握できます。 これらのチャネルには、POS（販売時点管理システム）、web、モバイル、CRM システムなどが含まれます。

**新機能**

| 機能 | 説明 |
| -----------| ---------- |
| [!DNL Query Editor] の改善 | 保存関数が追加され、クエリを保存して後で使用できるようになりました。Adobe Experience Platformの [!DNL Query Service] ユーザーインターフェイスに「参照」タブが追加され、組織内のユーザーが保存したクエリを表示できるようになりました。 表示中のクエリに関する有用なメタデータを表示する「クエリの詳細」パネルが実装されました。 |
| 新しい属性関数 | [!DNL Query Service] のAdobe定義関数。有効期限パラメーターを持つチャネル属性を問い合わせます。 |
| SQL 構文の強化 | iLike 構文のサポート。 |
| 定義済みの XDM スキーマを使用してデータセットを生成 | Create Table as Select（CTAS）クエリの新しい句が追加され、ターゲットスキーマを指定できるようになりました。 |

詳しくは、[クエリサービスのドキュメント](../../query-service/home.md)を参照してください。
