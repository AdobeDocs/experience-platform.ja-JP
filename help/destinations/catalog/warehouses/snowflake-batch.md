---
title: Snowflake バッチ接続
description: ライブ Snowflake データ共有を作成して、毎日のオーディエンスの更新を共有テーブルとして直接アカウントに受け取ります。
last-substantial-update: 2025-10-23T00:00:00Z
badgeUltimate: label="Ultimate" type="Positive"
exl-id: 6959ccd0-ba30-4750-a7de-d0a709292ef7
source-git-commit: 1b507e9846a74b7ac2d046c89fd7c27a818035ba
workflow-type: tm+mt
source-wordcount: '1659'
ht-degree: 18%

---

# Snowflake バッチ接続 {#snowflake-destination}

>[!AVAILABILITY]
>
>この宛先コネクタは限定提供で、[VA7 リージョン ](/help/landing/multi-cloud.md#azure-regions) でプロビジョニングされたReal-Time CDP Ultimateのお客様のみが利用できます。

## 概要 {#overview}

オーディエンスデータをSnowflake アカウントの動的テーブルに送信する場合は、この宛先を使用します。 動的テーブルを使用すると、物理的なデータコピーを必要とせずにデータにアクセスできます。

Snowflakeの宛先の仕組みと、AdobeとSnowflakeの間でのデータの転送方法については、次の節を参照してください。

### Snowflake データ共有の仕組み {#data-sharing}

この宛先は、[!DNL Snowflake] データ共有を使用します。つまり、データが物理的に書き出されたり、独自のSnowflake インスタンスに転送されたりすることはありません。 代わりに、Adobeでは、Adobe Snowflake環境内でホストされるライブテーブルへの読み取り専用アクセスが許可されます。 この共有テーブルに対してSnowflake アカウントから直接クエリを実行できますが、テーブルを所有しておらず、指定した保持期間を超えてテーブルを変更または保持することはできません。 Adobeは、共有テーブルのライフサイクルと構造を完全に管理します。

AdobeからSnowflake アカウントに初めてデータフローを設定した後、Adobeからの非公開リストへの登録を受け入れるように求められます。

![Snowflake プライベートリストの承認画面を示すスクリーンショット ](../../assets/catalog/cloud-storage/snowflake-batch/snowflake-accept-listing.png)

### データ保持と有効期間（TTL） {#ttl}

この統合を通じて共有されるすべてのデータでは、7 日間の固定有効期限（TTL）が設定されています。 最後のエクスポートから 7 日後、データフローがまだアクティブであるかどうかに関係なく、動的テーブルは自動的に期限切れになり、アクセスできなくなります。 7 日を超えてデータを保持する必要がある場合、TTL の有効期限が切れる前に、内容を独自のSnowflake インスタンスで所有するテーブルにコピーする必要があります。

>[!IMPORTANT]
>
>Experience Platformでデータフローを削除すると、動的テーブルがSnowflake アカウントから表示されなくなります。

### オーディエンスの更新動作 {#audience-update-behavior}

オーディエンスが [ バッチモード ](../../../segmentation/methods/batch-segmentation.md) で評価される場合、共有テーブルのデータは 24 時間ごとに更新されます。 つまり、オーディエンスメンバーシップの変更と共有テーブルに変更が反映される間に、最大 24 時間の遅延が発生する可能性があります。

### バッチデータ共有ロジック {#batch-data-sharing}

データフローがオーディエンスに対して初めて実行されると、バックフィルが実行され、現在修飾されているすべてのプロファイルが共有されます。 この最初のバックフィルの後、宛先はオーディエンスメンバーシップの完全なスナップショットを定期的に提供します。 各スナップショットは共有テーブル内の以前のデータを置き換えるため、履歴データなしでオーディエンスの最新の完全な表示を常に表示できます。

## ストリーミングとバッチデータ共有 {#batch-vs-streaming}

Experience Platformには、[Snowflake ストリーミングと ](snowflake.md)2}Snowflake バッチ [ の 2 種類のSnowflake宛先が用意されています。](snowflake-batch.md)

どちらの宛先でも、ゼロコピー方式でSnowflakeデータにアクセスできますが、各コネクタのユース ケースに関して推奨されるベスト プラクティスがいくつかあります。

次の表は、各データ共有方法が最も適しているシナリオの概要を示すことで、使用するコネクタを決定するのに役立ちます。

|  | 必要なときに [Snowflakeバッチ](snowflake-batch.md) を選択してください | 必要なときに [Snowflakeストリーミング](snowflake.md) を選択してください |
|--------|-------------------|----------------------|
| **更新頻度** | 定期的なスナップショット | リアルタイムでの継続的な更新 |
| **データ表示** | 以前のデータを置き換えるオーディエンススナップショットの完了 | プロファイルの変更に基づいた増分更新 |
| **ユースケースのフォーカス** | レイテンシが重視されない分析/ML ワークロード | リアルタイムの更新が必要な緊急のアクションシナリオ |
| **データ管理** | 最新の完全なスナップショットを常に表示 | オーディエンスメンバーシップの変更に基づく増分更新 |
| **シナリオの例** | ビジネスレポート、データ分析、ML モデルトレーニング | マーケティングキャンペーンの抑制、リアルタイムパーソナライゼーション |

ストリーミングデータ共有について詳しくは、[Snowflake ストリーミング接続 ](snowflake.md) ドキュメントを参照してください。

## ユースケース {#use-cases}

バッチデータ共有は、オーディエンスの完全なスナップショットが必要で、リアルタイムの更新が不要な状況に最適です。例えば、次のような場合です。

* **分析ワークロード**：オーディエンスメンバーシップの完全な表示を必要とするデータ分析、レポートまたはビジネスインテリジェンスタスクを実行する場合
* **機械学習ワークフロー**：機械学習モデルのトレーニングや、オーディエンスの完全なスナップショットのメリットを得る予測分析の実行に使用します
* **データウェアハウス**：オーディエンスデータの現在のコピーを独自のSnowflake インスタンスに保持する必要がある場合
* **定期的なレポート**：履歴変更のトラッキングを行わない最新のオーディエンス状態が必要な、通常のビジネスレポート用
* **ETL プロセス**：オーディエンスデータをバッチで変換または処理する必要がある場合

バッチデータ共有は、完全なスナップショットを提供することでデータ管理を簡素化し、増分更新や結合の変更を手動で管理する必要をなくします。

## 前提条件 {#prerequisites}

Snowflake接続を設定する前に、次の前提条件を満たしていることを確認してください。

* [!DNL Snowflake] アカウントにアクセスできます。
* お使いのSnowflake アカウントは非公開リストに登録されています。 自分または社内でSnowflakeに対するアカウント管理者権限を持つユーザーがこの機能を設定できます。

必要な権限について詳しくは、[[!DNL Snowflake]  ドキュメント ](https://docs.snowflake.com/en/collaboration/consumer-listings-access#access-a-private-listing) を参照してください。

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。 以下の 2 つの表は、このコネクタがサポートするオーディエンスを、_オーディエンスオリジン_ および _オーディエンスに含まれるプロファイルタイプ_ 別に示しています。

| オーディエンスオリジン | サポートあり | 説明 |
|---------|----------|----------|
| [!DNL Segmentation Service] | ✓ | Experience Platform [ セグメント化サービス ](../../../segmentation/home.md) を通じて生成されたオーディエンス。 |
| その他すべてのオーディエンスの接触チャネル | ✓ | このカテゴリには、[!DNL Segmentation Service] を通じて生成されたオーディエンス以外のすべてのオーディエンスの接触チャネルが含まれます。 [ 様々なオーディエンスのオリジン ](/help/segmentation/ui/audience-portal.md#customize) について確認する。 次に例を示します。 <ul><li> csv ファイルからExperience Platformへのカスタムアップロードオーディエンス [ 読み込み ](../../../segmentation/ui/audience-portal.md#import-audience)</li><li> 類似オーディエンス、 </li><li> 連合オーディエンス、 </li><li> Adobe Journey Optimizerなど、他のExperience Platform アプリで生成されたオーディエンス。 </li><li> その他。 </li></ul> |

{style="table-layout:auto"}

オーディエンスデータタイプでサポートされるオーディエンス：

| オーディエンスデータタイプ | サポートあり | 説明 | ユースケース |
|--------------------|-----------|-------------|-----------|
| [ 人物オーディエンス ](/help/segmentation/types/people-audiences.md) | ✓ | 顧客プロファイルに基づき、マーケティングキャンペーンの対象となる人物のグループを指定できます。 | 頻繁な購入者、買い物かご放棄者 |
| [アカウントのオーディエンス](/help/segmentation/types/account-audiences.md) | × | アカウントベースのマーケティング戦略のために特定の組織内の個人Targetします。 | マーケティングB2B |
| [見込み客オーディエンス](/help/segmentation/types/prospect-audiences.md) | × | まだ顧客ではないが、ターゲットオーディエンスと特徴を共有する個人Target。 | サードパーティデータを使用したプロスペクティング |
| [データセットのエクスポート](/help/catalog/datasets/overview.md) | × | Adobe Experience Platform Data Lake に保存された構造化データのコレクション。 | レポート、データサイエンスワークフロー |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
|---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL Audience export]** | [!DNL Snowflake] 宛先で使用される識別子（氏名、電話番号など）を使用して、オーディエンスのすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL Batch]** | この宛先は、Snowflakeデータ共有を通じて完全なオーディエンスメンバーシップのスナップショットを定期的に提供します。 各スナップショットは以前のデータを置き換えるため、オーディエンスの最新の完全な表示を常に入手できます。 |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、 **[!UICONTROL View Destinations]** と **[!UICONTROL Manage Destinations]** [アクセス制御 権限](/help/access-control/home.md#permissions)が必要です。 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

宛先を認証するには、「**[!UICONTROL Connect to destination]**」を選択し、アカウント名と、オプションでアカウントの説明を入力します。

![ 宛先への認証方法を示すサンプルスクリーンショット ](../../assets/catalog/cloud-storage/snowflake-batch/authenticate-destination.png)

### 宛先の詳細の入力 {#destination-details}

>[!CONTEXTUALHELP]
>id="platform_destinations_snowflake_batch_accountID"
>title="Snowflake アカウント ID を入力"
>abstract="アカウントが組織にリンクされている場合は、`OrganizationName.AccountName`<br><br> という形式を使用します。アカウントが組織にリンクされていない場合は、`AccountName` という形式を使用します"

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

![ 宛先の詳細を入力する方法を示すサンプルスクリーンショット ](../../assets/catalog/cloud-storage/snowflake-batch/configure-destination-details.png)

* **[!UICONTROL Name]**：今後この宛先を認識するための名前。
* **[!UICONTROL Description]**：今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL Snowflake Account ID]**:Snowflake アカウント ID。 アカウントが組織にリンクされているかどうかに応じて、次のアカウント ID 形式を使用します。
   * アカウントが組織にリンクされている場合：`OrganizationName.AccountName`。
   * アカウントが組織にリンクされていない場合：`AccountName`。
* **[!UICONTROL Account acknowledgment]**: Snowflake アカウント ID 確認をオンにして、アカウント ID が正しく、自分に属していることを確認します。

>[!IMPORTANT]
>
> 宛先名とExperience Platform サンドボックス名で使用される特殊文字は、Snowflakeではアンダースコア（`_`）に自動変換されます。 混乱を避けるために、宛先とサンドボックス名には特殊文字を使用しないでください。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)に関するガイドを参照してください。

宛先接続への詳細の入力を終えたら「**[!UICONTROL Next]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>* データをアクティブ化するには、**[!UICONTROL View Destinations]**、**[!UICONTROL Activate Destinations]**、**[!UICONTROL View Profiles]**、**[!UICONTROL View Segments]** [ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* *ID* を書き出すには、**[!UICONTROL View Identity Graph]** [ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。<br> ![ 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択します。](/help/destinations/assets/overview/export-identities-to-destination.png " 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択 "){width="100" zoomable="yes"}

この宛先に対してオーディエンスをアクティブ化する手順については、[バッチプロファイル書き出し宛先に対するオーディエンスデータのアクティブ化](/help/destinations/ui/activate-batch-profile-destinations.md)を参照してください。

### 属性のマッピング {#map}

この宛先に ID およびプロファイル属性を書き出すことができます。

![Experience Platformの宛先のマッピング画面を示すSnowflake ユーザーインターフェイス画像。](../../assets/catalog/cloud-storage/snowflake-batch/mapping.png)

[ 計算フィールドコントロール ](../../ui/data-transformations-calculated-fields.md) を使用して、配列に対する操作をエクスポートしたり実行したりできます。

ターゲット属性は、「**[!UICONTROL Attribute name]**」フィールドに指定した属性名を使用して、Snowflakeで自動的に作成されます。

## 書き出されたデータ／データ書き出しの検証 {#exported-data}

データは、動的テーブルを介してSnowflake アカウントにステージングされます。 データが正しく書き出されたことをSnowflake アカウントで確認します。

### データ構造 {#data-structure}

動的テーブルには、次の列が含まれています。

* **TS**: 各行が最後に更新された日時を表すタイムスタンプ列
* **マッピング属性**: アクティベーションワークフロー中に選択したすべてのマッピング属性は、Snowflakeの列ヘッダーとして表されます。
* **オーディエンス メンバーシップ**: データフローにマップされたオーディエンスメンバーシップは、対応するセルの `active` エントリを介して示されます

![ 動的テーブルデータを含むSnowflake インターフェイスを示すスクリーンショット ](../../assets/catalog/cloud-storage/snowflake-batch/data-validation.png)

## 既知の制限事項 {#known-limitations}

### デフォルトの結合ポリシー制限 {#default-merge-policy-restriction}

現在、デフォルトの結合ポリシーにマッピングされたオーディエンスのみを書き出すことができます。

### 地域での提供 {#regional-availability}

[!DNL Snowflake] のバッチ宛先は、現在、Experience Platform VA7 リージョンでプロビジョニングされたReal-Time CDPのお客様のみが使用できます。

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのように データガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。
