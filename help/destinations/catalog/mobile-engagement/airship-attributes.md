---
keywords: airship attributes;airship destination
title: 航空船属性の宛先
seo-title: 航空船属性の宛先
description: Airship内でターゲティングするために、Adobeオーディエンスデータをオーディエンス属性としてAirshipにシームレスに渡します。
seo-description: Airship内でターゲティングするために、Adobeオーディエンスデータをオーディエンス属性としてAirshipにシームレスに渡します。
translation-type: tm+mt
source-git-commit: 24c8dd0f01d7ea14b2fa5827722e797bd209f50c
workflow-type: tm+mt
source-wordcount: '1236'
ht-degree: 13%

---


# （ベータ） [!DNL Airship Attributes] 宛先 {#airship-attributes-destination}

>[!IMPORTANT]
>
>Adobe Experience Platformの [!DNL Airship Attributes] 目的地は現在ベータ段階です。 ドキュメントと機能は変更される場合があります。

## 概要 {#overview}

[!DNL Airship] は、顧客関与プラットフォームをリードし、顧客のライフサイクルのあらゆる段階で、意味のあるパーソナライズされたチャネルのメッセージをユーザに届けるのに役立ちます。

この統合は、ターゲティングまたはトリガーのた [!DNL Airship] めに、Adobeプロファイルデータを [属性](https://docs.airship.com/guides/audience/attributes/) としてに渡します。

詳しくは、 [!DNL Airship]航空船ドキュメント [を参照してください](https://docs.airship.com)。


>[!TIP]
>
>このドキュメントページはチー [!DNL Airship] ムが作成したページです。 お問い合わせや更新のご依頼は、 [support.airship.comから直接お問い合わせください](https://support.airship.com/)。

## 前提条件 {#prerequisites}

オーディエンスセグメントをに送信する前に、次の操作を行う [!DNL Airship]必要があります。

* プロジ [!DNL Airship] ェクトで属性を有効にします。
* 認証用のベアラートークンを生成します。

>[!TIP]
>
>まだアカウントを作成していない場合は、 [!DNL Airship] こ [のサインアップリンクを使用してアカウントを作成します](https://go.airship.eu/accounts/register/plan/starter/) 。

### 属性を有効にする {#enable-attributes}

Adobe Experience Platformプロファイルの属性は [!DNL Airship] 属性に似ており、このページで後述するマッピングツールを使用して、プラットフォームで相互に容易にマッピングできます。

[!DNL Airship] プロジェクトには、定義済みの属性とデフォルトの属性がいくつかあります。 カスタム属性がある場合は、最初に定義する必要があり [!DNL Airship] ます。 詳細は、「属性 [の設定と管理](https://docs.airship.com/tutorials/audience/attributes/) 」を参照してください。

### ベアラートークン {#bearer-token}

「 **[!UICONTROL 設定]** 」に移動し、 **[!UICONTROL Airshipダッシュボード]** の「APIs &amp; Integrations」に移動し、左側のメニューで「 [](https://go.airship.com)**** Tokens」を選択します。

「トークン **[!UICONTROL を作成]**」をクリックします。

トークンにわかりやすい名前(例：「Adobe属性の保存先」)を指定し、ロールに「すべてのアクセス」を選択します。

「トークン **[!UICONTROL の作成]** 」をクリックし、詳細を機密情報として保存します。

## 使用例 {#use-cases}

To help you better understand how and when you should use the [!DNL Airship Attributes] destination, here are sample use cases that Adobe Experience Platform customers can solve by using this destination.

### 使用例1

Adobe Experience Platform内で収集されたプロファイルデータを活用して、チャネル内のメッセージやリッチコンテンツをパーソナライズ [!DNL Airship]できます。 例えば、 [!DNL Experience Platform] プロファイルデータを利用して、内で場所属性を設定し [!DNL Airship]ます。 これにより、ホテルのブランドで各ユーザーの最も近いホテルの場所の画像を表示できます。

### 使用例2

Adobe Experience Platformの属性を活用して、 [!DNL Airship] プロファイルをさらに豊富にし、SDKや [!DNL Airship] 予測データと組み合わせます。 例えば、ある小売業者は忠誠度のステータスと場所のデータ（プラットフォームの属性）を持つセグメントを作成し、高ターゲットのメッセージを、ネバダ州ラスベガスに住む金忠誠度のユーザーに送信するためにデータを変換すると予測できます。 [!DNL Airship]

## Connect to [!DNL Airship Attributes] {#connect-airship-attributes}

「 **[!UICONTROL Destinations]** / **[!UICONTROL Catalog]**」で、「 **[!UICONTROL Mobile Engagement]** 」カテゴリまでスクロールします。 を選択 **[!DNL Airship Attributes]**&#x200B;し、「 **[!UICONTROL 設定]**」を選択します。

>[!NOTE]
>
>この宛先との接続が既に存在する場合は、宛先カードに **[!UICONTROL 「アクティブ化]** 」ボタンが表示されます。 「 **[!UICONTROL アクティブ化]** 」と「 **[!UICONTROL 設定]**」の違いについて詳しくは、表示先ワークスペースのドキュメントの「 [カタログ](../../ui/destinations-workspace.md#catalog) 」セクションを参照してください。

![Airship属性に接続](../../assets/catalog/mobile-engagement/airship/catalog.png)

In the **Account** step, if you had previously set up a connection to your [!DNL Airship Attributes] destination, select **[!UICONTROL Existing Account]** and select your existing connection. Or, you can select **[!UICONTROL New Account]** to set up a new connection to [!DNL Airship Attributes]. 「宛先 **[!UICONTROL に接続]** 」を選択し、 [!DNL Airship] ダッシュボードから生成したベアラトークンを使用して、Adobe Experience Platformを [!DNL Airship] プロジェクトに接続します。

>[!NOTE]
>
>Adobe Experience Platform supports credentials validation in the authentication process and displays an error message if you input incorrect credentials to your [!DNL Airship] account. このため、間違った資格情報を使用すると、ワークフローを完了することができません。

![Airship属性に接続](../../assets/catalog/mobile-engagement/airship/connect.png)

Once your credentials are confirmed and Adobe Experience Platform is connected to your [!DNL Airship] project, you can select **[!UICONTROL Next]** to proceed to the **[!UICONTROL Setup]** step.

In the **[!UICONTROL Authentication]** step, enter a **[!UICONTROL Name]** and a **[!UICONTROL Description]** for your activation flow.

また、この手順では、米国またはEUのデータセンターを選択し、この宛先に適用する [!DNL Airship] データセンターに応じて選択できます。 最後に、データをエクスポート先にエクスポートする1つ以上のマーケティングの使用例を選択します。 Adobe定義のマーケティングの使用例から選択するか、独自の使用例を作成することができます。 マーケティングの使用例の詳細については、Real-time CDP [（リアルタイムCDP）ページの「](../../../rtcdp/privacy/data-governance-overview.md#destinations) Data Governance（データ・ガバナンス）」を参照してください。 個々のAdobe定義マーケティングの使用例について詳しくは、 [データ使用ポリシーの概要を参照してください](../../../data-governance/policies/overview.md#core-actions)。

Select **[!UICONTROL Create Destination]** after you have filled in the fields above.

![Airship属性に接続](../../assets/catalog/mobile-engagement/airship/select-domain.png)

これで宛先が作成されました。後でセグメントをアクティブにする場合は、「**[!UICONTROL 保存して終了]**」を選択します。また、「**[!UICONTROL 次へ]**」を選択してワークフローを続行し、アクティブ化するセグメントを選択することもできます。In either case, see the next section, [Activate segments](#activate-segments), for the rest of the workflow.

## セグメントのアクティブ化 {#activate-segments}

セグメントをアクティブ化するに [!DNL Airship Attributes]は、次の手順に従います。

**[!UICONTROL 宛先／参照]**&#x200B;で、セグメントをアクティブ化する宛先を選択します。[!DNL Airship Attributes]

![活性化流](../../assets/catalog/mobile-engagement/airship/browse.png)

宛先の名前をクリックします。これにより、「アクティブ化」のフローに移動します。


宛先に対するアクティベーションフローが既に存在する場合は、その宛先に現在送信されているセグメントを確認できます。右側のパネルで「**[!UICONTROL アクティベーションの編集]**」を選択し、以下の手順に従ってアクティベーションの詳細を変更します。

![活性化流](../../assets/catalog/mobile-engagement/airship/activate.png)

Select **[!UICONTROL Activate]**. In the **[!UICONTROL Activate destination]** workflow, on the **[!UICONTROL Select Segments]** page, select which segments to send to [!DNL Airship Attributes].

![segments-to-destination](../../assets/catalog/mobile-engagement/airship/select-segments.png)

マッピング **[!UICONTROL 手順で]** 、 [XDM](../../../xdm/home.md) スキーマから、宛先スキーマにマップする属性とIDを選択します。 **** 新しいマッピングを追加選択してスキーマを参照し、対応するターゲットIDにマッピングします。

![IDマッピング初期画面](../../assets/catalog/mobile-engagement/airship/identity-mapping.png)

[!DNL Airship] 属性は、デバイスインスタンスを表すチャネル（iPhoneなど）またはユーザー名（ユーザー名）のいずれかに設定できます。このユーザーは、すべてのユーザーのデバイスを顧客IDなどの共通の識別子にマップします。 スキーマで主IDとしてテキスト形式の（ハッシュ化されていない）電子メールアドレスが存在する場合、「 **[!UICONTROL ソース属性]** 」で電子メールフィールドを選択し、 [!DNL Airship] ターゲットID **[!UICONTROL (ID]**)の右の列の指定されたユーザーにマップします。

![名前付きユーザーマッピング](../../assets/catalog/mobile-engagement/airship/mapping.png)

チャネルにマップする必要がある識別子（デバイスなど）の場合、ソースに基づいて適切なチャネルにマップします。 次の画像は、2つのマッピングの作成方法を示しています。

* IDFA iOS広告IDとiOS [!DNL Airship] チャネル
* Adobe `fullName` 属性を「フルネーム」 [!DNL Airship] 属性に設定

>[!NOTE]
>
>属性マッピングのターゲットフィールドを選択する際に、 [!DNL Airship] ダッシュボードに表示されるわかりやすい名前を使用します。

**IDのマップ**

ソースフィールドの選択：

![Airship属性に接続](../../assets/catalog/mobile-engagement/airship/select-source-identity.png)

ターゲットフィールドの選択：

![Airship属性に接続](../../assets/catalog/mobile-engagement/airship/select-target-identity.png)

**マップの属性**

ソース属性を選択：

![ソースフィールドの選択](../../assets/catalog/mobile-engagement/airship/select-source-attributes.png)

ターゲット属性を選択：

![ターゲットフィールドの選択](../../assets/catalog/mobile-engagement/airship/select-target-attribute.png)

マッピングの検証：

![チャネルマッピング](../../assets/catalog/mobile-engagement/airship/mapping.png)

現在、 **[!UICONTROL セグメントスケジュール]** ページでは、スケジュールは無効になっています。 「 **[!UICONTROL 次へ]** 」をクリックしてレビュー手順に進みます。

![現在、スケジュールは無効です](../../assets/catalog/mobile-engagement/airship/scheduling.png)

「**[!UICONTROL 確認]**」ページには、選択の概要が表示されます。「**[!UICONTROL キャンセル]**」を選択してフローを分割するか、「**[!UICONTROL 戻る]**」を選択して設定を変更する、または、「**[!UICONTROL 完了]**」を選択して確定し、宛先へのデータの送信を開始します。

>[!IMPORTANT]
>
>この手順では、Adobe Experience Platformがデータ使用ポリシーの違反を確認します。 次に、ポリシー違反の例を示します。 セグメントアクティベーションのワークフローは、違反を解決するまで完了できません。 ポリシー違反の解決方法について詳しくは、「データ管理ドキュメント」の「 [ポリシーの適用](../../../rtcdp/privacy/data-governance-overview.md#enforcement) 」を参照してください。

![confirm-selection](../../assets/common/data-policy-violation.png)

ポリシー違反が検出されなかった場合は、「 **[!UICONTROL Finish]** 」を選択して、選択を確定し、開始が宛先にデータを送信することを確認します。

![レビュー](../../assets/catalog/mobile-engagement/airship/review.png)

## データの使用とガバナンス {#data-usage-governance}

すべての [!DNL Adobe Experience Platform] 宛先は、データ処理時のデータ使用ポリシーに準拠しています。 データ・ガバナンスの [!DNL Adobe Experience Platform] 実施方法の詳細については、「 [Data Governance in Real-time CDP](../../../rtcdp/privacy/data-governance-overview.md)」を参照してください。
