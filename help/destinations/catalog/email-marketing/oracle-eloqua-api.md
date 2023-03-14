---
title: (API)OracleEloqua 接続
description: (API)Oracleの Eloqua の宛先を使用すると、アカウントデータを書き出し、ビジネスニーズに合わせてOracleEloqua 内でアクティブ化できます。
last-substantial-update: 2023-03-14T00:00:00Z
source-git-commit: 3197eddcf9fef2870589fdf9f09276a333f30cd1
workflow-type: tm+mt
source-wordcount: '1494'
ht-degree: 41%

---

# [!DNL (API) Oracle Eloqua] 接続

[[!DNL Oracle Eloqua]](https://www.oracle.com/cx/marketing/automation/) マーケターは、見込み客に合わせてパーソナライズされたカスタマーエクスペリエンスを提供しながら、キャンペーンを計画および実行できます。 統合されたリード管理と簡単なキャンペーン作成により、マーケターは適切なタイミングで適切なオーディエンスを購入者のジャーニーにエンゲージし、電子メール、ディスプレイ検索、ビデオ、モバイルなどの様々なチャネルでオーディエンスに到達できます。 セールスチームは、より迅速に取引を成立させ、リアルタイムのインサイトを通じてマーケティングの ROI を向上させることができます。

この [!DNL Adobe Experience Platform] [宛先](/help/destinations/home.md) は [連絡先の更新](https://docs.oracle.com/en/cloud/saas/marketing/eloqua-rest-api/op-api-rest-1.0-data-contact-id-put.html) 操作 [!DNL Oracle Eloqua] REST API。セグメント内の ID を [!DNL Oracle Eloqua].

[!DNL Oracle Eloqua] uses [基本認証](https://docs.oracle.com/en/cloud/saas/marketing/eloqua-rest-api/Authentication_Basic.html) 話をする [!DNL Oracle Eloqua] REST API。 [!DNL Oracle Eloqua] インスタンスを認証する手順は、さらに下の[宛先に対する認証](#authenticate)の節にあります。

## ユースケース {#use-cases}

マーケターは、Adobe Experience Platform プロファイルの属性に基づいて、ユーザーにパーソナライズされたエクスペリエンスを提供できます。 オフラインデータからセグメントを作成し、それらのセグメントを [!DNL Oracle Eloqua] に送信して、Adobe Experience Platform でセグメントとプロファイルが更新されるとすぐにユーザーのフィードに表示することができます。

## 前提条件 {#prerequisites}

### Experience Platform の前提条件 {#prerequisites-in-experience-platform}

[!DNL Oracle Eloqua] 宛先へのデータをアクティブ化する前に、[スキーマ](/help/xdm/schema/composition.md)、[データセット](https://experienceleague.adobe.com/docs/platform-learn/tutorials/data-ingestion/create-datasets-and-ingest-data.html?lang=ja)および[セグメント](https://experienceleague.adobe.com/docs/platform-learn/tutorials/segments/create-segments.html?lang=ja)を [!DNL Experience Platform] で作成する必要があります。

詳しくは、Experience Platformドキュメントを参照してください。 [セグメントメンバーシップの詳細スキーマフィールドグループ](/help/xdm/field-groups/profile/segmentation.md) セグメントのステータスに関するガイダンスが必要な場合。

### [!DNL Oracle Eloqua] 前提条件 {#prerequisites-destination}

Platform からにデータを書き出すには、以下を実行します。 [!DNL Oracle Eloqua] アカウントに [!DNL Oracle Eloqua] アカウント

#### [!DNL Oracle Eloqua] 資格情報の収集 {#gather-credentials}

[!DNL Oracle Eloqua] 宛先に対して認証を行う前に、以下の項目をメモしておきます。

| 資格情報 | 説明 |
| --- | --- |
| `Username` | ユーザー名 [!DNL Oracle Eloqua] アカウント |
| `Password` | ユーザーのパスワード [!DNL Oracle Eloqua] アカウント |

## ガードレール {#guardrails}

>[!NOTE]
>
>* [!DNL Oracle Eloqua] カスタム連絡先フィールドは、 **[!UICONTROL セグメントを選択]** 手順


* [!DNL Oracle Eloqua] には、250 個のカスタム連絡先フィールドの上限があります。
* 新しいセグメントを書き出す前に、Platform セグメントの数と、内の既存のセグメントの数を確認してください [!DNL Oracle Eloqua] この制限を超えないでください。
* この制限を超えると、Experience Platform中にエラーが発生します。 これは、 [!DNL Oracle Eloqua] API はリクエストの検証に失敗し、 — を使用して応答します。 *400:検証エラーが発生しました*  — 問題を説明するエラーメッセージ。
* 上記の制限に達した場合は、既存のマッピングを宛先から削除し、 [!DNL Oracle Eloqua] アカウントを使用して、さらに多くのセグメントを書き出すことができます。

* 詳しくは、 [OracleEloqua 連絡先フィールドの作成](https://docs.oracle.com/en/cloud/saas/marketing/eloqua-user/Help/ContactFields/Tasks/CreatingContactFields.htm) ページを参照してください。

## サポートされる ID {#supported-identities}

[!DNL Oracle Eloqua] では、以下の表で説明する ID の更新をサポートしています。[ID](/help/identity-service/namespaces.md) についての詳細情報。

| ターゲット ID | 例 | 説明 | 必須 |
|---|---|---|---|
| `EloquaId` | `111111` | 連絡先の一意の ID。 | ○ |

## 宛先への接続 {#connect}

>[!IMPORTANT]
>
>宛先に接続するには、**[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

**[!UICONTROL 宛先]**／**[!UICONTROL カタログ]**&#x200B;内で [!DNL (API) Oracle Eloqua] を検索します。または、 **[!UICONTROL メールマーケティング]** カテゴリ。

### 宛先に対する認証 {#authenticate}

以下の必須のフィールドに入力します。詳しくは、[ [!DNL Oracle Eloqua]  資格情報の収集](#gather-credentials)の節を参照してください。
* **[!UICONTROL パスワード]**:ユーザーのパスワード [!DNL Oracle Eloqua] アカウント
* **[!UICONTROL ユーザー名]**:ユーザー名 [!DNL Oracle Eloqua] アカウント

宛先を認証するには、「 **[!UICONTROL 宛先に接続]**」を選択します。
![認証方法を示す Platform UI のスクリーンショット。](../../assets/catalog/email-marketing/oracle-eloqua-api/authenticate-destination.png)

指定した詳細が有効な場合、UI で&#x200B;**[!UICONTROL 接続済み]**&#x200B;ステータスに緑色のチェックマークが付きます。その後、次の手順に進むことができます。

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横にアスタリスクが表示される場合は、そのフィールドが必須であることを示します。
![宛先の詳細を示す Platform UI のスクリーンショット。](../../assets/catalog/email-marketing/oracle-eloqua-api/destination-details.png)

* **[!UICONTROL 名前]**：今後この宛先を認識するための名前。
* **[!UICONTROL 説明]**：今後この宛先を識別するのに役立つ説明。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してセグメントをアクティブ化 {#activate}

>[!IMPORTANT]
>
>データをアクティブ化するには、**[!UICONTROL 宛先の管理]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]**&#x200B;および&#x200B;**[!UICONTROL セグメントの表示]**[に対するアクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先にオーディエンスセグメントをアクティベートする手順は、[ストリーミングセグメントの書き出し宛先へのプロファイルとセグメントのアクティベート](/help/destinations/ui/activate-segment-streaming-destinations.md)を参照してください。

### マッピングの考慮事項と例 {#mapping-considerations-example}

Adobe Experience Platform から [!DNL Oracle Eloqua] 宛先にオーディエンスデータを正しく送信するには、フィールドマッピングの手順を実行する必要があります。マッピングは、Platform アカウント内の Experience Data Model（XDM）スキーマフィールドと、ターゲット宛先から対応する同等のスキーマフィールドとの間にリンクを作成して構成されます。 

`EloquaID` は、ID に対応する属性を更新するために必要です。 この `emailAddress` がない場合も、API が以下に示すようにエラーをスローするのと同様に必要です。

```json
{
   "type":"ObjectValidationError",
   "container":{
      "type":"ObjectKey",
      "objectType":"Contact"
   },
   "property":"emailAddress",
   "requirement":{
      "type":"EmailAddressRequirement"
   },
   "value":"<null>"
}
```

で指定された属性 **[!UICONTROL ターゲットフィールド]** は、属性マッピングテーブルで説明されたとおりに名前を付ける必要があります。これらの属性は、リクエスト本文を形成します。

で指定された属性 **[!UICONTROL ソースフィールド]** このような制限に従わないでください。 ただし、にプッシュした際にデータ形式が正しくない場合は、必要に応じてマッピングできます。 [!DNL Oracle Eloqua] これはエラーを引き起こします。

例えば、 **[!UICONTROL ソースフィールド]** id 名前空間 `contact key`, `ABC ID` など から **[!UICONTROL ターゲットフィールド]** : `EloquaID` ID 値が、 [!DNL Oracle Eloqua].

XDM フィールドを [!DNL Oracle Eloqua] 宛先フィールドに正しくマッピングするには、次の手順に従います。

1. **[!UICONTROL マッピング]**&#x200B;手順で、「**[!UICONTROL 新しいマッピングを追加]**」を選択します。画面に新しいマッピング行が表示されます。
1. 内 **[!UICONTROL ソースフィールドを選択]** ウィンドウで、 **[!UICONTROL 属性を選択]** カテゴリを選択して XDM 属性を選択するか、 **[!UICONTROL ID 名前空間を選択]** ID を選択します。
1. 内 **[!UICONTROL ターゲットフィールドを選択]** ウィンドウで、 **[!UICONTROL ID 名前空間を選択]** ID を選択するか、 **[!UICONTROL カスタム属性を選択]** カテゴリを選択し、必要に応じて属性を選択します。
   * これらの手順を繰り返して、XDM プロファイルスキーマと [!DNL Oracle Eloqua] インスタンス： |ソースフィールド|ターゲットフィールド|必須| |—|—|—| |`xdm: personalEmail.address`|`Attribute: emailAddress`|はい | |`IdentityMap: Eid`|`Identity: EloquaId`|はい |

   * これらのマッピングの使用例を次に示します。
      ![属性マッピングを使用した Platform UI のスクリーンショットの例。](../../assets/catalog/email-marketing/oracle-eloqua-api/mappings.png)

      >[!IMPORTANT]
      >
      >両方の `emailAddress` および `EloquaId` ターゲット属性のマッピングは必須です。

宛先接続のマッピングの指定が完了したら、「 」を選択します。 **[!UICONTROL 次へ]**.

>[!NOTE]
>
>宛先は、連絡先フィールド情報をに送信する際に、各実行で選択されたセグメント名に、一意の識別子を自動的にサフィックス付けします。 [!DNL Oracle Eloqua]. これにより、セグメント名に対応する連絡先フィールド名が重複しなくなります。 詳しくは、 [データの書き出しを検証する](#exported-data) セクションのスクリーンショットの例 [!DNL Oracle Eloqua] セグメント名を使用して作成されたカスタム連絡先フィールドを含む連絡先詳細ページ。

## データの書き出しを検証する {#exported-data}

宛先が正しく設定されていることを検証するには、次の手順に従います。

1. 選択 **[!UICONTROL 宛先]** > **[!UICONTROL 参照]** をクリックし、宛先のリストに移動します。
1. 次に、宛先を選択し、 **[!UICONTROL アクティベーションデータ]** 」タブをクリックし、セグメント名を選択します。
   ![宛先のアクティベーションデータを示した Platform UI のスクリーンショットの例。](../../assets/catalog/email-marketing/oracle-eloqua-api/destinations-activation-data.png)

1. セグメント概要を監視し、プロファイルの数がセグメント内の数に対応していることを確認します。
   ![セグメントを示す Platform UI のスクリーンショットの例。](../../assets/catalog/email-marketing/oracle-eloqua-api/segment.png)

1. にログインします。 [!DNL Oracle Eloqua] web サイトに移動し、 **[!UICONTROL 連絡先の概要]** ページを開いて、セグメントのプロファイルが追加されたかどうかを確認します。 セグメントのステータスを確認するには、 **[!UICONTROL 連絡先の詳細]** ページを開き、選択したセグメント名をプレフィックスとして持つ連絡先フィールドが作成されたかどうかを確認します。

![Oracle名で作成されたカスタム連絡先フィールドを含む連絡先詳細ページを示す Eloqua UI のスクリーンショット。](../../assets/catalog/email-marketing/oracle-eloqua-api/contact.png)

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのようにデータガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。

## エラーとトラブルシューティング {#errors-and-troubleshooting}

宛先を作成する際に、次のエラーメッセージが表示される場合があります。 `400: There was a validation error` または `400 BAD_REQUEST`. この問題は、カスタム連絡先フィールドの上限である 250 件を超えた場合に発生します。詳しくは、 [guardrail](#guardrails) 」セクションに入力します。 このエラーを修正するには、 [!DNL Oracle Eloqua].
![エラーを示す Platform UI のスクリーンショット。](../../assets/catalog/email-marketing/oracle-eloqua-api/error.png)

詳しくは、 [[!DNL Oracle Eloqua] HTTP ステータスコード](https://docs.oracle.com/en/cloud/saas/marketing/eloqua-rest-api/APIRequests_HTTPStatusCodes.html) および [[!DNL Oracle Eloqua] 検証エラー](https://docs.oracle.com/en/cloud/saas/marketing/eloqua-rest-api/APIRequests_HTTPValidationErrors.html) のページで、ステータスとエラーコードの包括的なリストと説明を参照できます。

## その他のリソース {#additional-resources}

[!DNL Oracle ELoqua] ドキュメントからのその他の役に立つ情報は次のとおりです。
* [OracleEloqua マーケティング自動化](https://docs.oracle.com/en/cloud/saas/marketing/eloqua.html)
* [oracleEloquaMarketing Cloudサービス用 REST API](https://docs.oracle.com/en/cloud/saas/marketing/eloqua-rest-api/rest-endpoints.html)