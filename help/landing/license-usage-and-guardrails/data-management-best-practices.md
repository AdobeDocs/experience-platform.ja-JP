---
keywords: Experience Platform;ホーム;人気のあるトピック;データ管理;ライセンス使用権限;ライセンス;ベストプラクティス
title: データ管理ライセンス使用権限のベストプラクティス
description: Adobe Experience Platform でライセンス使用権限をより適切に管理するために使用できるベストプラクティスとツールについて説明します。
exl-id: f23bea28-ebd2-4ed4-aeb1-f896d30d07c2
source-git-commit: 252ca6c62b6b95e3a01211c15d7361146dee5116
workflow-type: tm+mt
source-wordcount: '2130'
ht-degree: 81%

---

# データ管理ライセンス使用権限のベストプラクティス

Adobe Experience Platform は、お客様のデータをリアルタイムに更新される堅牢な顧客プロファイルに変換し、AI によるインサイトを利用して、あらゆるチャネルにわたって適切なエクスペリエンスを提供できるようにするオープンシステムです。様々なタイプ、量、履歴のデータをソースを使用して Experience Platform に取り込み、そのデータをセグメント化やパーソナライゼーション、分析、機械学習などのユースケースに対応させることができます。

Platform は、作成できるプロファイルの数や取り込めるデータの量を設定したライセンスを提供します。任意のデータのソース、量または履歴を取り込めるので、データ量が増加するに従って、ライセンス使用権限を超過する可能性があります。

このドキュメントでは、Adobe Experience Platform でのライセンス使用権限をより適切に管理するために、従うべきベストプラクティスと使用できるツールの概要を説明します。

## Adobe Experience Platform データストレージについて

Experience Platformは、主に次の 2 つのデータリポジトリで構成されます。の [!DNL data lake] とプロファイルストア。

**[!DNL data lake]** は、主に次のような目的で使用されます。

* Experience Platform へのデータのオンボーディングのためのステージング領域として機能させる。
* すべての Experience Platform データのための長期間のデータストレージとして機能させる。
* データ分析およびデータサイエンスなどのユースケースを有効にする。

この **プロファイルストア** は、顧客プロファイルを作成する場所で、主に次の目的を果たします。

* リアルタイムエクスペリエンスをサポートするために使用されるプロファイルのデータストレージとして機能させる。
* セグメント化、アクティベーション、パーソナライゼーションなどのユースケースを有効にする。

>[!NOTE]
>
>[!DNL data lake] へのアクセスは、購入した製品 SKU によって異なります。製品 SKU について詳しくは、アドビ担当者にお問い合わせください。

## ライセンス使用状況 {#license-usage}

Experience Platform のライセンスを取得すると、SKU によって異なるライセンス使用権限が提供されます。

**[!DNL Addressable Audience]** - Experience Platform で契約により許可される顧客プロファイルの合計数（既知のプロファイルと匿名プロファイルの両方を含む）。

**[!DNL Profile Richness]** - Experience Platform におけるお客様のプロファイルデータの平均サイズ。リッチネスパックを購入することで、[!DNL Profile Richness] 使用権限を増やすことができます。

[!DNL Profile Richness] 指標は、購入したライセンスによって異なります。[!DNL Profile Richness] では、2 つの計算方法を利用できます。

* 任意の時点でAdobe Real-time Customer Data Platform（リアルタイム顧客プロファイルおよび ID サービス）に保存されるすべての実稼動データの合計を、 [!DNL Addressable Audience];
* Platform に保存されるすべてのデータの合計 ( [!DNL data lake]、リアルタイム顧客プロファイル、ID サービス ) を過去 12 ヶ月間に（Platform 内に保存するのではなく）ストリーミングする任意の時点のデータとデータ ( [!DNL Addressable Audience].

これらの指標の可用性と各指標の具体的な定義は、お客様の組織が購入したライセンスによって異なります。

## ライセンス使用状況ダッシュボード

Adobe Experience Platform UI には、組織の Platform ライセンス関連データのスナップショットを表示できるダッシュボードが用意されています。ダッシュボードのデータは、スナップショットが作成された特定の時点とまったく同じ内容を表示します。スナップショットは、データの近似値でもサンプルでもなく、ダッシュボードは、リアルタイムに更新されていません。

詳しくは、[Platform UI でのライセンス使用状況ダッシュボードの使用](../../dashboards/guides/license-usage.md#license-usage-dashboard-data)に関するガイドを参照してください。

## データ管理のベストプラクティス

次の節では、データをより良く管理するために従うべきベストプラクティスについて説明します。

### データの把握

Adobe Experience Platform では、すべてのデータが同じわけではありません。データには、密だが価値が低いものもあれば、疎だが価値が高いものもあります。生成されるとすぐに価値を失うデータもあれば、数年とは言わないまでも、数か月にわたって価値が高いデータもあります。

データの価値を把握する際に考慮すべき 3 つのディメンションがあります。

| ディメンション | 説明 | 例 |
| --- | --- | --- |
| ボリューム | 取り込まれたデータの量と合計を表します。 | Web クリック数 - 量は多く、忠実度は中程度です。価値が急速に低下する可能性があります。 |
| 期間 | 取り込まれたデータが価値を維持し続ける時間の長さを表します。 | オフライン購入 - 量や忠実度は中程度ですが、長期間にわたって価値が高い可能性があります。 |
| 忠実度 | データがいかに豊富な情報を持っているかを表わします。 | 顧客アカウント - 量は少ないが、忠実度は高いです。顧客の有効期間を超えて価値がある可能性があります。 |

### データ管理ツール {#data-management-tools}

データ使用量がライセンス使用権限の制限内に収まるようにするには、主に 2 つのシナリオを考慮する必要があります。

### どのデータを Platform に取り込むか？

データは、Platform の 1 つ以上のシステム ( [!DNL data lake] および/またはプロファイルストア。 これは、様々なユースケースに対する様々なデータが両方のシステムに存在する可能性があることを意味します。例えば、履歴データを [!DNL data lake]ですが、プロファイルストアにはありません。 プロファイル取り込み用のデータセットを有効にすることで、プロファイルストアに送信するデータを選択できます。

>[!NOTE]
>
>[!DNL data lake] へのアクセスは、購入した製品 SKU によって異なります。製品 SKU について詳しくは、アドビ担当者にお問い合わせください。

### どのデータを保持するか？

データ取り込みフィルターと有効期限ルールの両方を適用して、使用事例で古くなったデータを削除できます。 通常、行動データ（Analytics データなど）は、レコードデータ（CRM データなど）よりも大幅に多くのストレージを消費します。例えば、多くの Platform ユーザーは、レコードデータに比べて、行動データだけでプロファイルの最大 90％を占めています。したがって、行動データを管理することは、ライセンス使用権限の範囲内でコンプライアンスを確保するために重要です。

ライセンス使用権限の範囲内で使用するために活用できるツールが多数あります。

* [取り込みフィルター](#ingestion-filters)
* [プロファイルストア](#profile-service)

### 取り込みフィルター {#ingestion-filters}

取り込みフィルターを使用すると、ユースケースに必要なデータのみを取り込み、不要なすべてのイベントをフィルターで除外できます。

| 取り込みフィルター | 説明 |
| --- | --- |
| Adobe Audience Manager ソースフィルタリング | Adobe Audience Managerソース接続を作成する際に、 [!DNL data lake] およびリアルタイム顧客プロファイルを参照してください。Audience Managerデータ全体を取り込むのではありません。 詳しくは、[Audience Manager ソース接続の作成](../../sources/tutorials/ui/create/adobe-applications/audience-manager.md)に関するガイドを参照してください。 |
| Adobe Analytics データ準備 | Analytics ソース接続を作成する際に [!DNL Data Prep] 機能を使用すると、ユースケースに必要のないデータをフィルターで除外できます。[!DNL Data Prep] を使用して、どの属性／列をプロファイルに公開する必要があるかを定義できます。また、条件文を記述して、データをプロファイルに公開するのか、それとも [!DNL data lake] にだけ公開するのかを Platform に通知できます。詳しくは、[Analytics ソース接続の作成](../../sources/tutorials/ui/create/adobe-applications/analytics.md)に関するガイドを参照してください。 |
| プロファイル用データセットの有効化／無効化のサポート | データをリアルタイム顧客プロファイルに取り込むには、プロファイルストアで使用するデータセットを有効にする必要があります。 そうすることで、[!DNL Addressable Audience] と [!DNL Profile Richness] の使用権限が追加されます。データセットが顧客プロファイルのユースケースで不要になったら、そのデータセットのプロファイルへの統合を無効にして、データが確実にライセンスへの準拠を維持するようにできます。詳しくは、[プロファイル用データセットの有効化および無効化](../../catalog/datasets/enable-for-profile.md)に関するガイドを参照してください。 |
| Web SDK と Mobile SDK のデータ除外 | Web および Mobile SDK によって収集されるデータには、自動的に収集されるデータと、お客様の開発者が明示的に収集するデータの 2 つのタイプがあります。ライセンスのコンプライアンスをより適切に管理するために、コンテキスト設定により、SDK の設定で自動データ収集を無効にすることができます。また、カスタムデータは、開発者が削除したり、設定しないことも可能です。詳しくは、[SDK の基礎の設定](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/configuring-the-sdk.html?lang=ja#fundamentals)に関するガイドを参照してください。 |
| サーバーサイド転送のデータ除外 | サーバーサイド転送を使用して Platform にデータを送信する場合、ルールアクションのマッピングを削除してすべてのイベントにわたって除外するか、ルールに条件を追加して特定のイベントでのみデータを送信するようにすることで、送信するデータを除外できます。詳しくは、[イベントおよび条件](https://experienceleague.adobe.com/docs/experience-platform/tags/ui/rules.html?lang=ja#events-and-conditions-(if))に関するドキュメントを参照してください。 |

{style="table-layout:auto"}

### プロファイルストア {#profile-service}

プロファイルストアは、次のコンポーネントで構成されています。

| プロファイルストアコンポーネント | 説明 |
| --- | --- |
| プロファイルフラグメント | 個々の顧客プロファイルは、その顧客に対する単一のビューを形成する複数の&#x200B;**プロファイルフラグメント**&#x200B;で構成されます。例えば、顧客が複数のチャネルをまたいで自社のブランドとやり取りを行う場合、1 人の顧客に関連する複数の&#x200B;**プロファイルフラグメント**&#x200B;が複数のデータセットに表示されます。これらのフラグメントが Platform に取り込まれると、それらのフラグメントが ID グラフを使用してステッチされ、その顧客用に単一のプロファイルが作成されます。**プロファイルフラグメント**&#x200B;は、識別子としての ID 名前空間と、関連付けられたレコードデータや時系列データで構成されます。 |
| レコードデータ（属性） | プロファイルとは、多数の&#x200B;**属性**（**レコードデータ**&#x200B;とも呼ばれる）で構成される主体、組織または個人を表したものです。例えば、製品のプロファイルには SKU と説明が含まれ、人物のプロファイルには名、姓、電子メールアドレスなどの情報が含まれる場合があります。**レコードデータ**&#x200B;は、通常、量は小／中程度ですが、長期間にわたって価値があります。 |
| 時系列データ（行動） | **時系列データ**&#x200B;は、ユーザー行動に関する情報を提供します。時系列データは、標準スキーマクラスのエクスペリエンスデータモデル（XDM）[!DNL ExperienceEvent] で表され、イベント（買い物かごへの追加、リンククリック、ビデオ視聴など）を示すことができます。行動の価値は、時間と共に減少する可能性があります。 |
| ID 名前空間（ID） | 統合された顧客データは、**ID 名前空間**&#x200B;を使用して単一のプロファイルに結合され、ユーザーに関してより多くの情報が得られるにつれて、これらの ID をステッチできます。詳しくは、[ID 名前空間の概要](../../identity-service/namespaces.md)を参照してください。 |

{style="table-layout:auto"}



#### プロファイルストア構成レポート

プロファイルストアの構成を理解するのに役立つレポートが多数用意されています。 これらのレポートは、Experience Event の有効期限を設定する方法と場所に関する十分な情報に基づいた決定をおこない、ライセンスの使用状況を最適化するのに役立ちます。

* **Dataset Overlap Report API**：アドレス可能なオーディエンスに最も貢献するデータセットを公開します。このレポートを使用して、どのレポートを特定できるかを確認できます [!DNL ExperienceEvent] の有効期限を設定するデータセット。 詳しくは、[データセット重複レポートの生成](../../profile/tutorials/dataset-overlap-report.md)に関するチュートリアルを参照してください。
* **Identity Overlap Report API**：アドレス可能なオーディエンスに最も貢献する ID 名前空間を公開します。詳しくは、[ID 重複レポートの生成](../../profile/api/preview-sample-status.md#generate-the-identity-namespace-overlap-report)に関するチュートリアルを参照してください。
<!-- * **Unknown Profiles Report API**: Exposes the impact of applying pseudonymous expirations for different time thresholds. You can use this report to identify which pseudonymous expirations threshold to apply. See the tutorial on [generating the unknown profiles report](../../profile/api/preview-sample-status.md#generate-the-unknown-profiles-report) for more information.
-->

#### エクスペリエンスイベントの有効期限 {#event-expirations}

この機能を使用すると、ユースケースにとって有用ではなくなった行動データを、プロファイル対応データセットから自動的に削除できます。 概要については、 [エクスペリエンスイベントの有効期限](../../profile/event-expirations.md) データセットに対して有効化した後のこのプロセスの動作の詳細

## ライセンス使用状況のコンプライアンスに関するベストプラクティスのまとめ {#best-practices}

次に、ライセンス使用権限の順守をより確実にするために従うことのできる、推奨されるベストプラクティスのリストを示します。

* [ライセンス使用状況ダッシュボード](../../dashboards/guides/license-usage.md)を使用して、顧客の使用状況のトレンドを追跡および監視する。これにより、発生する可能性のある超過分に事前に対処できます。
* セグメント化およびパーソナライゼーションのユースケースに必要なイベントを特定して、[取り込みフィルター](#ingestion-filters)を設定する。これにより、ユースケースに必要な重要なイベントのみを送信できます。
* セグメント化およびパーソナライゼーションのユースケースに必要な[プロファイルのデータセットのみを有効](#ingestion-filters)にしていることを確認する。
* の設定 [エクスペリエンスイベントの有効期限](#event-expirations) を使用します。
* 定期的に [プロファイル構成レポート](#profile-store-composition-reports) を参照してください。 これにより、ライセンス使用量に最も貢献しているデータソースを把握できます。

## 機能の概要と可用性 {#feature-summary}

このドキュメントで説明するベストプラクティスとツールは、Adobe Experience Platform でのライセンス使用権限をより適切に管理するのに役立ちます。このドキュメントは、すべての Experience Platform のお客様に可視性と制御を提供するための追加機能がリリースされるたびに更新されます。

次の表に、ライセンス使用権限をより適切に管理するために、現在、自由に使用できる機能のリストを示します。

| 機能 | 説明 |
| --- | --- |
| [プロファイル用のデータセットを有効／無効にする](../../catalog/datasets/user-guide.md) | リアルタイム顧客プロファイルへのデータセット取り込みを有効または無効にします。 |
| [エクスペリエンスイベントの有効期限](../../profile/event-expirations.md) | プロファイル対応のデータセットに取り込まれるすべてのイベントに有効期限を適用します。 この機能を有効にするには、Adobeサポート担当者にお問い合わせください。 |
| [Adobe Analytics データ準備フィルター](../../sources/tutorials/ui/create/adobe-applications/analytics.md) | [!DNL Kafka] フィルターを適用して、不要なデータを取り込みから除外します |
| [Adobe Audience Manager ソースコネクタフィルター](../../sources/tutorials/ui/create/adobe-applications/audience-manager.md) | Audience Manager ソース接続フィルターを適用して、不要なデータを取り込みから除外します |
| [Alloy SDK データフィルター](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/configuring-the-sdk.html?lang=ja#fundamentals) | Alloy フィルターを適用して、不要なデータを取り込みから除外します |
| [イベント転送データフィルター](../../tags/ui/event-forwarding/overview.md) | サーバーサイド [!DNL Kafka] フィルターを適用して、不要なデータを取り込みから除外します詳しくは、[タグルール](../../tags/ui/managing-resources/rules.md)に関するドキュメントを参照してください。 |
| [ライセンス使用状況ダッシュボード UI](../../dashboards/guides/license-usage.md#license-usage-dashboard-data) | Experience Platform に関する組織のライセンス関連データのスナップショットを表示します |
| [Dataset Overlap Report API](../../profile/tutorials/dataset-overlap-report.md) | アドレス可能なオーディエンスに最も貢献するデータセットを出力します |
| [Identity Overlap Report API](../../profile/api/preview-sample-status.md#generate-the-identity-namespace-overlap-report) | アドレス可能なオーディエンスに最も貢献する ID 名前空間を出力します |

{style="table-layout:auto"}
