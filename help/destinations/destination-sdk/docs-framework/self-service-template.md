---
title: ドキュメントのセルフサービステンプレート//を宛先の名前に置き換えます。
description: このテンプレートを使用して、Adobe Experience Platformカタログの宛先の公開ドキュメントを作成します。//概要セクションの段落に置き換えます
exl-id: 99700474-8bf6-4176-acc1-38814e17c995
source-git-commit: d2452bf0e59866d3deca57090001c4c5a0935525
workflow-type: tm+mt
source-wordcount: '904'
ht-degree: 3%

---

# YOURDESTINATION {#your-destination}

*このテンプレートを使用する際は、斜体のすべての段落を置き換えるか削除します（この段落から始まります）。*

*まず、ページ上部のメタデータ（タイトルと説明）を更新します。このページのUICONTROLのインスタンスはすべて無視してください。 これは、機械翻訳プロセスでページをサポートされている複数の言語に正しく翻訳するのに役立つタグです。 提出後、ドキュメントにタグを追加します。*

## 概要 {#overview}

*顧客に提供する価値を含め、会社の概要を短く示します。製品ドキュメントのホームページへのリンクを含めて、詳細を読んでください。*

>[!IMPORTANT]
>
>このドキュメントページは、*YOURDESTINATION*&#x200B;チームが作成しました。 お問い合わせや更新のご依頼は、*リンクの挿入先またはメールアドレス（更新情報*&#x200B;の場合）から直接お問い合わせください。

## 前提条件 {#prerequisites}

*Adobe Experience Platformユーザーインターフェイスで宛先の設定を開始する前に顧客が認識しておく必要がある事項に関する情報を、この節に追加します。次の場合が考えられます。*

* *許可リスト*
* *電子メールハッシュの要件*
* *お客様側のアカウントの詳細*
* *プラットフォームに接続するためのAPIキーの取得方法*

*お客様に役立つ場合は、関連するドキュメントにリンクアウトできます。*

## サポートされるID {#supported-identities}

*この節では、宛先でサポートされているIDに関する情報を追加します。テーブルには、いくつかの標準値が事前入力されています。 宛先に適用しない値と、事前入力されていない値を削除します。*

** YOURDESTINATIONは、以下の表で説明するIDのアクティブ化をサポートします。[ID](https://experienceleague.adobe.com/docs/experience-platform/identity/namespaces.html?lang=ja#getting-started)の詳細を説明します。

| ターゲットID | 説明 | 注意点 |
|---|---|---|
| GAID | Google広告ID | ソースIDがGAID名前空間の場合は、GAIDターゲットIDを選択します。 |
| IDFA | Apple の広告主 ID | ソースIDがIDFA名前空間の場合は、IDFAターゲットIDを選択します。 |
| ECID | Experience Cloud ID | ECIDを表す名前空間。 この名前空間は、次のエイリアスでも参照できます。&quot;Adobe Marketing Cloud ID&quot;、&quot;Adobe Experience Cloud ID&quot;、&quot;Adobe Experience Platform ID&quot;。 詳しくは、[ECID](https://experienceleague.adobe.com/docs/experience-platform/identity/ecid.html)に関する次のドキュメントを参照してください。 |
| phone_sha256 | SHA256アルゴリズムでハッシュ化された電話番号 | プレーンテキストとSHA256ハッシュ化された電話番号の両方が、Adobe Experience Platformでサポートされています。 ソースフィールドにハッシュ化されていない属性が含まれている場合、「**[!UICONTROL 変換を適用]**」オプションをオンにして、アクティブ化時に[!DNL Platform]でデータを自動的にハッシュ化します。 |
| email_lc_sha256 | SHA256アルゴリズムでハッシュ化された電子メールアドレス | プレーンテキストとSHA256ハッシュ化された電子メールアドレスの両方が、Adobe Experience Platformでサポートされています。 ソースフィールドにハッシュ化されていない属性が含まれている場合、「**[!UICONTROL 変換を適用]**」オプションをオンにして、アクティブ化時に[!DNL Platform]でデータを自動的にハッシュ化します。 |
| extern_id | カスタムユーザーID | ソースIDがカスタム名前空間の場合は、このターゲットIDを選択します。 |

{style=&quot;table-layout:auto&quot;}

## 書き出しタイプ {#export-type}

**セグメントの書き出し**  - YOURDESTINATIONの宛先で使用されている識別子（名前、電話番号、その他）を使用して、セグメント（オーディエンス）のすべてのメンバーを書き出 ** します。

## ユースケース

*YOURDESTINATION*&#x200B;の宛先をいつどのように使用するかを理解しやすくするために、Adobe Experience Platformのお客様がこの宛先を使用して解決できる使用例を以下に示します。


### ユースケース 1

*モバイルメッセージプラットフォームの場合：*

*自宅でのレンタルや販売のプラットフォームでは、顧客のAndroidやiOSデバイスにモバイル通知をプッシュして、以前にレンタルを検索した地域に更新済みのリストが100件あることを知らせたいと考えています。*

### ユースケース 2

*ソーシャルネットワークプラットフォームの場合：*

*スポーツアパレルブランドは、ソーシャルメディアアカウントを通じて既存の顧客にリーチしたいと考えています。アパレルブランドは、独自のCRMからAdobe Experience Platformに電子メールアドレスを取り込み、独自のオフラインデータからセグメントを作成し、YOURDESTINATIONに送信して、顧客のソーシャルメディアフィードに広告を表示できます。*

## 宛先に接続 {#connect}

この宛先に接続するには、[宛先の設定に関するチュートリアル](https://experienceleague.adobe.com/docs/experience-platform/destinations/ui/connect-destination.html)で説明されている手順に従います。

### 接続パラメーター {#parameters}

[この宛先を設定](https://experienceleague.adobe.com/docs/experience-platform/destinations/ui/connect-destination.html)する際に、次の情報を指定する必要があります。

*新しい宛先を設定する際に顧客が入力する必要があるフィールドを追加します。これらのフィールドは宛先固有で、宛先SDKの設定によって異なります。 宛先のフィールドは、以下に示すフィールドとは異なる場合があります。*

* **[!UICONTROL 名前]**:将来この宛先を認識するための名前。
* **[!UICONTROL 説明]**:今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL アカウントID]**:宛先 ** アカウントID。


<!--

*Replace YOURDESTINATION with your destination name and TOBEFILLEDIN with the category that your destination belongs to.*

1. In **[!UICONTROL Destinations]** > **[!UICONTROL Catalog]**, scroll to the ***TOBEFILLEDIN*** category. Select ***YOURDESTINATION***, then select **[!UICONTROL Configure]**.


    >[!NOTE]
    >
    >If a connection with this destination already exists, you can see an **[!UICONTROL Activate]** button on the destination card. For more information about the difference between **[!UICONTROL Activate]** and **[!UICONTROL Configure]**, refer to the [Catalog](https://experienceleague.adobe.com/docs/experience-platform/destinations/ui/destinations-workspace.html#catalog) section of the destination workspace documentation.  

    ![Connect to YOURDESTINATION](./assets/yourdestination1.png)

2. In the **Account** step, if you had previously set up a connection to your *YOURDESTINATION* destination, select **[!UICONTROL Existing Account]** and select your existing connection. Or, you can select **[!UICONTROL New Account]** to set up a new connection to *YOURDESTINATION*. Select **[!UICONTROL Connect to destination]** to log in and connect Adobe Experience Cloud to your *YOURDESTINATION* account.

    >[!NOTE]
    >
    >Adobe Experience Platform supports credentials validation in the authentication process and displays an error message if you input incorrect credentials to your ***YOURDESTINATION*** account. This ensures that you don't complete the workflow with incorrect credentials.

3. Once your credentials are confirmed and Adobe Experience Cloud is connected to your ***YOURDESTINATION*** account, you can select **[!UICONTROL Next]** to proceed to the **[!UICONTROL Setup]** step.

4. In the **[!UICONTROL Authentication]** step, enter a **[!UICONTROL Name]** and a **[!UICONTROL Description]** for your activation flow and fill your account ID. <br> Also in this step, you can select any marketing action that should apply to this destination. Marketing actions indicate the intent for which data will be exported to the destination. You can select from Adobe-defined marketing actions or you can create your own marketing action. For more information about marketing actions, see the [Data usage policies overview](https://experienceleague.adobe.com/docs/experience-platform/data-governance/policies/overview.html).

    ![Connect to YOURDESTINATION](./assets/yourdestination2.png)

5. Your destination is now created. You can select **[!UICONTROL Save & Exit]** if you want to activate segments later on or you can select **[!UICONTROL Next]** to continue the workflow and select segments to activate. In either case, see the next section, [Activate segments](#activate-segments), for the rest of the workflow.

-->

## この宛先へのセグメントのアクティブ化 {#activate}

この宛先に対するオーディエンスセグメントをアクティブ化する手順については、[ストリーミングセグメントの書き出し先に対するプロファイルとセグメントのアクティブ化](https://experienceleague.adobe.com/docs/experience-platform/destinations/ui/activate/activate-segment-streaming-destinations.html?lang=en)を参照してください。

<!--

To activate segments to *YOURDESTINATION*, follow the steps below: 

1. In **[!UICONTROL Destinations > Browse]**, select the *YOURDESTINATION* destination where you want to activate your segments.

2. Click the name of the destination. This takes you to the Activate flow.
    ![activate-flow](./assets/yourdestination3.png)
    Note that if an activation flow already exists for a destination, you can see the segments that are currently being sent to the destination. Select **[!UICONTROL Edit activation]** in the right rail and follow the steps below to modify the activation details.
3. Select **[!UICONTROL Activate]**;
4. In the **[!UICONTROL Activate destination]** workflow, on the **[!UICONTROL Select Segments]** page, select which segments to send to *YOURDESTINATION*.
    ![segments-to-destination](./assets/activate-segments-google-customer-match.png)
5.  In the **[!UICONTROL Mapping]** step, select which attributes and identities from the source XDM schema to map to the target schema. Select **[!UICONTROL Add new mapping]** and browse your schema, select identity namespaces and map them to the corresponding target identity.  
![identity mapping initial screen](./assets/gcm-identity-mapping.png) <br>&nbsp;
   *Plain text email address as primary identity*: If you have plain text (unhashed) email addresses as primary identity in your schema, select the email field in your **[!UICONTROL Source Attributes]** and map to the Email field in the right column under **[!UICONTROL Target Identities]**, as shown below. Set up a mapping between any other attributes you plan to use.
   ![identity mapping step](./assets/ssd-template-identity.png) <br>&nbsp;
6. On the **[!UICONTROL Segment schedule]** page, you can set the start date for sending data to the destination.
7. On the **[!UICONTROL Review]** page, you can see a summary of your selection. Select **[!UICONTROL Cancel]** to break up the flow, **[!UICONTROL Back]** to modify your settings, or **[!UICONTROL Finish]** to confirm your selection and start sending data to the destination.

>[!IMPORTANT]
>
>In this step, Adobe Experience Platform checks for data usage policy violations. Shown below is an example where a policy is violated. You cannot complete the segment activation workflow until you have resolved the violation. For information on how to resolve policy violations, see [Policy enforcement](https://experienceleague.adobe.com/docs/experience-platform/data-governance/enforcement/auto-enforcement.html#enforcement) in the data governance documentation section.
 
![confirm-selection](./assets/data-policy-violation.png)

If no policy violations have been detected, select **[!UICONTROL Finish]** to confirm your selection and start sending data to the destination.

![confirm-selection](./assets/gcm-review.png)

-->

## 書き出されたデータ {#exported-data}

*宛先へのデータの書き出し方法に関する注意を追加します。これは、顧客が宛先と正しく統合されていることを確認するのに役立ちます。 例えば、以下のようなJSONのサンプルを提供できます。*

```
{
  "person": {
    "email": "yourstruly@adobe.con"
  },
  "segmentMembership": {
    "ups": {
      "7841ba61-23c1-4bb3-a495-00d3g5fe1e93": {
        "lastQualificationTime": "2020-05-25T21:24:39Z",
        "status": "exited"
      },
      "59bd2fkd-3c48-4b18-bf56-4f5c5e6967ae": {
        "lastQualificationTime": "2020-05-25T23:37:33Z",
        "status": "existing"
      }
    }
  },
  "identityMap": {
    "ecid": [
      {
        "id": "14575006536349286404619648085736425115"
      },
      {
        "id": "66478888669296734530114754794777368480"
      }
    ],
    "email_lc_sha256": [
      {
        "id": "655332b5fa2aea4498bf7a290cff017cb4"
      },
      {
        "id": "66baf76ef9de8b42df8903f00e0e3dc0b7"
      }
    ]
  }
}
```

## データの使用とガバナンス {#data-usage-governance}

すべての[!DNL Adobe Experience Platform]宛先は、データを処理する際のデータ使用ポリシーに準拠しています。 [!DNL Adobe Experience Platform]によるデータガバナンスの強制方法について詳しくは、「[データガバナンスの概要](https://experienceleague.adobe.com/docs/experience-platform/data-governance/home.html)」を参照してください。

## その他のリソース {#additional-resources}

*製品ドキュメントへのリンクや、顧客が成功するために重要と考えるその他のリソースへのリンクを提供できます。*
