---
title: Adobe Experience Platform リリースノート 2019年9月
description: Adobe Experience Platformの 2019 年 9 月のリリースノート。
doc-type: release notes
last-update: September 13, 2019
author: ens28527
exl-id: 7f503046-a3b4-4fdb-833c-4205b6e9fa04
source-git-commit: ce967ae176fce81aa26d92b3f0ee8be006808657
workflow-type: tm+mt
source-wordcount: '536'
ht-degree: 45%

---

# Adobe Experience Platform リリースノート

**リリース日：2019 年 9 月 10 日（PT）**

Adobe Experience Platform の既存の機能に対するアップデート：

* [[!DNL Data Ingestion]](#ingestion)
* [[!DNL Data Science Workspace]](#dsw)
* [[!DNL Query Service]](#query)

## [!DNL Data Ingestion] {#ingestion}

Adobe Experience Platform には、あらゆる種類のデータやデータのレイテンシを取り込むための豊富な機能が用意されています。Adobe Experience Platform [!DNL Data Ingestion] では、Batch API、Streaming API、ネイティブAdobeコネクタ、データ統合パートナー、Adobe Experience Platform UI など、データを取り込むための複数の方法を提供します。

**新機能**

| 機能 | 説明 |
| ----------- | ---------- |
| ストリーミング取得用の新しいドメイン | `dcs.data.adobe.net` ドメインは、新しい共通データ収集ドメイン `dcs.adobedc.net` に移動しました。ユーザーは、Adobe Experience Platform の改訂されたストリーミング取得ドキュメントに従って、実装を更新する必要があります。Adobe Experience Platform のストリーミング取得に関連するすべてのドキュメントは、新しいドメインを使用するように更新されています。 |

詳しくは、[データ取得ドキュメント](../../ingestion/home.md)を参照してください。

## [!DNL Data Science Workspace] {#dsw}

Adobe Experience Platform [!DNL Data Science Workspace] は、 [!DNL Experience Platform] これにより、データサイエンティストは、機械学習モデルを構築し運用することで、Adobeソリューションやサードパーティシステムをまたいで、データやコンテンツからインサイトをシームレスに生み出すことができます。 [!DNL Data Science Workspace] は、と緊密に統合されています [!DNL Platform] とは、XDM データの調査と準備、次にモデルの開発と運用を含め、エンドツーエンドのデータサイエンスライフサイクルを強化し、自動的にエンリッチメントをおこなうためのモデルの開発と運用を可能にします。 [!DNL Real-time Customer Profile] と機械学習インサイト

**新機能**

| 機能 | 説明 |
| -----------| ---------- |
| UI を介したサービスのスケジュール | 統合先 [!DNL Platform] UI を使用して、ユーザー定義のスケジュールでモデルのトレーニングとスコアリングを自動化するオーケストレーションサービス。 |
| [!DNL Service Gallery] | 再設計された [!DNL Service Gallery]. |
| [!DNL JupyterLab] 5.0.0 | [!DNL JupyterLab] UI の改善。 |

**既知の問題**

* 現在、 [!DNL Service Gallery] をクリックして、既存のサービスを削除します。 当面は、[Senesi の機械学習 API リファレンスを](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/sensei-ml-api.yaml)参照して、API 呼び出しを通じて既存のサービスを削除してください。
* この [!DNL Service Gallery] では、サービスのトレーニングとスコアリングの実行をフィルタリングするためのページネーションのサポートはありません。
* スケジュールに沿ったトレーニング実行またはスコアリング実行を設定する場合、 [!DNL Service Gallery]で、頻度を 1 時間に設定すると、スケジュールは適用されません。

詳しくは、「[Data Science Workspace の概要](../../data-science-workspace/home.md)」を参照してください。

## [!DNL Query Service] {#query}

[!DNL Query Service] は、標準の SQL を使用してAdobe Experience Platformでデータをクエリし、様々な分析およびデータ管理の使用例をサポートする機能を提供します。 これは、 [!DNL Data Lake] クエリの結果を新しいデータセットとして取り込み、レポートで使用します。 [!DNL Data Science Workspace]、またはへの取り込み [!DNL Real-time Customer Profile].

以下を使用できます。 [!DNL Query Service] データ分析のエコシステムを構築するために、様々なインタラクションチャネルをまたいだ顧客の全体像を作成します。 これらのチャネルには、POS（販売時点管理システム）、web、モバイル、CRM システムなどが含まれます。

**新機能**

| 機能 | 説明 |
| -----------| ---------- |
| の改善点 [!DNL Query Editor] | 保存関数が追加され、クエリを保存して後で使用できるようになりました。「参照」タブを [!DNL Query Service] 組織のユーザーが保存したクエリを表示する、Adobe Experience Platformのユーザーインターフェイス。 表示中のクエリに関する有用なメタデータを表示する「クエリの詳細」パネルが実装されました。 |
| 新しい属性関数 | Adobe定義関数 [!DNL Query Service] 有効期限パラメーターを使用してチャネル属性をクエリするには、次の手順を実行します。 |
| SQL 構文の強化 | iLike 構文のサポート。 |
| 定義済みの XDM スキーマを使用してデータセットを生成 | Create Table as Select（CTAS）クエリの新しい句が追加され、ターゲットスキーマを指定できるようになりました。 |

詳しくは、[クエリサービスのドキュメント](../../query-service/home.md)を参照してください。
