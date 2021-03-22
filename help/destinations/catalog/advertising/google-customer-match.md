---
keywords: Googleの顧客一致；Googleの顧客一致；Googleの顧客一致
title: Google Customer Match接続
description: Google Customer Matchでは、オンラインおよびオフラインのデータを使用して、Googleが所有し、運営するSearch、Shopping、Gmail、YouTubeなどのプロパティで顧客に連絡し、顧客と再び関与させることができます。
translation-type: tm+mt
source-git-commit: 709908196bb5df665c7e7df10dc58ee9f3b0edbf
workflow-type: tm+mt
source-wordcount: '1589'
ht-degree: 4%

---


# [!DNL Google Customer Match] connection

## 概要 {#overview}

[Google Customer ](https://support.google.com/google-ads/answer/6379332?hl=en) Matchを使用すると、オンラインおよびオフラインのデータを使用して、Googleが所有し、運営する次のようなプロパティを通じて顧客に連絡し、顧客と再び関わり合うことができます。 [!DNL Search]、 [!DNL Shopping]、、 [!DNL Gmail]および [!DNL YouTube]。

![Adobe Experience PlatformUIのGoogle Customer Matchの表示先](../../assets/catalog/advertising/google-customer-match/catalog.png)

## 使用例

[!DNL Google Customer Match]宛先の使い方と使い方を理解するために、Adobe Experience Platformのお客様がこの機能を使って解決できる使用例を以下に示します。

### ユースケース 1

[!DNL Google Search]と[!DNL Google Shopping]を通じて既存の顧客に連絡し、過去の購入や閲覧の履歴に基づいてオファーやアイテムをパーソナライズしたいと考えているスポーツアパレルブランドです。 アパレルブランドは、自社のCRMからExperience Platformに電子メールアドレスを取り込み、自社のオフラインデータからセグメントを作成できます。 その後、[!DNL Google Customer Match]にこれらのセグメントを送信して、[!DNL Search]と[!DNL Shopping]で使用し、広告費用を最適化できます。

### ユースケース 2

有名なテクノロジー会社が新しい電話を始めました。 この新しい電話モデルを推進するために、電話の新機能を以前のモデルを所有するお客様に知らせたいと考えています。

このリリースを促進するために、電子メールアドレスを識別子として使用し、CRMデータベースからExperience Platformに電子メールアドレスをアップロードします。 セグメントは、古い電話モデルを所有する顧客に基づいて作成されます。 次に、セグメントが[!DNL Google Customer Match]に送信されるので、会社は、現在の顧客、古い電話モデルを所有する顧客、および類似の顧客を[!DNL YouTube]でターゲットできます。

## [!DNL Google Customer Match]宛先{#data-governance}のデータ・ガバナンス

Experience Platform内の一部の宛先には、宛先プラットフォームに送信または受信するデータに対する特定のルールと義務があります。 データの制約事項と義務、およびそのデータをAdobe Experience Platformや目的地プラットフォームでどのように使用するかについては、責任を持って理解してください。 Adobe Experience Platformは、データ使用上の義務の一部を管理するのに役立つデータ管理ツールを提供しています。 [データ管理ツールとポリシーに](../../..//data-governance/labels/overview.md) ついて詳しく説明します。

## サポートされるID{#supported-identities}

[!DNL Google Customer Match] は、次の表に示すIDのアクティベーションをサポートしています。[ID](/help/identity-service/namespaces.md)の詳細を表示します。

| ターゲットID | 説明 | 注意点 |
|---|---|---|
| GAID | Google広告ID | ソースIDがGAID名前空間の場合は、このターゲットIDを選択します。 |
| IDFA | Apple の広告主 ID | ソースIDがIDFA名前空間の場合は、このターゲットIDを選択します。 |
| phone_sha256_e.164 | E164形式の電話番号。SHA256アルゴリズムでハッシュ化 | 平文とSHA256ハッシュの両方の電話番号がAdobe Experience Platformでサポートされています。 「[ID matching requirements](#id-matching-requirements-id-matching-requirements)」の説明に従い、プレーンテキストとハッシュ化された電話番号にそれぞれ適切な名前空間を使用します。 ソースフィールドにハッシュ化されていない属性が含まれている場合は、「**[!UICONTROL 変換を適用]**」オプションをオンにして、アクティベーション上のデータを自動的にハッシュ化します。[!DNL Platform] |
| email_lc_sha256 | SHA256アルゴリズムでハッシュ化された電子メールアドレス | プレーンテキストとSHA256ハッシュの電子メールアドレスの両方が、Adobe Experience Platformでサポートされています。 「[ID matching requirements](#id-matching-requirements-id-matching-requirements)」の説明に従い、プレーンテキストとハッシュ化された電子メールアドレスにそれぞれ適切な名前空間を使用します。 ソースフィールドにハッシュ化されていない属性が含まれている場合は、「**[!UICONTROL 変換を適用]**」オプションをオンにして、アクティベーション上のデータを自動的にハッシュ化します。[!DNL Platform] |
| user_id | カスタムユーザーID | ソースIDがカスタムターゲットの場合は、この名前空間IDを選択します。 |

## エクスポートの種類{#export-type}

**セグメントエクスポート**  — セグメント(オーディエンス)のすべてのメンバーを、 [!DNL Google Customer Match] 宛先で使用されている識別子（名前、電話番号など）と共にエクスポートします。

## [!DNL Google Customer Match] アカウントの前提条件  {#google-account-prerequisites}

Experience Platformで[!DNL Google Customer Match]宛先を設定する前に、[Googleサポートドキュメント](https://support.google.com/google-ads/answer/6299717)で説明されている[!DNL Customer Match]を使用する際のGoogleのポリシーを読み、それに従っていることを確認してください。

### 許可リスト{#allowlist}

>[!NOTE]
>
>Experience Platformの最初の[!DNL Google Customer Match]宛先を設定する前に、Googleの許可リストに追加する必要があります。 リンク先を作成する前に、Googleが以下に説明する許可リストプロセスを完了していることを確認してください。

Experience Platformで[!DNL Google Customer Match]のアップロード先を作成する前に、Googleに問い合わせて、Googleドキュメントの[Use Customer Match partners to upload your data](https://support.google.com/google-ads/answer/7361372?hl=ja&amp;ref_topic=6296507)の許可リスト手順に従う必要があります。

また、Googleの[User_ID](https://developers.google.com/adwords/api/docs/guides/remarketing#customer_match_with_email_address_address_or_user_id)を使用してデータをアップロードする場合は、アカウントを追加する必要がある2つ目のGoogle許可リストもあります。 アカウントを許可リストに追加するには、Googleのアカウントマネージャーにお問い合わせください。

## IDの一致要件{#id-matching-requirements}

[!DNL Google] 個人識別情報(PII)を明確に送信しないようにする必要があります。したがって、[!DNL Google Customer Match]に対してアクティブ化されたオーディエンスは、電子メールアドレスや電話番号など、*ハッシュ化された*&#x200B;識別子をキーオフにすることができます。

Adobe Experience Platformに取り込むIDのタイプに応じて、対応する要件に従う必要があります。

## 電話番号のハッシュ要件{#phone-number-hashing-requirements}

[!DNL Google Customer Match]で電話番号をアクティブにする方法は2つあります。

* **生の電話番号を取り込む**:生の電話番号を [!DNL E.164] 形式で取り込んで、アクティベーション時に自動的にハッシュ化 [!DNL Platform]されます。このオプションを選択する場合は、必ず生の電話番号を`Phone_E.164`名前空間に取り込むようにしてください。
* **ハッシュ化された電話番号を取り込む**:に取り込む前に電話番号を事前にハッシュ化でき [!DNL Platform]ます。このオプションを選択する場合は、ハッシュ化された電話番号を必ず`PHONE_SHA256_E.164`名前空間に取り込むようにしてください。

>[!NOTE]
>
>`Phone`名前空間に取り込まれた電話番号は、[!DNL Google Customer Match]では有効にできません。

## Eメールハッシュ要件{#hashing-requirements}

電子メールアドレスをAdobe Experience Platformに取り込む前にハッシュ化したり、Experience Platform内で明確な電子メールアドレスを使用して、アクティベーション上で[!DNL Platform]ハッシュ化したりできます。

Googleのハッシュ要件およびアクティベーションに関するその他の制限について詳しくは、Googleのドキュメントの次の節を参照してください。

* [[!DNL Customer Match] 電子メールアドレス、アドレスまたはユーザーIDを含む](https://developers.google.com/adwords/api/docs/guides/remarketing#customer_match_with_email_address_address_or_user_id)
* [[!DNL Customer Match] 考慮事項](https://developers.google.com/adwords/api/docs/guides/remarketing#customer_match_considerations)
* [電話番号との顧客一致](https://developers.google.com/adwords/api/docs/guides/remarketing#customer_match_with_phone_number)
* [モバイルデバイスIDとの一致](https://developers.google.com/adwords/api/docs/guides/remarketing#customer_match_with_mobile_device_ids)


Experience Platformでの電子メールアドレスの取り込みについて詳しくは、[バッチインジェストの概要](../../../ingestion/batch-ingestion/overview.md)および[ストリーミングインジェストの概要](../../../ingestion/streaming-ingestion/overview.md)を参照してください。

電子メールアドレスを自分でハッシュする場合は、上のリンクで説明したGoogleの要件に準拠していることを確認してください。

## カスタム名前空間{#custom-namespaces}の使用

`User_ID`名前空間を使用してGoogleにデータを送信する前に、[!DNL gTag]を使用して自分のIDを同期させてください。 詳細については、[Googleの公式文書](https://support.google.com/google-ads/answer/9199250)を参照してください。

<!-- Data from unhashed namespaces is automatically hashed by [!DNL Platform] upon activation.

Attribute source data is not automatically hashed. When your source field contains unhashed attributes, check the **[!UICONTROL Apply transformation]** option, to have [!DNL Platform] automatically hash the data on activation.
![Identity mapping transformation](../../assets/ui/activate-destinations/identity-mapping-transformation.png) -->

## 宛先に接続 {#connect-destination}

**[!UICONTROL 宛先]**/**[!UICONTROL カタログ]**&#x200B;で、**[!UICONTROL 広告]**&#x200B;カテゴリまでスクロールします。 「[!DNL Google Customer Match]」を選択し、「**[!UICONTROL 設定]**」を選択します。

![Google Customer Matchのリンク先に接続](../../assets/catalog/advertising/google-customer-match/connect.png)

>[!NOTE]
>
>この宛先との接続が存在する場合は、宛先カードに「**[!UICONTROL Activate]**」ボタンが表示されます。 「**[!UICONTROL アクティブ化]**」と「**[!UICONTROL 設定]**」の違いについて詳しくは、保存先のワークスペースドキュメントの「[カタログ](../../ui/destinations-workspace.md#catalog)」の節を参照してください。

**アカウント**&#x200B;の手順で、[!DNL Google Customer Match]宛先への接続を事前に設定している場合は、「**[!UICONTROL 既存のアカウント]**」を選択し、既存の接続を選択します。 または、「**[!UICONTROL 新しいアカウント]**」を選択して、[!DNL Google Customer Match]への新しい接続を設定できます。 ログインして[!DNL Google Ad]アカウントに接続するには、**[!UICONTROL [宛先に接続]]**&#x200B;を選択します。

>[!NOTE]
>
>Experience Platformは、認証プロセスでの秘密鍵証明書の検証をサポートします。 [!DNL Google Ad]アカウントに正しくない資格情報を入力した場合は、正しくない資格情報でワークフローが完了しないようにエラーメッセージが表示されます。

![Google Customer Matchの宛先に接続 — 認証手順](../../assets/catalog/advertising/google-customer-match/connection.png)

資格情報が確認され、Adobe Experience CloudがGoogleアカウントに接続されたら、**[!UICONTROL 「次へ]**」を選択して、**[!UICONTROL 認証]**&#x200B;の手順に進むことができます。

![資格情報の確認](../../assets/catalog/advertising/google-customer-match/connection-success.png)

**[!UICONTROL 認証]**&#x200B;手順で、**[!UICONTROL 名前]**&#x200B;と&#x200B;**[!UICONTROL 説明]**&#x200B;をアクティベーションフローに入力し、Googleの&#x200B;**[!UICONTROL アカウントID]**&#x200B;を入力します。

この手順では、この宛先に適用する&#x200B;**[!UICONTROL マーケティングアクション]**&#x200B;を選択することもできます。 マーケティングアクションは、データがエクスポート先にエクスポートされる意図を示します。 Adobe定義のマーケティングアクションから選択するか、独自のマーケティングアクションを作成することができます。 マーケティングアクションについて詳しくは、[データ使用ポリシーの概要](../../../data-governance/policies/overview.md)を参照してください。

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

## セグメントのアクティベーションが成功したことを確認します。 {#verify-activation}

アクティベーションの流れが完了したら、**[!UICONTROL Google Ads]**&#x200B;アカウントに切り替えます。 アクティブ化されたセグメントは、Googleアカウントに顧客リストとして表示されます。 提供するアクティブオーディエンスが100人を超えない限り、セグメントサイズに応じて、一部のユーザーは値を入力しないことに注意してください。

セグメントを[!DNL IDFA]と[!DNL GAID]の両方のモバイルIDにマッピングする場合、[!DNL Google Customer Match]はIDマッピングごとに個別のセグメントを作成します。 [!DNL Google Ads]アカウントに、[!DNL IDFA]と[!DNL GAID]のマッピング用の2つの異なるセグメントが表示されます。

## 追加のリソース{#additional-resources}

* [Google顧客マッチの統合 — ビデオチュートリアル](https://experienceleague.adobe.com/docs/platform-learn/tutorials/rtcdp/integrate-with-google-customer-match.html)