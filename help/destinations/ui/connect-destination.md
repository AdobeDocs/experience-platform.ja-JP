---
title: 新しい宛先接続の作成
type: Tutorial
description: Adobe Experience Platform で宛先に接続する方法、アラートを有効にする方法、接続した宛先に対するマーケティングアクションを設定する方法について説明します。
exl-id: 56d7799a-d1da-4727-ae79-fb2c775fe5a5
source-git-commit: ec6f055de02610e23f30051c4fed4f362e9fbc53
workflow-type: tm+mt
source-wordcount: '1280'
ht-degree: 64%

---

# 新しい宛先接続の作成

>[!IMPORTANT]
> 
>* 宛先に接続するには、**[!UICONTROL 宛先の表示]** および **[!UICONTROL 宛先の管理]**&#x200B;[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* データセットの書き出しをサポートする宛先に接続するには、**[!UICONTROL 宛先の表示]** および **[!UICONTROL データセット宛先の管理とアクティブ化]**&#x200B;[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

## 概要 {#overview}

オーディエンスデータを宛先に送信する前に、宛先プラットフォームへの接続を設定する必要があります。 この記事では、Adobe Experience Platform ユーザーインターフェイスを使用して、オーディエンスをアクティブ化したり、データセットを書き出したりできる、新しい宛先接続を設定する方法について説明します。

## カタログで目的の宛先を検索します。 {#setup}

1. **[!UICONTROL 接続]**／**[!UICONTROL 宛先]**&#x200B;に移動し、「**[!UICONTROL カタログ]**」タブを選択します。

   ![宛先カタログページを表示する Experience Platform UI のスクリーンショット。](../assets/ui/connect-destinations/catalog.png)

2. カタログ内の宛先カードには、宛先への既存の接続があるかどうか、宛先がオーディエンスのアクティブ化、データセットの書き出し、またはその両方をサポートしているかどうかに応じて、異なるアクション制御が含まれる場合があります。 宛先カードには、次のいずれかのコントロールが表示されます。

   * **[!UICONTROL 設定]**. オーディエンスをアクティブ化したりデータセットを書き出したりするには、まずこの宛先への接続を設定する必要があります。
   * **[!UICONTROL アクティブ化]**. この宛先への接続は既に設定されています。 この宛先では、オーディエンスのアクティベーションとデータセットの書き出しをサポートしています。
   * **[!UICONTROL オーディエンスをアクティブ化]**. この宛先への接続は既に設定されています。 この宛先では、オーディエンスのアクティベーションのみがサポートされます。

   これらのコントロールの違いについて詳しくは、宛先ワークスペードキュメントの[カタログ](../ui/destinations-workspace.md#catalog)の節を参照してください。

   使用可能なコントロールに応じて、**[!UICONTROL 設定]**、**[!UICONTROL アクティブ化]** または **[!UICONTROL オーディエンスをアクティブ化]** のいずれかを選択します。

   ![「設定」コントロールが強調表示された宛先カタログページを示す、Experience Platform UI のスクリーンショット。](../assets/ui/connect-destinations/set-up.png)

   ![ 「オーディエンスをアクティブ化」コントロールが強調表示された宛先カタログページを示す、Experience Platform UI のスクリーンショット。](../assets/ui/connect-destinations/activate-segments.png)

3. 「**[!UICONTROL 設定]**」を選択した場合、次の手順にスキップして、宛先に対して「[認証](#authenticate)」を行います。

   **[!UICONTROL アクティブ化]**、**[!UICONTROL オーディエンスをアクティブ化]** または **[!UICONTROL データセットを書き出し]** を選択した場合、既存の宛先接続のリストが表示されるようになります。

   「**[!UICONTROL 新しい宛先を設定]**」を選択すると、宛先への新しい接続を確立します。

   ![使用可能な宛先のリストと、「新しい宛先を設定」コントロールが強調表示されている、Experience Platform UI のスクリーンショット。](../assets/ui/connect-destinations/configure-new-destination.png)

## 宛先に対する認証 {#authenticate}

>[!CONTEXTUALHELP]
>id="platform_destinations_account_name"
>title="アカウント名"
>abstract="今後この宛先アカウントを簡単に識別するのに役立つ名前を入力します。これは、同じ宛先に対して複数の接続がある場合に特に便利です。"

宛先に接続する最初の手順は、宛先プラットフォームへの認証です。

接続先に応じて、認証する宛先パートナーのページに移動したり、Experience Platform ワークフローで直接認証資格情報を入力するように求められたりする場合があります。

新しい宛先接続を設定する際は、**[!UICONTROL アカウント名]** と、オプションで **[!UICONTROL 説明]** を指定する必要があります。 これらのフィールドは、すべての宛先で使用できます。

* **[!UICONTROL アカウント名]**：今後この宛先アカウントを簡単に識別できる名前を入力します。 これは、同じ宛先に対して複数の接続がある場合に特に便利です。
* **[!UICONTROL 説明]** （オプション）：ユーザーまたはユーザーのチームがアカウントを区別するのに役立つ、接続の目的や関連するビジネスコンテキストなどの追加の詳細を追加します。

これらのフィールドに明確で説明的な情報を提供すると、オーディエンスをアクティブ化する際に、正しい宛先アカウントを容易に管理および選択できます。

以下は、[!DNL Amazon S3] 宛先への認証に必要な入力の例です。 必要な入力に関する詳細な手順は、各宛先ドキュメントページに記載されています（例えば、[[!DNL Amazon S3]](/help/destinations/catalog/cloud-storage/amazon-s3.md#authenticate) と [[!DNL Facebook]](/help/destinations/catalog/social/facebook.md#authenticate) の認証セクションを参照）。

**[!DNL Amazon S3]の必須およびオプションの認証パラメーター**

![Amazon S3 の宛先への認証時の必須およびオプションの入力パラメーターを示す画像。](../assets/ui/connect-destinations/s3-new-acc.png)

## 接続パラメーターの設定 {#set-up-connection-parameters}

宛先への認証を既に設定している場合は、既存のアカウントを引き続き使用するか、新しいアカウントを設定できます。

接続先に応じて、異なる種類の接続パラメーターを入力するように求められる場合があります。 例えば、[!DNL Amazon S3] の宛先に接続する場合、[!DNL Amazon S3] バケット名とファイルを保存するフォルダーパスに関する詳細を指定するよう求められます。 以下は、[!DNL Amazon S3] 宛先と [!DNL Trade Desk] 宛先に必要な入力の 2 つの例です。 必要な入力に関する詳細な手順は、各宛先ドキュメントページで説明しています。

>[!IMPORTANT]
>
>以下の画像は説明用にのみ使用されています。 宛先の接続詳細は、宛先間で異なります。 宛先の接続詳細について詳しくは、各[宛先カタログ](../catalog/overview.md)ページ（[[!DNL Google Customer Match]](../catalog/advertising/google-customer-match.md#connect)、[[!DNL Trade Desk]](/help/destinations/catalog/advertising/tradedesk.md#connect) または [[!DNL Amazon S3]](/help/destinations/catalog/cloud-storage/amazon-s3.md#destination-details) など）の&#x200B;**宛先に接続**&#x200B;の節を参照してください。

**[!DNL Amazon S3]の必須およびオプションの入力パラメーター**

![Amazon S3 の宛先に接続する際の必須およびオプションの入力パラメーターを示す画像。](../assets/ui/connect-destinations/connect-destination-amazons3-example.png)

**[!DNL The Trade Desk]の必須およびオプションの入力パラメーター**

![Trade Desk 宛先に接続する際の必須およびオプションの入力パラメーターを示す画像](../assets/ui/connect-destinations/connect-destination-trade-desk-example.png)

### 書き出されたファイルのファイル形式オプションの設定 {#file-formatting-and-compression-options}

ファイルベースの宛先の場合は、書き出されたファイルの形式と圧縮の方法に関する様々な設定を指定できます。 使用可能なすべての形式および圧縮のオプションについて詳しくは、[ファイルベース宛先のファイル形式オプションの設定に関するチュートリアル](/help/destinations/ui/batch-destinations-file-formatting-options.md)を参照してください。

![ファイルタイプの選択と CSV ファイルの各種オプションを示す画像](/help/destinations/assets/ui/connect-destinations/file-formatting-options.png)

### Audience Activation、アカウントのアクティベーション、見込み客のアクティベーション、データセットの書き出しのための宛先接続の設定 {#segment-activation-or-dataset-exports}

一部のファイルベース宛先では、既知の顧客、アカウント顧客または見込み客への Audience Activation や、データセットの書き出しをサポートしています。 これらの宛先については、[ オーディエンスのアクティブ化 ](/help/destinations/ui/activate-batch-profile-destinations.md)、[ アカウント ](/help/destinations/ui/activate-account-audiences.md)、[ 見込み客 ](/help/destinations/ui/activate-prospect-audiences.md) または [ データセットの書き出し ](/help/destinations/ui/export-datasets.md) を可能にする接続を作成するかどうかを選択できます。

>[!WARNING]
>
>データセットを書き出す場合、JSON ファイルへの書き出しは、圧縮モードでのみサポートされます。 [!DNL Parquet] ファイルへの書き出しは、圧縮モードと非圧縮モードでサポートされています。

![ オーディエンスアクティベーションかデータセット書き出しを選択できるデータタイプ選択コントロールを示す画像 ](/help/destinations/assets/ui/connect-destinations/data-type-selection.png)

### 宛先アラートの有効化 {#enable-alerts}

1. （オプション）購読する宛先データフローアラートを選択します。 データフローを作成する際にアラートの配信を登録して、フロー実行のステータス、成功または失敗に関するアラートメッセージを受信できます。使用可能なアラートは、接続先の宛先タイプ（ファイルベースまたはストリーミング）によって異なります。宛先データフローアラートについて詳しくは、[コンテキスト内宛先アラートの配信登録](alerts.md)を参照してください。

   ![コンテキスト内宛先アラートの配信登録オプションがハイライト表示された新しい宛先を設定ダイアログ](../assets/ui/connect-destinations/subscribe-to-alerts.png)

2. 「**[!UICONTROL 次へ]**」を選択します。

   ![「次へ」コントロールがハイライト表示された新しい宛先を設定ダイアログ（ワークフローの次のステップに進むことができる）](../assets/ui/connect-destinations/next.png)

## マーケティングアクションの選択 {#select-marketing-actions}

1. 宛先に書き出すデータに適用できるマーケティングアクションを選択します。マーケティングアクションは、宛先にデータを書き出す目的を示します。 アドビが定義したマーケティングアクションから選択することも、独自のマーケティングアクションを作成することもできます。マーケティングアクションについて詳しくは、[データ使用ポリシーの概要](../../data-governance/policies/overview.md)ページを参照してください。

   ![使用可能なマーケティングアクションがハイライト表示された新しい宛先を設定ダイアログ（「宛先に接続」ワークフローを完了するために使用可能なコントロールもハイライト表示）](../assets/ui/connect-destinations/governance.png)

2. 「**[!UICONTROL 保存して終了]**」を選択して宛先設定を保存するか、「**[!UICONTROL 次へ]**」を選択してオーディエンスデータの[アクティベーションフロー](activation-overview.md)に進みます。

## 次の手順 {#next-steps}

このドキュメントでは、Experience Platform UI を使用して宛先への接続を確立する方法について説明しました。使用可能な接続パラメーターと必要な接続パラメーターは、宛先によって異なります。宛先タイプごとに必要な入力と使用可能なオプションについて詳しくは、[宛先カタログ](/help/destinations/catalog/overview.md)の宛先ドキュメントページも参照してください。

次は、宛先への [ オーディエンスのアクティブ化 ](/help/destinations/ui/activation-overview.md) または [ データセットの書き出し ](/help/destinations/ui/export-datasets.md) に進むことができます。