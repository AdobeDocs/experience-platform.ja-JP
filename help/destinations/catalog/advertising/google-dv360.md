---
keywords: DoubleClick Bid Manager;DoubleClick bid manager;DoubleClick;Display & Video 360;display 360;video 360;Video 360;Display 360;display and video
title: Google DisplayとVideo 360の接続
description: Display & Video 360（旧称 DoubleClick Bid Manager）は、ディスプレイ広告、ビデオ、モバイルの在庫ソースをまたいで、再ターゲティングと、オーディエンスにターゲットを絞ったデジタルキャンペーンの実行に使用できるツールです。
exl-id: bdd3b3fd-891f-44ec-bd47-daf7f3289f92
source-git-commit: d0112cb26fcb85ad91ba403f81ee7f11d0889046
workflow-type: tm+mt
source-wordcount: '709'
ht-degree: 40%

---

# [!DNL Google Display & Video 360] 接続

## 概要 {#overview}

[!DNL Display & Video 360]（旧称 ）は、ディスプレイ広告、ビデオ、モバイルの在庫ソースをまたいで、再ターゲティングと、オーディエンスにターゲットを絞ったデジタルキャンペーンの実行に使用できるツールです。[!DNL DoubleClick Bid Manager]

## 宛先の詳細 {#specifics}

次の[!DNL Google Display & Video 360]宛先に固有の詳細に注意してください。

* アクティブ化されたオーディエンスは、Google プラットフォームでプログラムを使用して作成されます。
* [!DNL Platform] は、現在、アクティブ化の成功を検証するための測定指標を含んでいません。統合を検証し、オーディエンスターゲティングの規模を理解するには、Google でのオーディエンス数を参照します。

>[!IMPORTANT]
>
>Google Display &amp; Video 360 を使用して最初の宛先を作成する場合で、以前に Experience Cloud ID サービスにおいて、Adobe Audience Manager や他のアプリケーションとの間で [ID 同期機能](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/idsync.html)を有効にしていないときには、アドビコンサルティングまたはカスタマーケアに連絡して、ID 同期を有効にしてください。以前にGoogle統合をAudience Managerで設定していた場合、設定したID同期はPlatformに引き継がれます。

## サポートされるID {#supported-identities}

[!DNL Google Ad Manager] では、以下の表で説明するIDのアクティブ化をサポートしています。

| ターゲットID | 説明 | 注意点 |
|---|---|---|
| GAID | [!DNL Google Advertising ID] | ソースIDがGAID名前空間の場合は、このターゲットIDを選択します。 |
| IDFA | [!DNL Apple ID for Advertisers] | ソースIDがIDFA名前空間の場合は、このターゲットIDを選択します。 |
| AAM UUID | [Adobe Audience Manager [!DNL Unique User ID]](https://experienceleague.adobe.com/docs/audience-manager/user-guide/reference/ids-in-aam.html)(別名 [!DNL Device ID])数値型で 38 桁のデバイス ID。Audience Manager はこの値を、操作するデバイスのそれぞれに関連付けます。 | Googleは、[AAM UUID](https://experienceleague.adobe.com/docs/audience-manager/user-guide/reference/ids-in-aam.html?lang=en)を使用してカリフォルニアのユーザーをターゲット設定し、他のすべてのユーザーのGoogle Cookie IDを使用します。 |
| [!DNL Google] cookie ID | [!DNL Google] cookie ID | [!DNL Google] は、このIDを使用してカリフォルニア以外のユーザーをターゲット設定します。 |
| RIDA | 広告用のRoku ID。 このIDは、Rokuデバイスを一意に識別します。 |  |
| MAID | Microsoft広告ID。 このIDは、Windows 10を実行しているデバイスを一意に識別します。 |  |
| Amazon Fire TV ID | このIDは、Amazon Fire TVを一意に識別します。 |  |

## 書き出しタイプ {#export-type}

**セグメントの書き出し**  — セグメント（オーディエンス）のすべてのメンバーをGoogleの宛先に書き出します。

## 前提条件 {#prerequisites}

### 許可リストへの登録

>[!NOTE]
>
>許可リストへの登録は、Platformで最初の[!DNL Google Display & Video 360]宛先を設定する前に行う必要があります。 宛先を作成する前に、Googleが以下に説明する許可リスト登録プロセスを完了していることを確認してください。

Platformで[!DNL Google Display & Video 360]の宛先を作成する前に、Googleに連絡して、許可されたデータプロバイダーのリストに登録するAdobeと、許可リストに追加するアカウントを要求する必要があります。 Google に連絡し、次の情報を提供します。

* **アカウントID**:AdobeのアカウントIDがGoogleに設定されている。アカウントID:87933855.
* **顧客ID**:Adobeの顧客アカウントID（Googleを使用）。顧客ID:89690775.
* **アカウントの種類**：**[!DNL Invite advertiser]** を使用して、Display &amp; Video 360 アカウントにある特定のブランドにのみオーディエンスを共有するか、**[!DNL Invite partner]** を使用して Display &amp; Video 360 アカウントのすべてのブランドにオーディエンスを共有します。

## 宛先に接続 {#connect}

この宛先に接続するには、[宛先の設定に関するチュートリアル](../../ui/connect-destination.md)で説明されている手順に従います。

### 接続パラメーター {#parameters}

[この宛先を設定](../../ui/connect-destination.md)する際に、次の情報を指定する必要があります。

* **[!UICONTROL 名前]**：この宛先の名前を入力します。
* **[!UICONTROL 説明]**：オプション。例えば、この宛先を使用しているキャンペーンを指定できます。
* **[!UICONTROL アカウントの種類]**：Google のアカウントに応じて、次のオプションを選択します。
   * Display &amp; Video 360 アカウントにある特定のブランドにのみオーディエンスを共有するには、`Invite Advertiser` を使用します。
   * Display &amp; Video 360 アカウントのすべてのブランドにオーディエンスを共有するには、`Invite Partner` を使用します。
* **[!UICONTROL アカウント ID]**：**[!DNL Invite partner]** または **[!DNL Invite advertiser]** のアカウント ID に Google アカウントの ID を入力します。通常、これは 6 桁または 7 桁の ID です。

>[!NOTE]
>
>[!DNL Google Display & Video 360]の宛先を設定する際は、[!DNL Google Account Manager]またはAdobeの担当者にお問い合わせのうえ、お持ちのアカウントの種類をご確認ください。

## この宛先へのセグメントのアクティブ化 {#activate}

この宛先に対してオーディエンスセグメントをアクティブ化する手順については、[ストリーミングセグメントの書き出し先へのオーディエンスデータのアクティブ化](../../ui/activate-segment-streaming-destinations.md)を参照してください。

## エクスポートされたデータ

データが[!DNL Google Display & Video 360]の宛先に正常に書き出されたかどうかを確認するには、[!DNL Google Display & Video 360]アカウントを確認します。 アクティブ化に成功した場合、オーディエンスがアカウントに入力されます。

## トラブルシューティング {#troubleshooting}

### 400 Bad Requestエラーメッセージ {#bad-request}

この宛先を設定すると、次のエラーが発生する場合があります。

`{"message":"Google Error: AuthorizationError.USER_PERMISSION_DENIED","code":"400 BAD_REQUEST"}`

このエラーは、顧客アカウントが[前提条件](#prerequisites)を満たしていない場合に発生します。 この問題を修正するには、Googleに連絡し、お使いのアカウントが許可リストに登録されていることを確認してください。