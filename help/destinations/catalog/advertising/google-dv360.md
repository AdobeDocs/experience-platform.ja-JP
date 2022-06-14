---
keywords: DoubleClick Bid Manager;DoubleClick bid manager;DoubleClick;Display & Video 360;display 360;video 360;Display 360;display 360;display and video
title: Google Display & Video 360 接続
description: Display & Video 360（旧称 DoubleClick Bid Manager）は、ディスプレイ広告、ビデオ、モバイルの在庫ソースをまたいで、再ターゲティングと、オーディエンスにターゲットを絞ったデジタルキャンペーンの実行に使用できるツールです。
exl-id: bdd3b3fd-891f-44ec-bd47-daf7f3289f92
source-git-commit: 898bd0d5d986bf26e62b0843de7cbb71b859aee3
workflow-type: tm+mt
source-wordcount: '830'
ht-degree: 42%

---

# [!DNL Google Display & Video 360] 接続

## 概要 {#overview}

[!DNL Display & Video 360]（旧称 ）は、ディスプレイ広告、ビデオ、モバイルの在庫ソースをまたいで、再ターゲティングと、オーディエンスにターゲットを絞ったデジタルキャンペーンの実行に使用できるツールです。[!DNL DoubleClick Bid Manager]

## 宛先の詳細 {#specifics}

次に示す詳細は、 [!DNL Google Display & Video 360] 宛先：

* アクティブ化されたオーディエンスは、Google プラットフォームでプログラムを使用して作成されます。
* [!DNL Platform] は、現在、アクティブ化の成功を検証するための測定指標を含んでいません。 統合を検証し、オーディエンスターゲティングの規模を理解するには、Google でのオーディエンス数を参照します。

>[!IMPORTANT]
>
>Google Display &amp; Video 360 を使用して最初の宛先を作成する場合で、以前に Experience Cloud ID サービスにおいて、Adobe Audience Manager や他のアプリケーションとの間で [ID 同期機能](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/idsync.html?lang=ja)を有効にしていないときには、アドビコンサルティングまたはカスタマーケアに連絡して、ID 同期を有効にしてください。以前にAudience ManagerでGoogle統合を設定していた場合、設定した ID 同期は Platform に引き継がれます。

## サポートされる ID {#supported-identities}

[!DNL Google Display & Video 360] では、以下の表で説明する id のアクティブ化をサポートしています。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| GAID | [!DNL Google Advertising ID] | ソース ID が GAID 名前空間の場合は、このターゲット ID を選択します。 |
| IDFA | [!DNL Apple ID for Advertisers] | ソース ID が IDFA 名前空間の場合は、このターゲット ID を選択します。 |
| AAM UUID | [Adobe Audience Manager [!DNL Unique User ID]](https://experienceleague.adobe.com/docs/audience-manager/user-guide/reference/ids-in-aam.html?lang=ja)別名 [!DNL Device ID]. 数値型で 38 桁のデバイス ID。Audience Manager はこの値を、操作するデバイスのそれぞれに関連付けます。 | Google使用 [AAM UUID](https://experienceleague.adobe.com/docs/audience-manager/user-guide/reference/ids-in-aam.html?lang=ja) を使用してカリフォルニアのユーザーをターゲット設定し、他のすべてのユーザーのGoogle Cookie ID を使用します。 |
| [!DNL Google] cookie ID | [!DNL Google] cookie ID | [!DNL Google] は、この ID を使用してカリフォルニア以外のユーザーをターゲットにします。 |
| RIDA | 広告用 Roku ID。 この ID は、Roku デバイスを一意に識別します。 |  |
| MAID | Microsoft Advertising ID。 この ID は、Windows 10 を実行しているデバイスを一意に識別します。 |  |
| Amazon Fire TV ID | この ID は、Amazon Fire TV を一意に識別します。 |  |

## エクスポートのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、次の表を参照してください。

| 項目 | タイプ | 備考 |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL セグメントエクスポート]** | セグメント（オーディエンス）のすべてのメンバーをGoogleの宛先に書き出します。 |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は、API ベースの接続です。 セグメント評価に基づいてExperience Platform内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。 詳細を表示 [ストリーミング先](/help/destinations/destination-types.md#streaming-destinations). |

## 前提条件 {#prerequisites}

### 許可リストへの登録 {#allow-listing}

>[!NOTE]
>
>許可リストへの登録は、最初の [!DNL Google Display & Video 360] の宛先を設定します。 宛先を作成する前に、Googleが以下に説明する許可リストへの登録プロセスを完了していることを確認してください。

を作成する前に [!DNL Google Display & Video 360] の宛先に設定する場合は、Googleに問い合わせて、許可されたデータプロバイダーのリストにAdobeを追加する方法と、お使いのアカウントを許可リストに追加する方法を依頼する必要があります。 Google に連絡し、次の情報を提供します。

* **アカウント ID**:AdobeのGoogleアカウント ID。 アカウント ID :87933855.
* **顧客 ID**:Adobeの顧客アカウント ID とGoogle。 顧客 ID :89690775.
* **アカウントの種類**：**[!DNL Invite advertiser]** を使用して、Display &amp; Video 360 アカウントにある特定のブランドにのみオーディエンスを共有するか、**[!DNL Invite partner]** を使用して Display &amp; Video 360 アカウントのすべてのブランドにオーディエンスを共有します。

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、 **[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions). 詳しくは、 [アクセス制御の概要](/help/access-control/ui/overview.md) または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。

### 接続パラメーター {#parameters}

この宛先を[設定](../../ui/connect-destination.md)するとき、次の情報を指定する必要があります。

* **[!UICONTROL 名前]**：この宛先に任意の名前を入力します。
* **[!UICONTROL 説明]**：オプション。例えば、この宛先を使用しているキャンペーンを指定できます。
* **[!UICONTROL アカウントの種類]**：Google のアカウントに応じて、次のオプションを選択します。
   * Display &amp; Video 360 アカウントにある特定のブランドにのみオーディエンスを共有するには、`Invite Advertiser` を使用します。
   * Display &amp; Video 360 アカウントのすべてのブランドにオーディエンスを共有するには、`Invite Partner` を使用します。
* **[!UICONTROL アカウント ID]**：**[!DNL Invite partner]** または **[!DNL Invite advertiser]** のアカウント ID に Google アカウントの ID を入力します。通常、これは 6 桁または 7 桁の ID です。

>[!NOTE]
>
>の設定時に [!DNL Google Display & Video 360] の宛先については、 [!DNL Google Account Manager] またはAdobe担当者を参照して、お持ちのアカウントの種類を確認してください。

## この宛先に対してセグメントをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>データをアクティブ化するには、 **[!UICONTROL 宛先の管理]**, **[!UICONTROL 宛先のアクティブ化]**, **[!UICONTROL プロファイルの表示]**、および **[!UICONTROL セグメントを表示]** [アクセス制御権限](/help/access-control/home.md#permissions). 詳しくは、 [アクセス制御の概要](/help/access-control/ui/overview.md) または製品管理者に問い合わせて、必要な権限を取得してください。

詳しくは、 [ストリーミングセグメントの書き出し先に対するオーディエンスデータのアクティブ化](../../ui/activate-segment-streaming-destinations.md) を参照してください。

## 書き出したデータ

データがに正常に書き出されたかどうかを確認するには、以下を実行します。 [!DNL Google Display & Video 360] 宛先、 [!DNL Google Display & Video 360] アカウント アクティブ化に成功した場合、オーディエンスがアカウントに入力されます。

## トラブルシューティング {#troubleshooting}

### 400 Bad Request エラーメッセージ {#bad-request}

この宛先を設定する際に、次のエラーが発生する場合があります。

`{"message":"Google Error: AuthorizationError.USER_PERMISSION_DENIED","code":"400 BAD_REQUEST"}`

このエラーは、顧客アカウントが [前提条件](#prerequisites). この問題を修正するには、Googleに連絡し、お使いのアカウントが許可リストに登録されていることを確認してください。