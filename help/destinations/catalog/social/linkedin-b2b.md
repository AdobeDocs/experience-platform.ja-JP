---
title: （企業） LinkedInとの連携
description: この宛先を使用して、Account-Based Marketing（ABM）ユースケースのアカウントオーディエンスをアクティブ化します。 ハッシュ化されたメールにもとづいて、オーディエンスのターゲティング、パーソナライゼーション、抑制にLinkedIn キャンペーンのプロファイルを活用できます。
exl-id: 68d2cca3-952b-49d0-8ea2-e776a233b752
source-git-commit: d946d3dbb09c1fe0163fba3a892b4c0f1b331f87
workflow-type: tm+mt
source-wordcount: '760'
ht-degree: 19%

---

# （企業） LinkedIn Match Audiences接続 {#companies-linkedin}

>[!AVAILABILITY]
>
>（企業） LinkedIn宛先にアカウントオーディエンスをアクティブ化する機能は、[の](/help/rtcdp/overview.md#rtcdp-b2b)Business-to-Business[および](/help/rtcdp/overview.md#rtcdp-b2p)Business-to-Person[!DNL Real-Time Customer Data Platform] エディションを購入する企業で使用できます。

この宛先を使用して、Account-Based Marketing（ABM）のユースケース向けに[ アカウントオーディエンス ](/help/segmentation/types/account-audiences.md)をアクティブ化します。 **[!UICONTROL (Companies) LinkedIn]**&#x200B;の企業間宛先を介して、ターゲットアカウント内の関連するペルソナと役割に広告を表示します。 LinkedIn プラットフォームでのアカウントターゲティングについて[詳しくは、](https://business.linkedin.com/marketing-solutions/cx/21/10/ad-targeting/account-targeting)LinkedInのドキュメントをご覧ください。

>[!TIP]
>
>個人レベル（または企業対消費者）のユースケースの場合、Adobeでは、[LinkedIn Matched Audience](/help/destinations/catalog/social/linkedin.md)の宛先を使用することをお勧めします。

![LinkedIn アカウントの宛先がExperience Platform UIに表示されます。](/help/destinations/assets/catalog/social/linkedin-b2b/linkedin-b2b-destination.png)

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスの由来 | サポートあり | 説明 |
|---------|----------|----------|
| [!DNL Segmentation Service] | ○ | Experience Platform [ セグメント化サービス ](../../../segmentation/home.md)を通じて生成されたオーディエンス。 |
| その他すべてのオーディエンスの生成元 | ○ | このカテゴリには、[!DNL Segmentation Service]を通じて生成されたオーディエンス以外のすべてのオーディエンスのオリジンが含まれます。 [様々なオーディエンスの起源](/help/segmentation/ui/audience-portal.md#customize)について読みます。 次に例を示します。 <ul><li> カスタムアップロードオーディエンス [がCSV ファイルからExperience Platformに](../../../segmentation/ui/audience-portal.md#import-audience)をインポートしました。</li><li> 類似オーディエンス， </li><li> 連合オーディエンス， </li><li> [!DNL Adobe Journey Optimizer]などの他のExperience Platform アプリで生成されたオーディエンス </li><li> その他。 </li></ul> |

{style="table-layout:auto"}



オーディエンスのデータタイプ別にサポートされるオーディエンス：

| オーディエンスのデータタイプ | サポートあり | 説明 | ユースケース |
|--------------------|-----------|-------------|-----------|
| [人物オーディエンス ](/help/segmentation/types/people-audiences.md) | ○ | 顧客プロファイルにもとづいて、マーケティング施策の特定のグループをターゲットにすることができます。 | 買い物客やカートの放棄が多い |
| [ アカウントオーディエンス ](/help/segmentation/types/account-audiences.md) | ○ | アカウントベースドマーケティング戦略のために、特定の組織内の個人をターゲットにします。 | B2B マーケティング |
| [見込みオーディエンス ](/help/segmentation/types/prospect-audiences.md) | × | まだ顧客ではないが、ターゲットオーディエンスと特徴を共有する個人をターゲットにします。 | サードパーティデータによる見込み顧客の開拓 |
| [ データセットの書き出し](/help/catalog/datasets/overview.md) | × | [!DNL Adobe Experience Platform] データ レイクに保存されている構造化データのコレクション。 | レポート，データサイエンスワークフロー |

{style="table-layout:auto"}


## 書き出しのタイプと頻度 {#export-type-and-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
|--------------|-----------|---------------------------|
| 書き出しタイプ | オーディエンスの書き出し | 名前や電話番号などの主要な識別子を使用して、あらゆるオーディエンスメンバーを書き出すことができます。 |
| 頻度 | ストリーミング | &quot;Always-on&quot; API ベースの接続。 更新は、プロファイルが変更された直後にダウンストリームに送信されます。 |

{style="table-layout:auto"}

## 前提条件 {#prerequisites}

アカウントオーディエンスをLinkedInに書き出すには、次の前提条件を満たしていることを確認してください。

### LinkedIn アカウントの前提条件 {#LinkedIn-account-prerequisites}

[!UICONTROL (Companies) LinkedIn Matched Audience]宛先を使用する前に、[!DNL LinkedIn Campaign Manager] アカウントが[!DNL Creative Manager]権限レベル以上であることを確認してください。

[!DNL LinkedIn Campaign Manager]のユーザー権限を編集する方法については、LinkedIn ドキュメントの「[Advertising アカウントのユーザー権限を追加、編集、削除](https://www.linkedin.com/help/lms/answer/5753)」を参照してください。

## 宛先への接続 {#connect}

>[!IMPORTANT]
>
>宛先に接続するには、**[!UICONTROL View Destinations]**&#x200B;および&#x200B;**[!UICONTROL Manage Destinations]** [ アクセス制御権限](/help/access-control/home.md#permissions)が必要です。 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

1. 宛先カタログで[!DNL (Companies) LinkedIn Matched Audiences]の宛先を検索し、**[!UICONTROL Set Up]**&#x200B;を選択します。
2. **[!UICONTROL Connect to destination]** を選択します。
   ![LinkedInに対する認証](/help/destinations/assets/catalog/social/linkedin-b2b/authenticate-linkedin-destination.png)
3. LinkedInの資格情報を入力し、**ログイン**&#x200B;を選択します。

LinkedInでのサインインプロセスが完了したら、次の手順に進むことができます。

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

* **[!UICONTROL Name]**：今後この宛先を認識する際に使用する名前。
* **[!UICONTROL Description]**：今後この宛先を特定するのに役立つ説明です。
* **[!UICONTROL Account ID]**：あなたの[!DNL LinkedIn Campaign Manager Account ID]。 このIDは[!DNL LinkedIn Campaign Manager] アカウントで見つけることができます。

これで、アカウントオーディエンスをLinkedInにアクティベートする準備が整いました。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
>
>* データをアクティブ化するには、**[!UICONTROL View Destinations]**、**[!UICONTROL Activate Destinations]**、**[!UICONTROL View Profiles]**&#x200B;および&#x200B;**[!UICONTROL View Segments]** [ アクセス制御権限](/help/access-control/home.md#permissions)が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* *ID*&#x200B;をエクスポートするには、**[!UICONTROL View Identity Graph]** [ アクセス制御権限](/help/access-control/home.md#permissions)が必要です。<br> ![ ワークフローで強調表示されているID名前空間を選択して、アカウントオーディエンスを宛先にアクティブ化します。](/help/destinations/assets/ui/activate-account-audiences/identity-namespace-highlighted.png " ワークフローで強調表示されたID名前空間を選択して、アカウントオーディエンスを宛先にアクティブ化します。"){width="100" zoomable="yes"}

この宛先に対してアカウントオーディエンスをアクティブ化する手順については、[ アカウントオーディエンスをアクティブ化](/help/destinations/ui/activate-account-audiences.md)を参照してください。

## **[!UICONTROL (Companies) LinkedIn Matched Audiences]**&#x200B;宛先にアカウントオーディエンスをアクティブ化する際に、マッピングステップで必須のマッピングペア {#required-mappings}

**[!UICONTROL (Companies) LinkedIn Matched Audiences]**&#x200B;宛先に対してアカウントオーディエンスをアクティブ化する場合、データを正常にエクスポートするには、次の2つのマッピングペアが必須であることに注意してください。

![LinkedInが必須フィールドをマッピングしています。](/help/destinations/assets/ui/activate-account-audiences/linkedin-mapping-required-fields.png)

| ソースフィールド | ターゲットフィールド |
|---------|----------|
| `accountName` | `companyName` |
| `accountKey.sourceKey` | `primaryId` （**[!UICONTROL Select Identity namespace]**&#x200B;を選択する際に&#x200B;**[!UICONTROL Target Field]** ビューでこのフィールドを選択します）。<br> ![ ワークフローで強調表示されているID名前空間を選択して、アカウントオーディエンスを宛先にアクティブ化します。](/help/destinations/assets/ui/activate-account-audiences/identity-namespace-highlighted.png " ワークフローで強調表示されたID名前空間を選択して、アカウントオーディエンスを宛先にアクティブ化します。"){width="100" zoomable="yes"} |

{style="table-layout:auto"}

## 書き出したデータ {#exported-data}

アクティブ化が成功すると、[!DNL LinkedIn][[!DNL LinkedIn Campaign Manager]で](https://www.linkedin.com/campaignmanager/login)個のカスタムオーディエンスがプログラムによって作成されます。 オーディエンスメンバーシップは、アクティベートされたオーディエンスに対してユーザーが適格または失格になった場合に調整されます。
