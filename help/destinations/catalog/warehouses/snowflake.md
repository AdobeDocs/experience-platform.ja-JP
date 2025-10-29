---
title: Snowflake ストリーミング接続
description: ライブ Snowflake データ共有を作成して、ストリーミングオーディエンスの更新を共有テーブルとして直接アカウントに受け取ります。
last-substantial-update: 2025-10-23T00:00:00Z
badgeUltimate: label="Ultimate" type="Positive"
exl-id: 4a00e46a-dedb-4dd3-b496-b0f4185ea9b0
source-git-commit: 1b507e9846a74b7ac2d046c89fd7c27a818035ba
workflow-type: tm+mt
source-wordcount: '1466'
ht-degree: 22%

---

# Snowflake ストリーミング接続 {#snowflake-destination}

>[!AVAILABILITY]
>
>この宛先コネクタは限定提供で、[VA7 リージョン &#x200B;](/help/landing/multi-cloud.md#azure-regions) でプロビジョニングされたReal-Time CDP Ultimateのお客様のみが利用できます。

## 概要 {#overview}

Snowflake宛先コネクタを使用して、AdobeのSnowflake インスタンスにデータを書き出します。このインスタンスは、Adobeが [&#x200B; 非公開リスト &#x200B;](https://other-docs.snowflake.com/en/collaboration/collaboration-listings-about) を通じてお使いのインスタンスと共有します。

Snowflakeの宛先の仕組みと、AdobeとSnowflakeの間でのデータの転送方法については、次の節を参照してください。

### Snowflake データ共有の仕組み {#data-sharing}

この宛先は、[!DNL Snowflake] データ共有を使用します。つまり、データが物理的に書き出されたり、独自のSnowflake インスタンスに転送されたりすることはありません。 代わりに、Adobeでは、Adobe Snowflake環境内でホストされるライブテーブルへの読み取り専用アクセスが許可されます。 この共有テーブルに対してSnowflake アカウントから直接クエリを実行できますが、テーブルを所有しておらず、指定した保持期間を超えてテーブルを変更または保持することはできません。 Adobeは、共有テーブルのライフサイクルと構造を完全に管理します。

AdobeのSnowflake インスタンスから初めて自分のインスタンスにデータを共有する際に、Adobeからの非公開リストを受け入れるように求められます。

![Snowflake プライベートリストの承認画面を示すスクリーンショット &#x200B;](../../assets/catalog/cloud-storage/snowflake/snowflake-accept-listing.png)

### データ保持と有効期間（TTL） {#ttl}

この統合を通じて共有されるすべてのデータでは、7 日間の固定有効期限（TTL）が設定されています。 最後の書き出しから 7 日後に、データフローがまだアクティブであるかどうかに関係なく、共有テーブルは自動的に期限切れになり、アクセスできなくなります。 7 日を超えてデータを保持する必要がある場合、TTL の有効期限が切れる前に、内容を独自のSnowflake インスタンスで所有するテーブルにコピーする必要があります。

### オーディエンスの更新動作 {#audience-update-behavior}

オーディエンスが [&#x200B; バッチモード &#x200B;](../../../segmentation/methods/batch-segmentation.md) で評価される場合、共有テーブルのデータは 24 時間ごとに更新されます。 つまり、オーディエンスメンバーシップの変更と共有テーブルに変更が反映される間に、最大 24 時間の遅延が発生する可能性があります。

### 増分エクスポートロジック {#incremental-export}

データフローが初めてオーディエンスに対して実行されると、バックフィルが実行され、現在認定されているすべてのプロファイルが共有されます。 この最初のバックフィルの後は、増分更新のみが共有テーブルに反映されます。 つまり、オーディエンスに追加されたプロファイルまたはオーディエンスから削除されたプロファイルです。 このアプローチにより、効率的な更新が保証され、共有テーブルが最新の状態に保たれます。

## ストリーミングとバッチデータ共有 {#batch-vs-streaming}

Experience Platformには、[Snowflake ストリーミングと &#x200B;](snowflake.md)2&rbrace;Snowflake バッチ [&#x200B; の 2 種類のSnowflake宛先が用意されています。](snowflake-batch.md)

次の表に、各データ共有方法が最も適したシナリオを概説することで、使用する宛先を決定するのに役立ちます。

|  | 必要に応じて [0&rbrace;Snowflake バッチ &rbrace; を選択](snowflake-batch.md) | 必要に応じて [0&rbrace;Snowflake ストリーミング &rbrace; を選択](snowflake.md) |
|--------|-------------------|----------------------|
| **更新頻度** | 定期的なスナップショット | リアルタイムでの継続的な更新 |
| **データ表示** | 以前のデータを置き換えるオーディエンススナップショットの完了 | プロファイルの変更に基づいた増分更新 |
| **ユースケースのフォーカス** | レイテンシが重視されない分析/ML ワークロード | リアルタイムの更新が必要な緊急のアクションシナリオ |
| **データ管理** | 最新の完全なスナップショットを常に表示 | オーディエンスメンバーシップの変更に基づく増分更新 |
| **シナリオの例** | ビジネスレポート、データ分析、ML モデルトレーニング | マーケティングキャンペーンの抑制、リアルタイムパーソナライゼーション |

バッチデータ共有について詳しくは、[Snowflake Batch connection](snowflake-batch.md) ドキュメントを参照してください。

## ユースケース {#use-cases}

ストリーミングデータ共有は、プロファイルがメンバーシップまたはその他の属性を変更したときに直ちに更新する必要があるシナリオに最適です。 これは、次のような、リアルタイムの応答性が必要なユースケースで重要です。

* **マーケティングキャンペーン抑制**：サービスへの新規登録や購入など、特定のアクションを実行したユーザーのマーケティングキャンペーンを直ちに抑制します
* **リアルタイムパーソナライゼーション**：ユーザーが web サイトを訪問したり、製品ページを表示したり、買い物かごに項目を追加したりした場合などに、プロファイル属性が変更されると、即座にユーザーエクスペリエンスが更新されます
* **即時のアクションシナリオ**：リアルタイムデータに基づいてクイック抑制とリターゲティングを実行して遅延を減らし、マーケティングキャンペーンをより関連性が高くタイムリーにします
* **効率とニュアンス**：ユーザーの行動の変化に迅速に対応できるので、マーケティング活動の効率とニュアンスを高めることができます
* **Real-time customer journey optimization**：セグメントメンバーシップやプロファイル属性が変更された場合は、即座にカスタマーエクスペリエンスを更新します

ストリーミングデータ共有は、セグメントの変更、ID マップの変更、属性の変更に基づいて継続的な更新を提供するので、待ち時間が重要で即時の更新が必要なシナリオに適しています。

## 前提条件 {#prerequisites}

Snowflake接続を設定する前に、次の前提条件を満たしていることを確認してください。

* [!DNL Snowflake] アカウントにアクセスできます。
* お使いのSnowflake アカウントは非公開リストに登録されています。 自分または社内でSnowflakeに対するアカウント管理者権限を持つユーザーがこの機能を設定できます。

必要な権限について詳しくは、[[!DNL Snowflake]  ドキュメント &#x200B;](https://docs.snowflake.com/en/collaboration/consumer-listings-access#access-a-private-listing) を参照してください。

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。 以下の 2 つの表は、このコネクタがサポートするオーディエンスを、_オーディエンスオリジン_ および _オーディエンスに含まれるプロファイルタイプ_ 別に示しています。

| オーディエンスオリジン | サポートあり | 説明 |
|---------|----------|----------|
| [!DNL Segmentation Service] | ✓ | Experience Platform [&#x200B; セグメント化サービス &#x200B;](../../../segmentation/home.md) を通じて生成されたオーディエンス。 |
| その他すべてのオーディエンスの接触チャネル | ✓ | このカテゴリには、[!DNL Segmentation Service] を通じて生成されたオーディエンス以外のすべてのオーディエンスの接触チャネルが含まれます。 [&#x200B; 様々なオーディエンスのオリジン &#x200B;](/help/segmentation/ui/audience-portal.md#customize) について確認する。 次に例を示します。 <ul><li> csv ファイルからExperience Platformへのカスタムアップロードオーディエンス [&#x200B; 読み込み &#x200B;](../../../segmentation/ui/audience-portal.md#import-audience)</li><li> 類似オーディエンス、 </li><li> 連合オーディエンス、 </li><li> Adobe Journey Optimizerなど、他のExperience Platform アプリで生成されたオーディエンス。 </li><li> その他。 </li></ul> |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
|---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL Audience export]** | [!DNL Snowflake] 宛先で使用される識別子（氏名、電話番号など）を使用して、オーディエンスのすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL Streaming]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。詳しくは、[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)を参照してください。 |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL View Destinations]** および **[!UICONTROL Manage Destinations]**&#x200B;[&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

宛先を認証するには、「**[!UICONTROL Connect to destination]**」を選択します。

![&#x200B; 宛先への認証方法を示すサンプルスクリーンショット &#x200B;](../../assets/catalog/cloud-storage/snowflake/authenticate-destination.png)

### 宛先の詳細の入力 {#destination-details}

>[!CONTEXTUALHELP]
>id="platform_destinations_snowflake_accountID"
>title="Snowflake アカウント ID を入力"
>abstract="アカウントが組織にリンクされている場合は、`OrganizationName.AccountName`<br><br> という形式を使用します。アカウントが組織にリンクされていない場合は、`AccountName` という形式を使用します"

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

![&#x200B; 宛先の詳細を入力する方法を示すサンプルスクリーンショット &#x200B;](../../assets/catalog/cloud-storage/snowflake/configure-destination-details.png)

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
>* データをアクティブ化するには、**[!UICONTROL View Destinations]**、**[!UICONTROL Activate Destinations]**、**[!UICONTROL View Profiles]**、**[!UICONTROL View Segments]** [&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* *ID* を書き出すには、**[!UICONTROL View Identity Graph]** [&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。<br> ![&#x200B; 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択します。](/help/destinations/assets/overview/export-identities-to-destination.png " 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択 "){width="100" zoomable="yes"}

この宛先にオーディエンスをアクティベートする手順は、[ストリーミングオーディエンスの書き出し宛先へのプロファイルとオーディエンスのアクティベート](/help/destinations/ui/activate-segment-streaming-destinations.md)を参照してください。

### 属性のマッピング {#map}

Snowflakeの宛先では、プロファイル属性とカスタム属性とのマッピングをサポートしています。

![Experience Platformの宛先のマッピング画面を示すSnowflake ユーザーインターフェイス画像。](../../assets/catalog/cloud-storage/snowflake/mapping.png)

ターゲット属性は、「**[!UICONTROL Attribute name]**」フィールドに指定した属性名を使用して、Snowflakeで自動的に作成されます。

## 書き出されたデータ／データ書き出しの検証 {#exported-data}

データが正しく書き出されたことをSnowflake アカウントで確認します。

## 既知の制限事項 {#known-limitations}

### デフォルトの結合ポリシー制限 {#default-merge-policy-restriction}

現在、デフォルトの結合ポリシーにマッピングされたオーディエンスのみを書き出すことができます。

### 地域での提供 {#regional-availability}

[!DNL Snowflake] ストリーミング先は現在、Experience Platform VA7 リージョンでプロビジョニングされたReal-Time CDPのお客様のみが利用できます。

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのように データガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。
