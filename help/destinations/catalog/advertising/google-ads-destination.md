---
keywords: Google広告；google 広告；google adwords;Google AdWords;Google Adwords
title: Google Ads 接続
description: Google 広告（旧称 Google AdWords）は、テキストベースの検索、グラフィック表示、YouTube ビデオ、アプリ内モバイルディスプレイをまたいで、企業がクリック課金広告を利用できるオンライン広告サービスです。
exl-id: 7143f476-49a8-42aa-bfb4-b11fc2b8f5c3
source-git-commit: dd18350387aa6bdeb61612f0ccf9d8d2223a8a5d
workflow-type: tm+mt
source-wordcount: '898'
ht-degree: 24%

---

# [!DNL Google Ads] 接続

## 概要 {#overview}

[!DNL Google Ads]( 旧称： [!DNL Google AdWords]は、テキストベースの検索、グラフィック表示をまたいで、企業がクリック課金広告を可能にするオンライン広告サービスです。 [!DNL YouTube] ビデオおよびアプリ内モバイルディスプレイ。

## 宛先の詳細 {#specifics}

次に示す詳細は、 [!DNL Google Ads] 宛先：

* アクティブ化されたオーディエンスは、 [!DNL Google] プラットフォーム。
* [!DNL Platform] は、現在、アクティブ化の成功を検証するための測定指標を含んでいません。 統合を検証し、オーディエンスターゲット設定サイズについて理解するには、Google でのオーディエンス数を参照します。

>[!IMPORTANT]
>
>を使用して最初の宛先を作成する場合は、 [!DNL Google Ads] 有効にしていない [ID 同期機能](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/idsync.html?lang=ja) 以前のExperience CloudID サービス (Audience Managerや他のアプリケーションを使用 ) では、Adobeコンサルティングまたはカスタマーケアに問い合わせて、ID 同期を有効にしてください。 以前にAudience ManagerでGoogle統合を設定していた場合、設定した ID 同期は Platform に引き継がれます。

## サポートされる ID {#supported-identities}

[!DNL Google Ad Manager] では、以下の表で説明する id のアクティブ化をサポートしています。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| GAID | [!DNL Google Advertising ID] | ソース ID が GAID 名前空間の場合は、このターゲット ID を選択します。 |
| IDFA | [!DNL Apple ID for Advertisers] | ソース ID が IDFA 名前空間の場合は、このターゲット ID を選択します。 |
| AAM UUID | [Adobe Audience Manager [!DNL Unique User ID]](https://experienceleague.adobe.com/docs/audience-manager/user-guide/reference/ids-in-aam.html?lang=ja)別名 [!DNL Device ID]. 数値型で 38 桁のデバイス ID。Audience Manager はこの値を、操作するデバイスのそれぞれに関連付けます。 | Google使用 [AAM UUID](https://experienceleague.adobe.com/docs/audience-manager/user-guide/reference/ids-in-aam.html?lang=ja) を使用してカリフォルニアのユーザーをターゲット設定し、他のすべてのユーザーのGoogle Cookie ID を使用します。 |
| [!DNL Google] cookie ID | [!DNL Google] cookie ID | [!DNL Google] は、この ID を使用してカリフォルニア以外のユーザーをターゲットにします。 |
| RIDA | 広告用 Roku ID。 この ID は、Roku デバイスを一意に識別します。 |  |
| MAID | Microsoft Advertising ID。 この ID は、Windows 10 を実行しているデバイスを一意に識別します。 |  |
| Amazon Fire TV ID | この ID は、Amazon Fire TV を一意に識別します。 |  |

{style=&quot;table-layout:auto&quot;}

## エクスポートのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、次の表を参照してください。

| 項目 | タイプ | 備考 |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL セグメントエクスポート]** | セグメント（オーディエンス）のすべてのメンバーをGoogleの宛先に書き出します。 |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は、API ベースの接続です。 セグメント評価に基づいてExperience Platform内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。 詳細を表示 [ストリーミング先](/help/destinations/destination-types.md#streaming-destinations). |

{style=&quot;table-layout:auto&quot;}

## 前提条件 {#prerequisites}

### 既存 [!DNL Google Ads] アカウント

>[!IMPORTANT]
>
> [!DNL Google] は非推奨です [!DNL Google Ads] cookie とサードパーティベンダーの統合に関する情報を含む )。 次の節の許可リスト手順を実行するには、 [!DNL Google Ads]. その結果、 [!DNL Google Ads] は [!DNL Google Customer Match] 統合とも呼ばれます。 詳しくは、 [!DNL Google Customer Match] 統合については、 [[!DNL Google Customer Match]](./google-customer-match.md) 接続。

### 許可リストへの登録 {#allow-listing}

>[!NOTE]
>
>許可リストは、最初の [!DNL Google Ads] の宛先を設定します。 以下に説明する許可リストプロセスが次の方法で完了していることを確認してください： [!DNL Google] 宛先を作成する前に、をクリックします。

を作成する前に [!DNL Google Ads] の宛先に設定する場合は、 [!DNL Google] Adobeを許可されたデータプロバイダーのリストに追加し、お使いのアカウントを許可リストに追加する場合。 連絡先 [!DNL Google] 次の情報を入力します。

* **アカウント ID**:AdobeのGoogleアカウント ID。 アカウント ID :87933855.
* **顧客 ID**:Adobeの顧客アカウント ID とGoogle。 顧客 ID :89690775.
* アカウントのタイプ：**AdWords**
* **Google AdWords ID**:これは ID です。 [!DNL Google]. 通常、ID の形式は 123-456-7890 です。

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、 **[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions). 詳しくは、 [アクセス制御の概要](/help/access-control/ui/overview.md) または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。

### 接続パラメーター {#parameters}

この宛先を[設定](../../ui/connect-destination.md)するとき、次の情報を指定する必要があります。

* **[!UICONTROL 名前]**：この宛先に任意の名前を入力します。
* **[!UICONTROL 説明]**：オプション。例えば、この宛先を使用しているキャンペーンを指定できます。
* **[!UICONTROL アカウントのタイプ]**：AdWords は利用可能な唯一のオプションです。
* **[!UICONTROL アカウント ID]**:アカウント ID に [!DNL Google Ads]. 通常、ID の形式は 123-456-7890 です。

### アラートの有効化 {#enable-alerts}

アラートを有効にして、宛先へのデータフローのステータスに関する通知を受け取ることができます。 リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートの詳細については、 [UI を使用した宛先アラートの購読](../../ui/alerts.md).

宛先接続の詳細の指定が完了したら、 **[!UICONTROL 次へ]**.

## この宛先に対してセグメントをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>データをアクティブ化するには、 **[!UICONTROL 宛先の管理]**, **[!UICONTROL 宛先のアクティブ化]**, **[!UICONTROL プロファイルの表示]**、および **[!UICONTROL セグメントを表示]** [アクセス制御権限](/help/access-control/home.md#permissions). 詳しくは、 [アクセス制御の概要](/help/access-control/ui/overview.md) または製品管理者に問い合わせて、必要な権限を取得してください。

詳しくは、 [ストリーミングセグメントの書き出し先に対するオーディエンスデータのアクティブ化](../../ui/activate-segment-streaming-destinations.md) を参照してください。

## 書き出したデータ

データがに正常に書き出されたかどうかを確認するには、以下を実行します。 [!DNL Google Ads] 宛先、 [!DNL Google Ads] アカウント アクティブ化に成功した場合、オーディエンスがアカウントに入力されます。

## トラブルシューティング {#troubleshooting}

### 400 Bad Request エラーメッセージ {#bad-request}

この宛先を設定する際に、次のエラーが発生する場合があります。

`{"message":"Google Error: AuthorizationError.USER_PERMISSION_DENIED","code":"400 BAD_REQUEST"}`

このエラーは、顧客アカウントが [前提条件](#prerequisites) または、顧客が既存の [!DNL Google Ads] アカウント

[!DNL Google] は非推奨です [!DNL Google Ads] cookie とサードパーティベンダーの統合に関する情報を含む )。 次の手順で [allow-list](#allow-listing) の手順では、 [!DNL Google Ads].

を使用するための推奨される方法 [!DNL Google Ads] は [[!DNL Google Customer Match]](google-customer-match.md) 統合とも呼ばれます。
