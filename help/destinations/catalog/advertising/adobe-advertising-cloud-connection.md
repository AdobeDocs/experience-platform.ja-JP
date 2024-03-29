---
title: Adobe Advertising Cloud DSP Connection
description: Adobe Advertising Cloud DSPはAdobe Real-time Customer Data Platformの統合された宛先で、認証済みのファーストパーティオーディエンスを承認済みの広告主やユーザーと共有して、キャンペーンをアクティブ化できます。
exl-id: 11ff7797-a9c6-4334-b843-ae9df9a48e54
source-git-commit: ba39f62cd77acedb7bfc0081dbb5f59906c9b287
workflow-type: tm+mt
source-wordcount: '1055'
ht-degree: 25%

---

# Adobe Advertising Cloud DSP Connection

## 概要 {#overview}

ザAdobe Advertising Cloud [!DNL Demand-Side Platform] (DSP) の宛先を使用すると、認証済みのファーストパーティオーディエンスを、DSPでキャンペーンのアクティベーション用に承認済みの広告主やユーザーと共有できます。 DSPとのReal-Time CDP統合について詳しくは、 [オーディエンスソースからの認証済みオーディエンスのアクティブ化について](https://experienceleague.adobe.com/docs/advertising-cloud/dsp/audiences/sources/source-about.html).

>[!IMPORTANT]
>
>このページはDSPチームが作成しました。 お問い合わせや更新のご依頼については、 Advertising Cloudサポートに直接お問い合わせください。 `adcloud_support@adobe.com`.

## ユースケース {#use-cases}

Advertising Cloud DSPの宛先をいつどのように使用するかを理解しやすくするために、Adobe Experience Platformのお客様がこの宛先を使用して解決できる使用例を以下に示します。

### ブランド広告の使用例

あるオンライン小売業者は、ターゲティングに cookie を使用せずに、ディスプレイキャンペーンを通じて価値の高い顧客を再ターゲット化したいと考えています。 小売業者は、Adobe Real-time Customer Data Platform(Real-Time CDP) アカウントからDSPアカウントに至る、価値の高い顧客のハッシュ化された電子メール ID で構成されるオーディエンスを共有します。 DSPは、ハッシュ化された電子メール ID を認証済みに変換します [!DNL RampIDs] DSPと LiveRamp のパートナーシップを通じて 結果 [!DNL RampIDs] を使用して、オーディエンスをターゲット設定するディスプレイキャンペーンで使用できます。

### エージェンシーの使用例

DSPアカウントを持つメディアエージェンシーは、接客業のトップブランドである顧客に代わってリターゲティングキャンペーンを実施しています。 同ブランドは、昨年にすべてのゲストを再ターゲット化し、新しいプロモーションオファーを提供したいと考えています。 ブランドは、のすべてのゲスト情報をホストします。 [!DNL Real-Time CDP]. ブランドは、ゲストのハッシュ化された電子メール ID で構成されるオーディエンスを共有できます [!DNL Real-Time CDP] メディアエージェンシーのDSPアカウントにアカウントし、メディアキャンペーンを通じてゲストをリターゲティングします。

## 前提条件 {#prerequisites}

* DSPのアカウントレベルとキャンペーンレベルの設定で、 [!DNL LiveRamp RampID]（顧客データをに変換します） [!DNL RampIDs] をクリックして、ターゲット設定可能なセグメントを作成します。 この設定は、DSPアカウントチームが実行します。 [!DNL RampID] は、DSPと [!DNL LiveRamp]お客様独自の [!DNL LiveRamp] 使用するメンバーシップ。
* Experience CloudアカウントのExperience Platform組織 ID。 ID は、 [!DNL Real-Time CDP] ユーザープロファイルページ。
* A [[!DNL Real-Time CDP] DSP内のソース](https://experienceleague.adobe.com/docs/advertising-cloud/dsp/audiences/sources/source-create.html) :campaign のアクティベーション用のオーディエンスを受け取る。 DSPアカウントチームは、組織 ID を使用してExperience Cloudを作成します。
* DSPアカウントまたは広告主のソースキー。 [[!DNL Real-Time CDP] ソースがDSPで作成されました](https://experienceleague.adobe.com/docs/advertising-cloud/dsp/audiences/sources/source-create.html). DSPアカウントチームがこのキーを共有します。 Experience Platform内で使用して、Advertising Cloud DSPの宛先への宛先接続を作成します。 [以下に説明する](#authenticate).
* 電子メールまたはハッシュ化された電子メールで構成される顧客データ。

## サポートされている ID {#supported-identities}

Adobe Advertising Cloud DSPの宛先では、以下の表で説明する ID のアクティブ化をサポートしています。 [ID](/help/identity-service/features/namespaces.md) についての詳細情報。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| email_lc_sha256 | SHA256 アルゴリズムでハッシュ化されたメールアドレス | Experience Platformは、プレーンテキストと SHA256 ハッシュ化された電子メールアドレスの両方をサポートしています。 ソースフィールドにハッシュ化されていない属性が含まれている場合は、 **[!UICONTROL 変換を適用]** オプションを使用して、Experience Platformがアクティベーション時にデータを自動的にハッシュ化するように設定する必要があります。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、次の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL オーディエンスの書き出し]** | Advertising Cloud DSPの宛先で使用される識別子（電子メールまたはハッシュ化された電子メール）を使用して、オーディエンスのすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンスの評価に基づいてExperience Platformでプロファイルが更新されると、コネクタは更新を宛先プラットフォームに送信します。 詳しくは、[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)を参照してください。 |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、 **[!UICONTROL 宛先の表示]** および **[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions) Experience Platform。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

宛先に接続するには、次の手順に従います： [宛先接続の作成](/help/destinations/ui/connect-destination.md) を使用します。 宛先の設定ワークフローで、以下の 2 つの節でリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

宛先に接続するには、 [!UICONTROL 接続タイプ] 」セクションで、 **[!UICONTROL 宛先に接続]**.:

* **[!UICONTROL アカウントまたは広告主キー]**：これ [!UICONTROL ソースキー] は、 [[!DNL Real-Time CDP] ソースはDSPユーザーインターフェイスで作成されます。](https://experienceleague.adobe.com/docs/advertising-cloud/dsp/audiences/sources/source-create.html). ソースを作成した後、DSPアカウントチームがこのキーを共有します。

![接続タイプフィールド](/help/destinations/assets/catalog/advertising/adobe-advertising-cloud-connection/authenticate-destination.png)

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

* **[!UICONTROL 名前]**：今後この宛先を認識するための名前。
* **[!UICONTROL 説明]**：今後この宛先を識別するのに役立つ説明。

![宛先の詳細フィールド](/help/destinations/assets/catalog/advertising/adobe-advertising-cloud-connection/destination-details.png)

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>* データをアクティブ化するには、 **[!UICONTROL 宛先の表示]**, **[!UICONTROL 宛先のアクティブ化]**, **[!UICONTROL プロファイルの表示]**、および **[!UICONTROL セグメントを表示]** [アクセス制御権限](/help/access-control/home.md#permissions). [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* 書き出す *id*、 **[!UICONTROL ID グラフを表示]** [アクセス制御権限](/help/access-control/home.md#permissions). <br> ![ワークフローでハイライト表示された ID 名前空間を選択して、宛先に対するオーディエンスをアクティブ化します。](/help/destinations/assets/overview/export-identities-to-destination.png "ワークフローでハイライト表示された ID 名前空間を選択して、宛先に対するオーディエンスをアクティブ化します。"){width="100" zoomable="yes"}

この宛先にオーディエンスをアクティベートする手順は、[ストリーミングオーディエンスの書き出し宛先へのプロファイルとオーディエンスのアクティベート](/help/destinations/ui/activate-segment-streaming-destinations.md)を参照してください。

## データの書き出しを検証する {#exported-data}

データオーディエンスがAdvertising Cloudと共有されたことを確認するには、以下を確認します。

* のデータフロー [!DNL Real-Time CDP] の宛先が正常に作成されました。

* DSPでは、オーディエンスは、 [!UICONTROL オーディエンス] > [!UICONTROL すべてのオーディエンス] または内から [!UICONTROL オーディエンスのターゲティング] 配置設定のセクション。 オーディエンスは、 [!UICONTROL Adobeセグメント] 」タブをクリックします。 [!UICONTROL Real-Time CDP] フォルダー。

![DSPオーディエンス設定のReal-Time CDPオーディエンス](/help/destinations/assets/catalog/advertising/adobe-advertising-cloud-connection/segments-in-dsp.png)

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。詳しくは、 [!DNL Adobe Experience Platform] データガバナンスを強制します。詳しくは、 [データガバナンスの概要](/help/data-governance/home.md).
