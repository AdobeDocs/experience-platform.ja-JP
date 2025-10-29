---
title: Mailchimp タグ
description: Mailchimp タグの宛先を使用すると、アカウントデータを書き出し、Mailchimp 内でアクティブ化して、連絡先と関わり合うことができます。
last-substantial-update: 2024-02-20T00:00:00Z
exl-id: 0f278ca8-4fcf-4c47-b538-9cffa45a3d90
source-git-commit: 1b507e9846a74b7ac2d046c89fd7c27a818035ba
workflow-type: tm+mt
source-wordcount: '1599'
ht-degree: 24%

---

# [!DNL Mailchimp Tags] 接続

[[!DNL Mailchimp]](https://mailchimp.com) *（[!DNL Intuit Mailchimp] とも呼ばれます）* は、企業がメーリングリストやメールマーケティングキャンペーンを使用して *（クライアント、顧客、その他の興味のある関係者）の連絡先を管理し、連絡を取るために使用する* 人気のあるマーケティング自動化プラットフォームおよびメールマーケティングサービスです。

[!DNL Mailchimp Tags] では [audiences](https://mailchimp.com/help/getting-started-audience/) および [tags](https://mailchimp.com/help/getting-started-tags/) を使用して連絡先情報を管理します。 タグは、連絡先を整理し、[!DNL Mailchimp] 内の内部分類に合わせてラベルを付けることができるラベルです。

興味や好みに基づいて連絡先を並べ替えるために使用する [!DNL Mailchimp Interest Categories] と比較して、[!DNL Mailchimp Tags] は、連絡先が興味を持つ可能性のある興味のあるトピックの購読を管理することを目的としています。 *Experience Platformには [!DNL Mailchimp Interest Categories] 用の接続もあります。[[!DNL Mailchimp Interest Categories]](/help/destinations/catalog/email-marketing/mailchimp-interest-categories.md) のページで確認できます*。

この [!DNL Adobe Experience Platform][&#x200B; 宛先 &#x200B;](/help/destinations/home.md) は [[!DNL Mailchimp batch subscribe or unsubscribe API]](https://mailchimp.com/developer/marketing/api/lists/batch-subscribe-or-unsubscribe/) エンドポイントを活用します。 新しいオーディエンス内でアクティブ化した後に **既存のオーディエンス内で** 新しい連絡先を追加 **または [!DNL Mailchimp] 既存の** しい連絡先のタグを更新 [!DNL Mailchimp] できます。 [!DNL Mailchimp Tags] は、Experience Platformから選択したオーディエンス名を [!DNL Mailchimp] 内のタグ名として使用します。

## ユースケース {#use-cases}

[!DNL Mailchimp Tags] 宛先を使用する方法とタイミングを理解しやすくするために、Adobe Experience Platform のお客様がこの宛先を使用して解決できるユースケースのサンプルを以下に示します。

### マーケティングキャンペーンの連絡先へのメールの送信 {#use-case-send-emails}

組織の販売部門が、キュレーションされた連絡先リストにメールベースのマーケティングキャンペーンをブロードキャストしたいと考えています。 連絡先リストは、様々なオフラインソースからバッチで受信されるので、追跡する必要があります。 チームは既存の [!DNL Mailchimp] オーディエンスを特定し、各リストの連絡先が追加されるExperience Platform オーディエンスの構築を開始します。 これらのオーディエンスを [!DNL Mailchimp Tags] に送信した後、選択した [!DNL Mailchimp] オーディエンスに連絡先が存在しない場合、連絡先が属するオーディエンス名を含む関連付けられたタグが追加されます。 [!DNL Mailchimp] オーディエンスに既に連絡先が存在する場合は、オーディエンスと同じ名前の新しいタグが追加されます。 でラベルが表示されるので、オフラインソース [!DNL Mailchimp] 簡単に識別できます。 データがに送信された後、オーディエンス [!DNL Mailchimp] マーケティングキャンペーンメールを送信します。

## 前提条件 {#prerequisites}

Experience Platformと [!DNL Mailchimp] で設定する必要がある前提条件と、[!DNL Mailchimp Tags] の宛先を使用する前に収集する必要がある情報については、以下の節を参照してください。

### Experience Platformの前提条件 {#prerequisites-in-experience-platform}

[!DNL Mailchimp Tags] の宛先へのデータをアクティブ化する前に、[&#x200B; スキーマ &#x200B;](/help/xdm/schema/composition.md)、[&#x200B; データセット &#x200B;](https://experienceleague.adobe.com/docs/platform-learn/tutorials/data-ingestion/create-datasets-and-ingest-data.html?lang=ja) および [&#x200B; オーディエンス &#x200B;](https://experienceleague.adobe.com/docs/platform-learn/tutorials/audiences/create-audiences.html?lang=ja) を [!DNL Experience Platform] で作成する必要があります。

### [!DNL Mailchimp Tags] の宛先の前提条件 {#prerequisites-destination}

Experience Platformから [!DNL Mailchimp Tags] アカウントにデータを書き出すには、次の前提条件に注意してください。

#### [!DNL Mailchimp] アカウントが必要です {#prerequisites-account}

[!DNL Mailchimp Tags] の宛先を作成する前に、まず [!DNL Mailchimp] アカウントがあることを確認する必要があります。 アカウントをまだお持ちでない場合は、[[!DNL Mailchimp]  サインアップページ &#x200B;](https://login.mailchimp.com/signup/) にアクセスし、アカウントを登録、作成してください。

#### API キー [!DNL Mailchimp] 収集 {#gather-credentials}

[!DNL Mailchimp] アカウントに対して **の宛先を認証するには、** [!DNL Mailchimp Interest Categories]API キー [!DNL Mailchimp] が必要です。 **宛先を認証** する際、**API キー** は [&#x200B; パスワード &#x200B;](#authenticate) として機能します。

**API キー** がない場合は、[!DNL Mailchimp] アカウントにログインし、[!DNL Mailchimp] ドキュメント [API キーの生成方法 &#x200B;](https://mailchimp.com/developer/marketing/guides/quick-start/#generate-your-api-key) を参照してください。

API キーの例は `0123456789abcdef0123456789abcde-us14` です。

>[!IMPORTANT]
>
>**API キー** を生成した場合は、生成後にアクセスできなくなるので、書き留めておきます。

#### [!DNL Mailchimp] データセンターの特定 {#identify-data-center}

次に、[!DNL Mailchimp] データセンターを特定する必要があります。 これをおこなうには、[!DNL Mailchimp] アカウントにログインし、アカウントの **API キーセクション** に移動します。

データセンター ID は、ブラウザーに表示される URL の最初のセクションです。 URL が *https://`us14`.mailchimp.com/account/api/* の場合、データセンターは `us14` です。

データセンター ID も、API キーに *key-dc* の形式で追加されます。例えば、API キーが `0123456789abcdef0123456789abcde-us14` の場合、データセンターは `us14` になります。

データセンターの値 *（この例では `us14`）* を書き留めます。 この値は、[&#x200B; 宛先の詳細を入力 &#x200B;](#destination-details) する際に必要になります。

詳細なガイダンスが必要な場合は、[[!DNL Mailchimp]  基本ドキュメント &#x200B;](https://mailchimp.com/developer/marketing/docs/fundamentals/#api-structure) を参照してください。

### ガードレール {#guardrails}

[!DNL Mailchimp] API によって課せられる制限について詳しくは、[&#x200B; の &#x200B;](https://mailchimp.com/developer/marketing/docs/fundamentals/#api-limits) レート制限 [!DNL Mailchimp] を参照してください。

## サポートされている ID {#supported-identities}

[!DNL Mailchimp] では、以下の表で説明する ID のアクティブ化をサポートしています。 [ID](/help/identity-service/features/namespaces.md) についての詳細情報。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| メール | 連絡先の電子メールアドレス。 | 必須 |

{style="table-layout:auto"}

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスオリジン | サポートあり | 説明 |
|---------|----------|----------|
| [!DNL Segmentation Service] | ✓ | Experience Platform [&#x200B; セグメント化サービス &#x200B;](../../../segmentation/home.md) を通じて生成されたオーディエンス。 |
| カスタムアップロード | ✓ | CSV ファイルから Experience Platform に[読み込まれた](../../../segmentation/ui/audience-portal.md#import-audience)オーディエンス。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
|---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL Profile-based]** | <ul><li>フィールドマッピングに従って、目的のスキーマフィールド *（例：メールアドレス、電話番号、姓）* と共に、オーディエンスのすべてのメンバーを書き出します。</li><li> Experience Platformで選択した各オーディエンスについて、対応する [!DNL Mailchimp Tags] セグメントのステータスがExperience Platformのオーディエンスステータスで更新されます。</li></ul> |
| 書き出し頻度 | **[!UICONTROL Streaming]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。詳しくは、[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)を参照してください。 |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

>[!IMPORTANT]
>
>宛先に接続するには、**[!UICONTROL Manage Destinations]** アクセス制御権限 [&#x200B; が必要 &#x200B;](/help/access-control/home.md#permissions) す。 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

**[!UICONTROL Destinations]**/**[!UICONTROL Catalog]** 内で、[!DNL Mailchimp Tags] を検索します。 または、**[!UICONTROL Email marketing]** カテゴリの下に配置することもできます。

### 宛先に対する認証 {#authenticate}

宛先に対する認証を行うには、以下の必須フィールドに入力し、「**[!UICONTROL Connect to destination]**」を選択します。

| フィールド | 説明 |
| --- | --- |
| **[!UICONTROL Username]** | [!DNL Mailchimp] ユーザー名。 |
| **[!UICONTROL Password]** | [!DNL Mailchimp] 収集 **資格情報** セクションでメモした [&#x200B;  [!DNL Mailchimp] API キー &#x200B;](#gather-credentials)。API キー <br>`{KEY}-{DC}` の形式を取ります。`{KEY}` の部分は [[!DNL Mailchimp] API キー &#x200B;](#gather-credentials) セクションに記載されている値を参照し、`{DC}` の部分は [[!DNL Mailchimp]  データセンター &#x200B;](#identify-data-center) を参照します。 <br>`{KEY}` の部分またはフォーム全体を指定できます。<br> 例えば、API キーが <br>*`0123456789abcdef0123456789abcde-us14`*の場合 <br>*`0123456789abcdef0123456789abcde`*または&#x200B;*`0123456789abcdef0123456789abcde-us14`*のいずれかを値として指定できます。 |

{style="table-layout:auto"}

![&#x200B; 認証方法を示すExperience Platform UI のスクリーンショット。](../../assets/catalog/email-marketing/mailchimp-tags/authenticate-destination.png)

指定した詳細が有効な場合、UI で **[!UICONTROL Connected]** ステータスに緑色のチェックマークが付きます。 その後、次の手順に進むことができます。

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

![&#x200B; 宛先の詳細を示すExperience Platform UI のスクリーンショット。](../../assets/catalog/email-marketing/mailchimp-tags/destination-details.png)

| フィールド | 説明 |
| --- | --- |
| **[!UICONTROL Name]** | 今後この宛先を認識するための名前。 |
| **[!UICONTROL Description]** | 今後この宛先を識別するのに役立つ説明。 |
| **[!UICONTROL Data center]** | [!DNL Mailchimp] アカウント `data center`。 詳しくは、[&#x200B; データセンターの特定  [!DNL Mailchimp]  に関する節 &#x200B;](#identify-data-center) 参照してください。 |
| **[!UICONTROL Audience Name (Please enter Data center first)]** | **[!UICONTROL Data center]** を入力すると、このドロップダウンに [!DNL Mailchimp] アカウントのオーディエンス名が自動的に入力されます。 Experience Platformのデータで更新するオーディエンスを選択します。 |

{style="table-layout:auto"}

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続への詳細の入力を終えたら「**[!UICONTROL Next]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>* データをアクティブ化するには、**[!UICONTROL View Destinations]**、**[!UICONTROL Activate Destinations]**、**[!UICONTROL View Profiles]**、**[!UICONTROL View Segments]** [&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* *ID* を書き出すには、**[!UICONTROL View Identity Graph]** [&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。<br> ![&#x200B; 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択します。](/help/destinations/assets/overview/export-identities-to-destination.png " 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択 "){width="100" zoomable="yes"}

この宛先にオーディエンスをアクティブ化する手順については、[&#x200B; ストリーミング宛先に対するオーディエンスのアクティブ化 &#x200B;](/help/destinations/ui/activate-segment-streaming-destinations.md) を参照してください。

### マッピングの考慮事項と例 {#mapping-considerations-example}

Adobe Experience Platform から [!DNL Mailchimp Tags] 宛先にオーディエンスデータを正しく送信するには、フィールドマッピングの手順を実行する必要があります。マッピングは、Experience Platform アカウント内の Experience Data Model （XDM）スキーマフィールドと、ターゲット宛先から対応する同等のスキーマフィールドとの間にリンクを作成して構成されます。

XDM フィールドを [!DNL Mailchimp Tags] の宛先フィールドに正しくマッピングするには、次の手順に従います。

1. **[!UICONTROL Mapping]** の手順で、「**[!UICONTROL Add new mapping]**」を選択します。 画面に新しいマッピング行が表示されます。
1. **[!UICONTROL Select source field]** ウィンドウで「**[!UICONTROL Select identity namespace]**」を選択し、「`Email` ID 名前空間」を選択します。

   ![Source フィールドを ID 名前空間のメールとして使用したExperience Platform UI のスクリーンショット。](../../assets/catalog/email-marketing/mailchimp-tags/source-field.png)

1. **[!UICONTROL Select target field]** ウィンドウで「**[!UICONTROL Select identity namespace]**」を選択し、「`Email` ID 名前空間」を選択します。

   ![Target フィールドを ID 名前空間のメールとして使用したExperience Platform UI のスクリーンショット。](../../assets/catalog/email-marketing/mailchimp-tags/target-field.png)

   XDM プロファイルスキーマと [!DNL Mailchimp Tags] の間のマッピングは、次のようになります。

   | ソースフィールド | ターゲットフィールド | 必須 |
   | --- | --- | --- |
   | `IdentityMap: Email` | `Identity: Email` | ○ |

   完了したマッピングの例を次に示します。
   ![&#x200B; フィールドのマッピングを示したExperience Platform UI のスクリーンショットの例。](../../assets/catalog/email-marketing/mailchimp-tags/mappings.png)

宛先接続のマッピングの指定が完了したら、「**[!UICONTROL Next]**」を選択します。

## データの書き出しを検証する {#exported-data}

宛先が正しく設定されていることを検証するには、次の手順に従います。

1. [[!DNL Mailchimp]](https://login.mailchimp.com/) アカウントにログインします。 次に、**[!DNL Audience]** / **[!DNL All Contacts]** ページに移動し、オーディエンスの連絡先が追加され、オーディエンス内の連絡先がオーディエンス名で更新されたかどうかを確認します。
   ![&#x200B; オーディエンスページを示す Mailchimp UI のスクリーンショット。](../../assets/catalog/email-marketing/mailchimp-tags/contacts.png)

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのようにデータガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。

## エラーとトラブルシューティング {#errors-and-troubleshooting}

ステータスとエラーコードの一覧と説明については、[[!DNL Mailchimp]  エラーページ &#x200B;](https://mailchimp.com/developer/marketing/docs/errors/) を参照してください。

## その他のリソース {#additional-resources}

[!DNL Mailchimp] ドキュメントからのその他の役に立つ情報は次のとおりです。

* [&#x200B; はじめに  [!DNL Mailchimp]](https://mailchimp.com/help/getting-started-with-mailchimp/)
* [&#x200B; オーディエンスの概要 &#x200B;](https://mailchimp.com/help/getting-started-audience/)
* [&#x200B; オーディエンスの作成 &#x200B;](https://mailchimp.com/help/create-audience/)
* [&#x200B; タグ使用の手引き &#x200B;](https://mailchimp.com/help/getting-started-tags/)
* [&#x200B; マーケティング API](https://mailchimp.com/developer/marketing/api/)
