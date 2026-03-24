---
title: Acxiom Prospect-Suppression
description: 1st パーティオーディエンスをAcxiomの宛先にエクスポートし、Acxiomが既知またはコンバージョンした顧客を除外できるようにします。 そして、Acxiom ソースコネクタを使用して、既知またはコンバージョンした顧客を削除した状態で、Acxiomから見込み客リストを取り込み、アクティベートします。
last-substantial-update: 2024-03-14T00:00:00Z
badge: label="ベータ版" type="Informative"
exl-id: d82e8cd3-970c-44af-99b0-ea154eb3655e
source-git-commit: d946d3dbb09c1fe0163fba3a892b4c0f1b331f87
workflow-type: tm+mt
source-wordcount: '1536'
ht-degree: 23%

---

# [!DNL Acxiom Prospect-Suppression]宛先接続

>[!NOTE]
>
>[!DNL Acxiom Prospect-Suppression]宛先はベータ版です。 この宛先コネクタとドキュメントページは、Acxiom チームによって作成および管理されます。 お問い合わせやアップデートのご依頼は、acxiom-adobe-help@acxiom.comまで直接お問い合わせください。

## 概要 {#overview}

[!DNL Acxiom Prospect-Suppression]を使用して、可能な限り最も生産的な見込みオーディエンスを提供します。 このコネクタは、ファーストパーティデータを[!DNL Real-Time Customer Data Platform]から安全に書き出し、受賞歴のあるハイジーンとID解決を実行して、抑制リストとして使用するデータファイルを生成します。 これは[!DNL Acxiom Global] データベースと照合され、見込み客リストをインポート用にカスタマイズできるようになります。 次に、[[!DNL Acxiom Prospecting Data Import]](/help/sources/connectors/data-partners/acxiom-prospecting-data-import.md) ソースコネクタを使用して、Acxiomから見込み客リストを[!DNL Real-Time CDP]に戻し、既知またはコンバージョン済みの顧客を削除します。

![&#x200B; マーケティングダイアグラムを使用してAcxiomに1st パーティデータを書き出し、見込み客データをReal-Time CDPに読み込む](/help/destinations/assets/catalog/data-partner/acxiom/marketing-workflow.png)

Acxiomは、パーソナライズされたエクスペリエンスの提供に重点を置いた、12,000以上のグローバルデータ属性からなる最大のカタログを提供し、業界で最もパフォーマンスの高いオーディエンスを提供します。 高品質なデータを無制限に組み合わせて、特定のキャンペーンニーズに合わせてオーディエンスを作成、配信できます。

このチュートリアルでは、[!DNL Acxiom Prospect-Suppression] ユーザーインターフェイスを使用して[!DNL Adobe Experience Platform]宛先接続とデータフローを作成する手順を説明します。 このコネクタは、Amazon S3をドロップポイントとして使用して、Acxiom見込み客サービスにデータを配信します。 Amazon S3 ドロップポイントへのファイルの書き出しを開始したら、Acxiom アカウント担当者にお問い合わせください。

![Acxiomの宛先が選択された宛先カタログ。](../../assets/catalog/data-partner/acxiom/image-destination-catalog.png)

## ユースケース {#use-cases}

[!DNL Acxiom Prospect-Suppression]宛先を使用する方法とタイミングをより理解しやすくするために、[!DNL Adobe Experience Platform]のお客様がこの宛先を使用して解決できるユースケースの例を次に示します。

### 見込み顧客データセットの抑制リストの作成 {#create-suppression-list}

アウトリーチ戦略の効果を高めたいと考えているマーケティング担当者は、多くの場合、抑制リストを作成します。 このリストには既存顧客と特定のセグメントが含まれており、ターゲットを絞ったキャンペーン中に見込み顧客の活動から除外することができます。 この戦略的アプローチは、オーディエンスを絞り込み、冗長なコミュニケーションを回避し、より集中的で効率的なマーケティング活動に貢献します。

たとえば、マーケターは、セグメンテーションと抑制基準にもとづいて、キャンペーンにターゲットを絞った見込み客プロファイルを追加することで、キャンペーンのリーチを広げることができます。

ユースケースは、宛先コネクタとソースコネクタの両方を組み合わせて実行します。

まず、この宛先コネクタを使用して既存の顧客プロファイルを書き出し、抑制ファイルとして使用します。 これにより、既存の顧客レコードが含まれなくなります。

Acxiomのサービスは、ファイルを検索して取得し、そのファイルを追加の選択条件とともに使用して、見込み客ファイルを生成します。 その後、対応する[[!DNL Acxiom Prospecting Data Import]](/help/sources/connectors/data-partners/acxiom-prospecting-data-import.md) ソースコネクタを使用して、見込み客プロファイルをAdobe [!DNL Real-Time CDP]に取り込みます。

## 前提条件 {#prerequisites}

>[!IMPORTANT]
>
>* 宛先に接続するには、**[!UICONTROL View Destinations]**&#x200B;および&#x200B;**[!UICONTROL Manage Destinations]**、**[!UICONTROL Activate Destinations]**、**[!UICONTROL View Profiles]**&#x200B;および&#x200B;**[!UICONTROL View Segments]** アクセス制御権限[が必要です。 &#x200B;](/help/access-control/home.md#permissions) [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* *ID*&#x200B;をエクスポートするには、**[!UICONTROL View Identity Graph]** [&#x200B; アクセス制御権限](/help/access-control/home.md#permissions)が必要です。<br> ![&#x200B; ワークフローで強調表示されているID名前空間を選択して、オーディエンスを宛先にアクティブ化します。](/help/destinations/assets/overview/export-identities-to-destination.png " ワークフローで強調表示されたID名前空間を選択して、オーディエンスを宛先にアクティブ化します。"){width="100" zoomable="yes"}

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスの由来 | サポートあり | 説明 |
|---------|----------|----------|
| [!DNL Segmentation Service] | ○ | Experience Platform [&#x200B; セグメント化サービス &#x200B;](../../../segmentation/home.md)を通じて生成されたオーディエンス。 |
| その他すべてのオーディエンスの生成元 | × | このカテゴリには、[!DNL Segmentation Service]を通じて生成されたオーディエンス以外のすべてのオーディエンスのオリジンが含まれます。 [様々なオーディエンスの起源](/help/segmentation/ui/audience-portal.md#customize)について読みます。 次に例を示します。 <ul><li> カスタムアップロードオーディエンス [がCSV ファイルからExperience Platformに](../../../segmentation/ui/audience-portal.md#import-audience)をインポートしました。</li><li> 類似オーディエンス， </li><li> 連合オーディエンス， </li><li> [!DNL Adobe Journey Optimizer]などの他のExperience Platform アプリで生成されたオーディエンス </li><li> その他。 </li></ul> |

{style="table-layout:auto"}




オーディエンスのデータタイプ別にサポートされるオーディエンス：

| オーディエンスのデータタイプ | サポートあり | 説明 | ユースケース |
|--------------------|-----------|-------------|-----------|
| [人物オーディエンス &#x200B;](/help/segmentation/types/people-audiences.md) | ○ | 顧客プロファイルにもとづいて、マーケティング施策の特定のグループをターゲットにすることができます。 | 買い物客やカートの放棄が多い |
| [&#x200B; アカウントオーディエンス &#x200B;](/help/segmentation/types/account-audiences.md) | × | アカウントベースドマーケティング戦略のために、特定の組織内の個人をターゲットにします。 | B2B マーケティング |
| [見込みオーディエンス &#x200B;](/help/segmentation/types/prospect-audiences.md) | × | まだ顧客ではないが、ターゲットオーディエンスと特徴を共有する個人をターゲットにします。 | サードパーティデータによる見込み顧客の開拓 |
| [&#x200B; データセットの書き出し](/help/catalog/datasets/overview.md) | × | [!DNL Adobe Experience Platform] データ レイクに保存されている構造化データのコレクション。 | レポート，データサイエンスワークフロー |

{style="table-layout:auto"}


## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
|------------------|--------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 書き出しタイプ | **[!UICONTROL Profile-based]** | [宛先のアクティベーションワークフロー](/help/destinations/ui/activate-batch-profile-destinations.md#select-attributes)のプロファイル属性選択画面で選択した目的のスキーマフィールド（例：メールアドレス、電話番号、姓）と共に、セグメントのすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL Batch]** | バッチ宛先では、ファイルが 3 時間、6 時間、8 時間、12 時間、24 時間の単位でダウンストリームプラットフォームに書き出されます。 詳しくは、[バッチ（ファイルベース）宛先](/help/destinations/destination-types.md#file-based)を参照してください。 |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

>[!IMPORTANT]
>
>宛先に接続するには、**[!UICONTROL View Destinations]**&#x200B;および&#x200B;**[!UICONTROL Manage Destinations]** [&#x200B; アクセス制御権限](/help/access-control/home.md#permissions)が必要です。 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つの節でリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

宛先に対して認証を行うには、必須フィールドに入力し、**[!UICONTROL Connect to destination]**&#x200B;を選択します。

Experience Platformでバケットにアクセスするには、次の資格情報に有効な値を指定する必要があります。

| 資格情報 | 説明 |
|---------------|----------------------------------------------------------------------------------------------------------|
| S3 アクセスキー | バケットのアクセスキーID。 この値は、[!DNL Acxiom] チームから取得できます。 |
| S3秘密鍵 | バケットの秘密鍵ID。 この値は、[!DNL Acxiom] チームから取得できます。 |
| バケット名 | これはファイルが共有されるバケットです。 この値は、[!DNL Acxiom] チームから取得できます。 |

### 新規アカウント {#new-account}

Acxiom Managed S3の新しい場所を定義するには：

![新規アカウント &#x200B;](../../assets/catalog/data-partner/acxiom/image-destination-new-account.png)

### 既存アカウント {#existing-account}

[!DNL Acxiom Prospect Suppression]宛先を使用して既に定義されているアカウントが、リストポップアップに表示されます。 選択すると、右側のパネルにアカウントの詳細が表示されます。 **[!UICONTROL Destinations]** > **[!UICONTROL Accounts]**&#x200B;に移動すると、UIから例を表示します。

![既存アカウント &#x200B;](../../assets/catalog/data-partner/acxiom/image-destination-account.png)

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

![宛先の詳細](../../assets/catalog/data-partner/acxiom/image-destination-details.png)

* **名前（必須）** – 宛先が保存される名前
* **説明** – 宛先の目的の短い説明
* **バケット名（必須）** - S3に設定されたAmazon S3 バケットの名前
* **フォルダーパス （必須）** - バケット内のサブディレクトリを使用する場合は、ルートパスを参照するためにパスを定義するか、&#39;/&#39;する必要があります。
* **ファイルの種類** – 書き出したファイルにExperience Platformで使用する形式を選択します。 現在、Acxiom処理で想定される唯一のファイルタイプはCSVです

>[!IMPORTANT]
>
>CSV オプションを選択すると、*区切り記号*、*引用符*、*エスケープ文字*、*空の値*、*ヌル値*、*圧縮形式*、*マニフェストファイルを含める* オプションが表示されます。次のドキュメントでは、これらの設定について詳しく説明しています。[書式設定](../../ui/batch-destinations-file-formatting-options.md)。

![CSV オプション &#x200B;](../../assets/catalog/data-partner/acxiom/image-destination-csv-options.png)

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の提供が完了したら、**[!UICONTROL Next]**&#x200B;を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
>
>* データをアクティブ化するには、**[!UICONTROL View Destinations]**、**[!UICONTROL Activate Destinations]**、**[!UICONTROL View Profiles]**&#x200B;および&#x200B;**[!UICONTROL View Segments]** [&#x200B; アクセス制御権限](/help/access-control/home.md#permissions)が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* *ID*&#x200B;をエクスポートするには、**[!UICONTROL View Identity Graph]** [&#x200B; アクセス制御権限](/help/access-control/home.md#permissions)が必要です。<br> ![&#x200B; ワークフローで強調表示されているID名前空間を選択して、オーディエンスを宛先にアクティブ化します。](/help/destinations/assets/overview/export-identities-to-destination.png " ワークフローで強調表示されたID名前空間を選択して、オーディエンスを宛先にアクティブ化します。"){width="100" zoomable="yes"}

この宛先に対してオーディエンスをアクティブ化する手順については、[バッチプロファイル書き出し宛先に対するオーディエンスデータのアクティブ化](/help/destinations/ui/activate-batch-profile-destinations.md)を参照してください。

### マッピングの提案 {#mapping-suggestions}

処理には名前と住所の要素が必要ですが、すべての要素が必要なわけではありませんが、可能な限り一致を成功させるのに役立ちます。  マッピングの提案は、次の表に、宛先側の属性のリストを示します。この属性は、顧客がプロファイル属性をマッピングできるAcxiom処理で使用されます。  これは、すべての要素が必要ではなく、ソース値はアカウントのニーズに依存するため、提案として扱う必要があります。

| ターゲットフィールド | Sourceの説明 |
|--------------|-------------------------------------------------------------|
| name | Experience Platformの`person.name.fullName`値。 |
| firstName | Experience Platformの`person.name.firstName`値。 |
| lastName | Experience Platformの`person.name.lastName`値。 |
| address1 | Experience Platformの`mailingAddress.street1`値。 |
| address2 | Experience Platformの`mailingAddress.street2`値。 |
| 都市 | Experience Platformの`mailingAddress.city`値。 |
| state | Experience Platformの`mailingAddress.state`値。 |
| zip | Experience Platformの`mailingAddress.postalCode`値。 |

{style="table-layout:auto"}

>[!NOTE]
>
>上記に記載されていない追加のフィールドは書き出しに含まれますが、Acxiom処理では無視されます。

## データフローのレビュー {#review-dataflow}

送信前にデータフローの概要を確認するには、レビューページを使用します

![レビュー](../../assets/catalog/data-partner/acxiom/image-destination-review.png)

## データの書き出しを検証する {#exported-data}

データが正常に書き出されたかどうかを確認するには、[!DNL Amazon S3 Storage] バケットを確認し、書き出したファイルに、期待されたプロファイルの母集団が含まれていることを確認します。

## 次の手順 {#next-steps}

このチュートリアルに従うことで、Experience Platformから[!DNL Acxiom]管理S3の場所にバッチデータを書き出すデータフローを正常に作成しました。 Acxiomの担当者に、アカウント名、ファイル名、バケットパスを連絡して、処理をセットアップできるようにする必要があります。

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのように データガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。

## その他のリソース {#additional-resources}

*Acxiom オーディエンスデータと配布：* https://www.acxiom.com/customer-data/audience-data-distribution/
