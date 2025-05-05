---
keywords: crm;CRM;CRM 宛先；アウトリーチ；アウトリーチ CRM 宛先
title: アウトリーチ接続
description: アウトリーチの宛先を使用すると、アカウントデータを書き出し、ビジネスニーズに合わせてアウトリーチ内でアクティブ化できます。
exl-id: 7433933d-7a4e-441d-8629-a09cb77d5220
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '1698'
ht-degree: 38%

---

# [!DNL Outreach] 接続

## 概要 {#overview}

[[!DNL Outreach]](https://www.outreach.io/) は、世界で最も B2B のバイヤーとセラーのインタラクションデータを扱う Sales Execution Platform で、販売データをインテリジェンスに変換するための独自の AI テクノロジーへの大量の投資を行っています。[!DNL Outreach] は、組織がセールスエンゲージメントを自動化、収益インテリジェンスに基づいて行動し、効率、予測可能性、成長を向上させるのに役立ちます。

この [!DNL Adobe Experience Platform][ 宛先 ](/help/destinations/home.md) は、[Outreach Update Resource API](https://api.outreach.io/api/v2/docs#update-an-existing-resource) を活用しており、[!DNL Outreach] の見込み客に対応する、オーディエンス内の ID を更新できます。

[!DNL Outreach] は、認証付与を使用する OAuth 2 を認証メカニズムとして使用して、[!DNL Outreach] [!DNL Update Resource API] と通信します。 [!DNL Outreach] インスタンスを認証する手順は、さらに下の [ 宛先に対する認証 ](#authenticate) の節にあります。

## ユースケース {#use-cases}

マーケターは、Adobe Experience Platform プロファイルの属性に基づいて、パーソナライズされたエクスペリエンスを見込み客に提供できます。 オフラインデータからオーディエンスを作成し、それらのオーディエンスを [!DNL Outreach] に送信して、Adobe Experience Platformでオーディエンスとプロファイルが更新されるとすぐに見込み客のフィードに表示することができます。

## 前提条件 {#prerequisites}

### Experience Platform の前提条件 {#prerequisites-in-experience-platform}

[!DNL Outreach] 宛先へのデータをアクティブ化する前に、[スキーマ](/help/xdm/schema/composition.md)、[データセット](https://experienceleague.adobe.com/docs/platform-learn/tutorials/data-ingestion/create-datasets-and-ingest-data.html?lang=ja)および[セグメント](https://experienceleague.adobe.com/docs/platform-learn/tutorials/segments/create-segments.html?lang=ja)を [!DNL Experience Platform] で作成する必要があります。

オーディエンスのステータスに関するガイダンスが必要な場合は、[ オーディエンスメンバーシップの詳細スキーマフィールドグループ ](/help/xdm/field-groups/profile/segmentation.md) に関するAdobeのドキュメントを参照してください。

### アウトリーチの前提条件 {#prerequisites-destination}

Experience Platformから [!DNL Outreach] アカウントにデータを書き出すには、[!DNL Outreach] で次の前提条件に注意してください。

#### アウトリーチアカウントが必要です {#prerequisites-account}

アカウントをまだお持ちでない場合は、[!DNL Outreach] [ ログイン ](https://accounts.outreach.io/users/sign_in) ページに移動し、アカウントを登録して作成してください。 詳しくは、[!DNL Outreach] サポート [ ページ ](https://support.outreach.io/hc/en-us/articles/207238607-Claim-Your-Outreach-Account) も参照してください。

[!DNL Outreach] CRM 宛先に対して認証を行う前に、以下の項目をメモしておきます。

| 資格情報 | 説明 |
|---|---|
| メール | [!DNL Outreach] アカウントのメール |
| パスワード | [!DNL Outreach] アカウントのパスワード |

#### カスタムフィールドラベルの設定 {#prerequisites-custom-fields}

[!DNL Outreach] では、[ 見込み客 ](https://support.outreach.io/hc/en-us/articles/360001557554-Outreach-Prospect-Profile-Overview) のカスタムフィールドをサポートしています。 詳しくは、[ アウトリーチでカスタムフィールドを追加する方法 ](https://support.outreach.io/hc/en-us/articles/219124908-How-To-Add-a-Custom-Field-in-Outreach) を参照してください。 識別を容易にするために、デフォルト値を維持するのではなく、対応するオーディエンス名に手動でラベルを更新することをお勧めします。 以下に例を示します。

カ [!DNL Outreach] タムフィールドを表示している見込み客の設定ページ
![ 設定ページのカスタムフィールドを示すアウトリーチ UI のスクリーンショット。](../../assets/catalog/crm/outreach/outreach-custom-fields.png)

オーディエンス名に一致する *ユーザーにとってわかりやすい* ラベルを持つカスタムフィールドを表示する見込み客の [!DNL Outreach] 設定ページ。 これらのラベルに対して、見込み客ページでオーディエンスステータスを表示できます。
![ 設定ページの関連するラベルを持つカスタムフィールドを示すアウトリーチ UI のスクリーンショット。](../../assets/catalog/crm/outreach/outreach-custom-field-labels.png)

>[!NOTE]
>
> ラベル名は識別を容易にするためにのみ使用されます。 見込み客の更新時には使用されません。

## ガードレール

[!DNL Outreach] API のレート制限は、ユーザーごとに 1 時間あたり 10,000 リクエストです。 この制限に達すると、次のメッセージを含む `429` 応答が返されます：`You have exceeded your permitted rate limit of 10,000; please try again at 2017-01-01T00:00:00.`。

このメッセージが表示された場合は、レートしきい値に準拠するようにオーディエンスの書き出しスケジュールを更新する必要があります。

詳しくは、[[!DNL Outreach]  ドキュメント ](https://api.outreach.io/api/v2/docs#rate-limiting) を参照してください。

## サポートされる ID {#supported-identities}

[!DNL Outreach] では、以下の表で説明する ID の更新をサポートしています。[ID](/help/identity-service/features/namespaces.md) についての詳細情報。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| `OutreachId` | <ul><li>[!DNL Outreach] 識別子。 これは、見込み客プロファイルに対応する数値です。</li><li>ID は、更新する見込み客の [!DNL Outreach] URL 内の ID と一致する必要があります。</li><li>詳しくは、[[!DNL Outreach] ドキュメント](https://api.outreach.io/api/v2/docs#update-an-existing-resource)を参照してください。</li></ul> | 必須 |

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL プロファイルベース]** | <ul><li> セグメントのすべてのメンバーを、フィールドマッピングに従って、必要なスキーマフィールドと共に書き出します&#x200B;*（例：メールアドレス、電話番号、姓）*。</li><li> [!DNL Outreach] の各セグメントのステータスは、[ オーディエンススケジュール ](#schedule-segment-export-example) 手順で提供された [!UICONTROL &#x200B; マッピング ID] 値に基づいて、Experience Platformの対応するオーディエンスステータスとともに更新されます。</li></ul> |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | <ul><li> ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。詳しくは、[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)を参照してください。</li></ul> |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
> 宛先に接続するには、**[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

**[!UICONTROL 宛先]**／**[!UICONTROL カタログ]**&#x200B;内で [!DNL Outreach] を検索します。または、CRM カテゴリの下に配置することもできます。

### 宛先に対する認証 {#authenticate}

宛先を認証するには、「**[!UICONTROL 宛先に接続]**」を選択します。

![ アウトリーチへの認証方法を示すExperience Platform UI のスクリーンショット。](../../assets/catalog/crm/outreach/authenticate-destination.png)

[!DNL Outreach] ログインページが表示されます。 メールを入力します。

![ アウトリーチ UI のスクリーンショットで、アウトリーチを認証するメールを入力するフィールドを示す ](../../assets/catalog/crm/outreach/authenticate-destination-login-email.png)

次に、パスワードを入力します。

![ アウトリーチ UI のスクリーンショットで、アウトリーチに対する認証を行うためのパスワード入力ステップのフィールドを示している ](../../assets/catalog/crm/outreach/authenticate-destination-login-password.png)

* **[!UICONTROL ユーザー名]**:[!DNL Outreach] アカウントのメールアドレス。
* **[!UICONTROL パスワード]**:[!DNL Outreach] アカウントのパスワード。

指定した詳細が有効な場合、UI で&#x200B;**接続済み**&#x200B;ステータスに緑色のチェックマークが付きます。その後、次の手順に進むことができます。

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横にアスタリスクが表示される場合は、そのフィールドが必須であることを示します。
![ アウトリーチ先の詳細を入力する方法を示すExperience Platform UI のスクリーンショット。](../../assets/catalog/crm/outreach/destination-details.png)

* **[!UICONTROL 名前]**：今後この宛先を認識するための名前。
* **[!UICONTROL 説明]**：今後この宛先を識別するのに役立つ説明。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>* データをアクティブ化するには、**[!UICONTROL 宛先の表示]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]** および **[!UICONTROL セグメントの表示]**&#x200B;[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* *ID* を書き出すには、**[!UICONTROL ID グラフの表示]**&#x200B;[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。<br> ![ 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択します。](/help/destinations/assets/overview/export-identities-to-destination.png " 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択 "){width="100" zoomable="yes"}

この宛先にオーディエンスをアクティベートする手順は、[ストリーミングオーディエンスの書き出し宛先へのプロファイルとオーディエンスのアクティベート](../../ui/activate-segment-streaming-destinations.md)を参照してください。

### マッピングの考慮事項と例 {#mapping-considerations-example}

Adobe Experience Platform から [!DNL Outreach] 宛先にオーディエンスデータを正しく送信するには、フィールドマッピングの手順を実行する必要があります。マッピングは、Experience Platform アカウント内の Experience Data Model （XDM）スキーマフィールドと、ターゲット宛先から対応する同等のスキーマフィールドとの間にリンクを作成して構成されます。 XDM フィールドを [!DNL Outreach] 宛先フィールドに正しくマッピングするには、次の手順に従います。

1. [!UICONTROL &#x200B; マッピング &#x200B;] 手順で、「**[!UICONTROL 新しいマッピングを追加]**」をクリックします。 画面に新しいマッピング行が表示されます。
   ![ 新しいマッピングの追加方法を示すExperience Platform UI のスクリーンショット ](../../assets/catalog/crm/outreach/add-new-mapping.png)

1. [!UICONTROL &#x200B; ソースフィールドを選択 &#x200B;] ウィンドウで、**[!UICONTROL ID 名前空間を選択]** カテゴリを選択して、目的のマッピングを追加します。
   ![Sourceのマッピングを示すExperience Platform UI のスクリーンショット ](../../assets/catalog/crm/outreach/source-mapping.png)

1. [!UICONTROL ターゲットフィールドを選択]ウィンドウで、ソースフィールドにマッピングするターゲットフィールドのタイプを選択します。
   * **[!UICONTROL ID 名前空間を選択]**：このオプションを選択して、ソースフィールドをリストから ID 名前空間にマッピングします。

     ![OutreachId.](../../assets/catalog/crm/outreach/target-mapping.png) を使用したターゲットマッピングを示すExperience Platform UI のスクリーンショット

   * XDM プロファイルスキーマと [!DNL Outreach] インスタンスの間に次のマッピングを追加します。

     | XDM プロファイルスキーマ | [!DNL Outreach] Instance | 必須 |
     |---|---|---|
     | `Oid` | `OutreachId` | ○ |

   * **[!UICONTROL カスタム属性を選択]**：このオプションを選択して、「[!UICONTROL 属性名]」フィールドに定義するカスタム属性にマッピングするソースフィールドを選択します。サポートされる属性の包括的なリストについては、[[!DNL Outreach]  見込み客ドキュメント ](https://api.outreach.io/api/v2/docs#prospect) を参照してください。

     ![LastName.](../../assets/catalog/crm/outreach/target-mapping-lastname.png) を使用したターゲットマッピングを示すExperience Platform UI のスクリーンショット

   * 例えば、更新する値に応じて、XDM プロファイルスキーマと [!DNL Outreach] インスタンスの間に次のようなマッピングを追加します。

     | XDM プロファイルスキーマ | [!DNL Outreach] Instance |
     |---|---|
     | `person.name.firstName` | `firstName` |
     | `person.name.lastName` | `lastName` |

   * これらのマッピングの使用例を次に示します。

     ![ ターゲットマッピングを示したExperience Platform UI のスクリーンショットの例。](../../assets/catalog/crm/outreach/mappings.png)

### オーディエンスの書き出しのスケジュールと例 {#schedule-segment-export-example}

* [ オーディエンスの書き出しをスケジュール ](../../ui/activate-segment-streaming-destinations.md) 手順を実行する場合は、Experience Platform オーディエンスを [!DNL Outreach] のカスタムフィールド属性に手動でマッピングする必要があります。

* これを行うには、各セグメントを選択し、「**[!UICONTROL マッピング ID]**」フィールドの [!DNL Outreach] にある *カスタムフィールド `N` ラベル* フィールドに対応する数値を入力します。

  >[!IMPORTANT]
  >
  > * [!UICONTROL &#x200B; マッピング ID] 内で使用される数値 *（`N`）* は、[!DNL Outreach] 内の数値でサフィックス付きのカスタム属性キーと一致する必要があります。 例：*ラベル `N` カスタムフィールド*。
  > * 指定する必要があるのは数値だけで、カスタムフィールドラベル全体は指定できません。
  > * [!DNL Outreach] では、最大 150 個のカスタムラベルフィールドをサポートしています。
  > * 詳しくは、[[!DNL Outreach]  見込み客ドキュメント ](https://api.outreach.io/api/v2/docs#prospect) を参照してください。

   * 例：

     | [!DNL Outreach] フィールド | Experience Platform マッピング ID |
     |---|---|
     | ラベル `4` カスタムフィールド | `4` |

     ![ スケジュールのオーディエンス書き出し時のマッピング ID の例を示すExperience Platform UI のスクリーンショット。](../../assets/catalog/crm/outreach/schedule-segment-export.png)

## データの書き出しを検証する {#exported-data}

宛先が正しく設定されていることを検証するには、次の手順に従います。

1. **[!UICONTROL 宛先]**／**[!UICONTROL 参照]** を選択して、宛先のリストに移動します。
   ![ 宛先の参照を示すExperience Platform UI のスクリーンショット。](../../assets/catalog/crm/outreach/browse-destinations.png)

1. 宛先を選択し、ステータスが「 **[!UICONTROL 有効]**」であることを確認します。
   ![ 選択した宛先の宛先データフロー実行を示したExperience Platform UI のスクリーンショット。](../../assets/catalog/crm/outreach/destination-dataflow-run.png)

1. 「**[!DNL Activation data]**」タブに切り替えて、オーディエンス名を選択します。
   ![ 宛先のアクティベーションデータを示したExperience Platform UI のスクリーンショット。](../../assets/catalog/crm/outreach/destinations-activation-data.png)

1. オーディエンスの概要を監視し、プロファイルの数がセグメント内で作成された数と一致していることを確認します。
   ![ セグメントの概要を示すExperience Platform UI のスクリーンショット。](../../assets/catalog/crm/outreach/segment.png)

1. [!DNL Outreach] web サイトににログインして、[!DNL Apps] / [!DNL Contacts] ページに移動し、オーディエンスのプロファイルが追加されたかどうかを確認します。 [!DNL Outreach] の各オーディエンスステータスが、[ オーディエンスのスケジュール設定 ](#schedule-segment-export-example) 手順で提供された [!UICONTROL &#x200B; マッピング ID] 値に基づいて、Experience Platformの対応するオーディエンスステータスで更新されたことがわかります。

![ 更新されたオーディエンスステータスを含むアウトリーチ見込み客ページを示すアウトリーチ UI のスクリーンショット。](../../assets/catalog/crm/outreach/outreach-prospect.png)

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのようにデータガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。

## エラーとトラブルシューティング {#errors-and-troubleshooting}

データフローの実行を確認すると、次のエラーメッセージが表示される場合があります。`Bad request reported while pushing events to the destination. Please contact the administrator and try again.`

![ 無効なリクエストエラーを示すExperience Platform UI のスクリーンショット。](../../assets/catalog/crm/outreach/error.png)

このエラーを修正するには、Experience Platformで指定した [!DNL Outreach] オーディエンスの [!UICONTROL &#x200B; マッピング ID] が有効であり、[!DNL Outreach] に存在することを確認します。

## その他のリソース {#additional-resources}

[[!DNL Outreach]  ドキュメント ](https://api.outreach.io/api/v2/docs/) には、問題のデバッグに使用できる [ エラー応答 ](https://api.outreach.io/api/v2/docs#error-responses) に関する詳細があります。
