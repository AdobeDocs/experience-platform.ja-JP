---
title: （事業者） LinkedIn 連携
description: この宛先を使用して、Account-Based Marketing（ABM）のユースケースのアカウントオーディエンスをアクティベートします。 ハッシュ化されたメールに基づいて、オーディエンスのターゲティング、パーソナライゼーションおよび抑制を行うための、LinkedIn キャンペーン用のプロファイルをアクティブ化します。
badgeB2B: label="B2B edition" type="Informative" url=" https://experienceleague.adobe.com/docs/experience-platform/rtcdp/intro/rtcdp-intro/overview.html?lang=ja#rtcdp-editions newtab=true"
badgeB2P: label="B2P エディション" type="Positive" url=" https://experienceleague.adobe.com/docs/experience-platform/rtcdp/intro/rtcdp-intro/overview.html?lang=ja#rtcdp-editions newtab=true"
exl-id: 68d2cca3-952b-49d0-8ea2-e776a233b752
source-git-commit: 1b507e9846a74b7ac2d046c89fd7c27a818035ba
workflow-type: tm+mt
source-wordcount: '649'
ht-degree: 23%

---

# （会社） LinkedIn Match Audiences 接続 {#companies-linkedin}

>[!AVAILABILITY]
>
>Real-Time Customer Data Platformの [Business-to-Business）エディションと &#x200B;](/help/rtcdp/overview.md#rtcdp-b2b)Business-to-Person[&#x200B; エディションを購入する企業は、アカウントオーディエンスを（会社） LinkedIn 宛先に対してアクティブ化する機能を利用 &#x200B;](/help/rtcdp/overview.md#rtcdp-b2p) きます。

この宛先を使用して、Account-Based Marketing（ABM）のユースケースの [&#x200B; アカウントオーディエンス &#x200B;](/help/segmentation/types/account-audiences.md) をアクティベートします。 **[!UICONTROL (Companies) LinkedIn]** の B2B の宛先を介して、Target アカウント内の関連するペルソナや役割にアドバタイズします。 LinkedIn プラットフォームの [&#x200B; アカウントターゲティングの詳細については &#x200B;](https://business.linkedin.com/marketing-solutions/cx/21/10/ad-targeting/account-targeting)、LinkedIn のドキュメントを参照してください。

>[!TIP]
>
>個々のレベル（または B2C）のユースケースについては、Adobeでは [LinkedIn でマッチしたオーディエンス &#x200B;](/help/destinations/catalog/social/linkedin.md) の宛先を使用することをお勧めします。

![Experience Platform UI に表示される LinkedIn アカウントの宛先。](/help/destinations/assets/catalog/social/linkedin-b2b/linkedin-b2b-destination.png)

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスオリジン | サポートあり | 説明 |
|---------|----------|----------|
| [!DNL Segmentation Service] | ✓ | Experience Platform [&#x200B; セグメント化サービス &#x200B;](../../../segmentation/home.md) を通じて生成されたオーディエンス。 |
| カスタムアップロード | X | CSV ファイルから Experience Platform に[読み込まれた](../../../segmentation/ui/overview.md#import-audience)オーディエンス。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-and-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
|--------------|-----------|---------------------------|
| 書き出しタイプ | オーディエンスのエクスポート | すべてのオーディエンスメンバーは、名前、電話番号などの主要な識別子で書き出されます。 |
| 頻度 | ストリーミング | 「常時」 API ベースの接続。 アップデートは、プロファイルの変更直後にダウンストリームに送信されます。 |

{style="table-layout:auto"}

## 前提条件 {#prerequisites}

アカウントオーディエンスを LinkedIn に書き出すには、以下の前提条件を満たしていることを確認してください。

### LinkedIn アカウントの前提条件 {#LinkedIn-account-prerequisites}

[!UICONTROL (Companies) LinkedIn Matched Audience] の宛先を使用する前に、[!DNL LinkedIn Campaign Manager] アカウントの権限レベルが [!DNL Creative Manager] 以上であることを確認してください。

[!DNL LinkedIn Campaign Manager] ユーザー権限の編集方法については、LinkedIn ドキュメントの [Advertising アカウントのユーザー権限の追加、編集、削除 &#x200B;](https://www.linkedin.com/help/lms/answer/5753) を参照してください。

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL View Destinations]** および **[!UICONTROL Manage Destinations]**&#x200B;[&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

1. 宛先カタログで [!DNL (Companies) LinkedIn Matched Audiences] の宛先を見つけて、「**[!UICONTROL Set Up]**」を選択します。
2. **[!UICONTROL Connect to destination]** を選択します。
   ![LinkedIn への認証 &#x200B;](/help/destinations/assets/catalog/social/linkedin-b2b/authenticate-linkedin-destination.png)
3. LinkedIn 資格情報を入力し、「**ログイン**」を選択します。

LinkedIn によるログインプロセスが完了したら、次の手順に進むことができます。

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

* **[!UICONTROL Name]**：今後この宛先を認識するための名前。
* **[!UICONTROL Description]**：今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL Account ID]**：あなたの [!DNL LinkedIn Campaign Manager Account ID]。 この ID は [!DNL LinkedIn Campaign Manager] アカウントで確認できます。

これで、LinkedIn に対してアカウントオーディエンスをアクティブ化する準備が整いました。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>* データをアクティブ化するには、**[!UICONTROL View Destinations]**、**[!UICONTROL Activate Destinations]**、**[!UICONTROL View Profiles]**、**[!UICONTROL View Segments]** [&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* *ID* を書き出すには、**[!UICONTROL View Identity Graph]** [&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。<br> ![&#x200B; 宛先に対してアカウントオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択。](/help/destinations/assets/ui/activate-account-audiences/identity-namespace-highlighted.png " 宛先に対してアカウントオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択 "){width="100" zoomable="yes"}

この宛先にアカウントオーディエンスをアクティブ化する手順については、[&#x200B; アカウントオーディエンスのアクティブ化 &#x200B;](/help/destinations/ui/activate-account-audiences.md) をお読みください。

## アカウントオーディエンスを **[!UICONTROL (Companies) LinkedIn Matched Audiences]** の宛先に対してアクティブ化する際に、マッピング手順で必要なマッピングペア {#required-mappings}

**[!UICONTROL (Companies) LinkedIn Matched Audiences]** の宛先に対してアカウントオーディエンスをアクティブ化する場合、データを正常に書き出すには、次の 2 つのマッピングペアが必須です。

![LinkedIn マッピングの必須フィールド。](/help/destinations/assets/ui/activate-account-audiences/linkedin-mapping-required-fields.png)

| ソースフィールド | ターゲットフィールド |
|---------|----------|
| `accountName` | `companyName` |
| `accountKey.sourceKey` | `primaryId` （**[!UICONTROL Select Identity namespace]** を選択する際に、**[!UICONTROL Target Field]** ビューでこのフィールドを選択します）。<br> ![&#x200B; 宛先に対してアカウントオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択。](/help/destinations/assets/ui/activate-account-audiences/identity-namespace-highlighted.png " 宛先に対してアカウントオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択 "){width="100" zoomable="yes"} |

{style="table-layout:auto"}

## 書き出したデータ {#exported-data}

アクティベーションに成功すると、[!DNL LinkedIn] しいカスタムオーディエンスが [[!DNL LinkedIn Campaign Manager]](https://www.linkedin.com/campaignmanager/login) でプログラムによって作成されます。 オーディエンスメンバーシップは、アクティブ化されたオーディエンスに対してユーザーが適格であるか、不適格であるかに応じて調整されます。
