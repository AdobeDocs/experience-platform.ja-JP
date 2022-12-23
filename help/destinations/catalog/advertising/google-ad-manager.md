---
keywords: google アドマネージャー;google 広告;ダブルクリック;DoubleClick AdX;DoubleClick;Google アドマネージャー;Google ad manager;DFP
title: Google Ad Manager の接続
description: Google Ad Manager（以前は DoubleClick for Publishers または DoubleClick AdX と呼ばれていました）は Google の広告提供プラットフォームです。パブリッシャーはビデオやモバイルアプリを通じて、Web サイト上の広告の表示を管理することができます。
exl-id: e93f1bd5-9d29-43a1-a9a6-8933f9d85150
source-git-commit: 94cd05ca8b5c8331b1b49e5172daf499918d2320
workflow-type: ht
source-wordcount: '955'
ht-degree: 100%

---

# [!DNL Google Ad Manager] 接続

## 概要 {#overview}

[!DNL Google Ad Manager]（以前は [!DNL DoubleClick for Publishers]（DFP）または [!DNL DoubleClick AdX] と呼ばれていました）は、[!DNL Google] の広告提供プラットフォームです。パブリッシャーはビデオやモバイルアプリを通じて、web サイト上の広告の表示を管理できます。

## 宛先の詳細 {#specifics}

[!DNL Google Ad Manager] の宛先に固有な次の詳細に注意ください。

* アクティブ化されたオーディエンスは、[!DNL Google] プラットフォームでプログラム的に作成されます。
* [!DNL Platform] には、現在、アクティベーションの成功を検証するための測定指標は含まれていません。統合を検証し、オーディエンスターゲット設定サイズについて理解するには、Google でのオーディエンス数を参照します。
* セグメントを [!DNL Google Ad Manager] 宛先にマッピングすると、セグメント名が即座に [!DNL Google Ad Manager] ユーザーインターフェイスに表示されます。
* セグメント母集団が [!DNL Google Ad Manager] で表示されるまで、24～48 時間かかります。また、[!DNL Google Ad Manager] で表示するには、セグメントのオーディエンスサイズが 50 以上のプロファイルにする必要があります。オーディエンスサイズが 50 未満のプロファイルを含むセグメントは、[!DNL Google Ad Manager] に入力されません。

## サポートされる ID {#supported-identities}

[!DNL Google Ad Manager] では、以下の表で説明する ID のアクティベーションをサポートしています。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| GAID | [!DNL Google Advertising ID] | ソース ID が GAID 名前空間の場合は、このターゲット ID を選択します。 |
| IDFA | [!DNL Apple ID for Advertisers] | ソース ID が IDFA 名前空間の場合は、このターゲット ID を選択します。 |
| AAM UUID | [Adobe Audience Manager [!DNL Unique User ID]](https://experienceleague.adobe.com/docs/audience-manager/user-guide/reference/ids-in-aam.html?lang=ja)、別名 [!DNL Device ID]。数値型で 38 桁のデバイス ID。Audience Manager はこの値を、操作するデバイスのそれぞれに関連付けます。 | Google は [AAM UUID](https://experienceleague.adobe.com/docs/audience-manager/user-guide/reference/ids-in-aam.html?lang=ja) を使用して、カリフォルニア州のユーザーをターゲット設定し、他のすべてのユーザーに対して Google Cookie ID を使用します。 |
| [!DNL Google] Cookie ID | [!DNL Google] Cookie ID | [!DNL Google] は、この ID を使用してカリフォルニア州以外のユーザーをターゲットします。 |
| RIDA | 広告用 Roku ID。 この ID は、Roku デバイスを一意に識別します。 |  |
| MAID | Microsoft Advertising ID。この ID は、Windows 10 を実行しているデバイスを一意に識別します。 |  |
| Amazon Fire TV ID | この ID は、Amazon Fire TV を一意に識別します。 |  |

{style=&quot;table-layout:auto&quot;}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL セグメントの書き出し]** | セグメント（オーディエンス）のすべてのメンバーを Google の宛先に書き出します。 |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は常に、API ベースの接続です。セグメント評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)の詳細についてはこちらを参照してください。 |

{style=&quot;table-layout:auto&quot;}

## 前提条件 {#prerequisites}

[!DNL Google Ad Manager] での最初の宛先を作成しようとしており、これまで（Audience Manager などのアプリケーションを使用して）Experience Cloud ID サービスで [ID 同期機能](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/idsync.html?lang=ja)を有効にしたことがない場合は、アドビのコンサルティングまたはカスタマーケアに連絡して ID 同期を有効にしてもらってください。以前に Audience Manager で [!DNL Google] 統合を設定していた場合、設定した ID 同期は Platform に引き継がれます。

### 許可リストへの登録 {#allow-listing}

>[!NOTE]
>
>Platform で最初の [!DNL Google Ad Manager] の宛先を設定する前に、許可リストへの登録は必須です。宛先を作成する前に、[!DNL Google] が以下に説明する許可リストへの登録プロセスを完了していることを確認してください。
>このルールの例外は、[Audience Manager](https://experienceleague.adobe.com/docs/audience-manager/user-guide/aam-home.html?lang=ja) 顧客の場合です。この Google の宛先への接続を Audience Manager で既に作成している場合は、許可リストへの登録プロセスを再度実行する必要はありません。次の手順に進んでください。

Platform で [!DNL Google Ad Manager] の宛先を作成する前に、[!DNL Google] に連絡して、許可されたデータプロバイダーのリストにアドビを追加し、お使いのアカウントを許可リストに追加してもらう必要があります。 [!DNL Google] に連絡し、次の情報を提供します。

* **アカウント ID**：アドビの Google アカウント ID です。アカウント ID：87933855。
* **顧客 ID**：アドビの Google 顧客アカウント ID です。顧客 ID：89690775。
* **ネットワークコード**：Google インターフェイスの&#x200B;**[!UICONTROL 管理者／グローバル設定]**&#x200B;と URL の下にある [!DNL Google Ad Manager] ネットワーク識別子です。
* **オーディエンスリンク ID**：[!DNL Google Ad Manager] ネットワーク（[!DNL Network code] ではない）に関連付けられた特定の識別子で、Google インターフェイスの&#x200B;**[!UICONTROL 管理者／グローバル設定]**&#x200B;にもあります。
* お使いのアカウントのタイプ。DFP by Google または AdX Buyer です。

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。

### 接続パラメーター {#parameters}

この宛先を[設定](../../ui/connect-destination.md)するとき、次の情報を指定する必要があります。

* **[!UICONTROL 名前]**：この宛先に希望する名前を入力します。
* **[!UICONTROL 説明]**：オプション。例えば、この宛先を使用しているキャンペーンを指定できます。
* **[!UICONTROL アカウントの種類]**：Google のアカウントに応じて、次のオプションを選択します。
   * [!DNL DoubleClick] for Publishers に `DFP by Google` を使用する
   * [!DNL Google AdX] に `AdX buyer` を使用する
* **[!UICONTROL アカウント ID]**：[!DNL Google] で オーディエンスリンク ID を入力します。 

>[!NOTE]
>
>[!DNL Google Ad Manager] の宛先を設定する際は、[!DNL Google Account Manager] またはアドビの担当者にお問い合わせの上、お持ちのアカウントの種類をご確認ください。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してセグメントをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>データをアクティブ化するには、**[!UICONTROL 宛先の管理]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]**&#x200B;および&#x200B;**[!UICONTROL セグメントの表示]**[に対するアクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先にオーディエンスセグメントをアクティブ化する手順は、[ストリーミングセグメント書き出し宛先に対するオーディエンスデータのアクティブ化](../../ui/activate-segment-streaming-destinations.md)を参照してください。

## 書き出したデータ {#exported-data}

データがに正常に [!DNL Google Ad Manager] の宛先に書き出されたかどうかを確認するには、[!DNL Google Ad Manager] アカウントを確認します。 アクティベーションに成功すると、オーディエンスがお使いのアカウントに入力されます。
