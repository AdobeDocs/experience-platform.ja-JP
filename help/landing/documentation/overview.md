---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Adobe Experience Platformドキュメント
topic: overview
translation-type: tm+mt
source-git-commit: 2e5668a8b1d5fb831188fbd4e453b9f4aa7474df
workflow-type: tm+mt
source-wordcount: '984'
ht-degree: 3%

---


# Adobe Experience Platformドキュメントの概要

>[!NOTE]
>Adobe Experience Platformドキュメントが最近移動されました。 新しいナビゲーションを確認し、既存のブックマークを更新してください。

## ドキュメントの種類

Adobe Experience Platformドキュメントは、ユーザーインターフェイスとAPIの両方に関する概要、チュートリアル、ガイドなど、複数の形式で提供されています。 サー [!DNL Experience Platform] ビスで使用可能な最も一般的なドキュメントの種類を簡単に説明します。

* **APIリファレンス：** ヘッダー、パラメーター、サンプルリクエスト、応答など、使用可能なエンドポイントに関する詳細については、各サービスのAPIリファレンスドキュメントを参照してください。 これらの参照資料は、ドキュメントと同じレベルの詳細を提供しません。 APIの使用例の詳細については、サービス固有の開発者ガイドを参照することをお勧めします。
* **開発者ガイド：** 各開発者ガイドでは、特定のサービスで使用可能なすべてのAPIエンドポイントの使用状況に関する詳細な情報を提供します。 このガイドには、利用可能なクエリパラメーター、サンプルリクエスト、サンプルの応答に加え、 [!DNL Platform] APIを呼び出す際に避けるための「了解事項」の概要が含まれています。
* **概要：** 概要には、サービスや機能の概要と、サービスや機能が他のサー [!DNL Platform] ビスとどのように相互作用するかが示されます。 概要は、内の新機能について学習する際に開始する最適な場所で [!DNL Platform]す。
* **トラブルシューティングガイド：** トラブルシューティングガイドを使用して、APIの使用時に発生する可能性のあるよくある質問とエラーメッセージに関する情報を探します。 トラブルシューティングガイドでは、一般的な質問とエラーに対するサポートが提供されます。また、各サービスでは、その領域に固有のトラブルシューティングガイドを提供します。 [!DNL Experience Platform]
* **チュートリアル：** チュートリアルは、UI、API、またはその両方の組み合わせを順を追って示したガイドで、特定の結果を導きます。 開発者ガイドとは異なり、APIチュートリアルでは、1つまたは2つのエンドポイントのみに焦点を当てることができ、APIの完全なリソースではありません。 同様に、UIチュートリアルでは、特定の手順についてのみ説明し、そのサービスで使用できる完全なユーザインターフェイスは説明しません。 チュートリアルは、多くの場合、より大規模なワークフローの一部であり、次に試すチュートリアルを説明する「次のステップ」を機能としています。
* **ユーザーガイド：** ユーザーガイドでは、特定のサービスの [!DNL Platform] UIで使用できるアクションについて概説します。 これらのドキュメントには、ユーザーインターフェイスを介したPlatformとの対話に重点を置いたスクリーンショットと手順が含まれています。 開発者ガイドと同様に、ユーザガイドでは、回避する「了解事項」を含む、使用可能なすべてのアクションとオプションについて概説します。 これは、UIを使用する場合に最も詳細なリソースです。

## [!DNL Experience Platform] services

以下に示すAdobe Experience Platformサービスおよび機能に関するドキュメントは、現在提供されています。 ここで説明する簡単な説明を通して、サービスの詳細を知ることができ、左側のナビゲーションにあるアルファベット順のリストから選択して、より深く理解することができます。

* **[!DNL Access control]:**[!DNL Experience Platform]アドビAdmin Consoleの製品プロファイルを利用して、ユーザーを権限およびサンドボックスにリンクします。
* **[!DNL Auditor]:**AuditorはAdobe Experience Cloudの実装について評価を行い、その改善方法についてポインターを与えます。 Auditor は、アドビ製品単体で、または複数のアドビ製品をまとめてさらに活用できるよう支援します。
* **[!DNL Catalog & Datasets]:**テナントで作成されたデータセット、データ系列、およびそれらに関連付けられたポリシーのメタデータを管理します。
* **[!DNL Data Access]:**データエクスポート用に登録されたデータセットの内容にアクセスできるようにします。
* **[!DNL Data Governance]:**Adobe Experience Platformにより、複数のエンタープライズシステムからのデータを統合し、マーケティング担当者が顧客を特定、理解、関与できるようにします。[!DNL Experience Platform]エンド・ツー・エンドの[!DNL Data Governance]インフラストラクチャを含むため、システム間でデータを適切に使用し、共有す[!DNL Platform]る場合にも適切に使用できます。
* **[!DNL Data Ingestion]（バッチ&amp;ストリーミング）:**バッチ取り込み、ストリーミング取り込み、[ソースコネクタを使用して、データをAdobe Experience Platformに取り込みます](#sources)。
* **[!DNL Data Science Workspace]:**[!DNL Data Science Workspace]独自のソリューションで使用できる、オファー定義の機械学習モデルと、独自のモデルを作成する機能。
* **[!DNL Debugger]:**用のAdobe Experience Cloud Debugger extension forは、Webページを[!DNL Chrome][!DNL Experience Cloud]調査し、ソリューションの実装方法に関する問題を見つけるのに役立ちます。
* **[!DNL Decisioning Service]:**Adobe Experience Platform上で実行するアプリケーションで、パーソナライズされた、最適化された、調整されたエクスペリエンスを作成します。
* **[!DNL Destinations]:**宛先は、よく使用されるアプリケーションと事前に構築された統合であり、リアルタイムの顧客データPlatformからのデータをシームレスにアクティベーションできます。 宛先を使用して、クロスチャネルマーケティングキャンペーン、電子メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。
* **ETL （抽出、変換、読み込み）:** Adobe Experience Platformとの統合に役立つデータ統合ツール用の高パフォーマンスのコネクターを作成します。
* **[!DNL Experience Platform Web SDK]（ベータ版）:**Adobe Experience PlatformWeb SDKのクライアント側のJavaScriptライブラリを使用すると、Adobe Experience Cloudのお客様はExperience Cloud内の様々なサービスとやり取りできます。
* **[!DNL Identity Service]:**複数のデバイスやチャネル間の行動からIDをブリッジし、個々の顧客の単一の表示を形成することで、顧客をより深く理解できます。
* **[!DNL Intelligent Services]:**マーケティング担当者やマーケティングアナリストが、人工知能と機械学習の機能を活用して、顧客体験を提供できます。
* **[!DNL Launch]:**Launchは、パーソナライズされた関連性の高いリアルタイムのエクスペリエンスを顧客に提供するために必要な、すべての分析、マーケティングおよび広告タグをシンプルにデプロイおよび管理する方法です。
* **[!DNL Observability Insights]:**主要な監視性指標をAdobe Experience Platformに公開し、使用状況の統計、過去の傾向、および様々な[!DNL Platform]機能に関するパフォーマンス指標に関するインサイトを提供します。
* **[!DNL Privacy Service]:**Privacy Serviceは、RESTful APIとユーザーインターフェイスを提供し、プライバシーに関する規則に準拠したデータアクセスおよび削除の要求を行うことができます。
* **[!DNL Profile](リアルタイム顧客プロファイル):**オンライン、オフライン、CRM、サードパーティのデータを含む複数のチャネルのデータを組み合わせて、各顧客の全体的な表示を確認[!DNL Real-time Customer Profiles]します。
* **[!DNL Query Service]:**SQLクエリを使用して、アドビのソリューションデータ、お客様のファーストパーティデータ、その他のPlatformデータなど、Adobe Experience Platformからデータを取得します。
* **[!DNL Real-time Customer Data Platform]:**リアルタイムCDPは、複数のエンタープライズ・データ・ソースを組み合わせて、1対1のパーソナライズされた顧客体験をすべてのチャネルとデバイスに提供するために、リアルタイムで統合プロファイルを作成します。
* **[!DNL Sandboxes]:**サンドボックスでは、1つの[!DNL Platform]インスタンスを個別の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立ちます。
* **[!DNL Segmentation]:**リアルタイム顧客プロファイルデータに基づいてセグメントを作成し、オーディエンスや電力消費者エクスペリエンスを生み出します。
* **[!DNL Sources]（接続）:**{#sources}アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからAdobe Experience Platformにデータを取り込みます。
* **XDM(Experience Data Model)**: アドビ主導のXDMは、顧客体験データを標準化し、顧客体験管理のスキーマを定義する取り組みです。 XDMスキーマを使用して、複数のコンポー [!DNL Experience Platform] ネント間でのデータの相互運用性をサポートします。