---
title: ドキュメントセルフサービステンプレート//宛先の名前で置き換えます。
description: このテンプレートを使用して、Adobe エクスペリエンスプラットフォームカタログに格納されている保存先のドキュメントを作成します。概要セクションの段落で置き換えます。
exl-id: 99700474-8bf6-4176-acc1-38814e17c995
source-git-commit: 2b1cde9fc913be4d3bea71e7d56e0e5fe265a6be
workflow-type: tm+mt
source-wordcount: '904'
ht-degree: 4%

---

# 出力先 {#your-destination}

*このテンプレートを使用するときは、イタリックの段落をすべて置換または削除します (この方法で開始されます)。*

*最初に、ページの上部にあるメタデータ (タイトルと説明) を更新します。 このページの UICONTROL のインスタンスはすべて無視してください。 このタグを使用すると、machine translation プロセスが、そのページをサポートする多言語に正しく変換できます。 ドキュメントを送信した後で、ドキュメントにタグを追加することができます。*

## 概要 {#overview}

*お客様に提供される価値など、会社の概要を簡潔に説明します。 詳しくは、製品マニュアルのホームページへのリンクを参照してください。*

>[!IMPORTANT]
>
>このドキュメントページは、ターゲットチームによって作成されてい ** ます。 問い合わせまたはアップデートのリクエストについては、 *挿入リンクまたは電子メールアドレスを使用して、アップデートについて直接ご連絡ください。*

## 前提条件 {#prerequisites}

*このセクションでは、お客様が注意する必要があることについての情報を追加してから、Adobe エクスペリエンスプラットフォームのユーザーインターフェイスを使用して宛先を設定します。 これは以下のとおりです。*

* *許可リストに追加されている必要があります*
* *電子メールのハッシュ化の要件*
* *自分で作成した取引先企業のあらゆる特性*
* *プラットフォームに接続するための API キーの取得方法*

*お客様に有益な資料が記載されている場合は、関連ドキュメントにリンクさせることができます。*

## サポートされている id {#supported-identities}

*このセクションで、宛先によってサポートされている id に関する情報を追加します。 Prefilled には、いくつかの標準的な値が含まれていることを示します。 コピー先に適用されない値と、prefilled ではない値を削除します。*

*以下の* 表で説明されている id の有効化は、宛先によってサポートされています。 Id について詳しくは [ ](https://experienceleague.adobe.com/docs/experience-platform/identity/namespaces.html?lang=ja#getting-started) 、こちらを参照してください。

| ターゲット Id | 説明 | 注意点 |
|---|---|---|
| GAID | Google 広告 ID | ソースアイデンティティが「ユーザー ID」名前空間である場合は、標的 id を選択します。 |
| IDFA | Apple の広告主 ID | ソース id が IDFA 名前空間である場合は、IDFA ターゲット id を選択します。 |
| ECID | Experience Cloud ID | ブール値 d を表す名前空間。 この名前空間は、次のエイリアスによって参照することもできます。 &quot;Adobe Marketing Cloud ID&quot;、&quot;adobe エクスペリエンス Cloud ID&quot;、&quot;Adobe エクスペリエンス Platform ID&quot;。 詳細については、次のドキュメントを参照してください [ ](https://experienceleague.adobe.com/docs/experience-platform/identity/ecid.html) 。 |
| phone_sha256 | SHA256 アルゴリズムを使用してハッシュされる電話番号 | Adobe エクスペリエンスプラットフォームでは、プレーンテキストと SHA256 ハッシュ電話番号の両方がサポートされています。 「ソース」フィールドにハッシュされていない属性がある場合は、「変換を適用」オプションをオンにして、 **** データをアクティブ化時に自動的にハッシュするようにし [!DNL Platform] ます。 |
| email_lc_sha256 | SHA256 アルゴリズムを使用してハッシュされた電子メールアドレス | プレーンテキストと SHA256 ハッシュ電子メールアドレスは、Adobe エクスペリエンスプラットフォームでサポートされています。 「ソース」フィールドにハッシュされていない属性がある場合は、「変換を適用」オプションをオンにして、 **** データをアクティブ化時に自動的にハッシュするようにし [!DNL Platform] ます。 |
| extern_id | カスタムユーザー Id | ソース id がカスタム名前空間である場合は、このターゲット id を選択します。 |

{style=&quot;table-layout:auto&quot;}

## 書き出しタイプ {#export-type}

**セグメント** の書き出し-宛先になる宛先に使用されている識別子 (氏名、電話番号など) を使用して、セグメントのすべてのメンバー (氏名、電話番号など) を書き出すことができ ** ます。

## ユースケース

出力先をどのように使用するかについて詳しく理解するに ** は、次の例に示すように、この送信先を使用して Adobe が実行できる、使用例をご確認ください。

### ユースケース #1

*モバイルメッセージプラットフォームについては、次のとおりです。*

*家庭用のレンタルおよび販売プラットフォームは、お客様の Android および iOS デバイスにモバイル通知をプッシュすることで、事前にレンタルの検索を行ったエリアに100アップデートされたリストがあることを知らせます。*

### ユースケース #2

*ソーシャルネットワークプラットフォームの場合:*

*スポーツ衣料品が、既存の顧客にソーシャルメディアアカウントを使用して到達することを望んでいます。 衣料ブランドは、自分の CRM から Adobe エクスペリエンスプラットフォームに電子メールアドレスを取り込むことができます。また、オフラインデータからセグメントを作成して、これらのセグメントを宛先に送信することによって、顧客のソーシャルメディアフィードに広告を表示することができます。*

## 目的の場所に接続します。 {#connect}

この送信先に接続するには、宛先の設定チュートリアルで説明されている手順に従って [ ](https://experienceleague.adobe.com/docs/experience-platform/destinations/ui/connect-destination.html) ください。

### 接続パラメーター {#parameters}

このコピー先を設定する際に、 [ ](https://experienceleague.adobe.com/docs/experience-platform/destinations/ui/connect-destination.html) 次の情報を入力する必要があります。

*新しい宛先を設定する際には、ユーザーが入力する必要のあるフィールドを追加します。 これらのフィールドは、ターゲットによって異なります。また、ターゲット SDK での設定によって異なります。 コピー先のフィールドは、次に示すものとは異なる場合があります。*

* ****「名前」: 今後その宛先について認識される名前を指定します。
* **[!UICONTROL 説明]** : 今後この宛先を確認するために使用される説明。
* **[!UICONTROL アカウント ID]** : *移行先の* アカウント id を指定します。


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

## セグメントをこの宛先にアクティブにします。 {#activate}

[ ](https://experienceleague.adobe.com/docs/experience-platform/destinations/ui/activate/activate-segment-streaming-destinations.html?lang=en) この宛先までの対象ユーザーセグメントをアクティブにする方法については、「プロファイルをアクティブ化」と「セグメントの書き出し先をストリーミングする」を参照してください。

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

## 書き出したデータ {#exported-data}

*データを転送先に書き出す方法についてのメモを追加します。 これにより、お客様が宛先に正しく統合されたことを確認することができます。 例えば、次の例のような JSON を提供することができます。*

```
{
  "person": {
    "email": "yourstruly@adobe.com"
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

## データの使用方法とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform]データを処理する場合、すべての宛先はデータ使用ポリシーに準拠しています。データガバナンスの適用方法について詳しくは [!DNL Adobe Experience Platform] 、 [ data ガバナンスの概要を参照 ](https://experienceleague.adobe.com/docs/experience-platform/data-governance/home.html?lang=ja) してください。

## その他のリソース {#additional-resources}

*お客様が成功するには、ご使用の製品ドキュメントまたはその他のリソースへのリンクを参照してください。*
