---
keywords: linkedin 接続；linkedin 接続；linkedin の宛先；linkedin;
title: Linkedin Matched Audiences 接続
description: ハッシュ化されたメールに基づいて、オーディエンスのターゲティング、パーソナライゼーションおよび抑制を行うための、LinkedIn キャンペーン用のプロファイルをアクティブ化します。
exl-id: 74c233e9-161a-4e4a-98ef-038a031feff0
source-git-commit: 653f43ac6afb25445fe8ef3c2832be8f1c4723fe
workflow-type: tm+mt
source-wordcount: '1234'
ht-degree: 32%

---

# [!DNL LinkedIn Matched Audiences] 接続

## 概要 {#overview}

ハッシュ化された電子メールとモバイル ID に基づいて、オーディエンスのターゲティング、パーソナライゼーションおよび抑制のために、[!DNL LinkedIn] キャンペーン用のプロファイルをアクティブ化します。

![Adobe Experience Platform UI での LinkedIn の宛先 ](../../assets/catalog/social/linkedin/catalog.png)

## ユースケース

[!DNL LinkedIn Matched Audiences] の宛先を使用する方法とタイミングをより深く理解するために、Adobe Experience Platformのお客様がこの機能を使用して解決できるユースケースを以下に示します。

あるソフトウェア会社は、会議を開催し、参加者と連絡を取り合い、会議の出席ステータスに基づいてパーソナライズされたオファーを表示したいと考えています。 会社は、メールアドレスまたはモバイルデバイス ID を [!DNL CRM] からAdobe Experience Platformに取り込むことができます。 その後、独自のオフラインデータからオーディエンスを作成し、これらのオーディエンスを [!DNL LinkedIn] ソーシャルプラットフォームに送信して、広告費用を最適化できます。

## サポートされている ID {#supported-identities}

[!DNL LinkedIn Matched Audiences] では、以下の表で説明する ID のアクティブ化をサポートしています。 [ID](/help/identity-service/features/namespaces.md) についての詳細情報。

>[!IMPORTANT]
>
>2025 年 9 月以降、[!DNL LinkedIn Matched Audiences] 宛先は [!DNL IDFA] （広告主の識別子） ID をサポートしなくなりました。  この変更は LinkedIn の要件に起因し、Experience Platformの宛先サービスのアップグレードには関係しません。


| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| GAID | GOOGLE ADVERTISING ID | ソース ID が GAID 名前空間の場合は、このターゲット ID を選択します。 |
| email_lc_sha256 | SHA256 アルゴリズムでハッシュ化されたメールアドレス | Adobe Experience Platform では、プレーンテキストと SHA256 でハッシュ化されたメールアドレスの両方がサポートされています。[ID の一致要件 ](#id-matching-requirements-id-matching-requirements) の節の手順に従って、プレーンテキストとハッシュ化されたメールに適切な名前空間をそれぞれ使用します。 ハッシュ化されていない属性がソースフィールドに含まれている場合は、「**[!UICONTROL 変換を適用]**」オプションをオンにして、アクティブ化時に [!DNL Experience Platform] がデータを自動的にハッシュ化するように設定します。 |

{style="table-layout:auto"}

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスオリジン | サポートあり | 説明 |
|---------|----------|----------|
| [!DNL Segmentation Service] | ✓ | Experience Platform [ セグメント化サービス ](../../../segmentation/home.md) を通じて生成されたオーディエンス。 |
| カスタムアップロード | ✓ | CSV ファイルから Experience Platform に[読み込まれた](../../../segmentation/ui/audience-portal.md#import-audience)オーディエンス。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL オーディエンスの書き出し]** | オーディ [!DNL LinkedIn Matched Audiences] ンスの宛先で使用される識別子（名前、電話番号など）を使用して、オーディエンスのすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)の詳細についてはこちらを参照してください。 |

{style="table-layout:auto"}

## LinkedIn アカウントの前提条件 {#LinkedIn-account-prerequisites}

[!UICONTROL LinkedIn でマッチしたオーディエンス &#x200B;] の宛先を使用する前に、[!DNL LinkedIn Campaign Manager] アカウントの権限レベルが [!DNL Creative Manager] 以上であることを確認してください。

[!DNL LinkedIn Campaign Manager] ユーザー権限の編集方法については、LinkedIn ドキュメントの [Advertising アカウントのユーザー権限の追加、編集、削除 ](https://www.linkedin.com/help/lms/answer/5753) を参照してください。

## ID の一致要件 {#id-matching-requirements}

[!DNL LinkedIn Matched Audiences] では、個人を特定できる情報（PII）が明確に送信されないことが必要です。 したがって、[!DNL LinkedIn Matched Audiences] に対してアクティブ化されたオーディエンスは、メールアドレスやモバイルデバイス ID などの *ハッシュ化された* 識別子）をキーオフにすることができます。

Adobe Experience Platformに取り込む ID のタイプに応じて、対応する要件に従う必要があります。

## メールハッシュ要件 {#email-hashing-requirements}

メールアドレスをAdobe Experience Platformに取り込む前にハッシュ化したり、Experience Platformでメールアドレスを明確に使用して、アクティベーション時 [!DNL Experience Platform] ハッシュ化したりできます。

Experience Platformでのメールアドレスの取り込みについて詳しくは、[ バッチ取り込みの概要 ](/help/ingestion/batch-ingestion/overview.md) および [ ストリーミング取り込みの概要 ](/help/ingestion/streaming-ingestion/overview.md) を参照してください。

メールアドレスを自分でハッシュ化することを選択する場合は、次の要件に必ず従ってください。

* メール文字列から先頭と末尾のすべてのスペースをトリミングします。 （例：`johndoe@example.com` ではなく `<space>johndoe@example.com<space>`）
* メール文字列をハッシュ化する場合は、小文字の文字列もハッシュ化します。
   * 例：`example@email.com` ではなく `EXAMPLE@EMAIL.COM`;
* ハッシュ化された文字列がすべて小文字であることを確認します
   * 例：`55e79200c1635b37ad31a378c39feb12f120f116625093a19bc32fff15041149` ではなく `55E79200C1635B37AD31A378C39FEB12F120F116625093A19bC32FFF15041149`;
* ひもに塩をかけるな。

>[!NOTE]
>
>ハッシュ化されていない名前空間のデータは、アクティベーション時に [!DNL Experience Platform] によって自動的にハッシュ化されます。
>&#x200B;> 属性ソースデータは、自動的にはハッシュ化されません。
> 
> [ID マッピング ](../../ui/activate-segment-streaming-destinations.md#mapping) 手順で、ハッシュ化されていない属性がソースフィールドに含まれている場合は、「**[!UICONTROL 変換を適用]**」オプションをオンにして、アクティブ化時にデータを自動的 [!DNL Experience Platform] ハッシュ化するように設定します。
> 
> 「**[!UICONTROL 変換を適用]**」オプションは、属性をソースフィールドとして選択した場合にのみ表示されます。 名前空間を選択した場合は表示されません。

![ID マッピング変換 ](../../assets/ui/activate-destinations/identity-mapping-transformation.png)

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL 宛先の表示]** および **[!UICONTROL 宛先の管理]**&#x200B;[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

次のビデオでは、[!DNL LinkedIn Matched Audiences] しい宛先を設定し、オーディエンスをアクティブ化する手順も示します。

>[!VIDEO](https://video.tv.adobe.com/v/3411787/?quality=12&learn=on&captions=jpn)

>[!NOTE]
>
>Adobe Experience Platform のユーザーインターフェイスは頻繁に更新され、このビデオが録画された後に変更されている可能性があります。 最新情報については、[ 宛先設定のチュートリアル ](../../ui/connect-destination.md) を参照してください。

### 宛先に対する認証 {#authenticate}

1. 宛先カタログで [!DNL LinkedIn Matched Audiences] の宛先を見つけて、「**[!UICONTROL 設定]**」を選択します。
2. **[!UICONTROL 宛先に接続]** を選択します。
   ![LinkedIn への認証 ](/help/destinations/assets/catalog/social/linkedin/authenticate-linkedin-destination.png)
3. LinkedIn 資格情報を入力し、「**ログイン**」を選択します。

### 認証資格情報を更新 {#refresh-authentication-credentials}

LinkedIn トークンは 60 日ごとに期限切れになります。 トークンの有効期限は、「**[!UICONTROL アカウント]**&#x200B;**[[!UICONTROL または]](../../ui/destinations-workspace.md#accounts)** 参照 **[[!UICONTROL タブの「アカウントの有効期限]](../../ui/destinations-workspace.md#browse)** 列から監視できます。

トークンの有効期限が切れると、宛先へのデータの書き出しは機能しなくなります。 この状況を回避するには、次の手順を実行して再認証を行います。

1. **[!UICONTROL 宛先]**/**[!UICONTROL アカウント]** に移動します。
2. （任意）ページで使用可能なフィルターを使用して、LinkedIn アカウントのみを表示します。
   ![LinkedIn アカウントのみを表示するようにフィルター ](/help/destinations/assets/catalog/social/linkedin/refresh-oauth-filters.png)
3. 更新するアカウントを選択し、省略記号を選択して **[!UICONTROL 詳細を編集]** を選択します。
   ![ 詳細コントロールの編集を選択 ](/help/destinations/assets/catalog/social/linkedin/refresh-oauth-edit-details.png)
4. モーダルウィンドウで、「**[!UICONTROL OAuth を再接続]**」を選択し、LinkedIn 資格情報を使用して再認証します。
   ![ 「OAuth に再接続」オプションを使用したモーダルウィンドウ ](/help/destinations/assets/catalog/social/linkedin/reconnect-oauth-control.png)

>[!SUCCESS]
> 
>認証資格情報が更新され、有効期限が 60 日にリセットされます。

### 宛先の詳細の入力 {#destination-details}

>[!CONTEXTUALHELP]
>id="platform_destinations_connect_linkedin_accountid"
>title="アカウント ID"
>abstract="LinkedIn キャンペーンマネージャーアカウント ID。この ID は、LinkedIn キャンペーンマネージャーアカウントで確認できます。"

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

* **[!UICONTROL 名前]**：今後この宛先を認識するための名前。
* **[!UICONTROL 説明]**：今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL アカウント ID]**：お使いの [!DNL LinkedIn Campaign Manager Account ID]。 この ID は [!DNL LinkedIn Campaign Manager] アカウントで確認できます。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>* データをアクティブ化するには、**[!UICONTROL 宛先の表示]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]** および **[!UICONTROL セグメントの表示]**&#x200B;[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* *ID* を書き出すには、**[!UICONTROL ID グラフの表示]**&#x200B;[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。<br> ![ 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択します。](/help/destinations/assets/overview/export-identities-to-destination.png " 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択 "){width="100" zoomable="yes"}

この宛先にオーディエンスをアクティブ化する手順については、[ストリーミングオーディエンス書き出し宛先に対するオーディエンスデータのアクティブ化](../../ui/activate-segment-streaming-destinations.md)を参照してください。

## 書き出したデータ {#exported-data}

アクティベーションに成功すると、[!DNL LinkedIn] しいカスタムオーディエンスが [[!DNL LinkedIn Campaign Manager]](https://www.linkedin.com/campaignmanager/login) でプログラムによって作成されます。 オーディエンスメンバーシップは、アクティブ化されたオーディエンスに対してユーザーが適格であるか、不適格であるかに応じて調整されます。

>[!TIP]
>
>Adobe Experience Platformと [!DNL LinkedIn Matched Audiences] の統合は、履歴オーディエンスバックフィルをサポートします。 宛先に対してオーディエンスをアクティブ化すると、すべてのオーディエンス選定履歴が [!DNL LinkedIn] に送信されます。
