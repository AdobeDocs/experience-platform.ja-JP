---
keywords: airship tags;airship destination
title: 航空船タグの宛先
seo-title: 航空船タグの宛先
description: AdobeオーディエンスデータをAirship内でのターゲティング用のオーディエンスタグとしてAirshipにシームレスに渡します。
seo-description: AdobeオーディエンスデータをAirship内でのターゲティング用のオーディエンスタグとしてAirshipにシームレスに渡します。
translation-type: tm+mt
source-git-commit: 4b1bf5bbce57a22529c5d025c5bae10557400d54
workflow-type: tm+mt
source-wordcount: '1236'
ht-degree: 14%

---


# （ベータ） [!DNL Airship Tags] 宛先 {#airship-tags-destination}

>[!IMPORTANT]
>
>Adobe Experience Platformの [!DNL Airship Tags] 目的地は現在ベータ段階です。 ドキュメントと機能は変更される場合があります。

## 概要

[!DNL Airship] は、顧客関与プラットフォームをリードし、顧客のライフサイクルのあらゆる段階で、意味のあるパーソナライズされたチャネルのメッセージをユーザに届けるのに役立ちます。

この統合により、Adobe Experience Platformセグメントデータはタグ [!DNL Airship] として [](https://docs.airship.com/guides/audience/tags/) に渡され、ターゲティングまたはトリガーが行われます。

詳しくは、 [!DNL Airship]航空船ドキュメント [を参照してください](https://docs.airship.com)。


>[!TIP]
>
>このドキュメントページはチー [!DNL Airship] ムが作成したページです。 お問い合わせや更新のご依頼は、 [support.airship.comから直接お問い合わせください](https://support.airship.com/)。

## 前提条件

Adobe Experience Platformセグメントをに送信する前に、次の操作を行う [!DNL Airship]必要があります。

* プロジェクトでタググループを作成し [!DNL Airship] ます。
* 認証用のベアラートークンを生成します。

>[!TIP]
> 
>まだアカウントを作成していない場合は、 [!DNL Airship] こ [のサインアップリンクを使用してアカウントを作成します](https://go.airship.eu/accounts/register/plan/starter/) 。

### タググループ

Adobe Experience Platformのセグメントの概念はAirshipの [タグ](https://docs.airship.com/guides/audience/tags/) （実装にわずかな違いがあります）に似ています。 この統合は、Experience Platformセグメント内のユーザーの [メンバーシップのステータスを](https://experienceleague.adobe.com/docs/experience-platform/xdm/mixins/profile/segmentation.html?lang=en#mixins) 、タグの存在または存在しない状態にマッピングし [!DNL Airship] ます。 例えば、プロファイルセグメントがに `xdm:status` 変更された場合、タグはプラットフォームに追加され `realized`るか、このプラットフォームがマッピングされる [!DNL Airship] チャネルに名前付けされます。 に `xdm:status` 変更した場合、タグ `exited`は削除されます。

この統合を有効にするには、 *タググループ* を [!DNL Airship] namedに作成し `adobe-segments`ます。

>[!IMPORTANT]
>
>新しいタググループを作成する場合 **は** 、「[!DNL Allow these tags to be set only from your server]」と表示されるラジオボタンをチェックしないでください。 これを行うと、Adobeタグの統合が失敗します。

タググループの作成手順については、「タググループの [管理](https://docs.airship.com/tutorials/manage-project/messaging/tag-groups) 」を参照してください。

### ベアラートークン

1. 「 **[!UICONTROL 設定]** 」に移動し、 **[!UICONTROL Airshipダッシュボード]** の「APIs &amp; Integrations」に移動し、左側のメニューで「 [](https://go.airship.com)**** Tokens」を選択します。
1. 「トークン **[!UICONTROL を作成]**」をクリックします。
1. トークンにわかりやすい名前(「Adobeタグの宛先」など)を指定し、ロールに「すべてのアクセス」を選択します。
1. 「トークン **[!UICONTROL の作成]** 」をクリックし、詳細を機密情報として保存します。


## 使用例

To help you better understand how and when you should use the [!DNL Airship Tags] destination, here are sample use cases that Adobe Experience Platform customers can solve by using this destination.

### 使用例1

小売業者やエンターテイメントプラットフォームは、忠誠度の高い顧客に対してユーザープロファイルを作成し、モバイルキャンペーンでのメッセージターゲット設定のために、それらのセグメント [!DNL Airship] をに渡すことができます。

### 使用例2

ユーザーがAdobe Experience Platform内の特定のセグメントに入る、または離れるときに、1対1のメッセージをリアルタイムでトリガーします。

例えば、ある小売業者がプラットフォームでジーンズブランド固有のセグメントを設定するとします。 あの小売業者は、誰かがジーンズの好みを特定のブランドにしたらすぐにモバイルメッセージをトリガーできるようになりました。

## Connect to [!DNL Airship Tags] {#connect-airship-tags}

1. 「 **[!UICONTROL Destinations]** / **[!UICONTROL Catalog]**」で、「 **[!UICONTROL Mobile Engagement]** 」カテゴリまでスクロールします。 を選択 **[!DNL Airship Tags]**&#x200B;し、「 **[!UICONTROL 設定]**」を選択します。

   >[!NOTE]
   >
   >この宛先との接続が既に存在する場合は、宛先カードに **[!UICONTROL 「アクティブ化]** 」ボタンが表示されます。 「 **[!UICONTROL アクティブ化]** 」と「 **[!UICONTROL 設定]**」の違いについて詳しくは、表示先ワークスペースのドキュメントの「 [カタログ](/help/rtcdp/destinations/destinations-workspace.md#catalog) 」セクションを参照してください。

   ![航空船タグに接続](/help/rtcdp/destinations/assets/airship-tags-in-catalog.png)

2. In the **Account** step, if you had previously set up a connection to your [!DNL Airship Tags] destination, select **[!UICONTROL Existing Account]** and select your existing connection. Or, you can select **[!UICONTROL New Account]** to set up a new connection to [!DNL Airship Tags]. 「宛先 **[!UICONTROL に接続]** 」を選択し、 [!DNL Airship] ダッシュボードから生成したベアラトークンを使用して、Adobe Experience Platformを [!DNL Airship] プロジェクトに接続します。

   >[!NOTE]
   >
   >Adobe Experience Platform supports credentials validation in the authentication process and displays an error message if you input incorrect credentials to your [!DNL Airship] account. このため、間違った資格情報を使用すると、ワークフローを完了することができません。

   ![航空船タグに接続](/help/rtcdp/destinations/assets/airshiptags1-connect-account.png)

3. Once your credentials are confirmed and Adobe Experience Platform is connected to your [!DNL Airship] project, you can select **[!UICONTROL Next]** to proceed to the **[!UICONTROL Setup]** step.

4. In the **[!UICONTROL Authentication]** step, enter a **[!UICONTROL Name]** and a **[!UICONTROL Description]** for your activation flow. <br> また、この手順では、米国またはEUのデータセンターを選択し、この宛先に適用する [!DNL Airship] データセンターに応じて選択できます。 最後に、データをエクスポート先にエクスポートする1つ以上のマーケティングの使用例を選択します。 Adobe定義のマーケティングの使用例から選択するか、独自の使用例を作成することができます。 マーケティングの使用例の詳細については、Real-time CDP [（リアルタイムCDP）ページの「](/help/rtcdp/privacy/data-governance-overview.md#destinations) Data Governance（データ・ガバナンス）」を参照してください。 個々のAdobe定義マーケティングの使用例について詳しくは、 [データ使用ポリシーの概要を参照してください](/help/data-governance/policies/overview.md#core-actions)。 <br> 上記のフィールドに入力したら、 **[!UICONTROL 「宛先を作成]** 」を選択します。

   ![航空船タグに接続](/help/rtcdp/destinations/assets/airshiptags2-select-domain.png)

5. これで宛先が作成されました。後でセグメントをアクティブにする場合は、「**[!UICONTROL 保存して終了]**」を選択します。また、「**[!UICONTROL 次へ]**」を選択してワークフローを続行し、アクティブ化するセグメントを選択することもできます。In either case, see the next section, [Activate segments](#activate-segments), for the rest of the workflow.

## セグメントのアクティブ化 {#activate-segments}

セグメントをアクティブ化するに [!DNL Airship Tags]は、次の手順に従います。

1. **[!UICONTROL 宛先／参照]**&#x200B;で、セグメントをアクティブ化する宛先を選択します。[!DNL Airship Tags]![活性化流](/help/rtcdp/destinations/assets/airship-tags-activate1.png)
2. 宛先の名前をクリックします。これにより、「アクティブ化」のフローに移動します。
宛先に対するアクティベーションフローが既に存在する場合は、その宛先に現在送信されているセグメントを確認できます。右側のパネルで「**[!UICONTROL アクティベーションの編集]**」を選択し、以下の手順に従ってアクティベーションの詳細を変更します。![活性化流](/help/rtcdp/destinations/assets/airship-tags-activate2.png)
3. **[!UICONTROL アクティブ化]**&#x200B;を選択します。
4. In the **[!UICONTROL Activate destination]** workflow, on the **[!UICONTROL Select Segments]** page, select which segments to send to [!DNL Airship Tags].
   ![segments-to-destination](/help/rtcdp/destinations/assets/airshiptags3-select-segments.png)
5. マッピング **[!UICONTROL 手順で]** 、 [XDM](https://docs.adobe.com/content/help/ja-JP/experience-platform/xdm/home.html) スキーマから、宛先スキーマにマップする属性とIDを選択します。 **** 新しいマッピングを追加選択してスキーマを参照し、対応するターゲットIDにマッピングします。
   ![IDマッピング初期画面](/help/rtcdp/destinations/assets/gcm-identity-mapping.png)
   [!DNL Airship] タグは、デバイスインスタンスを表すチャネル（iPhoneなど）またはすべてのユーザーのデバイスを顧客IDなどの共通の識別子にマップする名前付きユーザーに設定できます。 スキーマで主IDとしてテキスト形式の（ハッシュ化されていない）電子メールアドレスが存在する場合、「 **[!UICONTROL ソース属性]** 」で電子メールフィールドを選択し、 [!DNL Airship] ターゲットID **[!UICONTROL (ID]**)の右の列の指定されたユーザーにマップします。
   ![名前付きユーザーマッピング](/help/rtcdp/destinations/assets/airshiptags7-mappingoption2.png):チャネル（デバイス）にマップする必要がある識別子を、ソースに基づいて適切なチャネルにマップします。 次の画像は、Google広告IDを [!DNL Airship] Androidチャネルにマップする方法を示しています。
   ![航空船タグに接続](/help/rtcdp/destinations/assets/airshiptags4-select-source-identity.png)
   ![航空船タグに接続](/help/rtcdp/destinations/assets/airshiptags5-select-target-identity.png)
   ![チャネルマッピング](/help/rtcdp/destinations/assets/airshiptags6-mappingoption1.png)

6. 現在、 **[!UICONTROL セグメントスケジュール]** ページでは、スケジュールは無効になっています。 「 **[!UICONTROL 次へ]** 」をクリックしてレビュー手順に進みます。
7. 「**[!UICONTROL 確認]**」ページには、選択の概要が表示されます。「**[!UICONTROL キャンセル]**」を選択してフローを分割するか、「**[!UICONTROL 戻る]**」を選択して設定を変更する、または、「**[!UICONTROL 完了]**」を選択して確定し、宛先へのデータの送信を開始します。

>[!IMPORTANT]
>
>この手順では、Adobe Experience Platformがデータ使用ポリシーの違反を確認します。 次に、ポリシー違反の例を示します。 セグメントアクティベーションのワークフローは、違反を解決するまで完了できません。 ポリシー違反の解決方法について詳しくは、「データ管理ドキュメント」の「 [ポリシーの適用](/help/rtcdp/privacy/data-governance-overview.md#enforcement) 」を参照してください。

![confirm-selection](/help/rtcdp/destinations/assets/data-policy-violation.png)

ポリシー違反が検出されなかった場合は、「 **[!UICONTROL Finish]** 」を選択して、選択を確定し、開始が宛先にデータを送信することを確認します。

![confirm-selection](/help/rtcdp/destinations/assets/Airship-tags-review.png)


## データの使用とガバナンス {#data-usage-governance}

すべての [!DNL Adobe Experience Platform] 宛先は、データ処理時のデータ使用ポリシーに準拠しています。 データ・ガバナンスの [!DNL Adobe Experience Platform] 実施方法の詳細については、「 [Data Governance in Real-time CDP](/help/rtcdp/privacy/data-governance-overview.md)」を参照してください。

