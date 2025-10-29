---
keywords: メール；メール；メール；メールの宛先；sendgrid;sendgrid の宛先
title: SendGrid 接続
description: SendGrid 宛先を使用すると、ファーストパーティデータを書き出し、SendGrid 内でビジネスニーズに合わせてアクティブ化できます。
exl-id: 6f22746f-2043-4a20-b8a6-097d721f2fe7
source-git-commit: 1b507e9846a74b7ac2d046c89fd7c27a818035ba
workflow-type: tm+mt
source-wordcount: '1475'
ht-degree: 22%

---

# [!DNL SendGrid] 接続

## 概要 {#overview}

[SendGrid](https://www.sendgrid.com) は、トランザクションメールとマーケティングメール用の一般的な顧客コミュニケーションプラットフォームです。

この [!DNL Adobe Experience Platform][&#x200B; 宛先 &#x200B;](/help/destinations/home.md) は [[!DNL SendGrid Marketing Contacts API]](https://api.sendgrid.com/v3/marketing/contacts) を利用しているので、ファーストパーティのメールプロファイルを書き出し、ビジネスニーズに合わせて新しい SendGrid オーディエンス内でそれらをアクティブ化できます。

SendGrid は、SendGrid API と通信するための認証メカニズムとして API ベアラートークンを使用します。

## 前提条件 {#prerequisites}

宛先の設定を開始する前に、次の項目が必要です。

1. SendGrid アカウントが必要です。
   * SendGrid [&#x200B; サインアップ &#x200B;](https://signup.sendgrid.com/) ページに移動して、SendGrid アカウントをまだ持っていない場合は、登録して作成します。
1. SendGrid ポータルにログインした後、API トークンも生成する必要があります。
1. SendGrid web サイトに移動し、**[!DNL Settings]** / **[!DNL API Keys]** ページにアクセスします。 または、[SendGrid ドキュメント &#x200B;](https://app.sendgrid.com/settings/api_keys) を参照して、SendGrid アプリの適切なセクションにアクセスします。
1. 最後に、「**[!DNL Create API Key]**」ボタンを選択します。
   * 実行するアクションに関するガイダンスが必要な場合は、[SendGrid ドキュメント &#x200B;](https://docs.sendgrid.com/ui/account-and-settings/api-keys#creating-an-api-key) を参照してください。
   * プログラムによって API キーを生成する場合は、[SendGrid ドキュメント &#x200B;](https://docs.sendgrid.com/api-reference/api-keys/create-api-keys) を参照してください。

![](../../assets/catalog/email-marketing/sendgrid/01-api-key.jpg)

SendGrid 宛先へのデータをアクティブ化する前に、[schema](https://experienceleague.adobe.com/docs/experience-platform/xdm/schema/composition.html?lang=ja)、[dataset](https://experienceleague.adobe.com/docs/platform-learn/tutorials/data-ingestion/create-datasets-and-ingest-data.html) および [segments](https://experienceleague.adobe.com/docs/platform-learn/tutorials/segments/create-segments.html) を [!DNL Experience Platform] で作成する必要があります。 このページで後述する [limits](#limits) の節も参照してください。

>[!IMPORTANT]
>
>* メールプロファイルからメーリングリストを作成するために使用する SendGrid API では、プロファイルごとに一意のメールアドレスを指定する必要があります。 これは、*電子メール* または *代替電子メール* の値として使用されるかどうかに関係しません。 SendGrid 接続では、電子メールと代替電子メールの両方の値のマッピングをサポートしているので、使用するすべての電子メールアドレスが *データセット* の各プロファイル内で一意であることを確認してください。 そうしないと、メールプロファイルが SendGrid に送信される際にエラーが発生し、そのメールプロファイルはデータの書き出しには表示されません。
>
>* 現在、プロファイルがExperience Platformでオーディエンスから削除された場合、SendGrid から削除する機能はありません。

## サポートされている ID {#supported-identities}

SendGrid では、以下の表に示す ID のアクティブ化をサポートしています。 [ID](/help/identity-service/features/namespaces.md) についての詳細情報。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| メール | メールアドレス | メモ [!DNL Adobe Experience Platform] では、プレーンテキストと SHA256 でハッシュ化されたメールアドレスの両方がサポートされています。 ハッシュ化されていない属性が Experience Platform ソースフィールドに含まれている場合は、「**[!UICONTROL Apply transformation]**」オプションをオンにして、アクティブ化時にがデータ [!DNL Experience Platform] 自動的にハッシュ化するように設定します。<br/><br/> **SendGrid** は、ハッシュ化されたメールアドレスをサポートしていないので、変換のないプレーンテキストデータのみが宛先に送信されます。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
|---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL Profile-based]** | [宛先のアクティベーションワークフロー](/help/destinations/ui/activate-batch-profile-destinations.md#select-attributes)のプロファイル属性選択画面で選択した目的のスキーマフィールド（例：メールアドレス、電話番号、姓）と共に、セグメントのすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL Streaming]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)の詳細についてはこちらを参照してください。 |

{style="table-layout:auto"}

## ユースケース {#use-cases}

SendGrid 宛先を使用する方法とタイミングをより深く理解するために、[!DNL Experience Platform] 顧客がこの宛先を使用して解決できるサンプルユースケースを以下に示します。

### 複数のマーケティングアクティビティ用のマーケティングリストの作成

SendGrid を使用するマーケティングチームは、SendGrid 内にメーリングリストを作成し、メールアドレスを入力できます。 SendGrid 内で作成されたメーリングリストは、その後、複数のマーケティングアクティビティに使用できます。

## 宛先に接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL View Destinations]** および **[!UICONTROL Manage Destinations]**&#x200B;[&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

1. [!DNL Adobe Experience Platform] コンソール内で、**宛先** に移動します。

1. **カタログ** タブを選択し、*SendGrid* を検索します。 次に、「**設定**」を選択します。 宛先への接続を確立すると、UI のラベルが「**セグメントのアクティブ化**」に変わります。
   ![](../../assets/catalog/email-marketing/sendgrid/02-catalog.jpg)

1. SendGrid 宛先の設定に役立つウィザードが表示されます。 **新しい宛先を設定** を選択して新しい宛先を作成します。
   ![](../../assets/catalog/email-marketing/sendgrid/03.jpg)

1. 「**新規アカウント**」オプションを選択し、「**Bearer トークン** 値を入力します。 この値は、前述の *前提条件の節* で説明した [API キー &#x200B;](#prerequisites) です。
   ![](../../assets/catalog/email-marketing/sendgrid/04.jpg)

1. **宛先に接続** を選択します。 指定した SendGrid *API キー* が有効で、UI に **接続済み** ステータスと緑色のチェックマークが表示された場合、次の手順に進んで、追加の情報フィールドに入力できます。

![](../../assets/catalog/email-marketing/sendgrid/05.jpg)

### 宛先の詳細の入力 {#destination-details}

この宛先を[設定](https://experienceleague.adobe.com/docs/experience-platform/destinations/ui/connect-destination.html?lang=ja)するとき、次の情報を指定する必要があります。

* **[!UICONTROL Name]**：今後この宛先を認識するための名前。
* **[!UICONTROL Description]**：今後この宛先を識別するのに役立つオプションの説明です。

![](../../assets/catalog/email-marketing/sendgrid/06.jpg)

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続への詳細の入力を終えたら「**[!UICONTROL Next]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>* データをアクティブ化するには、**[!UICONTROL View Destinations]**、**[!UICONTROL Activate Destinations]**、**[!UICONTROL View Profiles]**、**[!UICONTROL View Segments]** [&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* *ID* を書き出すには、**[!UICONTROL View Identity Graph]** [&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。<br> ![&#x200B; 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択します。](/help/destinations/assets/overview/export-identities-to-destination.png " 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択 "){width="100" zoomable="yes"}

この宛先にオーディエンスをアクティベートする手順は、[ストリーミングオーディエンスの書き出し宛先へのプロファイルとオーディエンスのアクティベート](/help/destinations/ui/activate-segment-streaming-destinations.md)を参照してください。

この宛先に固有の詳細については、以下の画像を参照してください。

1. SendGrid にエクスポートする 1 つ以上のオーディエンスを選択します。
   ![](../../assets/catalog/email-marketing/sendgrid/11.jpg)

1. **[!UICONTROL Mapping]** の手順では、**[!UICONTROL Add new mapping]** を選択した後、マッピングページを表示して、ソース XDM フィールドを SendGrid API ターゲットフィールドにマッピングします。 以下の画像は、Experience Platformと SendGrid の間で ID 名前空間をマッピングする方法を示しています。 以下に示すように、**[!UICONTROL Source field]** *Email* を **[!UICONTROL Target field]** *external_id* にマッピングする必要があります。
   ![](../../assets/catalog/email-marketing/sendgrid/13.jpg)
   ![](../../assets/catalog/email-marketing/sendgrid/14.jpg)
   ![](../../assets/catalog/email-marketing/sendgrid/15.jpg)
   ![](../../assets/catalog/email-marketing/sendgrid/16.jpg)

1. 同様に、SendGrid 宛先に書き出す目的の [!DNL Adobe Experience Platform] 属性をマッピングします。
   ![](../../assets/catalog/email-marketing/sendgrid/17.jpg)
   ![](../../assets/catalog/email-marketing/sendgrid/18.jpg)

1. マッピングが完了したら、「**[!UICONTROL Next]**」を選択してレビュー画面に進みます。
   ![](../../assets/catalog/email-marketing/sendgrid/22.png)

1. **[!UICONTROL Finish]** を選択して設定を完了します。
   ![](../../assets/catalog/email-marketing/sendgrid/23.jpg)

[SendGrid マーケティング連絡先/連絡先を追加または更新 API](https://docs.sendgrid.com/api-reference/contacts/add-or-update-a-contact) 用に設定できる、サポートされる属性マッピングの包括的なリストを以下に示します。

| ソースフィールド | ターゲットフィールド | タイプ | 説明 | 制限 |
|---|---|---|---|---|
| xdm:<br/> homeAddress.street1 | xdm:<br/> address_line_1 | 文字列 | アドレスの最初のライン。 | 最大長：<br/> 100 文字 |
| xdm:<br/> homeAddress.street2 | xdm:<br/> address_line_2 | 文字列 | 住所の 2 行目（オプション）。 | 最大長：<br/> 100 文字 |
| xdm:<br/> _extcondev.alternate_emails | xdm:<br/> alternate_emails | 文字列の配列 | 連絡先に関連付けられている追加の E メール。 | <ul><li>最大：5 項目</li><li>最小：0 項目</li></ul> |
| xdm:<br/> homeAddress.city | xdm:<br/> city | 文字列 | 連絡先の市区町村です。 | 最大長：<br/> 60 文字 |
| xdm:<br/> homeAddress.country | xdm:<br/> country | 文字列 | 連絡先の国。 フルネームまたは略語を指定できます。 | 最大長：<br/> 50 文字 |
| identityMap:<br/> Email | ID:<br/> external_id | 文字列 | 連絡先のプライマリメール。 これは有効なメールである必要があります。 | 最大長：<br/> 254 文字 |
| xdm:<br/> person.name.firstName | xdm:<br/> first_name | 文字列 | 連絡先の名前 | 最大長：<br/> 50 文字 |
| xdm:<br/> person.name.lastName | xdm:<br/> last_name | 文字列 | 連絡先の姓 | 最大長：<br/> 50 文字 |
| xdm:<br/> homeAddress.postCode | xdm:<br/> postal_code | 文字列 | 連絡先の郵便番号。 | |
| xdm:<br/> homeAddress.stateProvince | xdm:<br/> state_province_region | 文字列 | 連絡先の都道府県または地域。 | 最大長：<br/> 50 文字 |

## SendGrid 内のデータエクスポートの検証 {#validate}

宛先が正しく設定されていることを検証するには、次の手順に従います。

1. **[!UICONTROL Destinations]**/**[!UICONTROL Browse]** を選択して、宛先のリストに移動します。
   ![](../../assets/catalog/email-marketing/sendgrid/25.jpg)

1. 宛先を選択し、ステータスが「**[!UICONTROL enabled]**」であることを確認します。
   ![](../../assets/catalog/email-marketing/sendgrid/26.jpg)

1. 「**[!DNL Activation data]**」タブに切り替えて、オーディエンス名を選択します。
   ![](../../assets/catalog/email-marketing/sendgrid/27.jpg)

1. オーディエンスの概要を監視し、プロファイルの数がデータセット内で作成された数に対応していることを確認します。
   ![](../../assets/catalog/email-marketing/sendgrid/28.jpg)

1. [SendGrid マーケティングリスト/リスト API の作成 &#x200B;](https://docs.sendgrid.com/api-reference/lists/create-list) は、*list_name* 属性の値とデータエクスポートのタイムスタンプを結合して、SendGrid 内に一意の連絡先リストを作成するために使用されます。 SendGrid サイトに移動し、名前パターンに準拠する新しい連絡先リストが作成されているかどうかを確認します。
   ![](../../assets/catalog/email-marketing/sendgrid/29.jpg)
   ![](../../assets/catalog/email-marketing/sendgrid/30.jpg)

1. 新しく作成した連絡先リストを選択し、作成したデータセットの新しいメールレコードが新しい連絡先リスト内に入力されているかどうかを確認します。

1. さらに、いくつかのメールをチェックして、フィールドマッピングが正しいかどうかを検証します。
   ![](../../assets/catalog/email-marketing/sendgrid/31.jpg)
   ![](../../assets/catalog/email-marketing/sendgrid/32.jpg)

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのようにデータガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。

## その他のリソース {#additional-resources}

この SendGrid 宛先は、以下の API を利用します。

* [SendGrid マーケティングリスト/リスト API の作成 &#x200B;](https://docs.sendgrid.com/api-reference/lists/create-list)
* [SendGrid マーケティング連絡先/連絡先 API の追加または更新 &#x200B;](https://docs.sendgrid.com/api-reference/contacts/add-or-update-a-contact)

### 制限 {#limits}

* [SendGrid マーケティング連絡先/連絡先を追加または更新 API](https://api.sendgrid.com/v3/marketing/contacts) は、30,000 件の連絡先または 6 MB のデータのいずれか低い方を受け入れることができます。
