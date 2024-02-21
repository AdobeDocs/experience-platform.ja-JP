---
title: Mailchimp タグ
description: Mailchimp タグの宛先を使用すると、アカウントデータを書き出し、Mailchimp 内で有効化して連絡先とやり取りすることができます。
source-git-commit: b32d619b78502e38e98a410bd14379992543c63c
workflow-type: tm+mt
source-wordcount: '1646'
ht-degree: 27%

---

# [!DNL Mailchimp Tags] 接続

[[!DNL Mailchimp]](https://mailchimp.com) *( 別名 [!DNL Intuit Mailchimp])* は、企業が連絡先の管理や連絡に使用する、人気の高いマーケティングオートメーションプラットフォームおよび電子メールマーケティングサービスです *（顧客、顧客その他の利害関係人）* メーリングリストと電子メールマーケティングキャンペーンの使用

[!DNL Mailchimp Tags] uses [audiences](https://mailchimp.com/help/getting-started-audience/) および [タグ](https://mailchimp.com/help/getting-started-tags/) 連絡先情報を管理する。 タグは、連絡先を整理し、内部分類のためにラベル付けする際に使用するラベルです。 [!DNL Mailchimp].

比較対象 [!DNL Mailchimp Interest Categories] 顧客の興味や好みに基づいて連絡先を並べ替える際に使用します。 [!DNL Mailchimp Tags] は、連絡先が興味を持つ可能性のあるトピックの購読を管理するためのものです。 *注意：Experience Platformは、 [!DNL Mailchimp Interest Categories]を使用する場合は、 [[!DNL Mailchimp Interest Categories]](/help/destinations/catalog/email-marketing/mailchimp-interest-categories.md) ページに貼り付けます。*

この [!DNL Adobe Experience Platform] [宛先](/help/destinations/home.md) は、 [[!DNL Mailchimp batch subscribe or unsubscribe API]](https://mailchimp.com/developer/marketing/api/lists/batch-subscribe-or-unsubscribe/) endpoint. 以下が可能です。 **新規連絡先の追加** または **既存のタグを更新 [!DNL Mailchimp] 連絡先** 既存の [!DNL Mailchimp] オーディエンスをアクティブ化した後で、新しいオーディエンス内でアクティブ化した場合。 [!DNL Mailchimp Tags] は、Platform から選択したオーディエンス名を内のタグ名として使用します [!DNL Mailchimp].

## ユースケース {#use-cases}

[!DNL Mailchimp Tags] 宛先を使用する方法とタイミングを理解しやすくするために、Adobe Experience Platform のお客様がこの宛先を使用して解決できるユースケースのサンプルを以下に示します。

### マーケティングキャンペーン用の連絡先へのメールの送信 {#use-case-send-emails}

組織の販売部門は、電子メールベースのマーケティングキャンペーンを厳選された連絡先リストにブロードキャストしたいと考えています。 連絡先リストは、様々なオフラインソースから一括で受け取るので、トラッキングする必要があります。 チームが既存の [!DNL Mailchimp] オーディエンスを作成し、各Experience Platformの連絡先を追加するリストオーディエンスの作成を開始します。 これらのオーディエンスをに送信した後 [!DNL Mailchimp Tags]選択したに存在しない連絡先がある場合は、 [!DNL Mailchimp] オーディエンスに関連付けられ、連絡先が属するオーディエンス名を含むタグが追加されます。 既に [!DNL Mailchimp] audience オーディエンスの名前を持つ新しいタグが追加されます。 ラベルが [!DNL Mailchimp] オフラインソースは簡単に識別できます。 データがに送信された後 [!DNL Mailchimp] オーディエンスにマーケティングキャンペーンの電子メールを送信します。

## 前提条件 {#prerequisites}

Experience Platformおよびで設定する必要がある前提条件については、以下の節を参照してください。 [!DNL Mailchimp] また、を使用する前に収集する必要がある情報についても同様です。 [!DNL Mailchimp Tags] 宛先。

### Experience Platformの前提条件 {#prerequisites-in-experience-platform}

に対してデータをアクティブ化する前に [!DNL Mailchimp Tags] 宛先の [スキーマ](/help/xdm/schema/composition.md), a [データセット](https://experienceleague.adobe.com/docs/platform-learn/tutorials/data-ingestion/create-datasets-and-ingest-data.html?lang=ja)、および [audiences](https://experienceleague.adobe.com/docs/platform-learn/tutorials/audiences/create-audiences.html) 次で作成： [!DNL Experience Platform].

### の前提条件 [!DNL Mailchimp Tags] 宛先 {#prerequisites-destination}

Platform からにデータを書き出すための次の前提条件に注意してください。 [!DNL Mailchimp Tags] アカウント：

#### [!DNL Mailchimp] アカウントが必要です {#prerequisites-account}

事前に [!DNL Mailchimp Tags] 宛先の名前を指定する場合は、まず [!DNL Mailchimp] アカウント。 まだ [[!DNL Mailchimp] 登録ページ](https://login.mailchimp.com/signup/) をクリックして、アカウントを登録および作成します。

#### 収集 [!DNL Mailchimp] API キー {#gather-credentials}

次が必要です： [!DNL Mailchimp] **API キー** 認証を行う [!DNL Mailchimp Interest Categories] の宛先に対して、 [!DNL Mailchimp] アカウント。 The **API キー** ～の役割を果たす **パスワード** いつ [宛先の認証](#authenticate).

次の条件を満たしていない場合、 **API キー**&#x200B;を使用し、 [!DNL Mailchimp] アカウントを使用し、 [!DNL Mailchimp] に関するドキュメント [API キーを生成する方法](https://mailchimp.com/developer/marketing/guides/quick-start/#generate-your-api-key).

API キーの例は次のとおりです。 `0123456789abcdef0123456789abcde-us14`.

>[!IMPORTANT]
>
>次の場合、 **API キー**&#x200B;生成後はアクセスできなくなるので、書き留めてください。

#### を特定します。 [!DNL Mailchimp] データセンター {#identify-data-center}

次に、 [!DNL Mailchimp] データセンター これをおこなうには、 [!DNL Mailchimp] アカウントに移動し、 **API キーの節** 」をクリックします。

データセンター ID は、ブラウザーに表示される URL の最初のセクションです。 URL が *https://`us14`.mailchimp.com/account/api/*&#x200B;を設定した場合、データセンターは `us14`.

また、このデータセンター ID はフォームの API キーにも追加されます *key-dc*；例えば、API キーがの場合、 `0123456789abcdef0123456789abcde-us14`を設定した場合、データセンターは `us14`.

データセンターの価値を書き留める *(`us14` （この例では）*. この値は、 [宛先の詳細を入力](#destination-details).

詳しいガイダンスが必要な場合は、 [[!DNL Mailchimp] 基本ドキュメント](https://mailchimp.com/developer/marketing/docs/fundamentals/#api-structure).

### ガードレール {#guardrails}

詳しくは、 [!DNL Mailchimp] [レート制限](https://mailchimp.com/developer/marketing/docs/fundamentals/#api-limits) を参照してください。 [!DNL Mailchimp] API.

## サポートされている ID {#supported-identities}

[!DNL Mailchimp] では、以下の表で説明する id のアクティブ化をサポートしています。 [ID](/help/identity-service/features/namespaces.md) についての詳細情報。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| メール | 連絡先の電子メールアドレス。 | 必須 |

{style="table-layout:auto"}

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスの起源 | サポートあり | 説明 |
---------|----------|----------|
| [!DNL Segmentation Service] | ✓ | Experience Platform [セグメント化サービス](../../../segmentation/home.md). |
| カスタムアップロード | ✓ | CSV ファイルから Experience Platform に[読み込まれた](../../../segmentation/ui/overview.md#import-audience)オーディエンス。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL プロファイルベース]** | <ul><li>オーディエンスのすべてのメンバーを、目的のスキーマフィールドと共に書き出します *（例：電子メールアドレス、電話番号、姓）*（フィールドマッピングに従います）。</li><li> Platform で選択した各オーディエンスに対して、 [!DNL Mailchimp Tags] セグメントのステータスが、Platform からのオーディエンスのステータスに更新されます。</li></ul> |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。詳しくは、[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)を参照してください。 |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

>[!IMPORTANT]
>
>宛先に接続するには、**[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

Within **[!UICONTROL 宛先]** > **[!UICONTROL カタログ]**、を検索します。 [!DNL Mailchimp Tags]. または、 **[!UICONTROL 電子メールマーケティング]** カテゴリ。

### 宛先に対する認証 {#authenticate}

宛先を認証するには、以下の必須フィールドに入力し、を選択します。 **[!UICONTROL 宛先に接続]**.

| フィールド | 説明 |
| --- | --- |
| **[!UICONTROL ユーザー名]** | お使いの [!DNL Mailchimp] ユーザー名。 |
| **[!UICONTROL パスワード]** | お使いの [!DNL Mailchimp] **API キー**&#x200B;君が書いていた [収集 [!DNL Mailchimp] 資格情報](#gather-credentials) 」セクションに入力します。<br> API キーは、 `{KEY}-{DC}`( ここで `{KEY}` 部分は、 [[!DNL Mailchimp] API キー](#gather-credentials) セクションおよび `{DC}` 部分は [[!DNL Mailchimp] データセンター](#identify-data-center). <br>次のいずれかを指定できます。 `{KEY}` の一部またはフォーム全体を読み込みます。<br> 例えば、API キーが <br>*`0123456789abcdef0123456789abcde-us14`*,<br> 次のいずれかを指定できます。*`0123456789abcdef0123456789abcde`*または&#x200B;*`0123456789abcdef0123456789abcde-us14`*を値として使用します。 |

{style="table-layout:auto"}

![認証方法を示す Platform UI のスクリーンショット。](../../assets/catalog/email-marketing/mailchimp-tags/authenticate-destination.png)

指定した詳細が有効な場合、UI で&#x200B;**[!UICONTROL 接続済み]**&#x200B;ステータスに緑色のチェックマークが付きます。その後、次の手順に進むことができます。

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

![宛先の詳細を示す Platform UI のスクリーンショット。](../../assets/catalog/email-marketing/mailchimp-tags/destination-details.png)

| フィールド | 説明 |
| --- | --- |
| **[!UICONTROL 名前]** | 将来この宛先を認識するための名前。 |
| **[!UICONTROL 説明]** | 今後この宛先を識別するのに役立つ説明。 |
| **[!UICONTROL データセンター]** | お使いの [!DNL Mailchimp] アカウント `data center`. 詳しくは、 [特定 [!DNL Mailchimp] データセンター](#identify-data-center) 」の節を参照してください。 |
| **[!UICONTROL オーディエンス名（最初にデータセンターを入力してください）]** | 入力後、 **[!UICONTROL データセンター]**&#x200B;の場合、このドロップダウンには、 [!DNL Mailchimp] アカウント。 Platform のデータで更新するオーディエンスを選択します。 |

{style="table-layout:auto"}

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>* データをアクティブ化するには、 **[!UICONTROL 宛先の表示]**, **[!UICONTROL 宛先のアクティブ化]**, **[!UICONTROL プロファイルの表示]**、および **[!UICONTROL セグメントを表示]** [アクセス制御権限](/help/access-control/home.md#permissions). [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* 書き出す *id*、 **[!UICONTROL ID グラフを表示]** [アクセス制御権限](/help/access-control/home.md#permissions). <br> ![ワークフローでハイライト表示された ID 名前空間を選択して、宛先に対するオーディエンスをアクティブ化します。](/help/destinations/assets/overview/export-identities-to-destination.png "ワークフローでハイライト表示された ID 名前空間を選択して、宛先に対するオーディエンスをアクティブ化します。"){width="100" zoomable="yes"}

読み取り [ストリーミング先に対するオーディエンスのアクティブ化](/help/destinations/ui/activate-segment-streaming-destinations.md) を参照してください。

### マッピングの考慮事項と例 {#mapping-considerations-example}

Adobe Experience Platform から [!DNL Mailchimp Tags] 宛先にオーディエンスデータを正しく送信するには、フィールドマッピングの手順を実行する必要があります。マッピングは、Platform アカウント内の Experience Data Model(XDM) スキーマフィールドと、ターゲット宛先から対応する同等のスキーマフィールドとの間にリンクを作成することで構成されます。

XDM フィールドを [!DNL Mailchimp Tags] 宛先フィールドには、次の手順に従います。

1. **[!UICONTROL マッピング]**&#x200B;手順で、「**[!UICONTROL 新しいマッピングを追加]**」を選択します。画面に新しいマッピング行が表示されます。
1. Adobe Analytics の **[!UICONTROL ソースフィールドを選択]** ウィンドウ：選択 **[!UICONTROL ID 名前空間を選択]** をクリックし、 `Email` id 名前空間。

   ![ID 名前空間からの電子メールとしての「ソース」フィールドを含む、Platform UI のスクリーンショット。](../../assets/catalog/email-marketing/mailchimp-tags/source-field.png)

1. Adobe Analytics の **[!UICONTROL ターゲットフィールドを選択]** ウィンドウ：選択 **[!UICONTROL ID 名前空間を選択]** をクリックし、 `Email` id 名前空間。

   ![ID 名前空間からの電子メールとしての Target フィールドを含む、Platform UI のスクリーンショット。](../../assets/catalog/email-marketing/mailchimp-tags/target-field.png)

   XDM プロファイルスキーマとのマッピング [!DNL Mailchimp Tags] は次のようになります。 | ソースフィールド | ターゲットフィールド | 必須 | | — | — | — | |`IdentityMap: Email`|`Identity: Email`| はい |

   完了したマッピングの例を次に示します。
   ![フィールドマッピングを示す Platform UI のスクリーンショットの例。](../../assets/catalog/email-marketing/mailchimp-tags/mappings.png)

宛先接続のマッピングの指定が完了したら、「 」を選択します。 **[!UICONTROL 次へ]**.

## データの書き出しを検証する {#exported-data}

宛先が正しく設定されていることを検証するには、次の手順に従います。

1. にログインします。 [[!DNL Mailchimp]](https://login.mailchimp.com/) アカウント。 次に、 **[!DNL Audience]** > **[!DNL All Contacts]** ページを開き、オーディエンスからの連絡先が追加されたかどうか、およびオーディエンス内の連絡先がオーディエンス名で更新されたかどうかを確認します。
   ![Audience ページを示す mailchimp UI のスクリーンショット。](../../assets/catalog/email-marketing/mailchimp-tags/contacts.png)

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのようにデータガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。

## エラーとトラブルシューティング {#errors-and-troubleshooting}

詳しくは、 [[!DNL Mailchimp] エラーページ](https://mailchimp.com/developer/marketing/docs/errors/) を参照してください。

## その他のリソース {#additional-resources}

以下に示すその他の役に立つ情報 [!DNL Mailchimp] 以下のドキュメントです。
* [の概要 [!DNL Mailchimp]](https://mailchimp.com/help/getting-started-with-mailchimp/)
* [Audiences の概要](https://mailchimp.com/help/getting-started-audience/)
* [オーディエンスの作成](https://mailchimp.com/help/create-audience/)
* [タグの概要](https://mailchimp.com/help/getting-started-tags/)
* [マーケティング API](https://mailchimp.com/developer/marketing/api/)
