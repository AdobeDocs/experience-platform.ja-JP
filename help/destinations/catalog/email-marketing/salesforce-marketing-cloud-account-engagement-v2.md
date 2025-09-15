---
title: （V2） Salesforce Marketing Cloud アカウントのエンゲージメント
description: （V2）Salesforce Marketing Cloud Account Engagement （旧称 Pardot）の宛先を使用してプロファイルデータを書き出し、ビジネスニーズに合ったバッチ処理を使用してSalesforce Marketing Cloud Account Engagement 内でアクティブ化する方法を説明します。
badge: label="Alpha" type="Informative"
hide: true
hidefromtoc: true
source-git-commit: d1405237698271607fa672ccae1ac731d66df263
workflow-type: tm+mt
source-wordcount: '1809'
ht-degree: 20%

---

# [!DNL (V2) Salesforce Marketing Cloud Account Engagement] 接続

[[!DNL Salesforce Marketing Cloud Account Engagement]](https://www.salesforce.com/products/marketing-cloud/marketing-automation/) （旧称 [!DNL Pardot]）宛先を使用すると、Adobe Experience Platform プロファイルデータをSalesforceの B2B マーケティング自動化プラットフォームに書き出すことができます。

この統合により、Adobe Experience Platformの顧客プロファイルと [!DNL Salesforce Marketing Cloud Account Engagement] のマーケティングキャンペーンとの間でシームレスなデータ同期が可能になります。

この宛先では、[[!DNL Salesforce Import API v5]](https://developer.salesforce.com/docs/marketing/pardot/guide/import-v5.html) を使用して、バッチデータの書き出しを効率的に処理します。


>[!IMPORTANT]
> 
> これは、[Salesforce Marketing Cloud アカウントエンゲージメント ](/help/destinations/catalog/email-marketing/salesforce-marketing-cloud-account-engagement.md) の宛先の V2 バージョンです。 このバージョンは、以前の宛先に代わるものであり、現在Alpha リリースにあります。
> &#x200B;> <br>
> &#x200B;> 現在 [Salesforce Marketing Cloud Account Engagement](/help/destinations/catalog/email-marketing/salesforce-marketing-cloud-account-engagement.md) の以前のバージョンを使用している場合は、**2026 年 1 月** より前に、この V2 バージョンに移行する必要があります。 2026 年 1 月以降、Adobeは以前のバージョンを廃止し、使用できなくなります。


## ユースケース {#use-cases}

[!DNL (V2) Marketing Cloud Account Engagement] の宛先を使用する方法とタイミングをより深く理解するために、Adobe Experience Platformのお客様がこの宛先を使用して解決できるユースケースのサンプルを以下に示します。

### B2B リード管理 {#use-case-lead-management}

Adobe Experience Platformから [!DNL Salesforce Marketing Cloud Account Engagement] にリードデータを同期させて、包括的なリードの育成とスコアリングを行います。 マーケティングチームは、Experience Platformで豊富なオーディエンスプロファイルを作成し、それらを [!DNL Salesforce Marketing Cloud Account Engagement] にエクスポートして、自動化された B2B マーケティングキャンペーンを実現できます。

### キャンペーンの自動化 {#use-case-campaign-automation}

Adobe Experience Platformで定義したオーディエンスを使用して、[!DNL Salesforce Marketing Cloud Account Engagement] でマーケティングキャンペーンのトリガーを設定できます。 ターゲットオーディエンスを [!DNL Salesforce] に書き出した後、それらを使用してメールキャンペーンを実行し、育成、スコアリング、キャンペーンセグメント化を通じてリードを管理できます。

### プロファイルエンリッチメント {#use-case-profile-enrichment}

Adobe Experience Platformの豊富な顧客データを使用して、[!DNL Salesforce Marketing Cloud Account Engagement] 見込み客プロファイルを強化します。 包括的なプロファイル属性を書き出して、より詳細な見込み客レコードを [!DNL Salesforce Marketing Cloud Account Engagement] に作成し、ターゲティングとパーソナライゼーションを向上させます。

## 前提条件 {#prerequisites}

Experience Platformと [!DNL Salesforce] で設定する必要がある前提条件と、[!DNL (V2) Marketing Cloud Account Engagement] の宛先を使用する前に収集する必要がある情報については、以下の節を参照してください。

### Experience Platform の前提条件 {#prerequisites-in-experience-platform}

[!DNL (V2) Marketing Cloud Account Engagement] の宛先へのデータをアクティブ化する前に、[ スキーマ ](/help/xdm/schema/composition.md)、[ データセット ](../../../catalog/datasets/overview.md) および [ オーディエンス ](../../../segmentation/types/overview.md) を [!DNL Experience Platform] で作成する必要があります。

### [!DNL Salesforce Marketing Cloud Account Engagement] 前提条件 {#prerequisites-destination}

Experience Platformから [!DNL Marketing Cloud Account Engagement] アカウントにデータを書き出すには、次の前提条件に注意してください。

#### [!DNL Marketing Cloud Account Engagement] アカウントが必要です {#prerequisites-account}

[!DNL Marketing Cloud Account Engagement]Marketing Cloud Account Engagement[ 製品のサブスクリプションを持つ ](https://www.salesforce.com/products/marketing-cloud/marketing-automation/) アカウントは続行する必要があります。

#### [!DNL Marketing Cloud Account Engagement] 資格情報の収集 {#gather-credentials}

[!DNL (V2) Marketing Cloud Account Engagement] の宛先に対して認証を行う前に、以下の項目を書き留めてください。

| 資格情報 | 説明 |
| --- | --- |
| **[!UICONTROL アカウントエンゲージメント事業部 ID]** | [!DNL Salesforce] アカウント エンゲージメントのビジネスユニット ID。 ID を見つける方法については、Salesforce[ ドキュメント ](https://help.salesforce.com/s/articleView?id=000381973&type=1) を参照してください。 |

{style="table-layout:auto"}

## サポートされている ID {#supported-identities}

[!DNL (V2) Marketing Cloud Account Engagement] では、以下の表で説明する ID のアクティブ化をサポートしています。 [ID](/help/identity-service/features/namespaces.md) についての詳細情報。

これらの識別子のいずれかを使用して一致するものが見つかった場合、既存のアカウントエンゲージメント見込み客レコードは、Adobe Experience Platformのデータを使用して更新されます。 一致するものが見つからない場合、アカウントエンゲージメントに新しい見込み客レコードが作成されます。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| `matchId` | アカウントエンゲージメントの見込み客 ID | これら 3 つの ID のうち少なくとも 1 つが必要です |
| `matchSalesforceId` | 見込み客のSalesforce リード /連絡先 ID | これら 3 つの ID のうち少なくとも 1 つが必要です |
| `matchEmail` | 見込み客のメールアドレス | これら 3 つの ID のうち少なくとも 1 つが必要です |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL プロファイルベース]** | <ul><li>フィールドマッピングに従って、目的のスキーマフィールド *（例：メールアドレス、電話番号、姓）* と共に、オーディエンスのすべてのメンバーを書き出します。</li><li>この宛先では、Salesforce Import API v5 を使用したプロファイルデータのバッチエクスポートがサポートされています。</li></ul> |
| 書き出し頻度 | **[!UICONTROL バッチ]** | <ul><li>**初期エクスポート**：マッピング直後の完全エクスポート</li><li>**後続のエクスポート**：増分エクスポートを 3 時間ごとに行います</li><li>このスケジュールは固定されており、Alphaでカスタマイズすることはできません</li></ul> |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

>[!IMPORTANT]
>
>宛先に接続するには、**[!UICONTROL 宛先の表示]** および **[!UICONTROL 宛先の管理]**&#x200B;[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

宛先を認証するには、「**[!UICONTROL 宛先に接続]**」を選択します。

![Salesforce Marketing Cloud アカウントエンゲージメント V2 宛先接続ワークフロー ](../../assets/catalog/email-marketing/salesforce-marketing-cloud-account-engagement-v2/connect-to-destination.png "Salesforce Marketing Cloud アカウントエンゲージメント V2 宛先接続ワークフロー ")

[!DNL Salesforce] のログインページにリダイレクトされます。 [!DNL Marketing Cloud Account Engagement] アカウントの資格情報を入力し、「**[!UICONTROL ログイン]**」を選択します。

![Salesforceのログインページ ](../../assets/catalog/email-marketing/salesforce-marketing-cloud-account-engagement-v2/salesforce-auth.png "Salesforceのログインページ。")

次に、「**[!UICONTROL 許可]**」を選択して、**Adobe Experience Platform** アプリに対し、[!DNL Salesforce Marketing Cloud Account Engagement] アカウントにアクセスするための権限を付与します。 *これは 1 回だけ行う必要があります*。

![Salesforce アプリにMarketing Cloud Account Engagement へのアクセス権を付与するためのExperience Platform アプリのスクリーンショット確認ポップアップ。](../../assets/catalog/email-marketing/salesforce-marketing-cloud-account-engagement-v2/allow-app.png)

指定した詳細が有効な場合、UI で「*（V2）Salesforce Marketing Cloud アカウントエンゲージメントアカウントに正常に接続しました*」というメッセージと **[!UICONTROL 接続済み]** ステータスが緑のチェックマークと共に表示されます。

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

* **[!UICONTROL 名前]**：今後この宛先を認識するための名前。
* **[!UICONTROL 説明]**：今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL アカウントエンゲージメント事業部門 ID]**：お客様の [!DNL Salesforce] `Account Engagement Business Unit ID`。
* **[!UICONTROL Account Engagement API]**:Account Engagement API の実稼動（`https://pi.pardot.com`）エンドポイントとデモ（`https://pi.demo.pardot.com`）エンドポイントのどちらを使用するかを選択します。
* **[!UICONTROL アカウントエンゲージメントキャンペーン ID]**：すべての [!DNL Account Engagement] 見込み客は、キャンペーンに関連付ける必要があります。 キャンペーン ID を設定しない場合、Salesforce アカウントにデフォルトが存在すると、アカウントエンゲージメントは自動的にキャンペーン ID を割り当てようとします。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>* データをアクティブ化するには、**[!UICONTROL 宛先の表示]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]** および **[!UICONTROL セグメントの表示]**&#x200B;[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* *ID* を書き出すには、**[!UICONTROL ID グラフの表示]**&#x200B;[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。<br> ![ 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択します。](/help/destinations/assets/overview/export-identities-to-destination.png " 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択 "){width="100" zoomable="yes"}

この宛先に対してオーディエンスをアクティブ化する手順については、[バッチプロファイル書き出し宛先に対するオーディエンスデータのアクティブ化](/help/destinations/ui/activate-batch-profile-destinations.md)を参照してください。

### マッピングの考慮事項と例 {#mapping-considerations-example}

Adobe Experience Platformから [!DNL (V2) Marketing Cloud Account Engagement] の宛先にオーディエンスデータを送信するには、エクスペリエンスデータモデル（XDM）スキーマフィールドを、宛先の対応するフィールドにマッピングする必要があります。

サポートされているフィールドの一覧については、[Salesforce見込み客 API v5 ドキュメント ](https://developer.salesforce.com/docs/marketing/pardot/guide/prospect-v5.html) を参照してください。 [ カスタムフィールド ](https://developer.salesforce.com/docs/marketing/pardot/guide/custom-field-v5.html) は、Alpha リリースではサポートされていません。

#### サポートされる属性 {#supported-attributes}

Salesforce Marketing Cloud アカウントエンゲージメントの宛先では、以下の表に示すターゲット属性をサポートしています。

| 属性 | タイプ | 説明 |
|---------|----------|----------|
| `salesforceId` | 文字列 | 見込顧客のSalesforce ID |
| `salesforceOwnerId` | 整数 | 見込み客オーナーのSalesforce ユーザー ID |
| `salutation` | 文字列 | 見込み客の敬称（例、Mr.、Ms.、Dr.） |
| `score` | 整数 | アカウントエンゲージメントにおける見込み客のスコア |
| `source` | 文字列 | 見込顧客レコードのソース |
| `state` | 文字列 | 見込顧客の都道府県 |
| `territory` | 文字列 | 見込顧客に割り当てられたテリトリー |
| `userId` | 整数 | 見込顧客に関連付けられたユーザー ID |
| `website` | 文字列 | 見込み客の Web サイトの URL |
| `yearsInBusiness` | 文字列 | 見込み客が事業を開始した年数 |
| `zip` | 文字列 | 見込顧客の郵便番号 |

#### 必須のマッピング {#required-mappings}

データのマッピングを開始する前に、以下の必須フィールドマッピングを確認してください。

| ターゲットフィールド | タイプ | 必須 | 使用するタイミング |
|---|---|---|---|
| `email` | 属性 | 常に必須 | 見込み客のメールアドレス。 これは、`matchId` または `matchSalesforceId` がない場合にアカウントエンゲージメントで見込み客レコードを見つけて照合するためのプライマリ ID です。<br> **注意：** アカウントエンゲージメントの「同じメールアドレスで複数の見込み客を許可」機能を使用すると、同じメールで複数の見込み客がある場合、メールのみに依存していると曖昧さが生じる可能性があります。 このような場合、アカウントエンゲージメントは、通常、見込み客を最新のアクティビティで更新するようにデフォルトで設定されます。 |
| `matchId` | ID | これら 3 つの ID のうち少なくとも 1 つが必要です | 個々の見込み客レコードのアカウントエンゲージメントによって生成された一意の ID。 アカウントエンゲージメント見込み客 ID が既にある場合、特に複数の見込み客が同じメールアドレスを共有している場合に、正しい見込み客に更新を適用するには、この機能を使用します。 |
| `matchSalesforceId` | ID | これら 3 つの ID のうち少なくとも 1 つが必要です | Salesforceのリードまたは連絡先のSalesforce ID。 見込み客がすでにSalesforceと同期されている場合に、アカウントエンゲージメントとSalesforceの間でデータの一貫性を維持するために使用します。 |
| `matchEmail` | ID | これら 3 つの ID のうち少なくとも 1 つが必要です | 照合に使用する見込み客の E メールアドレス。 特定のアカウントエンゲージメント見込み客 ID またはSalesforce ID がない場合は、これを代替識別子として使用します。 メモ：複数の見込み客が同じメールアドレスを共有する場合、アカウントエンゲージメントは通常、デフォルトで最新のアクティビティで見込み客を更新します。 |

正しいフィールドをマッピングするには、次の手順に従います。

1. **[!UICONTROL マッピング]**&#x200B;手順で、「**[!UICONTROL 新しいマッピングを追加]**」を選択します。画面に新しいマッピング行が表示されます。
1. **[!UICONTROL ソースフィールドを選択]** ウィンドウで、**[!UICONTROL 属性を選択]** カテゴリを選択して XDM 属性を選択するか、**[!UICONTROL ID 名前空間を選択]** を選択して ID を選択します。
1. **[!UICONTROL ターゲットフィールドを選択]** ウィンドウで、**[!UICONTROL ID 名前空間を選択]** を選択し、ID を選択するか、**[!UICONTROL カスタム属性を選択]** カテゴリを選択して、標準のアカウントエンゲージメント見込み客フィールドのリストから指定します。

![XDM フィールドおよび ID のSalesforce Marketing Cloud アカウントエンゲージメント V2 フィールドへのマッピング ](../../assets/catalog/email-marketing/salesforce-marketing-cloud-account-engagement-v2/mapping.png "XDM フィールドおよび ID のSalesforce Marketing Cloud アカウントエンゲージメント V2 フィールドへのマッピングの例 ")

## データの書き出しを検証する {#exported-data}

宛先が正しく設定されていることを検証するには、次の手順に従います。

1. 選択したオーディエンスの 1 つに移動します。 「**[!DNL Activation data]**」タブを選択します。**[!UICONTROL マッピング ID]** 列には、[!DNL Marketing Cloud Account Engagement Prospects] ページ内で生成されたカスタムフィールドの名前が表示されます。
   ![ 選択したセグメントのマッピング ID を示すExperience Platform UI のスクリーンショットの例。](../../assets/catalog/email-marketing/salesforce-marketing-cloud-account-engagement-v2/selected-segment-mapping-id.png)

1. [[!DNL Salesforce]](https://login.salesforce.com/) web サイトにログインします。 次に、**[!DNL Account Engagement]** / **[!DNL Prospects]** / **[!DNL Pardot Prospects]** ページに移動し、オーディエンスの見込み客が追加または更新されたかどうかを確認します。 または、[[!DNL Account Engagement]](https://pi.pardot.com/) にアクセスして **[!DNL Prospects]** ページにアクセスすることもできます。
   ![ 見込み客ページを示すSalesforce UI のスクリーンショット。](../../assets/catalog/email-marketing/salesforce-marketing-cloud-account-engagement-v2/prospects.png)

1. 見込み客が更新されたかどうかを確認するには、見込み客を選択し、カスタム見込み客フィールドがExperience Platform オーディエンスステータスで更新されたかどうかを確認します。
   選択した見込み客ページを示す ![Salesforce UI のスクリーンショット。カスタム見込み客フィールドがオーディエンスステータスで更新されます。](../../assets/catalog/email-marketing/salesforce-marketing-cloud-account-engagement-v2/prospect.png)

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのようにデータガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。

## その他のリソース {#additional-resources}

* [!DNL Marketing Cloud Account Engagement] [API ドキュメント ](https://developer.salesforce.com/docs/marketing/pardot/guide/overview.html)
* [Salesforce読み込み API v5 ドキュメント ](https://developer.salesforce.com/docs/marketing/pardot/guide/import-v5.html)
* [Salesforce見込み客 API v5 ドキュメント ](https://developer.salesforce.com/docs/marketing/pardot/guide/prospect-v5.html)