---
keywords: Experience Platform;home;popular topics;CJA;journey analytics;customer journey analytics;campaign orchestration;orchestration;customer journey;journey;journey orchestration;capability;workflow
solution: Experience Platform
title: Adobe Experience Platform ドキュメント
topic: overview
description: Adobe Experience Platformのドキュメントは、ユーザーインターフェイスと API の両方の概要、チュートリアル、ガイドなど、複数の形式で提供されています。Experience Platformサービスで使用できる最も一般的なドキュメントの種類を簡単に説明します。
translation-type: tm+mt
source-git-commit: e94278c1b64e1d940f55861518c78cca24822c1b
workflow-type: tm+mt
source-wordcount: '1084'
ht-degree: 53%

---


# Adobe Experience Platform ドキュメントの概要

>[!NOTE]
>
>Adobe Experience Platform のドキュメントは最近移動されました。新しいナビゲーションを確認し、既存のブックマークを更新してください。

## ドキュメントの種類

Adobe Experience Platformのドキュメントは、ユーザーインターフェイスと API の両方の概要、チュートリアル、ガイドなど、複数の形式で提供されています。Here is a brief description of the most common documentation types that are available for [!DNL Experience Platform] services:

* **API リファレンス：**&#x200B;使用可能なエンドポイント（ヘッダー、パラメーター、サンプルリクエスト、応答など）の詳細については、各サービスの API リファレンスドキュメントを参照してください。これらの参考資料は、ドキュメントと同程度の詳細を提供していません。API の使用例の詳細については、サービス固有の開発者ガイドを参照することをお勧めします。
* **開発者ガイド：**&#x200B;各開発者ガイドは、特定のサービスで使用できるすべての API エンドポイントの使用状況に関する詳細な情報を提供します。The guide includes available query parameters, sample requests, and sample responses, as well as outlining &quot;gotchas&quot; to avoid when making calls to [!DNL Platform] APIs.
* **概要：**[!DNL Platform]概要では、サービスや機能の概要、および他の サービスとの相互作用について説明します。The overview is the best place to start when learning about new features and functionality within [!DNL Platform].
* **トラブルシューティングガイド：**&#x200B;トラブルシューティングガイドを使用して、API を使用する際に遭遇する可能性が高い、よくある質問とエラーメッセージに関する情報を見つけます。The [!DNL Experience Platform] troubleshooting guide provides support for general questions and errors, while individual services provide troubleshooting guides specific to their area.
* **チュートリアル：**&#x200B;チュートリアルは、UI、API、またはその両方を順に示し、特定の結果へと導きます。開発者ガイドとは異なり、API チュートリアルは 1 つまたは 2 つのエンドポイントのみに焦点を当てている可能性があり、完全な API リソースではありません。同様に、UI チュートリアルでは、そのサービスで使用できる完全なユーザインターフェイスではなく、特定の手順にのみ焦点を当てる場合があります。チュートリアルは、多くの場合、より大規模なワークフローの一部であり、次に試すチュートリアルを説明する「次のステップ」を示しています。
* **ユーザーガイド：**[!DNL Platform]ユーザーガイドでは、特定のサービスの UI で使用できるアクションの概要を説明しています。これらのドキュメントには、ユーザーインターフェイスを介した Platform の操作に焦点を当てたスクリーンショットと手順が含まれています。開発者ガイドと同様に、ユーザーガイドでは、回避べき「了解事項」を含む、使用可能なすべてのアクションとオプションの概要を説明しています。これは、UI を操作する際の最も詳細なリソースです。

## [!DNL Experience Platform] services

現在、以下に示す Adobe Experience Platform のサービスおよび機能に関するドキュメントが提供されています。ここで説明する簡単な説明を通して、サービスの詳細を学び、左側のナビゲーションにあるアルファベット順のリストから選択して、さらに深く理解することができます。

* **[!DNL Access control]:**[!DNL Experience Platform] は、Adobe Admin Consoleの製品プロファイルを利用して、ユーザーを権限とサンドボックスにリンクします。
* **[!DNL Auditor]：** Auditor は、Adobe Experience Cloud の実装を評価し、その改善方法を示します。Auditor は、アドビ製品単体で、または複数のアドビ製品をまとめてさらに活用できるよう支援します。
* **[!DNL Catalog & Datasets]:** テナントで作成されたデータセット、データ系列、およびそれらに関連付けられたポリシーのメタデータを管理します。
* **[!DNL Data Access]:** データエクスポート用に登録されたデータセットの内容にアクセスできるようにします。
* **[!DNL Data Governance]:** Adobe Experience Platformは、複数のエンタープライズシステムからのデータを統合し、マーケターが顧客を識別、把握、関与をより適切に行えるようにします。 [!DNL Experience Platform] エンド・ツー・エンドの [!DNL Data Governance] インフラストラクチャを含むため、システム間でデータを適切に使用し、共有す [!DNL Platform] る場合にも適切に使用できます。
* **[!DNL Data Ingestion]（バッチ&amp;ストリーミング）:** バッチ取り込み、ストリーミング取り込み、 [ソースコネクタを使用して、データをAdobe Experience Platformに取り込み](#sources)ます。
* **[!DNL Data Science Workspace]:**[!DNL Data Science Workspace] 独自のソリューションで使用できる、オファー定義の機械学習モデルと、独自のモデルを作成する機能。
* **[!DNL Debugger]:** forのAdobe Experience Cloudデバッガ拡張機能は、Webページを [!DNL Chrome][!DNL Experience Cloud] 調べ、ソリューションの実装方法に関する問題を見つけるのに役立ちます。
* **[!DNL Decisioning Service]:** Adobe Experience Platform上で実行するアプリで、パーソナライズされた、最適化された、調整されたエクスペリエンスを作成します。
* **[!DNL Destinations]:** 宛先は、よく使用されるアプリケーションとの事前に構築された統合であり、リアルタイム顧客データプラットフォームからのデータをシームレスにアクティベーションできます。 宛先を使用して、クロスチャネルマーケティングキャンペーン、電子メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。
* **ETL（抽出、変換、読み込み）:** Adobe Experience Platform と統合するための、データ統合ツール用高性能コネクターを作成します。
* **[!DNL Experience Platform Web SDK]（ベータ版）:** Adobe Experience Platform Web SDK クライアントサイドの JavaScript ライブラリを使用すると、Adobe Experience Cloud のお客様は Experience Cloud の様々なサービスを操作できます。
* **[!DNL Identity Service]:** 複数のデバイスやチャネル間の行動からIDをブリッジし、個々の顧客の単一の表示を形成することで、顧客をより深く理解できます。
* **[!DNL Intelligent Services]：**&#x200B;マーケターやマーケティングアナリストが、顧客体験を提供する際に人工知能とマシンラーニングの能力を活用できるようにします。
* **[!DNL Launch]：** Launch は、顧客に対してパーソナライズされた関連性の高いリアルタイムエクスペリエンスを提供するために必要な、すべての解析、マーケティング、広告タグを簡単にデプロイおよび管理できるシンプルな方法を提供します。
* **[!DNL Observability Insights]:** Adobe Experience Platformの主要な監視性指標を公開し、使用状況の統計、過去の傾向、および様々な [!DNL Platform] 機能のパフォーマンス指標に関するインサイトを提供します。
* **[!DNL Privacy Service]：** Privacy Service は、プライバシー規制に準拠したデータアクセスおよび削除リクエストをおこなうことができる、RESTful API およびユーザーインターフェイスを提供します。
* **[!DNL Profile](リアルタイム顧客プロファイル):** オンライン、オフライン、CRM、サードパーティのデータを含む複数のチャネルのデータを組み合わせて、各顧客の全体的な表示を確認 [!DNL Real-time Customer Profiles]します。
* **[!DNL Query Service]:** SQLクエリを使用して、Adobeソリューションデータ、お客様のファーストパーティデータ、その他のプラットフォームデータなど、Adobe Experience Platformからデータを取得します。
* **[!DNL Real-time Customer Data Platform]:** リアルタイムCDPは、複数のエンタープライズ・データ・ソースを組み合わせて、1対1のパーソナライズされた顧客体験をすべてのチャネルとデバイスに提供するために、リアルタイムで統合プロファイルを作成します。
* **[!DNL Sandboxes]:** サンドボックスでは、1つの [!DNL Platform] インスタンスを個別の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立ちます。
* **[!DNL Segmentation]:** リアルタイム顧客プロファイルデータに基づいてセグメントを作成し、オーディエンスや電力消費者エクスペリエンスを生み出します。
* **[!DNL Sources]（接続）:**{#sources} Adobeアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからAdobe Experience Platformにデータを取り込みます。
* **XDM（エクスペリエンスデータモデル）**：顧客体験データを標準化し、顧客体験管理のスキーマを定義する取り組みです。XDM schemas to support the interoperability of data across [!DNL Experience Platform] components.

## アプリケーションサービスの構築 [!DNL Experience Platform]

左側のナビゲーションのサービスに加えて、AdobeはExperience Platform上に他のアプリケーションサービスを構築しています。 これらのサービスのドキュメントは、次にリンクする独自のドキュメントリポジトリにあります。

* [[!DNLCustomer Journey Analytics]](https://docs.adobe.com/content/help/ja-JP/analytics-platform/using/cja-landing.html)
* [[!DNLJourney Orchestration]](https://docs.adobe.com/content/help/ja-JP/journeys/using/journey-orchestration-home.html)