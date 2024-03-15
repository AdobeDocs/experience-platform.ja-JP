---
title: Acxiom Prospect-Suppression
description: ファーストパーティオーディエンスを Acxiom の宛先に書き出して、Acxiom が既知の顧客やコンバージョンされた顧客を抑制できるようにします。 次に、Acxiom ソースコネクタを使用して、既知の顧客または変換済みの顧客を削除し、Acxiom から見込み客リストを取り込み、有効化します。
last-substantial-update: 2024-03-14T00:00:00Z
badge: ベータ版
source-git-commit: c35eec2b83f92a7fb165bad13213ec50a6c9863e
workflow-type: tm+mt
source-wordcount: '1466'
ht-degree: 26%

---

# [!DNL Acxiom Prospect-Suppression] 宛先接続

>[!NOTE]
>
>The [!DNL Acxiom Prospect-Suppression] の宛先はベータ版です。 この宛先コネクタとドキュメントページは、Acxiom チームが作成および管理します。 お問い合わせや更新のご依頼は、acxiom-adobe-help@acxiom.comから直接お問い合わせください。

## 概要 {#overview}

用途 [!DNL Acxiom Prospect-Suppression] 可能な限り生産的な見込み客を提供する このコネクタは、Real-time Customer Data Platformからファーストパーティデータを安全に書き出し、受賞歴のある衛生および ID 解決を通じて実行します。この解決策は、抑制リストとして使用するデータファイルを生成します。 これは、 [!DNL Acxiom Global] 見込み客リストをインポート用にカスタマイズできるデータベース。 次に、 [[!DNL Acxiom Prospecting Data Import]](/help/sources/connectors/data-partners/acxiom-prospecting-data-import.md) 既知の顧客またはコンバージョン済みの顧客を削除し、ソースコネクタから見込み客リストに Acxiom からReal-Time CDPに戻ります。

![ファーストパーティデータを Acxiom に書き出し、見込み客データをReal-Time CDPに再度読み込むマーケティング図](/help/destinations/assets/catalog/data-partner/acxiom/marketing-workflow.png)

Acxiom は、12,000 を超えるグローバルデータ属性を持つ業界最高のパフォーマンスを誇るオーディエンスを提供し、パーソナライズされたエクスペリエンスの提供に特に焦点を当てています。 高品質データの無制限の組み合わせをタップして、特定のキャンペーンのニーズに合わせてオーディエンスを作成および配信します。

このチュートリアルでは、 [!DNL Acxiom Prospect-Suppression] Adobe Experience Platformユーザーインターフェイスを使用した宛先接続とデータフロー。 このコネクタは、Amazon S3 をドロップポイントとして使用して、Acxiom 見込み客サービスにデータを配信するために使用されます。 Amazon S3 ドロップポイントへのファイルのエクスポートを開始したら、Acxiom アカウント担当者に問い合わせてください。

![Acxiom の宛先が選択された宛先カタログ。](../../assets/catalog/data-partner/acxiom/image-destination-catalog.png)

## ユースケース {#use-cases}

をいつどのように使用するかをより深く理解するのに役立ちます。 [!DNL Acxiom Prospect-Suppression] の宛先について、Adobe Experience Platformのお客様がこの宛先を使用して解決できる使用例を以下に示します。

### 見込みデータセット用の抑制リストの作成 {#create-suppression-list}

マーケティングの専門家は、アウトリーチ戦略の効果を高めるために、しばしば抑制リストの作成を採用しています。 このリストには、既存の顧客と特定のセグメントが含まれ、ターゲットキャンペーン中の予測アクティビティから確実に除外されます。 この戦略的アプローチは、オーディエンスを絞り込み、冗長なコミュニケーションを回避し、より焦点を絞った効率的なマーケティング活動に貢献します。

例えば、マーケターは、指定したセグメント化と抑制の条件に基づいて、ターゲット化された見込み客プロファイルをキャンペーンに追加し、キャンペーンの範囲を広げたい場合があります。

このユースケースは、宛先コネクタとソースコネクタの両方を組み合わせて実行します。

最初に、この宛先コネクタを使用して既存の顧客プロファイルを書き出し、抑制ファイルとして使用します。 これにより、既存の顧客レコードは含まれなくなります。

Acxiom のサービスは、ファイルを検索し、取得して追加の選択条件と共に使用し、見込み客ファイルを生成します。 次に、対応する [[!DNL Acxiom Prospecting Data Import]](/help/sources/connectors/data-partners/acxiom-prospecting-data-import.md) 見込み客プロファイルをAdobe Real-Time CDPに取り込むためのソースコネクタ。

## 前提条件 {#prerequisites}

>[!IMPORTANT]
>
>* 宛先に接続するには、 **[!UICONTROL 宛先の表示]** および **[!UICONTROL 宛先の管理]**, **[!UICONTROL 宛先のアクティブ化]**, **[!UICONTROL プロファイルの表示]**、および **[!UICONTROL セグメントを表示]** [アクセス制御権限](/help/access-control/home.md#permissions). [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* 書き出す *id*、 **[!UICONTROL ID グラフを表示]** [アクセス制御権限](/help/access-control/home.md#permissions). <br> ![ワークフローでハイライト表示された ID 名前空間を選択して、宛先に対するオーディエンスをアクティブ化します。](/help/destinations/assets/overview/export-identities-to-destination.png "ワークフローでハイライト表示された ID 名前空間を選択して、宛先に対するオーディエンスをアクティブ化します。"){width="100" zoomable="yes"}

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスの起源 | サポートあり | 説明 |
|-----------------------------|-----------|---------------------------------------------------------------------------------------------------------------------|
| [!DNL Segmentation Service] | ✓ | Experience Platform [セグメント化サービス](../../../segmentation/home.md). |
| カスタムアップロード | x | CSV ファイルから Experience Platform に[読み込まれた](../../../segmentation/ui/overview.md#import-audience)オーディエンス。 |

{style="table-layout:auto"}


## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
|------------------|--------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 書き出しタイプ | **[!UICONTROL プロファイルベース]** | [宛先のアクティベーションワークフロー](/help/destinations/ui/activate-batch-profile-destinations.md#select-attributes)のプロファイル属性選択画面で選択した目的のスキーマフィールド（例：メールアドレス、電話番号、姓）と共に、セグメントのすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL バッチ]** | バッチ宛先では、ファイルが 3 時間、6 時間、8 時間、12 時間、24 時間の単位でダウンストリームプラットフォームに書き出されます。 詳しくは、[バッチ（ファイルベース）宛先](/help/destinations/destination-types.md#file-based)を参照してください。 |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、 **[!UICONTROL 宛先の表示]** および **[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions). 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つの節でリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

宛先に対して認証するには、必須フィールドに入力し、「**[!UICONTROL 宛先に接続]**」を選択します。

Experience Platformでバケットにアクセスするには、次の資格情報に有効な値を指定する必要があります。

| 資格情報 | 説明 |
|---------------|----------------------------------------------------------------------------------------------------------|
| S3 アクセスキー | バケットのアクセスキー ID。 この値は、 [!DNL Acxiom] チーム。 |
| S3 秘密鍵 | バケットの秘密鍵 ID。 この値は、 [!DNL Acxiom] チーム。 |
| バケット名 | これはファイルが共有されるグループです。 この値は、 [!DNL Acxiom] チーム。 |

### 新しいアカウント

新しい Acxiom Managed S3 の場所を定義するには：

![新しいアカウント](../../assets/catalog/data-partner/acxiom/image-destination-new-account.png)

### 既存のアカウント

アカウントは、 [!DNL Acxiom Prospect Suppression] 宛先がリストポップアップに表示されます。 選択すると、右側のパネルにアカウントの詳細が表示されます。 に移動したときに UI から例を表示します。 **[!UICONTROL 宛先]** > **[!UICONTROL アカウント]**:

![既存のアカウント](../../assets/catalog/data-partner/acxiom/image-destination-account.png)

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

![宛先の詳細](../../assets/catalog/data-partner/acxiom/image-destination-details.png)

* **名前（必須）**  — 宛先が保存される名前
* **説明** ・目的の簡単な説明
* **バケット名（必須）** - S3 で設定されているAmazon S3 バケットの名前
* **フォルダーパス（必須）**  — バケット内のサブディレクトリを使用する場合は、パスを定義する必要があります。または、ルートパスを参照するために「/」を定義する必要があります。
* **ファイルタイプ**  — 書き出すファイルに使用する形式Experience Platformを選択します。 現在、Acxiom 処理で必要なファイルタイプは CSV のみです

>[!IMPORTANT]
>
>「 CSV 」オプションを選択する場合、 *区切り*, *引用符文字*, *エスケープ文字*, *空の値*, *Null 値*, *圧縮形式*、および *マニフェストファイルを含める* オプションが表示されます。次のドキュメントでは、これらの設定の詳細を説明します。 [書式設定オプションの設定](../../ui/batch-destinations-file-formatting-options.md).

![CSV オプション](../../assets/catalog/data-partner/acxiom/image-destination-csv-options.png)

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
>
>* データをアクティブ化するには、 **[!UICONTROL 宛先の表示]**, **[!UICONTROL 宛先のアクティブ化]**, **[!UICONTROL プロファイルの表示]**、および **[!UICONTROL セグメントを表示]** [アクセス制御権限](/help/access-control/home.md#permissions). [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* 書き出す *id*、 **[!UICONTROL ID グラフを表示]** [アクセス制御権限](/help/access-control/home.md#permissions). <br> ![ワークフローでハイライト表示された ID 名前空間を選択して、宛先に対するオーディエンスをアクティブ化します。](/help/destinations/assets/overview/export-identities-to-destination.png "ワークフローでハイライト表示された ID 名前空間を選択して、宛先に対するオーディエンスをアクティブ化します。"){width="100" zoomable="yes"}

この宛先に対してオーディエンスをアクティブ化する手順については、[バッチプロファイル書き出し宛先に対するオーディエンスデータのアクティブ化](/help/destinations/ui/activate-batch-profile-destinations.md)を参照してください。

### マッピング候補

処理には名前とアドレスの要素が必要ですが、すべての要素をできるだけ提供する必要はありません。これにより、照合が正常におこなわれます。  マッピングの候補は、Acxiom 処理でプロファイル属性のマッピング先となる属性のリストを次の表に示します。  すべての要素が必要とは限らず、ソースの値はアカウントのニーズに応じて異なるので、これを提案として扱う必要があります。

| ターゲットフィールド | ソースの説明 |
|--------------|-------------------------------------------------------------|
| name | The `person.name.fullName` Experience Platformの値。 |
| firstName | The `person.name.firstName` Experience Platformの値。 |
| lastName | The `person.name.lastName` Experience Platformの値。 |
| address1 | The `mailingAddress.street1` Experience Platformの値。 |
| address2 | The `mailingAddress.street2` Experience Platformの値。 |
| 都市 | The `mailingAddress.city` Experience Platformの値。 |
| state | The `mailingAddress.state` Experience Platformの値。 |
| 郵便番号 | The `mailingAddress.postalCode` Experience Platformの値。 |

{style="table-layout:auto"}

>[!NOTE]
>
>上記のリストにない追加のフィールドは書き出しに含まれますが、Acxiom の処理では無視されます。

## データフローのレビュー

レビューページを使用して、送信前のデータフローの概要を確認します。

![レビュー](../../assets/catalog/data-partner/acxiom/image-destination-review.png)

## データの書き出しを検証する {#exported-data}

データが正常に書き出されたかどうかを確認するには、[!DNL Amazon S3 Storage] バケットを確認し、書き出したファイルに、期待されたプロファイルの母集団が含まれていることを確認します。

## 次の手順

このチュートリアルに従うことで、バッチデータをExperience Platformからに書き出すデータフローを正常に作成しました [!DNL Acxiom] 管理された S3 の場所。 処理を設定できるよう、アカウント名、ファイル名、およびバケットパスを Acxiom 担当者に問い合わせる必要があります。

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのように データガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。

## その他のリソース {#additional-resources}

*Acxiom オーディエンスのデータと配信：* https://www.acxiom.com/customer-data/audience-data-distribution/
