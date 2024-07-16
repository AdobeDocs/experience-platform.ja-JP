---
title: Mailchimp の興味カテゴリ
description: Mailchimp （Intuit Mailchimp とも呼ばれます）は、企業がメーリングリストやメールマーケティングキャンペーンを使用して連絡先（クライアント、顧客、その他の興味のある関係者）を管理し、連絡を取るために使用する、人気のあるマーケティング自動化プラットフォームおよびメールマーケティングサービスです。 このコネクタを使用すると、興味や好みに基づいて連絡先を並べ替えることができます。
last-substantial-update: 2023-05-24T00:00:00Z
exl-id: bdce8295-7305-4d54-81c1-7fa3e580ce70
source-git-commit: ba39f62cd77acedb7bfc0081dbb5f59906c9b287
workflow-type: tm+mt
source-wordcount: '2299'
ht-degree: 19%

---

# [!DNL Mailchimp Interest Categories] 接続

[[!DNL Mailchimp]](https://mailchimp.com) は、企業がメーリングリストやメールマーケティングキャンペーンを使用して連絡先 *（クライアント、顧客、その他の興味のある関係者）を管理し、連絡を取るために使用する* 人気のあるマーケティング自動化プラットフォームおよびメールマーケティングサービスです。 このコネクタを使用すると、興味や好みに基づいて連絡先を並べ替えることができます。

[!DNL Mailchimp Interest Categories] では、[audiences](https://mailchimp.com/help/getting-started-audience/)、[groups](https://mailchimp.com/help/getting-started-with-groups/) および interest カテゴリ *（グループ名またはグループタイトルとも呼ばれます）* を使用します。 各 [!DNL Mailchimp] グループは、興味のあるカテゴリのリストです。 連絡先は、web サイト上のサインアップフォームを通じて 1 つ以上の興味カテゴリを購読すると、興味カテゴリに関連付けられます。 オーディエンス内では、連絡先をグループ化して興味のあるカテゴリに関連付け、それらの連絡先を使用してセグメントを作成することもできます。 これらのオーディエンスを使用して、ターゲットキャンペーンメールを登録した連絡先にブロードキャストできます。

<!--
Compared to [!DNL Mailchimp Tags] which you would use for internal classification, [!DNL Mailchimp Interest Categories] is meant to manage subscriptions to topics of interest that your contacts might be interested in. *Note, Experience Platform also has a connection for [!DNL Mailchimp Tags], you can check it out on the [[!DNL Mailchimp Tags]](/help/destinations/catalog/email-marketing/mailchimp-tags.md) page.*
-->

この [!DNL Adobe Experience Platform] [ 宛先 ](/help/destinations/home.md) は、[[!DNL Mailchimp batch subscribe or unsubscribe API]](https://mailchimp.com/developer/marketing/api/lists/batch-subscribe-or-unsubscribe/) API を使用して [ 興味のあるカテゴリ ](https://mailchimp.com/developer/marketing/api/interest-categories/) を作成し、選択した各 Platform オーディエンスから対応する興味のあるカテゴリに連絡先を追加します。 新しいセグメント内でアクティブ化した後、既存のオーディエンス内で **新しい連絡先を追加** または **既存の [!DNL Mailchimp] しい連絡先の情報を更新** し、その後 **目的のグループに追加したり** 既存の [!DNL Mailchimp] オーディエンスから削除したりできます。 [!DNL Mailchimp Interest Groups] は、Platform から選択したオーディエンス名を [!DNL Mailchimp] 内の関心のあるカテゴリとして使用します。

## ユースケース {#use-cases}

[!DNL Mailchimp Interest Categories] 宛先を使用する方法とタイミングを理解しやすくするために、Adobe Experience Platform のお客様がこの宛先を使用して解決できるユースケースのサンプルを以下に示します。

### マーケティングキャンペーンの連絡先へのメールの送信 {#use-case-send-emails}

スポーツグッズのウェブサイトの販売部門は、サッカーに興味があると自分自身を識別した連絡先のリストにメールベースのマーケティングキャンペーンを放送したいと考えています。 連絡先のリストは、web サイトの開発チームから受け取ったデータ書き出しのバッチとして分離されているので、追跡する必要があります。 チームは、既存の [!DNL Mailchimp] オーディエンスを特定し、各リストの連絡先が追加されるExperience Platformオーディエンスの構築を開始します。 これらのオーディエンスを [!DNL Mailchimp Interest Categories] に送信した後、選択した [!DNL Mailchimp] オーディエンスに連絡先が存在しない場合、連絡先が属するオーディエンス名を持つグループに追加されます。 [!DNL Mailchimp] オーディエンスまたはグループに既に連絡先が存在する場合、その情報は更新されます。 データが [!DNL Mailchimp Interest Categories] に送信されると、セールスチームは、マーケティングキャンペーンメールを選択して、[!DNL Mailchimp] オーディエンス内のサッカーの興味グループに送信できます。

## 前提条件 {#prerequisites}

Experience Platformと [!DNL Mailchimp] で設定する必要がある前提条件と、[!DNL Mailchimp Interest Categories] の宛先を使用する前に収集する必要がある情報については、以下の節を参照してください。

### Experience Platformの前提条件 {#prerequisites-in-experience-platform}

[!DNL Mailchimp Interest Categories] 宛先へのデータをアクティブ化する前に、[スキーマ](/help/xdm/schema/composition.md)、[データセット](https://experienceleague.adobe.com/docs/platform-learn/tutorials/data-ingestion/create-datasets-and-ingest-data.html)および[セグメント](https://experienceleague.adobe.com/docs/platform-learn/tutorials/segments/create-segments.html)を [!DNL Experience Platform] で作成する必要があります。

### [!DNL Mailchimp Interest Categories] の宛先の前提条件 {#prerequisites-destination}

Platform から [!DNL Mailchimp] アカウントにデータを書き出すには、次の前提条件に注意してください。

#### [!DNL Mailchimp] アカウントが必要です {#prerequisites-account}

[!DNL Mailchimp Interest Categories] の宛先を作成する前に、まず [!DNL Mailchimp] アカウントがあることを確認する必要があります。 アカウントをまだお持ちでない場合は、[[!DNL Mailchimp]  サインアップページ ](https://login.mailchimp.com/signup/) にアクセスし、アカウントを登録、作成してください。

#### API キー [!DNL Mailchimp] 収集 {#gather-credentials}

[!DNL Mailchimp] アカウントに対して [!DNL Mailchimp Interest Categories] の宛先を認証するには、[!DNL Mailchimp] **API キー** が必要です。 **宛先を認証** する際、**API キー** は [ パスワード ](#authenticate) として機能します。

**API キー** がない場合は、アカウントにログインし、[[!DNL Mailchimp] API キーの生成 ](https://mailchimp.com/developer/marketing/guides/quick-start/#generate-your-api-key) ドキュメントを参照して作成してください。

API キーの例は `0123456789abcdef0123456789abcde-us14` です。

>[!IMPORTANT]
>
>**API キー** を生成した場合は、生成後にアクセスできなくなるので、書き留めておきます。

#### データセンター [!DNL Mailchimp] 特定 {#identify-data-center}

次に、[!DNL Mailchimp] データセンターを特定する必要があります。 これをおこなうには、[!DNL Mailchimp] アカウントにログインし、アカウントの **API キーセクション** に移動します。

値は、ブラウザーに表示される URL の最初の部分です。 URL が *https://`us14`.mailchimp.com/account/api/* の場合、データセンターは `us14` です。

また、API キーにも *key-dc* の形式で追加されます。API キーが `0123456789abcdef0123456789abcde-us14` の場合、データセンターは `us14` になります。

データセンターの値 *（この例では `us14`）を書き留めておきます*、この値は [ 宛先の詳細を入力 ](#destination-details) する際に必要になります。

詳細なガイダンスが必要な場合は、[[!DNL Mailchimp]  基本ドキュメント ](https://mailchimp.com/developer/marketing/docs/fundamentals/#api-structure) を参照してください。

### ガードレール {#guardrails}

各 [!DNL Mailchimp] オーディエンスには、1 つのグループまたは同じオーディエンス内の複数のグループにわたって、最大 60 個のグループ名（または興味のあるカテゴリ）を含めることができます。 詳しくは、[!DNL Mailchimp][groups](https://mailchimp.com/help/getting-started-with-groups/) を参照してください。 この制限に達すると、[!DNL Mailchimp] API から `400 BAD_REQUEST Cannot have more than 60 interests per list (Across all categories)` メッセージがエラー応答として返されます。

また、[!DNL Mailchimp] API によって課せられる制限について詳しくは、[!DNL Mailchimp] の [ レート制限 ](https://mailchimp.com/developer/marketing/docs/fundamentals/#api-limits) を参照してください。

## サポートされている ID {#supported-identities}

[!DNL Mailchimp] では、以下の表で説明する ID のアクティブ化をサポートしています。 [ID](/help/identity-service/features/namespaces.md) についての詳細情報。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| メール | 連絡先メールアドレス | 必須 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL プロファイルベース]** | <ul><li>セグメントのすべてのメンバーを、フィールドマッピングに従って、必要なスキーマフィールドと共に書き出します&#x200B;*（例：メールアドレス、電話番号、姓）*。</li><li> Platform で選択した各オーディエンスについて、対応する [!DNL Mailchimp Interest Categories] セグメントのステータスが、Platform のオーディエンスステータスで更新されます。</li></ul> |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンスの評価に基づいてExperience Platform内でプロファイルが更新されると、コネクタは更新を宛先プラットフォームに送信します。 詳しくは、[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)を参照してください。 |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

>[!IMPORTANT]
>
>宛先に接続するには、**[!UICONTROL 宛先の表示]** および **[!UICONTROL 宛先の管理]**[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

**[!UICONTROL 宛先]**/**[!UICONTROL カタログ]** 内で、[!DNL Mailchimp Interest Categories] を検索します。 または、**[!UICONTROL メールマーケティング]** カテゴリの下に配置することもできます。

### 宛先に対する認証 {#authenticate}

宛先に対して認証するには、以下の必須フィールドに入力し、「**[!UICONTROL 宛先に接続]**」を選択します。

| フィールド | 説明 |
| --- | --- |
| **[!UICONTROL ユーザー名]** | [!DNL Mailchimp Interest Categories] ユーザー名。 |
| **[!UICONTROL パスワード]** | [ 収集  [!DNL Mailchimp]  資格情報 **セクションでメモした [!DNL Mailchimp]** API キー ](#gather-credentials)。API キー <br>`{KEY}-{DC}` の形式を取ります。`{KEY}` の部分は [[!DNL Mailchimp] API キー ](#gather-credentials) セクションに記載されている値を参照し、`{DC}` の部分は [[!DNL Mailchimp]  データセンター ](#identify-data-center) を参照します。 <br>`{KEY}` の部分またはフォーム全体を指定できます。<br> 例えば、API キーが <br>*`0123456789abcdef0123456789abcde-us14`*の場合 <br>*`0123456789abcdef0123456789abcde`*または&#x200B;*`0123456789abcdef0123456789abcde-us14`*のいずれかを値として指定できます。 |

{style="table-layout:auto"}

![認証方法を示す Platform UI のスクリーンショット。](../../assets/catalog/email-marketing/mailchimp-interest-categories/authenticate-destination.png)

指定した詳細が有効な場合、UI で&#x200B;**[!UICONTROL 接続済み]**&#x200B;ステータスに緑色のチェックマークが付きます。その後、次の手順に進むことができます。

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

![宛先の詳細を示す Platform UI のスクリーンショット。](../../assets/catalog/email-marketing/mailchimp-interest-categories/destination-details.png)

| フィールド | 説明 |
| --- | --- |
| **[!UICONTROL 名前]** | 今後この宛先を認識するための名前。 |
| **[!UICONTROL 説明]** | 今後この宛先を識別するのに役立つ説明。 |
| **[!UICONTROL データセンター]** | [!DNL Mailchimp] アカウント `data center`。 詳しくは、[ データセンターの特定  [!DNL Mailchimp]  に関する節 ](#identify-data-center) 参照してください。 |
| **[!UICONTROL オーディエンス名（最初にデータセンターを選択してください）]** | **[!UICONTROL データセンター]** を選択すると、このドロップダウンに [!DNL Mailchimp] アカウントのオーディエンス名が自動的に入力されます。 Platform のデータで更新するオーディエンスを選択します。 |
| **[!UICONTROL 関心のあるカテゴリ（データセンターとオーディエンス名を最初に選択してください）]** | **[!UICONTROL オーディエンス名]** を選択すると、このドロップダウンには [!DNL Mailchimp] アカウントの興味/関心グループのカテゴリ名が自動的に入力されます。 Platform のデータで更新するカテゴリ名を選択します。 |

{style="table-layout:auto"}

>[!TIP]
>
> 「**[!UICONTROL パスワード]**」フィールドに指定した API キーまたは **[!UICONTROL データセンター]** の値が正しくない場合、UI は [!DNL Mailchimp] しい API エラー応答を表示します。*`No options are available. Please verify the values selected for the following dependent fields: dataCenter`* を以下に示します。 この場合、「オーディエンス名 **[!UICONTROL フィールドから値を選択することはできません（最初にデータセンターを選択してください）]**。 このエラーを修正するには、正しい値を指定します。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>* データをアクティブ化するには、**[!UICONTROL 宛先の表示]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]** および **[!UICONTROL セグメントの表示]**[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* *ID* を書き出すには、**[!UICONTROL ID グラフの表示]**[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。<br> ![ 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択します。](/help/destinations/assets/overview/export-identities-to-destination.png " 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択 "){width="100" zoomable="yes"}

この宛先にオーディエンスをアクティベートする手順は、[ストリーミングオーディエンスの書き出し宛先へのプロファイルとオーディエンスのアクティベート](/help/destinations/ui/activate-segment-streaming-destinations.md)を参照してください。

### マッピングの考慮事項と例 {#mapping-considerations-example}

Adobe Experience Platformから [!DNL Mailchimp Interest Categories] の宛先にオーディエンスデータを正しく送信するには、フィールドマッピングの手順を実行する必要があります。 マッピングは、Platform アカウント内の Experience Data Model （XDM）スキーマフィールドと、ターゲット宛先から対応する同等のスキーマフィールドとの間にリンクを作成して構成されます。

XDM フィールドを [!DNL Mailchimp Interest Categories] の宛先フィールドに正しくマッピングするには、次の手順に従います。

1. **[!UICONTROL マッピング]**&#x200B;手順で、「**[!UICONTROL 新しいマッピングを追加]**」を選択します。これで、新しいマッピング行が画面に表示されます。
1. **[!UICONTROL ソースフィールドを選択]** ウィンドウで、**[!UICONTROL 属性を選択]** カテゴリを選択して XDM 属性を選択するか、**[!UICONTROL ID 名前空間を選択]** を選択して ID を選択します。
1. **[!UICONTROL ターゲットフィールドを選択]** ウィンドウで、**[!UICONTROL ID 名前空間を選択]** を選択して、ID を選択するか、**[!UICONTROL 属性を選択]** カテゴリを選択して、[!DNL Mailchimp] API から入力された属性のリストから選択します。 *選択した [!DNL Mailchimp] オーディエンスに追加したカスタム属性も、ターゲットフィールドとして選択できます。*

   XDM プロファイルスキーマと [!DNL Mailchimp Interest Categories] の間で使用できるマッピングは次のとおりです。
|Sourceフィールド | ターゲットフィールド |備考 |
| — | — | — |
|`IdentityMap: Email`|`Identity: email`|必須：はい |
|`xdm: person.name.firstName`|`Attribute: FNAME`| |
|`xdm: person.name.lastName`|`Attribute: LNAME`| |
|`xdm: person.birthDayAndMonth`|`Attribute: BIRTHDAY`| |

   さらに、[!DNL Mailchimp] オーディエンス内のターゲットと呼ばれる特別なター `merge field` ットフィールドも `ADDRESS` ります。 [[!DNL Mailchimp]  ドキュメント ](https://mailchimp.com/developer/marketing/docs/merge-fields/) では、必要なキーとして `addr1`、`city`、`state`、`zip`、およびオプションのキー `addr2` と `country` を定義しています。 これらのフィールドの値は、文字列にする必要があります。 `ADDRESS` のいずれかのフィールドマッピングが存在する場合、宛先は `ADDRESS` オブジェクトを [!DNL Mailchimp] API に渡して更新します。 マッピングされない `ADDRESS` フィールドの値は、デフォルトで `NULL` になる国を除き、デフォルトで `US` になります。

   「`ADDRESS`」フィールドで使用できるマッピングは次のとおりです。

   | ソースフィールド | ターゲットフィールド |
   | --- | --- |
   | `xdm: workAddress.street1` | `Attribute: ADDRESS.addr1` |
   | `xdm: workAddress.street2` | `Attribute: ADDRESS.addr2` |
   | `xdm: workAddress.city` | `Attribute: ADDRESS.city` |
   | `xdm: workAddress.state` | `Attribute: ADDRESS.state` |
   | `xdm: workAddress.postalCode` | `Attribute: ADDRESS.zip` |
   | `xdm: workAddress.country` | `Attribute: ADDRESS.country` |

   例えば、`country` の値を連絡先の既存の住所フィールド `addr1`、`city`、`state` で更新し、`zip` の値を `132, My Street, Kingston`、`New York`、`New York`、`12401` で更新するとします。 `country` を更新するには、変更 *（存在する場合）を使用して既存の値を渡し* 国の新しい値を渡す必要があります。 したがって、データセットの値は、`132, My Street, Kingston`、`New York`、`New York`、`12401` および `US` である必要があります。 繰り返しになりますが、`country` のみを渡し、`addr1`、`city`、`state` および `zip` の値を指定しない場合、これらは `NULL` によって上書きされます。

   完了したマッピングの例を次に示します。
   ![ フィールドマッピングを示した Platform UI のスクリーンショットの例。](../../assets/catalog/email-marketing/mailchimp-interest-categories/mappings.png)

宛先接続のマッピングの指定が完了したら、「**[!UICONTROL 次へ]**」を選択します。

## データの書き出しを検証する {#exported-data}

宛先が正しく設定されていることを検証するには、次の手順に従います。

* [[!DNL Mailchimp]](https://login.mailchimp.com/) アカウントにログインします。 次に、**[!DNL Audience]** ページに移動します。 次に、**[!DNL Manage Contacts]** メニューを展開し、「**[!DNL Groups]**」を選択します。

![ オーディエンスグループページを示す Mailchimp UI のスクリーンショット。](../../assets/catalog/email-marketing/mailchimp-interest-categories/audience-groups.png)

* グループを選択し、選択したオーディエンスが、Platform のオーディエンス名の後に自動生成されたサフィックスを持つカテゴリとして作成されているかどうかを確認します。
   * この宛先は、選択したセグメントの名前を使用して、[[!DNL Mailchimp]  興味カテゴリの追加 API](https://mailchimp.com/developer/marketing/api/interest-categories/add-interest-category/) を使用して、興味カテゴリを作成します。 新しい宛先を作成し、同じオーディエンスを再度アクティブ化する場合、[!DNL Mailchimp] では、既存のセグメントと新しいセグメントを区別するためにサフィックスが追加されます。
* グループにメールが存在しない連絡先は、新しく作成されたカテゴリに追加されます。
* グループ内に既に存在する連絡先の場合、属性フィールドのデータが更新され、新しく作成されたカテゴリに連絡先が追加されます。

![ オーディエンスグループのカテゴリを示した Mailchimp UI のスクリーンショット。](../../assets/catalog/email-marketing/mailchimp-interest-categories/audience-groups-category.png)

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのようにデータガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。

## エラーとトラブルシューティング {#errors-and-troubleshooting}

### API キーまたはデ [!DNL Mailchimp] タセンターの値が正しくない場合にエラーが発生しました {#incorrect-credentials-error}

「**[!UICONTROL パスワード]**」フィールドに指定した API キーまたは **[!UICONTROL データセンター]** の値が正しくない場合、UI は [!DNL Mailchimp] しい API エラー応答を表示します。*`No options are available. Please verify the values selected for the following dependent fields: dataCenter`* を以下に示します。 この場合、「オーディエンス名 **[!UICONTROL フィールドから値を選択することはできません（最初にデータセンターを選択してください）]**。

![Mailchimp API キーまたはデータセンターの値が正しくない場合のエラーを示す Platform UI のスクリーンショット。](../../assets/catalog/email-marketing/mailchimp-interest-categories/error.png)

このエラーを修正して次の手順に進むには、正しい値を指定する必要があります。 [Identify [!DNL Mailchimp] data center](#identify-data-center) を参照してください。
ガイダンスが必要な場合は [Gather [!DNL Mailchimp] API キー ](#gather-credentials) の節を参照してください。

### グループ名の制限 [!DNL Mailchimp] 超えるとエラーが発生する {#group-name-limits-error}

宛先を作成すると、「*`Cannot have more than 60 interests per list (Across all categories)`*」または「*`400 BAD_REQUEST`*」というエラーメッセージが表示される場合があります。 これは、[ ガードレール ](#guardrails) セクションで説明しているように、1 つのグループ内または同じオーディエンス制限内の複数のグループにわたって 60 個のグループ名（または興味のあるカテゴリ）を超えた場合に発生します。 このエラーを修正するには、[!DNL Mailchimp] のグループ名の制限を超えていないことを確認してください。

### [!DNL Mailchimp] ステータスおよびエラーコード

ステータスとエラーコードの一覧と説明については、[[!DNL Mailchimp]  エラーページ ](https://mailchimp.com/developer/marketing/docs/errors/) を参照してください。

## その他のリソース {#additional-resources}

[!DNL Mailchimp] ドキュメントからのその他の役に立つ情報は次のとおりです。
* [ はじめに  [!DNL Mailchimp]](https://mailchimp.com/help/getting-started-with-mailchimp/)
* [ オーディエンスの概要 ](https://mailchimp.com/help/getting-started-audience/)
* [ オーディエンスの作成 ](https://mailchimp.com/help/create-audience/)
* [ グループ使用の手引き ](https://mailchimp.com/help/getting-started-with-groups/)
* [ 新しいオーディエンスグループの作成 ](https://mailchimp.com/help/create-new-audience-group/)
* [ 興味のカテゴリ ](https://mailchimp.com/developer/marketing/api/interest-categories/)
* [ マーケティング API](https://mailchimp.com/developer/marketing/api/)
