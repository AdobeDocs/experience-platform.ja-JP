---
keywords: google アドマネージャー;google 広告;ダブルクリック;DoubleClick AdX;DoubleClick;Google アドマネージャー;Google ad manager;DFP
title: Google Ad Manager の接続
description: Google Ad Manager（以前は DoubleClick for Publishers または DoubleClick AdX と呼ばれていました）は Google の広告提供プラットフォームです。パブリッシャーはビデオやモバイルアプリを通じて、Web サイト上の広告の表示を管理することができます。
exl-id: e93f1bd5-9d29-43a1-a9a6-8933f9d85150
source-git-commit: 16365865e349f8805b8346ec98cdab89cd027363
workflow-type: tm+mt
source-wordcount: '993'
ht-degree: 69%

---

# [!DNL Google Ad Manager] 接続

## 概要 {#overview}

[!DNL Google Ad Manager]（以前は [!DNL DoubleClick for Publishers]（DFP）または [!DNL DoubleClick AdX] と呼ばれていました）は、[!DNL Google] の広告提供プラットフォームです。パブリッシャーはビデオやモバイルアプリを通じて、web サイト上の広告の表示を管理できます。

## 宛先の詳細 {#specifics}

[!DNL Google Ad Manager] の宛先に固有な次の詳細に注意ください。

* アクティブ化されたオーディエンスは、[!DNL Google] プラットフォームでプログラム的に作成されます。
* [!DNL Platform] には、現在、アクティベーションの成功を検証するための測定指標は含まれていません。統合を検証し、オーディエンスターゲット設定サイズについて理解するには、Google でのオーディエンス数を参照します。
* オーディエンスを [!DNL Google Ad Manager] 宛先の場合、オーディエンス名は [!DNL Google Ad Manager] ユーザーインターフェイス。
* セグメント母集団が [!DNL Google Ad Manager] で表示されるまで、24～48 時間かかります。また、で表示するには、オーディエンスのオーディエンスサイズが 50 以上のプロファイルである必要があります [!DNL Google Ad Manager]. サイズが 50 未満のプロファイルを持つオーディエンスは、 [!DNL Google Ad Manager].

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

{style="table-layout:auto"}

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるすべてのオーディエンスについて説明します。

この宛先では、Experience Platform [セグメント化サービス](../../../segmentation/home.md).

*さらに*&#x200B;の場合、この宛先では、以下の表で説明するオーディエンスのアクティブ化もサポートされます。

| オーディエンスタイプ | 説明 |
---------|----------|
| カスタムアップロード | オーディエンス [インポート済み](../../../segmentation/ui/overview.md#import-audience) を CSV ファイルからExperience Platformに追加します。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL オーディエンスの書き出し]** | オーディエンスのすべてのメンバーをGoogleの宛先に書き出します。 |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいてExperience Platform内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。 [ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)の詳細についてはこちらを参照してください。 |

{style="table-layout:auto"}

## 前提条件 {#prerequisites}

[!DNL Google Ad Manager] での最初の宛先を作成しようとしており、これまで（Audience Manager などのアプリケーションを使用して）Experience Cloud ID サービスで [ID 同期機能](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/idsync.html?lang=ja)を有効にしたことがない場合は、アドビのコンサルティングまたはカスタマーケアに連絡して ID 同期を有効にしてもらってください。以前に Audience Manager で [!DNL Google] 統合を設定していた場合、設定した ID 同期は Platform に引き継がれます。

### 許可リストへの登録 {#allow-listing}

Platform で最初の [!DNL Google Ad Manager] の宛先を設定する前に、許可リストへの登録は必須です。宛先を作成する前に、以下に説明する許可リスト登録プロセスを完了してください。

1. 次に示す手順に従います： [Google Ad Manager のドキュメント](https://support.google.com/admanager/answer/3289669?hl=ja) Adobeをリンクされたデータ管理プラットフォーム (DMP) として追加する。
2. Adobe Analytics の [!DNL Google Ad Manager] インターフェイス、に移動します。 **[!UICONTROL 管理者]** > **[!UICONTROL グローバル設定]** > **[!UICONTROL ネットワーク設定]**、を有効にします。 **[!UICONTROL API アクセス]** スライダー。

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。

### 接続パラメーター {#parameters}

>[!CONTEXTUALHELP]
>id="platform_destinations_gam_appendSegmentID"
>title="オーディエンス名へのオーディエンス ID の追加"
>abstract="Google アドマネージャーのオーディエンス名に Experience Platform のオーディエンス ID を含めるには、`Audience Name (Audience ID)` のように、このオプションを選択します。"

この宛先を[設定](../../ui/connect-destination.md)するとき、次の情報を指定する必要があります。

* **[!UICONTROL 名前]**：この宛先に希望する名前を入力します。
* **[!UICONTROL 説明]**：オプション。例えば、この宛先を使用しているキャンペーンを指定できます。
* **[!UICONTROL アカウント ID]**: [!DNL Audience Link ID] から [!DNL Google] アカウント。 これは、 [!DNL Google Ad Manager] ネットワーク ( [!DNL Network code]) をクリックします。 これは、の下にあります。 **[!UICONTROL 管理者/グローバル設定]** （内） [!DNL Google Ad Manager] インターフェイス。
* **[!UICONTROL アカウントタイプ]**：Google のアカウントに応じて、次のいずれかのオプションを選択します。
   * [!DNL DoubleClick] for Publishers に `DFP by Google` を使用する
   * [!DNL Google AdX] に `AdX buyer` を使用する
* **[!UICONTROL オーディエンス名にオーディエンス ID を追加する]**:Google Ad Manager でオーディエンス名にExperience Platformのオーディエンス ID を含める場合は、次のように指定します。 `Audience Name (Audience ID)`.

>[!NOTE]
>
>[!DNL Google Ad Manager] の宛先を設定する際は、[!DNL Google Account Manager] またはアドビの担当者にお問い合わせの上、お持ちのアカウントの種類をご確認ください。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続への詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対するオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>データをアクティブ化するには、**[!UICONTROL 宛先の管理]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]**&#x200B;および&#x200B;**[!UICONTROL セグメントの表示]** [に対するアクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

詳しくは、 [ストリーミングオーディエンスの書き出し先に対するオーディエンスデータのアクティブ化](../../ui/activate-segment-streaming-destinations.md) を参照してください。

## 書き出したデータ {#exported-data}

データがに正常に [!DNL Google Ad Manager] の宛先に書き出されたかどうかを確認するには、[!DNL Google Ad Manager] アカウントを確認します。 アクティベーションに成功すると、オーディエンスがお使いのアカウントに入力されます。

## トラブルシューティング {#troubleshooting}

この宛先の使用中にエラーが発生し、AdobeまたはGoogleに連絡する必要がある場合は、次の ID を手元に置いてください。

AdobeのGoogleアカウント ID は次のとおりです。

* **[!UICONTROL アカウント ID]**: 87933855
* **[!UICONTROL 顧客 ID]**: 89690775