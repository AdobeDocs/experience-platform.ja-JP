---
keywords: Googleの顧客一致；Googleの顧客一致；Googleの顧客一致
title: Google Customer Match接続
description: Google Customer Matchでは、オンラインおよびオフラインのデータを使用して、Googleが所有し、運営するSearch、Shopping、Gmail、YouTubeなどのプロパティで顧客に連絡し、顧客と再び関与させることができます。
translation-type: tm+mt
source-git-commit: 494b41265a0eec71ec15c7896eb8c652b3164e18
workflow-type: tm+mt
source-wordcount: '1420'
ht-degree: 5%

---


# [!DNL Google Customer Match] connection

[Google Customer ](https://support.google.com/google-ads/answer/6379332?hl=en) Matchを使用すると、オンラインおよびオフラインのデータを使用して、Googleが所有し、運営する次のようなプロパティを通じて顧客に連絡し、顧客と再び関わり合うことができます。 [!DNL Search]、 [!DNL Shopping]、、 [!DNL Gmail]および [!DNL YouTube]。

![Adobe Experience PlatformUIのGoogle Customer Matchの表示先](../../assets/catalog/advertising/google-customer-match/catalog.png)

## 使用例

[!DNL Google Customer Match]の宛先を使用する方法とタイミングをより深く理解するために、以下はAdobe Experience Platformのお客様がこの機能を使用して解決できる使用例です。

### ユースケース 1

[!DNL Google Search]と[!DNL Google Shopping]を通じて既存の顧客に連絡し、過去の購入や閲覧の履歴に基づいてオファーやアイテムをパーソナライズしたいと考えているスポーツアパレルブランドです。 アパレルブランドは、自社のCRMからExperience Platformに電子メールアドレスを取り込み、自社のオフラインデータからセグメントを作成し、[!DNL Search]と[!DNL Shopping]で使用する[!DNL Google Customer Match]に送信して、広告費用を最適化できます。

### ユースケース 2

有名なテクノロジー会社が、新しい電話を発売したばかり。 この新しい電話モデルを推進するために、電話の新機能を、以前のモデルの電話を所有するお客様に知らせたいと考えています。

このリリースを促進するために、電子メールアドレスを識別子として使用し、CRMデータベースからExperience Platformに電子メールアドレスをアップロードします。 セグメントは、古い電話モデルを所有し、[!DNL Google Customer Match]に送信された顧客に基づいて作成され、現在の顧客、古い電話モデルを所有する顧客、および同様の顧客を[!DNL YouTube]にターゲットできます。

## 宛先の詳細{#destination-specs}

### [!DNL Google Customer Match]宛先{#data-governance}のデータ・ガバナンス

Experience Platform内の宛先には、宛先プラットフォームに送信または受信するデータに対して、特定のルールと義務を持つ場合があります。 データの制約事項と義務、およびそのデータをAdobe Experience Platformや目的地プラットフォームでどのように使用するかについては、責任を持って理解してください。 Adobe Experience Platformは、データ使用上の義務の一部を管理するのに役立つデータ管理ツールを提供しています。 [データ管理ツールとポリシーに](../../..//data-governance/labels/overview.md) ついて詳しく説明します。

### 書き出しの種類とID {#export-type}

**セグメントのエクスポート**  — セグメント(オーディエンス)のすべてのメンバーを、識別子（名前、電話番号など）と共にエクスポートします。[!DNL Google Customer Match]宛先で使用されます。

**ID**  - Googleで顧客IDとして生の電子メールまたはハッシュされた電子メールを使用できます。

### [!DNL Google Customer Match] アカウントの前提条件  {#google-account-prerequisites}

Experience Platformで[!DNL Google Customer Match]宛先を設定する前に、[Googleサポートドキュメント](https://support.google.com/google-ads/answer/6299717)で説明されている[!DNL Customer Match]を使用する際のGoogleのポリシーを読み、それに従っていることを確認してください。

### 許可リスト{#allowlist}

>[!NOTE]
>
>Experience Platformの最初の[!DNL Google Customer Match]宛先を設定する前に、Googleの許可リストに追加する必要があります。 リンク先を作成する前に、Googleが以下に説明する許可リストプロセスを完了していることを確認してください。

Experience Platformで[!DNL Google Customer Match]のアップロード先を作成する前に、Googleに問い合わせて、Googleドキュメントの[Use Customer Match partners to upload your data](https://support.google.com/google-ads/answer/7361372?hl=ja&amp;ref_topic=6296507)の許可リスト手順に従う必要があります。

さらに、Googleの[User_ID](https://developers.google.com/adwords/api/docs/guides/remarketing#customer_match_with_email_address_address_or_user_id)を使用してデータをアップロードする場合は、アカウントを追加する必要がある2つ目のGoogle許可リストもあります。 Googleのアカウントマネージャーに問い合わせて、許可リストに追加されていることを確認してください。

### IDの一致要件{#id-matching-requirements}

[!DNL Google] 個人識別情報(PII)を明確に送信しないようにする必要があります。したがって、[!DNL Google Customer Match]に対してアクティブ化されたオーディエンスは、電子メールアドレスや電話番号など、*ハッシュ化された*&#x200B;識別子をキーオフにすることができます。

Adobe Experience Platformに取り込むIDのタイプに応じて、対応する要件を満たす必要があります。

#### 電話番号のハッシュ要件{#phone-number-hashing-requirements}

[!DNL Google Customer Match]で電話番号をアクティブにする方法は2つあります。

* **生の電話番号を取り込む**:生の電話番号を [!DNL E.164] 形式で取り込んで、アクティベーション時に自動的にハッシュ化 [!DNL Platform]されます。このオプションを選択する場合は、必ず生の電話番号を`Phone_E.164`名前空間に取り込むようにしてください。
* **ハッシュ化された電話番号を取り込む**:に取り込む前に電話番号を事前にハッシュ化でき [!DNL Platform]ます。このオプションを選択する場合は、ハッシュ化された電話番号を必ず`PHONE_SHA256_E.164`名前空間に取り込むようにしてください。

>[!NOTE]
>
>`Phone`名前空間に取り込まれた電話番号は、[!DNL Google Customer Match]では有効にできません。

#### Eメールハッシュ要件{#hashing-requirements}

電子メールアドレスをAdobe Experience Platformに取り込む前にハッシュするか、Experience Platform内で電子メールアドレスを明確に扱ってアクティベーション上でアルゴリズムハッシュするかを選択できます。

Googleのハッシュ要件およびアクティベーションに関するその他の制限について詳しくは、Googleのドキュメントの次の節を参照してください。

* [[!DNL Customer Match] 電子メールアドレス、アドレスまたはユーザーIDを含む](https://developers.google.com/adwords/api/docs/guides/remarketing#customer_match_with_email_address_address_or_user_id)
* [[!DNL Customer Match] 考慮事項](https://developers.google.com/adwords/api/docs/guides/remarketing#customer_match_considerations)
* [電話番号との顧客一致](https://developers.google.com/adwords/api/docs/guides/remarketing#customer_match_with_phone_number)
* [モバイルデバイスIDとの一致](https://developers.google.com/adwords/api/docs/guides/remarketing#customer_match_with_mobile_device_ids)


Experience Platformでの電子メールアドレスの取り込みについて詳しくは、[バッチインジェストの概要](../../../ingestion/batch-ingestion/overview.md)および[ストリーミングインジェストの概要](../../../ingestion/streaming-ingestion/overview.md)を参照してください。

電子メールアドレスを自分でハッシュする場合は、上のリンクで説明したGoogleの要件に準拠していることを確認してください。

#### カスタム名前空間{#custom-namespaces}の使用

`User_ID`名前空間を使用してGoogleにデータを送信する前に、[!DNL gTag]を使用して自分のIDを同期させてください。 詳細については、[Googleの公式文書](https://support.google.com/google-ads/answer/9199250)を参照してください。

<!-- Data from unhashed namespaces is automatically hashed by [!DNL Platform] upon activation.

Attribute source data is not automatically hashed. When your source field contains unhashed attributes, check the **[!UICONTROL Apply transformation]** option, to have [!DNL Platform] automatically hash the data on activation.
![Identity mapping transformation](../../assets/ui/activate-destinations/identity-mapping-transformation.png) -->

## 宛先に接続 {#connect-destination}

**[!UICONTROL 宛先]**/**[!UICONTROL カタログ]**&#x200B;で、**[!UICONTROL 広告]**&#x200B;カテゴリまでスクロールします。 「[!DNL Google Customer Match]」を選択し、「**[!UICONTROL 設定]**」を選択します。

![Google Customer Matchのリンク先に接続](../../assets/catalog/advertising/google-customer-match/connect.png)

>[!NOTE]
>
>この宛先との接続が既に存在する場合は、宛先カードに「**[!UICONTROL Activate]**」ボタンが表示されます。 「**[!UICONTROL アクティブ化]**」と「**[!UICONTROL 設定]**」の違いについて詳しくは、保存先のワークスペースドキュメントの「[カタログ](../../ui/destinations-workspace.md#catalog)」の節を参照してください。

**アカウント**&#x200B;の手順で、[!DNL Google Customer Match]宛先への接続を事前に設定している場合は、「**[!UICONTROL 既存のアカウント]**」を選択し、既存の接続を選択します。 または、「**[!UICONTROL 新しいアカウント]**」を選択して、[!DNL Google Customer Match]への新しい接続を設定できます。 **[!UICONTROL [宛先に接続]**]を選択してログインし、[!DNL Google Ad]アカウントにAdobe Experience Cloudを接続します。

>[!NOTE]
>
>Experience Platformは、認証プロセスで資格情報の検証をサポートし、[!DNL Google Ad]アカウントに正しくない資格情報を入力すると、エラーメッセージを表示します。 このため、間違った資格情報を使用すると、ワークフローを完了することができません。

![Google Customer Matchの宛先に接続 — 認証手順](../../assets/catalog/advertising/google-customer-match/connection.png)

資格情報が確認され、Adobe Experience CloudがGoogleアカウントに接続されたら、**[!UICONTROL 「次へ]**」を選択して、**[!UICONTROL 認証]**&#x200B;の手順に進むことができます。

![資格情報の確認](../../assets/catalog/advertising/google-customer-match/connection-success.png)

**[!UICONTROL 認証]**&#x200B;手順で、**[!UICONTROL 名前]**&#x200B;と&#x200B;**[!UICONTROL 説明]**&#x200B;をアクティベーションフローに入力し、Googleの&#x200B;**[!UICONTROL アカウントID]**&#x200B;を入力します。

この手順では、この宛先に適用する&#x200B;**[!UICONTROL マーケティングアクション]**&#x200B;を選択することもできます。 マーケティングアクションは、データをエクスポート先にエクスポートする意図を示します。 Adobe定義のマーケティングアクションから選択するか、独自のマーケティングアクションを作成することができます。 マーケティングアクションについて詳しくは、[データ使用ポリシーの概要](../../../data-governance/policies/overview.md)を参照してください。

上記のフィールドに入力した後、「**[!UICONTROL 宛先を作成]**」を選択します。

>[!IMPORTANT]
>
> * **[!UICONTROL 「PIIと結合]**」マーケティングアクションは、デフォルトで[!DNL Google Customer Match]宛先に対して選択されており、削除できません。
> * [!DNL Google Customer Match]宛先用。 **[!UICONTROL アカウント]** IDは、Googleを使用する顧客のクライアントIDです。IDの形式はxxx-xxx-xxxxです。


![Google Customer Matchの接続 — 認証手順](../../assets/catalog/advertising/google-customer-match/authentication.png)

これで宛先が作成されました。後でセグメントをアクティブにする場合は、「**[!UICONTROL 保存して終了]**」を選択します。また、「**[!UICONTROL 次へ]**」を選択してワークフローを続行し、アクティブ化するセグメントを選択することもできます。どちらの場合も、残りのワークフローについては、次の[「 [!DNL Google Customer Match]](#activate-segments)にセグメントをアクティブにする」を参照してください。

## セグメントを[!DNL Google Customer Match] {#activate-segments}にアクティブ化

[!DNL Google Customer Match]にセグメントをアクティブ化する方法については、[宛先へのデータのアクティブ化](../../ui/activate-destinations.md)を参照してください。


**[!UICONTROL セグメントスケジュール]**&#x200B;の手順で、[!DNL IDFA]または[!DNL GAID]セグメントを[!DNL Google Customer Match]に送信する際に、[!UICONTROL アプリID]を指定する必要があります。

![Google Customer Match App ID](../../assets/catalog/advertising/google-customer-match/gcm-destination-appid.png)

[!DNL App ID]を見つける方法の詳細については、[Googleの公式文書](https://developers.google.com/adwords/api/docs/reference/v201809/AdwordsUserListService.CrmBasedUserList#appid)を参照してください。







<!-- 
To activate segments to [!DNL Google Customer Match], follow the steps below: 

In **[!UICONTROL Destinations > Browse]**, select the [!DNL Google Customer Match] destination where you want to activate your segments.

Click the name of the destination. This takes you to the Activate flow.

![activate-flow](../../assets/catalog/advertising/google-customer-match/activate-flow.png)

Note that if an activation flow already exists for a destination, you can see the segments that are currently being sent to the destination. Select **[!UICONTROL Edit activation]** in the right rail and follow the steps below to modify the activation details.

Select **[!UICONTROL Activate]**. In the **[!UICONTROL Activate destination]** workflow, on the **[!UICONTROL Select Segments]** page, select which segments to send to [!DNL Google Customer Match].

![segments-to-destination](../../assets/catalog/advertising/google-customer-match/activate-segments.png)

In the **[!UICONTROL Identity mapping]** step, select which attributes to be included as an identity in this destination. Select **[!UICONTROL Add new mapping]** and browse your schema, select email and/or hashed email, and map them to the corresponding target identity.

![identity mapping initial screen](../../assets/catalog/advertising/google-customer-match/identity-mapping.png) 

**Plain text email address as primary identity**: If you have plain text (unhashed) email addresses as primary identity in your schema, select the email field in your **[!UICONTROL Source Attributes]** and map to the Email field in the right column under **[!UICONTROL Target Identities]**, as shown below:

![select plain text emails identity](../../assets/catalog/advertising/google-customer-match/raw-email.gif) 

**Hashed email address as primary identity**: If you have hashed email addresses as primary identity in your schema, select the hashed email field in your **[!UICONTROL Source Attributes]** and map to the Email_LC_SHA256 field in the right column under **[!UICONTROL Target Identities]**, as shown below:

![select hashed emails identity](../../assets/catalog/advertising/google-customer-match/hashed-emails.gif)

On the **[!UICONTROL Segment schedule]** page, you can set the start date for sending data to the destination.

On the **[!UICONTROL Review]** page, you can see a summary of your selection. Select **[!UICONTROL Cancel]** to break up the flow, **[!UICONTROL Back]** to modify your settings, or **[!UICONTROL Finish]** to confirm your selection and start sending data to the destination.

>[!IMPORTANT]
>
>In this step, Real-time CDP checks for data usage policy violations. Shown below is an example where a policy is violated. You cannot complete the segment activation workflow until you have resolved the violation. For information on how to resolve policy violations, see [Policy enforcement](../../../rtcdp/privacy/data-governance-overview.md#enforcement) in the data governance documentation section.
 
![confirm-selection](../../assets/common/data-policy-violation.png)

If no policy violations have been detected, select **[!UICONTROL Finish]** to confirm your selection and start sending data to the destination.

![confirm-selection](../../assets/catalog/advertising/google-customer-match/review.png) -->

## セグメントのアクティベーションが成功したことを確認します。 {#verify-activation}

アクティベーションの流れが完了したら、**[!UICONTROL Google Ads]**&#x200B;アカウントに切り替えます。 アクティブ化されたセグメントは、Googleアカウントに顧客リストとして表示されます。 セグメントサイズに応じて、提供するアクティブなオーディエンスが100人を超えない限り、一部のユーザーは入力されないことに注意してください。

セグメントを[!DNL IDFA]と[!DNL GAID]の両方のモバイルIDにマッピングする場合、[!DNL Google Customer Match]はIDマッピングごとに個別のセグメントを作成します。 [!DNL Google Ads]アカウントには、[!DNL IDFA]用と[!DNL GAID]用の2つの異なるセグメントが表示されます。

## その他のリソース {#additional-resources}

* [Google顧客マッチの統合 — ビデオチュートリアル](https://experienceleague.adobe.com/docs/platform-learn/tutorials/rtcdp/integrate-with-google-customer-match.html)