---
keywords: Google 広告；Google 広告；Google アドワード；Google AdWords;Google アドワード
title: Google 広告接続
description: Google 広告（旧称 Google AdWords）は、テキストベースの検索、グラフィック表示、YouTube ビデオ、アプリ内モバイルディスプレイをまたいで、企業がクリック課金広告を利用できるオンライン広告サービスです。
exl-id: 7143f476-49a8-42aa-bfb4-b11fc2b8f5c3
source-git-commit: f04ea9aed586c8582286de82bfeee3f6f04cc360
workflow-type: tm+mt
source-wordcount: '708'
ht-degree: 19%

---

# [!DNL Google Ads] 接続

## 概要 {#overview}

[!DNL Google Ads]（旧称） [!DNL Google AdWords]は、テキストベースの検索、グラフィックディスプレイ、ビデオ、アプリ内モバイルディスプレイをまたいで、企業がクリック課金広告を利用できるオンライン広告サービスで [!DNL YouTube] す。

## 宛先の詳細 {#specifics}

次の [!DNL Google Ads] の宛先に固有の詳細に注意してください。

* アクティブ化されたオーディエンスは、[!DNL Google] プラットフォームでプログラムによって作成されます。
* [!DNL Platform] 現在、アクティベーションの成功を検証するための測定指標は含まれていません。統合を検証し、オーディエンスターゲット設定サイズについて理解するには、Google でのオーディエンス数を参照します。

>[!IMPORTANT]
>
>[!DNL Google Ads] で最初の宛先を作成したい場合で、以前に (Audience Managerや他のアプリケーションで )Experience CloudID サービスで [ID 同期機能 ](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/idsync.html?lang=ja) を有効にしていない場合は、Adobeコンサルティングまたはカスタマーケアに問い合わせて ID 同期を有効にしてください。 以前に Google 統合をAudience Managerで設定していた場合、設定した ID 同期は Platform に引き継がれます。

## サポートされる ID {#supported-identities}

[!DNL Google Ad Manager] では、以下の表で説明する ID のアクティブ化をサポートしています。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| GAID | [!DNL Google Advertising ID] | ソース ID が GAID 名前空間の場合は、このターゲット ID を選択します。 |
| IDFA | [!DNL Apple ID for Advertisers] | ソース ID が IDFA 名前空間の場合は、このターゲット ID を選択します。 |
| AAM UUID | [Adobe Audience Manager [!DNL Unique User ID]](https://experienceleague.adobe.com/docs/audience-manager/user-guide/reference/ids-in-aam.html)（別名） [!DNL Device ID]。数値型で 38 桁のデバイス ID。Audience Manager はこの値を、操作するデバイスのそれぞれに関連付けます。 | Google は、[AAM UUID](https://experienceleague.adobe.com/docs/audience-manager/user-guide/reference/ids-in-aam.html?lang=en) を使用してカリフォルニア州のユーザーをターゲット設定し、他のすべてのユーザーの Google Cookie ID を使用します。 |
| [!DNL Google] cookie ID | [!DNL Google] cookie ID | [!DNL Google] は、この ID を使用してカリフォルニア以外のユーザーをターゲット設定します。 |
| RIDA | 広告用の Roku ID。 この ID は、Roku デバイスを一意に識別します。 |  |
| MAID | Microsoft 広告 ID。 この ID は、Windows 10 を実行しているデバイスを一意に識別します。 |  |
| Amazon Fire TV ID | この ID は、Amazon Fire TV を一意に識別します。 |  |

## 書き出しタイプ {#export-type}

**セグメントの書き出し**  — セグメント（オーディエンス）のすべてのメンバーを Google の宛先に書き出します。

## 前提条件 {#prerequisites}

### 既存の [!DNL Google Ads] アカウント

>[!IMPORTANT]
>
> [!DNL Google] では、サードパーテ [!DNL Google Ads] ィベンダーとの新しい cookie 統合が非推奨（廃止予定）となりました。次の節の許可リスト手順を実行するには、[!DNL Google Ads] との既存の統合が必要です。 その結果、[!DNL Google Ads] を使用する場合の推奨アプローチは、[!DNL Google Customer Match] 統合を設定することです。 [!DNL Google Customer Match] 統合の作成の詳細については、[[!DNL Google Customer Match]](./google-customer-match.md) 接続の作成に関するチュートリアルを参照してください。

### 許可リストへの登録 {#allow-listing}

>[!NOTE]
>
>許可リストは、Platform で最初の [!DNL Google Ads] 宛先を設定する前に必須です。 宛先を作成する前に、[!DNL Google] が以下に説明する許可リストプロセスを完了していることを確認してください。

Platform で [!DNL Google Ads] の宛先を作成する前に、[!DNL Google] に問い合わせて、許可されたデータプロバイダーのリストにAdobeを登録し、お使いのアカウントを許可リストに追加する必要があります。 [!DNL Google] に問い合わせ、次の情報を入力します。

* **アカウント ID**:Adobeのアカウント ID が Google に送信されます。アカウント ID:87933855.
* **顧客 ID**:Adobeの顧客アカウント ID（Google を使用）。顧客 ID:89690775.
* アカウントのタイプ：**AdWords**
* **Google AdWords ID**:これは、を使用した ID で [!DNL Google]す。通常、ID の形式は 123-456-7890 です。

## 宛先に接続 {#connect}

この宛先に接続するには、[ 宛先の設定に関するチュートリアル ](../../ui/connect-destination.md) で説明されている手順に従います。

### 接続パラメーター {#parameters}

[ この宛先を設定 ](../../ui/connect-destination.md) する際は、次の情報を指定する必要があります。

* **[!UICONTROL 名前]**：この宛先の名前を入力します。
* **[!UICONTROL 説明]**：オプション。例えば、この宛先を使用しているキャンペーンを指定できます。
* **[!UICONTROL アカウントのタイプ]**：AdWords は利用可能な唯一のオプションです。
* **[!UICONTROL アカウント ID]**:アカウント ID にを入力しま [!DNL Google Ads]す。通常、ID の形式は 123-456-7890 です。

## この宛先へのセグメントのアクティブ化 {#activate}

この宛先に対してオーディエンスセグメントをアクティブ化する手順については、[ ストリーミングセグメントの書き出し先へのオーディエンスデータのアクティブ化 ](../../ui/activate-segment-streaming-destinations.md) を参照してください。

## エクスポートされたデータ

データが [!DNL Google Ads] の宛先に正常に書き出されたかどうかを確認するには、[!DNL Google Ads] アカウントを確認します。 アクティブ化に成功した場合は、オーディエンスがアカウントに入力されます。

## トラブルシューティング {#troubleshooting}

### 400 Bad Request エラーメッセージ {#bad-request}

この宛先を設定すると、次のエラーが発生する場合があります。

`{"message":"Google Error: AuthorizationError.USER_PERMISSION_DENIED","code":"400 BAD_REQUEST"}`

このエラーは、顧客アカウントが [ 前提条件 ](#prerequisites) に従っていない場合、または顧客が既存の [!DNL Google Ads] アカウントを使用せずに宛先を設定しようとした場合に発生します。

[!DNL Google] では、サードパーテ [!DNL Google Ads] ィベンダーとの新しい cookie 統合が非推奨（廃止予定）となりました。[allow-list](#allow-listing) 手順を実行するには、[!DNL Google Ads] との既存の統合が必要です。

[!DNL Google Ads] を使用する場合の推奨アプローチは、[[!DNL Google Customer Match]](google-customer-match.md) 統合を設定することです。
