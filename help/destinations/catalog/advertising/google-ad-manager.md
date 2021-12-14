---
keywords: google ad manager;google ad;doubleclick;DoubleClick AdX;DoubleClick;Google Ad Manager;Google ad manager;DFP
title: Google Ad Manager 接続
description: Google Ad Manager（以前は DoubleClick for Publishers または DoubleClick AdX と呼ばれていました）は Google の広告提供プラットフォームです。パブリッシャーはビデオやモバイルアプリを通じて、Web サイト上の広告の表示を管理することができます。
exl-id: e93f1bd5-9d29-43a1-a9a6-8933f9d85150
source-git-commit: 27e5c64f31b9a68252d262b531660811a0576177
workflow-type: tm+mt
source-wordcount: '653'
ht-degree: 27%

---

# [!DNL Google Ad Manager] 接続

## 概要 {#overview}

[!DNL Google Ad Manager]( 旧称： [!DNL DoubleClick for Publishers] (DFP) または [!DNL DoubleClick AdX]は、から提供される広告サービングプラットフォームです [!DNL Google] これにより、パブリッシャーは、ビデオやモバイルアプリを通じて、Web サイト上の広告の表示を管理できます。

## 宛先の詳細 {#specifics}

次に示す詳細は、 [!DNL Google Ad Manager] 宛先：

* アクティブ化されたオーディエンスは、 [!DNL Google] プラットフォーム。
* [!DNL Platform] は、現在、アクティブ化の成功を検証するための測定指標を含んでいません。 統合を検証し、オーディエンスターゲット設定サイズについて理解するには、Google でのオーディエンス数を参照します。

## サポートされる ID {#supported-identities}

[!DNL Google Ad Manager] では、以下の表で説明する id のアクティブ化をサポートしています。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| GAID | [!DNL Google Advertising ID] | ソース ID が GAID 名前空間の場合は、このターゲット ID を選択します。 |
| IDFA | [!DNL Apple ID for Advertisers] | ソース ID が IDFA 名前空間の場合は、このターゲット ID を選択します。 |
| AAM UUID | [Adobe Audience Manager [!DNL Unique User ID]](https://experienceleague.adobe.com/docs/audience-manager/user-guide/reference/ids-in-aam.html)別名 [!DNL Device ID]. 数値型で 38 桁のデバイス ID。Audience Manager はこの値を、操作するデバイスのそれぞれに関連付けます。 | Google使用 [AAM UUID](https://experienceleague.adobe.com/docs/audience-manager/user-guide/reference/ids-in-aam.html?lang=en) を使用してカリフォルニアのユーザーをターゲット設定し、他のすべてのユーザーのGoogle Cookie ID を使用します。 |
| [!DNL Google] cookie ID | [!DNL Google] cookie ID | [!DNL Google] は、この ID を使用してカリフォルニア以外のユーザーをターゲットにします。 |
| RIDA | 広告用 Roku ID。 この ID は、Roku デバイスを一意に識別します。 |  |
| MAID | Microsoft Advertising ID。 この ID は、Windows 10 を実行しているデバイスを一意に識別します。 |  |
| Amazon Fire TV ID | この ID は、Amazon Fire TV を一意に識別します。 |  |

## 書き出しタイプ {#export-type}

**セグメントの書き出し** ：セグメント（オーディエンス）のすべてのメンバーをGoogleの宛先に書き出します。

## 前提条件

を使用して最初の宛先を作成する場合は、 [!DNL Google Ad Manager] 有効にしていない [ID 同期機能](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/idsync.html?lang=ja) 以前のExperience CloudID サービス (Audience Managerや他のアプリケーションを使用 ) では、Adobeコンサルティングまたはカスタマーケアに問い合わせて、ID 同期を有効にしてください。 以前に [!DNL Google] Audience Manager内の統合、設定した ID 同期は Platform に引き継がれます。

## 許可リスト

>[!NOTE]
>
>許可リストは、最初の [!DNL Google Ad Manager] の宛先を設定します。 以下に説明する許可リストプロセスが次の方法で完了していることを確認してください： [!DNL Google] 宛先を作成する前に、をクリックします。

を作成する前に [!DNL Google Ad Manager] の宛先に設定する場合は、 [!DNL Google] Adobeを許可されたデータプロバイダーのリストに追加し、お使いのアカウントを許可リストに追加する場合。 連絡先 [!DNL Google] 次の情報を入力します。

* **アカウント ID**:AdobeのGoogleアカウント ID。 アカウント ID :87933855.
* **顧客 ID**:Adobeの顧客アカウント ID とGoogle。 顧客 ID :89690775.
* **ネットワーク ID**：これは のアカウントです。[!DNL Google Ad Manager]
* **オーディエンスリンク ID**：これは のアカウントです。[!DNL Google Ad Manager]
* アカウントの種類。Google DFP または AdX 購入者。

## 宛先に接続 {#connect}

この宛先に接続するには、 [宛先設定のチュートリアル](../../ui/connect-destination.md).

### 接続パラメーター {#parameters}

While [設定](../../ui/connect-destination.md) この宛先には、次の情報を指定する必要があります。

* **[!UICONTROL 名前]**：この宛先の名前を入力します。
* **[!UICONTROL 説明]**：オプション。例えば、この宛先を使用しているキャンペーンを指定できます。
* **[!UICONTROL アカウントの種類]**：Google のアカウントに応じて、次のオプションを選択します。
   *  for Publishers - `DFP by Google` を使用[!DNL DoubleClick]
   * 用途 `AdX buyer` 対象 [!DNL Google AdX]
* **[!UICONTROL アカウント ID]**:アカウント ID に [!DNL Google]. これは、ネットワーク ID またはオーディエンスリンク ID です。通常、これは 8 桁の ID です。

>[!NOTE]
>
>の設定時に [!DNL Google Ad Manager] の宛先については、 [!DNL Google Account Manager] またはAdobe担当者を参照して、お持ちのアカウントの種類を確認してください。

## この宛先へのセグメントのアクティブ化 {#activate}

詳しくは、 [ストリーミングセグメントの書き出し先に対するオーディエンスデータのアクティブ化](../../ui/activate-segment-streaming-destinations.md) を参照してください。

## 書き出されたデータ

データがに正常に書き出されたかどうかを確認するには、以下を実行します。 [!DNL Google Ad Manager] 宛先、 [!DNL Google Ad Manager] アカウント アクティブ化に成功した場合、オーディエンスがアカウントに入力されます。
