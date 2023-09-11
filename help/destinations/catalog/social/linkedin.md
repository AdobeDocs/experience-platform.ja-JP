---
keywords: linkedin 接続；linkedin 接続；linkedin 宛先；linkedin;linkedin;
title: Linkedin Matched Audiences 接続
description: ハッシュ化された電子メールに基づいて、オーディエンスのターゲティング、パーソナライゼーションおよび抑制のためのLinkedInキャンペーンのプロファイルをアクティブ化します。
exl-id: 74c233e9-161a-4e4a-98ef-038a031feff0
source-git-commit: 72225ac673ed921b5857a14070660134949e7e3e
workflow-type: tm+mt
source-wordcount: '1067'
ht-degree: 47%

---

# [!DNL LinkedIn Matched Audiences] 接続

## 概要 {#overview}

のプロファイルをアクティブ化する [!DNL LinkedIn] ハッシュ化された電子メールとモバイル ID に基づいて、オーディエンスのターゲティング、パーソナライゼーションおよび抑制のためのキャンペーンを作成します。

![Adobe Experience Platform UI でのlinkedInの宛先](../../assets/catalog/social/linkedin/catalog.png)

## ユースケース

を使用する方法とタイミングをより深く理解するために [!DNL LinkedIn Matched Audiences] の宛先に関しては、Adobe Experience Platformのお客様がこの機能を使用して解決できる使用例を以下に示します。

ソフトウェア会社が会議を開催し、参加者と連絡を取り合い、参加者の出席状況に基づいてパーソナライズされたオファーを表示したいと考えています。 会社は、独自の電子メールアドレスやモバイルデバイス ID を取り込むことができます [!DNL CRM] Adobe Experience Platformに その後、独自のオフラインデータからオーディエンスを構築し、それらのオーディエンスを [!DNL LinkedIn] ソーシャルプラットフォームを使用して、広告費用を最適化します。

## サポートされている ID {#supported-identities}

[!DNL LinkedIn Matched Audiences] では、以下の表で説明する ID のアクティベーションをサポートしています。[ID](/help/identity-service/namespaces.md) についての詳細情報。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| GAID | Google Advertising ID | ソース ID が GAID 名前空間の場合は、このターゲット ID を選択します。 |
| IDFA | Apple の広告主 ID | ソース ID が IDFA 名前空間の場合は、このターゲット ID を選択します。 |
| email_lc_sha256 | SHA256 アルゴリズムでハッシュ化されたメールアドレス | Adobe Experience Platform では、プレーンテキストと SHA256 でハッシュ化されたメールアドレスの両方がサポートされています。詳しくは、 [ID 一致要件](#id-matching-requirements-id-matching-requirements) を参照し、適切な名前空間を、それぞれプレーンテキストとハッシュ化された電子メールに使用してください。 ハッシュ化されていない属性がソースフィールドに含まれている場合は、「**[!UICONTROL 変換を適用]**」オプションをオンにして、アクティブ化時に [!DNL Platform] がデータを自動的にハッシュ化するように設定します。 |

{style="table-layout:auto"}

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスの起源 | サポートあり | 説明 |
---------|----------|----------|
| [!DNL Segmentation Service] | ✓ | Experience Platform [セグメント化サービス](../../../segmentation/home.md). |
| カスタムアップロード | ✓ | CSV ファイルから Experience Platform に[読み込まれた](../../../segmentation/ui/overview.md#import-audience)オーディエンス。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL オーディエンスの書き出し]** | オーディエンスのすべてのメンバーを、 [!DNL LinkedIn Matched Audiences] 宛先。 |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)の詳細についてはこちらを参照してください。 |

{style="table-layout:auto"}

## LinkedInアカウントの前提条件 {#LinkedIn-account-prerequisites}

使用する前に、 [!UICONTROL LinkedIn Matched Audience] 宛先、 [!DNL LinkedIn Campaign Manager] アカウントに [!DNL Creative Manager] 権限レベル以上。

[!DNL LinkedIn Campaign Manager] ユーザー権限の編集方法については、「[Add, Edit, and Remove User Permissions on Advertising Accounts](https://www.linkedin.com/help/lms/answer/5753)」（LinkedIn ドキュメント）を参照してください。

## ID 一致要件 {#id-matching-requirements}

[!DNL LinkedIn Matched Audiences] では、個人を特定できる情報 (PII) を明確に送信しないことが求められています。 したがって、オーディエンスは [!DNL LinkedIn Matched Audiences] キーオフできる *ハッシュ* 識別子（電子メールアドレスやモバイルデバイス ID など）。

Adobe Experience Platformに取り込む ID のタイプに応じて、対応する要件を満たす必要があります。

## 電子メールのハッシュ要件 {#email-hashing-requirements}

電子メールアドレスをAdobe Experience Platformに取り込む前にハッシュ化したり、電子メールアドレスをExperience Platformで明確に使用したり、 [!DNL Platform] アクティベーション時にハッシュ化します。

E メールアドレスの取り込みについて詳しくは、Experience Platformの [バッチ取得の概要](/help/ingestion/batch-ingestion/overview.md) そして [ストリーミング取り込みの概要](/help/ingestion/streaming-ingestion/overview.md).

電子メールアドレスを自分でハッシュ化する場合は、次の要件に従ってください。

* 電子メール文字列から先頭および末尾の空白文字をすべてトリミングします。 例： `johndoe@example.com`，ではない `<space>johndoe@example.com<space>`;
* 電子メール文字列をハッシュする場合は、必ず小文字の文字列をハッシュ化するようにしてください。
   * 例：`example@email.com`（`EXAMPLE@EMAIL.COM` ではない）
* ハッシュ化された文字列がすべて小文字であることを確認します。
   * 例：`55e79200c1635b37ad31a378c39feb12f120f116625093a19bc32fff15041149`（`55E79200C1635B37AD31A378C39FEB12F120F116625093A19bC32FFF15041149` ではない）
* 文字列にソルトを使用しないでください。

>[!NOTE]
>
>ハッシュ化されていない名前空間のデータは、によって自動的にハッシュ化されます。 [!DNL Platform] 有効化時。
> 属性ソースデータは自動的にハッシュ化されません。
> 
> 期間 [ID マッピング](../../ui/activate-segment-streaming-destinations.md#mapping) ステップ、ソースフィールドにハッシュ化されていない属性が含まれている場合は、 **[!UICONTROL 変換を適用]** オプション、 [!DNL Platform] 有効化時に、データを自動的にハッシュ化します。
> 
> The **[!UICONTROL 変換を適用]** オプションは、属性をソースフィールドとして選択した場合にのみ表示されます。 名前空間を選択した場合は表示されません。

![ID マッピング変換](../../assets/ui/activate-destinations/identity-mapping-transformation.png)

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

次のビデオでは、 [!DNL LinkedIn Matched Audiences] の宛先に移動して、オーディエンスをアクティブ化します。

>[!VIDEO](https://video.tv.adobe.com/v/332599/?quality=12&learn=on&captions=eng)

>[!NOTE]
>
>Adobe Experience Platform のユーザーインターフェイスは頻繁に更新され、このビデオが録画された後に変更されている可能性があります。 最新の情報については、 [宛先設定のチュートリアル](../../ui/connect-destination.md).

### 宛先に対する認証 {#authenticate}

1. 次を検索： [!DNL LinkedIn Matched Audiences] 宛先カタログの宛先および「 」を選択します。 **[!UICONTROL 設定]**.
2. 選択 **[!UICONTROL 宛先に接続]**.
   ![linkedInへの認証](/help/destinations/assets/catalog/social/linkedin/authenticate-linkedin-destination.png)
3. linkedIn資格情報を入力し、「 」を選択します。 **ログイン**.

### 宛先の詳細の入力 {#destination-details}

>[!CONTEXTUALHELP]
>id="platform_destinations_connect_linkedin_accountid"
>title="アカウント ID"
>abstract="LinkedIn キャンペーンマネージャーアカウント ID。この ID は、LinkedIn キャンペーンマネージャーアカウントで確認できます。"

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

* **[!UICONTROL 名前]**：今後この宛先を認識するための名前。
* **[!UICONTROL 説明]**：今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL アカウント ID]**: [!DNL LinkedIn Campaign Manager Account ID]. この ID は、 [!DNL LinkedIn Campaign Manager] アカウント。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>データをアクティブ化するには、**[!UICONTROL 宛先の管理]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]**&#x200B;および&#x200B;**[!UICONTROL セグメントの表示]** [に対するアクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先にオーディエンスをアクティブ化する手順については、[ストリーミングオーディエンス書き出し宛先に対するオーディエンスデータのアクティブ化](../../ui/activate-segment-streaming-destinations.md)を参照してください。

## 書き出したデータ {#exported-data}

アクティベーションが成功した場合、 [!DNL LinkedIn] カスタムオーディエンスは、 [[!DNL LinkedIn Campaign Manager]](https://www.linkedin.com/campaignmanager/login). ユーザーがアクティブ化されたオーディエンスの対象として認定または不適格となるので、オーディエンスメンバーシップは追加および削除されます。

>[!TIP]
>
>Adobe Experience Platformと [!DNL LinkedIn Matched Audiences] は、履歴オーディエンスのバックフィルをサポートします。 すべての履歴オーディエンスの資格がに送信されます。 [!DNL LinkedIn] オーディエンスを宛先に対してアクティブ化する場合。
