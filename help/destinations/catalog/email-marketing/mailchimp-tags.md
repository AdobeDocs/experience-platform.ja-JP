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

[[!DNL Mailchimp]](https://mailchimp.com) *（別名 [!DNL Intuit Mailchimp]）* は、企業が連絡先の管理や連絡に使用する、一般的なマーケティング自動化プラットフォームおよびメールマーケティングサービスです *（顧客、顧客その他の利害関係者）* メーリングリストとメールマーケティングキャンペーンの使用。

[!DNL Mailchimp Tags] 使用 [オーディエンス](https://mailchimp.com/help/getting-started-audience/) および [タグ](https://mailchimp.com/help/getting-started-tags/) にアクセスして連絡先情報を管理します。 タグは、連絡先を整理し、内の内部分類用にラベルを付けることができるラベルです [!DNL Mailchimp].

比較対象： [!DNL Mailchimp Interest Categories] 興味や好みに基づいて連絡先を並べ替えるために使用する [!DNL Mailchimp Tags] は、連絡先が興味を持つ可能性のある関心のあるトピックの購読を管理することを目的としています。 *なお、Experience Platformはに接続しています。 [!DNL Mailchimp Interest Categories]は、次の URL で確認できます [[!DNL Mailchimp Interest Categories]](/help/destinations/catalog/email-marketing/mailchimp-interest-categories.md) ページ。*

この [!DNL Adobe Experience Platform] [宛先](/help/destinations/home.md) は [[!DNL Mailchimp batch subscribe or unsubscribe API]](https://mailchimp.com/developer/marketing/api/lists/batch-subscribe-or-unsubscribe/) エンドポイント。 次のことができます **新しい連絡先の追加** または **既存のタグの更新 [!DNL Mailchimp] 連絡先** 既存の [!DNL Mailchimp] 新しいオーディエンス内でアクティブ化した後のオーディエンス。 [!DNL Mailchimp Tags] は、Platform から選択したオーディエンス名を内のタグ名として使用します [!DNL Mailchimp].

## ユースケース {#use-cases}

[!DNL Mailchimp Tags] 宛先を使用する方法とタイミングを理解しやすくするために、Adobe Experience Platform のお客様がこの宛先を使用して解決できるユースケースのサンプルを以下に示します。

### マーケティングキャンペーンの連絡先へのメールの送信 {#use-case-send-emails}

組織の販売部門が、キュレーションされた連絡先リストにメールベースのマーケティングキャンペーンをブロードキャストしたいと考えています。 連絡先リストは、様々なオフラインソースからバッチで受信されるので、追跡する必要があります。 チームが既存のを識別します [!DNL Mailchimp] オーディエンスを作成し、各リストの連絡先を追加するExperience Platformオーディエンスの作成を開始します。 これらのオーディエンスをに送信した後 [!DNL Mailchimp Tags]選択したの連絡先に存在しない場合。 [!DNL Mailchimp] オーディエンス、連絡先が属するオーディエンス名を含む関連するタグが追加されます。 に既に連絡先が存在する場合 [!DNL Mailchimp] オーディエンス オーディエンスの名前を持つ新しいタグが追加されます。 でラベルが表示されるためです。 [!DNL Mailchimp] オフラインソースは簡単に識別できます。 データがに送信された後 [!DNL Mailchimp] マーケティングキャンペーンのメールをオーディエンスに送信します。

## 前提条件 {#prerequisites}

Experience Platformで設定する必要がある前提条件については、以下の節を参照してください [!DNL Mailchimp] を使用する前に収集する必要のある情報については、を参照してください [!DNL Mailchimp Tags] の宛先。

### Experience Platformの前提条件 {#prerequisites-in-experience-platform}

へのデータのアクティブ化の前に [!DNL Mailchimp Tags] の宛先。には必要です [スキーマ](/help/xdm/schema/composition.md), a [データセット](https://experienceleague.adobe.com/docs/platform-learn/tutorials/data-ingestion/create-datasets-and-ingest-data.html?lang=ja)、および [オーディエンス](https://experienceleague.adobe.com/docs/platform-learn/tutorials/audiences/create-audiences.html) 作成場所 [!DNL Experience Platform].

### の前提条件 [!DNL Mailchimp Tags] 宛先 {#prerequisites-destination}

Platform からにデータを書き出すには、次の前提条件に注意してください [!DNL Mailchimp Tags] アカウント :

#### [!DNL Mailchimp] アカウントが必要です {#prerequisites-account}

を作成する前に [!DNL Mailchimp Tags] の宛先。最初に [!DNL Mailchimp] アカウント。 まだ訪問していない場合は [[!DNL Mailchimp] 登録ページ](https://login.mailchimp.com/signup/) をクリックしてアカウントを登録および作成します。

#### 収集 [!DNL Mailchimp] API キー {#gather-credentials}

必要なのは [!DNL Mailchimp] **API キー** を認証します [!DNL Mailchimp Interest Categories] に対する宛先 [!DNL Mailchimp] アカウント。 この **API キー** 次の役割を果たす **パスワード** 実行する場合 [宛先の認証](#authenticate).

お持ちでない場合 **API キー**、にログイン [!DNL Mailchimp] アカウントおよびを参照 [!DNL Mailchimp] のドキュメント [api キーの生成方法](https://mailchimp.com/developer/marketing/guides/quick-start/#generate-your-api-key).

API キーの例はです。 `0123456789abcdef0123456789abcde-us14`.

>[!IMPORTANT]
>
>を生成する場合 **API キー**&#x200B;生成するとアクセスできなくなるので、書き留めておいてください。

#### を特定する [!DNL Mailchimp] データセンター {#identify-data-center}

次に、を識別する必要があります [!DNL Mailchimp] データセンター。 これを行うには、にログインします [!DNL Mailchimp] アカウントと **API キーセクション** あなたのアカウント。

データセンター ID は、ブラウザーに表示される URL の最初のセクションです。 URL がの場合 *https://`us14`.mailchimp.com/account/api/*&#x200B;その場合、データセンターは `us14`.

データセンター ID も、の形式で API キーに追加されます *key-dc*。例えば、API キーがの場合 `0123456789abcdef0123456789abcde-us14`その場合、データセンターは `us14`.

データセンターの価値を書き留める *（`us14` この例では）*. この値は、次の場合に必要になります [宛先の詳細を入力](#destination-details).

詳しいガイダンスが必要な場合は、を参照してください。 [[!DNL Mailchimp] 基本ドキュメント](https://mailchimp.com/developer/marketing/docs/fundamentals/#api-structure).

### ガードレール {#guardrails}

を参照してください。 [!DNL Mailchimp] [レート制限](https://mailchimp.com/developer/marketing/docs/fundamentals/#api-limits) によって課される制限に関する詳細な情報 [!DNL Mailchimp] API です。

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
| [!DNL Segmentation Service] | ✓ | Experience Platformを通じて生成されたオーディエンス [セグメント化サービス](../../../segmentation/home.md). |
| カスタムアップロード | ✓ | CSV ファイルから Experience Platform に[読み込まれた](../../../segmentation/ui/audience-portal.md#import-audience)オーディエンス。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL プロファイルベース]** | <ul><li>オーディエンスのすべてのメンバーを、目的のスキーマフィールドと共に書き出します *（例：メールアドレス、電話番号、姓）*&#x200B;フィールドマッピングに従って調整します。</li><li> Platform で選択した各オーディエンスに対応するは、 [!DNL Mailchimp Tags] セグメントステータスが、Platform のオーディエンスステータスで更新されます。</li></ul> |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。詳しくは、[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)を参照してください。 |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

>[!IMPORTANT]
>
>宛先に接続するには、**[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

内 **[!UICONTROL 宛先]** > **[!UICONTROL カタログ]**、検索： [!DNL Mailchimp Tags]. または、の下に配置することもできます。 **[!UICONTROL メールマーケティング]** カテゴリ。

### 宛先に対する認証 {#authenticate}

宛先に対して認証するには、以下の必須フィールドに入力し、を選択します。 **[!UICONTROL 宛先への接続]**.

| フィールド | 説明 |
| --- | --- |
| **[!UICONTROL ユーザー名]** | あなたの [!DNL Mailchimp] ユーザー名。 |
| **[!UICONTROL パスワード]** | あなたの [!DNL Mailchimp] **API キー**&#x200B;それはあなたが書き留めていた [収集 [!DNL Mailchimp] 資格情報](#gather-credentials) セクション。<br> API キーの形式は次のとおりです `{KEY}-{DC}`、ここで `{KEY}` 部分は、でメモされた値を指します [[!DNL Mailchimp] API キー](#gather-credentials) セクションと `{DC}` 部分はを参照します [[!DNL Mailchimp] データセンター](#identify-data-center). <br>次のいずれかを指定できます `{KEY}` 部分またはフォーム全体。<br> 例えば、API キーがの場合 <br>*`0123456789abcdef0123456789abcde-us14`*,<br> 次のいずれかを指定できます&#x200B;*`0123456789abcdef0123456789abcde`*または&#x200B;*`0123456789abcdef0123456789abcde-us14`*値として。 |

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
| **[!UICONTROL データセンター]** | あなたの [!DNL Mailchimp] アカウント `data center`. を参照してください。 [識別 [!DNL Mailchimp] データセンター](#identify-data-center) ガイダンスのセクション。 |
| **[!UICONTROL オーディエンス名（最初にデータセンターを入力してください）]** | を入力した後 **[!UICONTROL データセンター]**：このドロップダウンには、のオーディエンス名が自動的に入力されます [!DNL Mailchimp] アカウント。 Platform のデータで更新するオーディエンスを選択します。 |

{style="table-layout:auto"}

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>* データをアクティブ化するには、 **[!UICONTROL 宛先の表示]**, **[!UICONTROL 宛先のアクティブ化]**, **[!UICONTROL プロファイルの表示]**、および **[!UICONTROL セグメントの表示]** [アクセス制御権限](/help/access-control/home.md#permissions). [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* エクスポートする *id*、が必要です **[!UICONTROL ID グラフの表示]** [アクセス制御権限](/help/access-control/home.md#permissions). <br> ![宛先に対してオーディエンスをアクティブ化するために、ワークフローで強調表示されている ID 名前空間を選択します。](/help/destinations/assets/overview/export-identities-to-destination.png "宛先に対してオーディエンスをアクティブ化するために、ワークフローで強調表示されている ID 名前空間を選択します。"){width="100" zoomable="yes"}

Read [ストリーミング宛先に対するオーディエンスのアクティブ化](/help/destinations/ui/activate-segment-streaming-destinations.md) この宛先に対してオーディエンスをアクティブ化する手順については、を参照してください。

### マッピングの考慮事項と例 {#mapping-considerations-example}

Adobe Experience Platform から [!DNL Mailchimp Tags] 宛先にオーディエンスデータを正しく送信するには、フィールドマッピングの手順を実行する必要があります。マッピングは、Platform アカウント内の Experience Data Model （XDM）スキーマフィールドと、ターゲット宛先から対応する同等のスキーマフィールドとの間にリンクを作成して構成されます。

XDM フィールドをに正しくマッピングするには [!DNL Mailchimp Tags] 宛先フィールドは、次の手順に従います。

1. **[!UICONTROL マッピング]**&#x200B;手順で、「**[!UICONTROL 新しいマッピングを追加]**」を選択します。画面に新しいマッピング行が表示されます。
1. が含まれる **[!UICONTROL ソースフィールドを選択]** ウィンドウ、を選択 **[!UICONTROL ID 名前空間を選択]** を選択し、 `Email` id 名前空間。

   ![Source フィールドを ID 名前空間からのメールとして使用した Platform UI のスクリーンショット。](../../assets/catalog/email-marketing/mailchimp-tags/source-field.png)

1. が含まれる **[!UICONTROL ターゲットフィールドを選択]** ウィンドウ、を選択 **[!UICONTROL ID 名前空間を選択]** を選択し、 `Email` id 名前空間。

   ![ターゲットフィールドを ID 名前空間のメールとして使用した Platform UI のスクリーンショット。](../../assets/catalog/email-marketing/mailchimp-tags/target-field.png)

   XDM プロファイルスキーマとの間のマッピング [!DNL Mailchimp Tags] 次のようになります。 |Sourceフィールド | ターゲットフィールド |必須 | | — | — | — | |`IdentityMap: Email`|`Identity: Email`|はい |

   完了したマッピングの例を次に示します。
   ![フィールドマッピングを示した Platform UI のスクリーンショットの例。](../../assets/catalog/email-marketing/mailchimp-tags/mappings.png)

宛先接続のマッピングの指定が完了したら、以下を選択します。 **[!UICONTROL 次]**.

## データの書き出しを検証する {#exported-data}

宛先が正しく設定されていることを検証するには、次の手順に従います。

1. にログイン [[!DNL Mailchimp]](https://login.mailchimp.com/) アカウント。 次に、に移動します **[!DNL Audience]** > **[!DNL All Contacts]** オーディエンスの連絡先が追加されたかどうか、およびオーディエンス内の連絡先がオーディエンス名で更新されたかどうかをページで確認します。
   ![オーディエンスページを示す Mailchimp UI のスクリーンショット。](../../assets/catalog/email-marketing/mailchimp-tags/contacts.png)

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのようにデータガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。

## エラーとトラブルシューティング {#errors-and-troubleshooting}

を参照してください。 [[!DNL Mailchimp] エラーページ](https://mailchimp.com/developer/marketing/docs/errors/) ステータスコードとエラーコードの包括的なリストと説明。

## その他のリソース {#additional-resources}

その他の役に立つ情報 [!DNL Mailchimp] ドキュメントは以下のとおりです。
* [入門 [!DNL Mailchimp]](https://mailchimp.com/help/getting-started-with-mailchimp/)
* [オーディエンスの概要](https://mailchimp.com/help/getting-started-audience/)
* [オーディエンスの作成](https://mailchimp.com/help/create-audience/)
* [タグ使用の手引き](https://mailchimp.com/help/getting-started-tags/)
* [マーケティング API](https://mailchimp.com/developer/marketing/api/)
