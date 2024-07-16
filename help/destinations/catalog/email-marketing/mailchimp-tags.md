---
title: Mailchimp タグ
description: Mailchimp タグの宛先を使用すると、アカウントデータを書き出し、Mailchimp 内でアクティブ化して、連絡先と関わり合うことができます。
last-substantial-update: 2024-02-20T00:00:00Z
exl-id: 0f278ca8-4fcf-4c47-b538-9cffa45a3d90
source-git-commit: c35b43654d31f0f112258e577a1bb95e72f0a971
workflow-type: tm+mt
source-wordcount: '1646'
ht-degree: 27%

---

# [!DNL Mailchimp Tags] 接続

[[!DNL Mailchimp]](https://mailchimp.com) *（[!DNL Intuit Mailchimp] とも呼ばれます）* は、企業がメーリングリストやメールマーケティングキャンペーンを使用して *（クライアント、顧客、その他の興味のある関係者）の連絡先を管理し、連絡を取るために使用する* 人気のあるマーケティング自動化プラットフォームおよびメールマーケティングサービスです。

[!DNL Mailchimp Tags] では [audiences](https://mailchimp.com/help/getting-started-audience/) および [tags](https://mailchimp.com/help/getting-started-tags/) を使用して連絡先情報を管理します。 タグは、連絡先を整理し、[!DNL Mailchimp] 内の内部分類に合わせてラベルを付けることができるラベルです。

興味や好みに基づいて連絡先を並べ替えるために使用する [!DNL Mailchimp Interest Categories] と比較して、[!DNL Mailchimp Tags] は、連絡先が興味を持つ可能性のある興味のあるトピックの購読を管理することを目的としています。 *Experience Platformにも [!DNL Mailchimp Interest Categories] への接続があるので、[[!DNL Mailchimp Interest Categories]](/help/destinations/catalog/email-marketing/mailchimp-interest-categories.md) のページで確認できます。*

この [!DNL Adobe Experience Platform][ 宛先 ](/help/destinations/home.md) は [[!DNL Mailchimp batch subscribe or unsubscribe API]](https://mailchimp.com/developer/marketing/api/lists/batch-subscribe-or-unsubscribe/) エンドポイントを活用します。 新しいオーディエンス内でアクティブ化した後に **既存のオーディエンス内で** 新しい連絡先を追加 **または** 既存の [!DNL Mailchimp] しい連絡先のタグを更新 [!DNL Mailchimp] できます。 [!DNL Mailchimp Tags] は、Platform から選択したオーディエンス名を [!DNL Mailchimp] 内のタグ名として使用します。

## ユースケース {#use-cases}

[!DNL Mailchimp Tags] 宛先を使用する方法とタイミングを理解しやすくするために、Adobe Experience Platform のお客様がこの宛先を使用して解決できるユースケースのサンプルを以下に示します。

### マーケティングキャンペーンの連絡先へのメールの送信 {#use-case-send-emails}

組織の販売部門が、キュレーションされた連絡先リストにメールベースのマーケティングキャンペーンをブロードキャストしたいと考えています。 連絡先リストは、様々なオフラインソースからバッチで受信されるので、追跡する必要があります。 チームは、既存の [!DNL Mailchimp] オーディエンスを特定し、各リストの連絡先が追加されるExperience Platformオーディエンスの構築を開始します。 これらのオーディエンスを [!DNL Mailchimp Tags] に送信した後、選択した [!DNL Mailchimp] オーディエンスに連絡先が存在しない場合、連絡先が属するオーディエンス名を含む関連付けられたタグが追加されます。 [!DNL Mailchimp] オーディエンスに既に連絡先が存在する場合は、オーディエンスと同じ名前の新しいタグが追加されます。 でラベルが表示されるので、オフラインソース [!DNL Mailchimp] 簡単に識別できます。 データがに送信された後、オーディエンス [!DNL Mailchimp] マーケティングキャンペーンメールを送信します。

## 前提条件 {#prerequisites}

Experience Platformと [!DNL Mailchimp] で設定する必要がある前提条件と、[!DNL Mailchimp Tags] ースの宛先を使用する前に収集する必要がある情報については、以下の節を参照してください。

### Experience Platformの前提条件 {#prerequisites-in-experience-platform}

[!DNL Mailchimp Tags] の宛先へのデータをアクティブ化する前に、[ スキーマ ](/help/xdm/schema/composition.md)、[ データセット ](https://experienceleague.adobe.com/docs/platform-learn/tutorials/data-ingestion/create-datasets-and-ingest-data.html?lang=ja) および [ オーディエンス ](https://experienceleague.adobe.com/docs/platform-learn/tutorials/audiences/create-audiences.html) を [!DNL Experience Platform] で作成する必要があります。

### [!DNL Mailchimp Tags] の宛先の前提条件 {#prerequisites-destination}

Platform から [!DNL Mailchimp Tags] アカウントにデータを書き出すには、次の前提条件に注意してください。

#### [!DNL Mailchimp] アカウントが必要です {#prerequisites-account}

[!DNL Mailchimp Tags] の宛先を作成する前に、まず [!DNL Mailchimp] アカウントがあることを確認する必要があります。 アカウントをまだお持ちでない場合は、[[!DNL Mailchimp]  サインアップページ ](https://login.mailchimp.com/signup/) にアクセスし、アカウントを登録、作成してください。

#### API キー [!DNL Mailchimp] 収集 {#gather-credentials}

[!DNL Mailchimp] アカウントに対して [!DNL Mailchimp Interest Categories] の宛先を認証するには、[!DNL Mailchimp] **API キー** が必要です。 **宛先を認証** する際、**API キー** は [ パスワード ](#authenticate) として機能します。

**API キー** がない場合は、[!DNL Mailchimp] アカウントにログインし、[!DNL Mailchimp] ドキュメント [API キーの生成方法 ](https://mailchimp.com/developer/marketing/guides/quick-start/#generate-your-api-key) を参照してください。

API キーの例は `0123456789abcdef0123456789abcde-us14` です。

>[!IMPORTANT]
>
>**API キー** を生成した場合は、生成後にアクセスできなくなるので、書き留めておきます。

#### [!DNL Mailchimp] データセンターの特定 {#identify-data-center}

次に、[!DNL Mailchimp] データセンターを特定する必要があります。 これをおこなうには、[!DNL Mailchimp] アカウントにログインし、アカウントの **API キーセクション** に移動します。

データセンター ID は、ブラウザーに表示される URL の最初のセクションです。 URL が *https://`us14`.mailchimp.com/account/api/* の場合、データセンターは `us14` です。

データセンター ID も、API キーに *key-dc* の形式で追加されます。例えば、API キーが `0123456789abcdef0123456789abcde-us14` の場合、データセンターは `us14` になります。

データセンターの値 *（この例では `us14`）* を書き留めます。 この値は、[ 宛先の詳細を入力 ](#destination-details) する際に必要になります。

詳細なガイダンスが必要な場合は、[[!DNL Mailchimp]  基本ドキュメント ](https://mailchimp.com/developer/marketing/docs/fundamentals/#api-structure) を参照してください。

### ガードレール {#guardrails}

[!DNL Mailchimp] API によって課せられる制限について詳しくは、[!DNL Mailchimp] の [ レート制限 ](https://mailchimp.com/developer/marketing/docs/fundamentals/#api-limits) を参照してください。

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
| [!DNL Segmentation Service] | ✓ | Experience Platform[ セグメント化サービス ](../../../segmentation/home.md) を通じて生成されたオーディエンス。 |
| カスタムアップロード | ✓ | CSV ファイルから Experience Platform に[読み込まれた](../../../segmentation/ui/audience-portal.md#import-audience)オーディエンス。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL プロファイルベース]** | <ul><li>フィールドマッピングに従って、目的のスキーマフィールド *（例：メールアドレス、電話番号、姓）* と共に、オーディエンスのすべてのメンバーを書き出します。</li><li> Platform で選択した各オーディエンスについて、対応する [!DNL Mailchimp Tags] セグメントのステータスが Platform のオーディエンスのステータスで更新されます。</li></ul> |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。詳しくは、[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)を参照してください。 |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

>[!IMPORTANT]
>
>宛先に接続するには、**[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

**[!UICONTROL 宛先]**/**[!UICONTROL カタログ]** 内で、[!DNL Mailchimp Tags] を検索します。 または、**[!UICONTROL メールマーケティング]** カテゴリの下に配置することもできます。

### 宛先に対する認証 {#authenticate}

宛先に対して認証するには、以下の必須フィールドに入力し、「**[!UICONTROL 宛先に接続]**」を選択します。

| フィールド | 説明 |
| --- | --- |
| **[!UICONTROL ユーザー名]** | [!DNL Mailchimp] ユーザー名。 |
| **[!UICONTROL パスワード]** | [ 収集  [!DNL Mailchimp]  資格情報 **セクションでメモした [!DNL Mailchimp]** API キー ](#gather-credentials)。API キー <br>`{KEY}-{DC}` の形式を取ります。`{KEY}` の部分は [[!DNL Mailchimp] API キー ](#gather-credentials) セクションに記載されている値を参照し、`{DC}` の部分は [[!DNL Mailchimp]  データセンター ](#identify-data-center) を参照します。 <br>`{KEY}` の部分またはフォーム全体を指定できます。<br> 例えば、API キーが <br>*`0123456789abcdef0123456789abcde-us14`*の場合 <br>*`0123456789abcdef0123456789abcde`*または&#x200B;*`0123456789abcdef0123456789abcde-us14`*のいずれかを値として指定できます。 |

{style="table-layout:auto"}

![認証方法を示す Platform UI のスクリーンショット。](../../assets/catalog/email-marketing/mailchimp-tags/authenticate-destination.png)

指定した詳細が有効な場合、UI で&#x200B;**[!UICONTROL 接続済み]**&#x200B;ステータスに緑色のチェックマークが付きます。その後、次の手順に進むことができます。

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

![宛先の詳細を示す Platform UI のスクリーンショット。](../../assets/catalog/email-marketing/mailchimp-tags/destination-details.png)

| フィールド | 説明 |
| --- | --- |
| **[!UICONTROL 名前]** | 今後この宛先を認識するための名前。 |
| **[!UICONTROL 説明]** | 今後この宛先を識別するのに役立つ説明。 |
| **[!UICONTROL データセンター]** | [!DNL Mailchimp] アカウント `data center`。 詳しくは、[ データセンターの特定  [!DNL Mailchimp]  に関する節 ](#identify-data-center) 参照してください。 |
| **[!UICONTROL オーディエンス名（最初にデータセンターを入力してください）]** | **[!UICONTROL データセンター]** を入力すると、このドロップダウンに [!DNL Mailchimp] アカウントのオーディエンス名が自動的に入力されます。 Platform のデータで更新するオーディエンスを選択します。 |

{style="table-layout:auto"}

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>* データをアクティブ化するには、**[!UICONTROL 宛先の表示]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]** および **[!UICONTROL セグメントの表示]**[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* *ID* を書き出すには、**[!UICONTROL ID グラフの表示]**[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。<br> ![ 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択します。](/help/destinations/assets/overview/export-identities-to-destination.png " 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択 "){width="100" zoomable="yes"}

この宛先にオーディエンスをアクティブ化する手順については、[ ストリーミング宛先に対するオーディエンスのアクティブ化 ](/help/destinations/ui/activate-segment-streaming-destinations.md) を参照してください。

### マッピングの考慮事項と例 {#mapping-considerations-example}

Adobe Experience Platform から [!DNL Mailchimp Tags] 宛先にオーディエンスデータを正しく送信するには、フィールドマッピングの手順を実行する必要があります。マッピングは、Platform アカウント内の Experience Data Model （XDM）スキーマフィールドと、ターゲット宛先から対応する同等のスキーマフィールドとの間にリンクを作成して構成されます。

XDM フィールドを [!DNL Mailchimp Tags] の宛先フィールドに正しくマッピングするには、次の手順に従います。

1. **[!UICONTROL マッピング]**&#x200B;手順で、「**[!UICONTROL 新しいマッピングを追加]**」を選択します。画面に新しいマッピング行が表示されます。
1. **[!UICONTROL ソースフィールドを選択]** ウィンドウで、「**[!UICONTROL ID 名前空間を選択]** を選択して、`Email` ID 名前空間を選択します。

   ![Source フィールドを ID 名前空間のメールとして使用した Platform UI のスクリーンショット。](../../assets/catalog/email-marketing/mailchimp-tags/source-field.png)

1. **[!UICONTROL ターゲットフィールドを選択]** ウィンドウで、「**[!UICONTROL ID 名前空間を選択]** を選択して、`Email` ID 名前空間を選択します。

   ![ ターゲットフィールドを ID 名前空間のメールとして使用した Platform UI のスクリーンショット。](../../assets/catalog/email-marketing/mailchimp-tags/target-field.png)

   XDM プロファイルスキーマと [!DNL Mailchimp Tags] の間のマッピングは、次のようになります。
|Sourceフィールド | ターゲットフィールド |必須 |
| — | — | — |
|`IdentityMap: Email`|`Identity: Email`|はい |

   完了したマッピングの例を次に示します。
   ![ フィールドマッピングを示した Platform UI のスクリーンショットの例。](../../assets/catalog/email-marketing/mailchimp-tags/mappings.png)

宛先接続のマッピングの指定が完了したら、「**[!UICONTROL 次へ]**」を選択します。

## データの書き出しを検証する {#exported-data}

宛先が正しく設定されていることを検証するには、次の手順に従います。

1. [[!DNL Mailchimp]](https://login.mailchimp.com/) アカウントにログインします。 次に、**[!DNL Audience]** / **[!DNL All Contacts]** ページに移動し、オーディエンスの連絡先が追加され、オーディエンス内の連絡先がオーディエンス名で更新されたかどうかを確認します。
   ![ オーディエンスページを示す Mailchimp UI のスクリーンショット。](../../assets/catalog/email-marketing/mailchimp-tags/contacts.png)

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのようにデータガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。

## エラーとトラブルシューティング {#errors-and-troubleshooting}

ステータスとエラーコードの一覧と説明については、[[!DNL Mailchimp]  エラーページ ](https://mailchimp.com/developer/marketing/docs/errors/) を参照してください。

## その他のリソース {#additional-resources}

[!DNL Mailchimp] ドキュメントからのその他の役に立つ情報は次のとおりです。
* [ はじめに  [!DNL Mailchimp]](https://mailchimp.com/help/getting-started-with-mailchimp/)
* [ オーディエンスの概要 ](https://mailchimp.com/help/getting-started-audience/)
* [ オーディエンスの作成 ](https://mailchimp.com/help/create-audience/)
* [ タグ使用の手引き ](https://mailchimp.com/help/getting-started-tags/)
* [ マーケティング API](https://mailchimp.com/developer/marketing/api/)
