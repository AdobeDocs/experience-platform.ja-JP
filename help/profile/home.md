---
title: リアルタイム顧客プロファイルの概要
description: リアルタイム顧客プロファイルは、様々なソースからのデータを結合し、そのデータへのアクセスを個々の顧客プロファイルおよび関連する時系列イベントの形式で提供します。この機能を使用すると、マーケターは、複数のチャネルにわたって、オーディエンスとの調整された一貫した関連性のあるエクスペリエンスを促進できます。
exl-id: c93d8d78-b215-4559-a806-f019c602c4d2
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '1826'
ht-degree: 90%

---

# [!DNL Real-Time Customer Profile] の概要

Adobe Experience Platform を使用すると、顧客がいつどこからブランドとやり取りしても、顧客に合わせて調整された、一貫性と関連性のある体験を提供できます。[!DNL Real-Time Customer Profile] では、オンライン、オフライン、CRM、サードパーティなど、複数のチャネルのデータを組み合わせた、各顧客の全体像を確認できます。[!DNL Profile] を使用すると、個別の顧客データを統合ビューに統合し、顧客インタラクションごとに実用的なタイムスタンプ付きの説明を提供できます。この概要は、[!DNL Experience Platform] での [!DNL Real-Time Customer Profile] の役割と使い方を理解するのに役立ちます。

## Experience Platform での [!DNL Profile]

次の図では、リアルタイム顧客プロファイルと Experience Platform 内のその他のサービスとの関係がハイライト表示されています。

![リアルタイム顧客プロファイルと Adobe Experience Platform のその他のサービスとの関係。次の図は、プロファイルが Adobe Experience Platform のコアコンポーネントの 1 つであることを示しています。](images/profile-overview/profile-in-platform.png)

## プロファイルについて

[!DNL Real-Time Customer Profile] は様々なエンタープライズシステムのデータを結合し、関連する時系列イベントを使用して、顧客プロファイルの形でそのデータへのアクセスを提供します。この機能を使用すると、マーケターは、複数のチャネルにわたって、オーディエンスとの調整された一貫した関連性のあるエクスペリエンスを促進できます。以下の節では、Experience Platform内でプロファイルを効果的に構築し、維持するために理解しておく必要がある中心概念の一部を重点的に説明します。

### プロファイルエンティティの構成

リアルタイム顧客プロファイルは、**プライマリエンティティ**&#x200B;と呼ばれるメインエンティティと、様々なサポートエンティティで構成されます。Experience Platform のコンテキストでは、プライマリエンティティは通常&#x200B;**プロファイルエンティティ**&#x200B;であり、個人の特性、行動およびオーディエンスメンバーシップで構成されています。その他のエンティティには次のものがあり、セグメント化エンジンでプロファイルのプライマリエンティティ外部のデータを利用できます。

- **ディメンションエンティティ**：イベントやプロファイルレコード間で共有される情報のデータモデリングプロセスを簡略化するために使用されるエンティティ。これは、ルックアップエンティティまたは分類エンティティとも呼ばれます。
- **B2B エンティティ**：プロファイルと B2B アカウントおよびオポチュニティとの関係を表すエンティティ。

![プロファイルエンティティの構成を説明する図。](./images/profile-overview/profile-entity-composition.png)

>[!IMPORTANT]
>
>ディメンションエンティティと B2B エンティティはプライマリエンティティの外部にしか存在しないので、これらはバッチセグメント化にのみ使用されます。

ディメンションエンティティと B2B エンティティは、**スキーマ関係**&#x200B;を介してプライマリエンティティにリンクされます。詳しくは、次のドキュメントを参照してください。

- [ルックアップエンティティに対する 1 対 1 のスキーマ関係の作成](../xdm/tutorials/relationship-ui.md)
- [B2B エンティティに対する多対 1 のスキーマ関係の作成](../xdm/tutorials/relationship-b2b.md)

### プロファイルデータストア

[!DNL Real-Time Customer Profile] は、取得されたデータを処理し、Adobe Experience Platform [!DNL Identity Service] を使用してて ID マッピングを通じて関連データを結合しますが、独自のデータを [!DNL Profile] データストア内に保持します。[!DNL Profile] ストアは、Data Lake 内のカタログデータと ID グラフ内の [!DNL Identity Service] データとは別のものです。

プロファイルストアはMicrosoft Azure Cosmos DB インフラストラクチャを使用し、Experience Platform Data Lake はMicrosoft Azure Data Lake ストレージを使用します。

### プロファイルガードレール

Experience Platform は、リアルタイム顧客プロファイルがサポートできない[エクスペリエンスデータモデル（XDM）スキーマ](../xdm/home.md)の作成を回避するのに役立つ一連のガードレールを提供します。これには、パフォーマンスの低下を引き起こすソフトリミットや、エラーやシステムの破損を引き起こすハードリミットが含まれます。 ガイドラインのリストや使用例など、詳細については、[プロファイルガードレール](guardrails.md)のドキュメントをお読みください。

### プロファイルダッシュボード {#profile-dashboard}

Experience Platform UI には、毎日のスナップショット中にキャプチャされたリアルタイム顧客プロファイルのデータに関する重要な情報を表示できるダッシュボードが用意されています。UI の [!DNL Profile] ダッシュボードにアクセスして操作する方法、およびダッシュボードに表示される指標に関する詳細については、『[プロファイルダッシュボード UI ガイド](ui/profile-dashboard.md)』を参照してください。

### プロファイルフラグメントと結合プロファイル {#profile-fragments-vs-merged-profiles}

個々の顧客プロファイルは、その顧客に対する単一のビューを形成する複数のプロファイルフラグメントで構成されます。例えば、顧客が複数のチャネルをまたがって自社のブランドとやり取りを行う場合、1 人の顧客に関連する複数のプロファイルフラグメントが複数のデータセットに表示されます。これらのフラグメントがExperience Platformに取り込まれると、それらのフラグメントが結合され、その顧客用に単一のプロファイルが作成されます。

つまり、プロファイルフラグメントは一意のプライマリ ID と、特定のデータセット内のその ID に対応する[レコード](#record-data)または[イベント](#time-series-events)データを表します。

複数のデータセットからのデータが競合する場合（例えば、1 つのフラグメントリストが顧客について「独身」、他のリストが「既婚」としている）、[結合ポリシー](#merge-policies)が個人のプロファイルに優先順位を付け、含める情報を決定します。したがって、各プロファイルは通常、複数のデータセットからの複数のフラグメントで構成され、Experience Platform内のプロファイルフラグメントの合計数は、結合されたプロファイルの合計数よりも常に多くなる可能性が高くなります。

### レコードデータ {#record-data}

プロファイルとは、多数の属性（レコードデータとも呼ばれます）で構成される主体、組織または個人を表したものです。例えば、製品のプロファイルには SKU と説明が含まれ、人のプロファイルには名、姓、電子メールアドレスなどの情報が含まれる場合があります。[!DNL Experience Platform] を使用すると、プロファイルをカスタマイズして、ビジネスに関連する特定のデータを使用できます。標準 [!DNL Experience Data Model] （XDM）のクラス [!DNL XDM Individual Profile] は、顧客レコードデータを記述するときにスキーマを構築するための推奨クラスであり、Experience Platform サービス間の多くの対話に不可欠なデータを提供します。 [!DNL Experience Platform] でのスキーマの使用に関する詳細については、まず[XDM システムの概要](../xdm/home.md)をお読みください。

### 時系列イベント {#time-series-events}

時系列データは、主体が直接または間接的にアクションを実行した時点のシステムのスナップショットと、イベント自体を詳細に記述したデータを提供します。時系列データは、標準のスキーマクラス XDM ExperienceEvent で表され、イベント（買い物かごへの追加、リンククリック、ビデオ視聴など）を示すことができます。時系列データは、セグメント化ルールの基にするために使用でき、また、イベントはプロファイルのコンテキストで個別にアクセスできます。

### ID

どの企業も、個人的に感じる方法で顧客とコミュニケーションをとりたいと思っています。ただし、関連するデジタルエクスペリエンスを顧客に提供する際の課題の 1 つは、多くの場合、タブレット、携帯電話、ノートパソコンなどの様々なデジタルチャネルに広がっている、切断されたデータを結び付ける方法を理解することです。[!DNL Identity Service] を使うと、複数のチャネルの ID をリンクし、各顧客の ID グラフを作成することで、顧客の全体像をまとめることができます。詳しくは、「[ID サービスの概要](../identity-service/home.md)」を参照してください。

### 結合ポリシー

複数のソースからデータを集め、それを組み合わせて各顧客の完全な表示を確認する場合、統合ポリシーは、データの優先順位付け方法と、どのデータを組み合わせて顧客プロファイルを作成するかを決定するために [!DNL Experience Platform] が使用するルールです。

複数のデータセットから得られたデータが競合する場合、そのデータの処理方法と使用する値は結合ポリシーで決まります。RESTful API またはユーザーインターフェイスを通じて、新しい結合ポリシーの作成、既存のポリシーの管理、組織のデフォルトの結合ポリシーの設定を行えます。

結合ポリシーとその Experience Platform 内での役割について詳しくは、まず[結合ポリシーの概要](merge-policies/overview.md)をお読みください。

### 結合スキーマ {#profile-fragments-and-union-schemas}

[!DNL Real-Time Customer Profile] の主な特徴の一つは、マルチチャネルデータの統合です。[!DNL Real-Time Customer Profile] を使用してエンティティにアクセスする場合、データセット全体のそのエンティティすべての、プロファイルフラグメントの結合されたビューを提供できます。これは「結合ビュー」と呼ばれ、結合スキーマによって実現できます。

UI の結合スキーマへのアクセス方法など、結合スキーマについて詳しくは、『[結合スキーマ UI ガイド](ui/union-schema.md)』を参照してください。

<!-- ### (Alpha) Computed attributes

>[!IMPORTANT]
>
>Computed attribute functionality is in alpha. The documentation and functionality are subject to change.

Computed attributes are functions used to aggregate event-level data into profile-level attributes. These functions are automatically computed so that they can be used across segmentation, activation, and personalization. These computations help you to easily answer questions related to things like lifetime purchase value, time between purchases, or number of application opens, without requiring you to manually perform complex calculations each time the information is needed. For more information on computed attributes, including understanding the role computed attributes play within Adobe Experience Platform, please begin by reading the [computed attributes overview](computed-attributes/overview.md). -->

## プロファイルとオーディエンス

Adobe Experience Platform [!DNL Segmentation Service] は、個々の顧客向けのエクスペリエンスを強化するために必要なオーディエンスを生成します。オーディエンスを作成すると、そのオーディエンスの ID が、適合するすべてのプロファイルのオーディエンスメンバーシップのリストに追加されます。セグメントルールは、RESTful API とセグメントビルダーのユーザーインターフェイスを使用して作成され、[!DNL Real-Time Customer Profile] データに適用されます。セグメント化の詳細については、「[セグメント化サービスの概要](../segmentation/home.md)」を参照してください。

### ストリーミングの取得とストリーミングのセグメント化

リアルタイム入力は、ストリーミング取得と呼ばれるプロセスを通じて可能になります。プロファイルと時系列データが取り込まれると、[!DNL Real-Time Customer Profile] は、する前に、ストリーミングセグメント化と呼ばれる継続的なプロセスを通じて、そのデータをオーディエンスに含めるか除外するかを自動的に決定してから、既存のデータと結合して和集合表示を更新します。その結果、顧客がブランドとやり取りする際に、瞬時に計算を行い、顧客に対して強化された個別的なエクスペリエンスを提供する意思決定を行うことができます。取得される間、データが正しく取得され、データセットの基になるスキーマに適合していることを確認する検証も行われます。取得中の検証の詳細については、まず「[データ取得の質の概要](../ingestion/quality/overview.md)」を読んでください。

## データを [!DNL Profile] に取り込む

[!DNL Experience Platform] でレコードと時系列データを [!DNL Profile] に送信するように構成して、リアルタイムのストリーミング取得とバッチ取得をサポートできます。詳しくは、[リアルタイム顧客プロファイルへのデータの追加](tutorials/add-profile-data.md)方法を概要するチュートリアルを参照してください。

>[!NOTE]
>
>[!DNL Analytics Cloud]、[!DNL Marketing Cloud] および [!DNL Advertising Cloud] などの Adobe ソリューションで収集されたデータは [!DNL Experience Platform] に流れ、[!DNL Profile] に取り込まれます。

### プロファイル取得指標

Observability Insights を使用すると、Adobe Experience Platform で主要指標を公開できます。様々な [!DNL Experience Platform] 機能の [!DNL Experience Platform] の使用統計とパフォーマンス指標に加え、プロファイル関連の特定の指標を使用して、受信リクエストの割合、成功した取得の割合、取得済みレコードサイズなどを把握できます。詳しくは、[Observability Insights API の概要](../observability/api/overview.md)を読み、リアルタイム顧客プロファイル指標の完全なリストについては、[利用可能な指標](../observability/api/metrics.md#available-metrics)に関するドキュメントを参照してください。

## プロファイルストアデータの更新

場合によっては、組織のプロファイルストアのデータを更新する必要があります。 例えば、レコードを修正したり、属性値を変更したりする必要がある場合があります。これは、バッチの取り込みを通じて行うことができ、upsert タグで構成されたプロファイル対応のデータセットが必要です。属性更新用にデータセットを構成する方法について詳しくは、[プロファイルとアップサートのデータセットの有効化](../catalog/datasets/enable-upsert.md)に関するチュートリアルを参照してください。

## データガバナンスと [!DNL Privacy]

データガバナンスは、顧客データを管理し、データの使用に適用される規制、制限、ポリシーへのコンプライアンスを確保するために使用される一連の戦略とテクノロジーです。

データガバナンスはデータへのアクセスに関連しており、様々なレベルにおいて、[!DNL Experience Platform] 内で重要な役割を果たします。

- データ使用のラベル付け
- データアクセスポリシー
- マーケティングアクションのデータのアクセス制御

データガバナンスは、いくつかのポイントで管理されています。これには、[!DNL Experience Platform] に取得するデータと、特定のマーケティングアクションの取得後にアクセスできるデータの決定が含まれます。詳しくは、まず「[データガバナンスの概要](../data-governance/home.md)」を参照してください。

### オプトアウトおよびデータのプライバシー要求の処理

[!DNL Experience Platform] を使用すると、顧客は [!DNL Real-Time Customer Profile] 内でのデータの使用と保存に関連するオプトアウトリクエストを送信できます。オプトアウトリクエストの処理方法について詳しくは、[オプトアウトリクエストの処理](../segmentation/tutorials/consents.md)に関するドキュメントを参照してください。

## 次の手順とその他のリソース

Experience Platform UI または Profile API を使用したリアルタイム顧客プロファイルデータの操作について詳しくは、まず[プロファイル UI ガイド](ui/user-guide.md)または[API 開発者ガイド](api/overview.md)をお読みください。
