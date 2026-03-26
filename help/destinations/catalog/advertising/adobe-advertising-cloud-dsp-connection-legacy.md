---
title: 従来のAdobe Advertising Cloud DSP接続
description: Adobe Advertising Cloud DSPは、Adobe Real-Time Customer Data Platform向けの統合された配信先であり、承認済みのファーストパーティオーディエンスを、承認済みの広告主やユーザーと共有してキャンペーンに活用することができます。
exl-id: 11ff7797-a9c6-4334-b843-ae9df9a48e54
source-git-commit: 20427c4c8826905a77fac04d055d523b12a6f739
workflow-type: tm+mt
source-wordcount: '1033'
ht-degree: 21%

---

# 従来の[!DNL Adobe Advertising Cloud] DSP接続

>[!NOTE]
>
>以前はAdobe Advertising DSPと呼ばれていました。 新しい[Adobe Advertising DSP接続](/help/destinations/catalog/advertising/adobe-advertising-dsp-connection.md)には、従来の接続と同じ機能が含まれており、追加のID タイプがサポートされています。 ベストプラクティスは、新しいAdobe Advertising DSP接続を使用することです。

## 概要 {#overview}

[!DNL Adobe Advertising Cloud] [!DNL Demand-Side Platform] （DSP）宛先は、DSPを使用してキャンペーンをアクティブ化するために、認証済みのファーストパーティオーディエンスを、承認済みの広告主およびユーザーと共有します。 DSPとの[!DNL Real-Time CDP]統合について詳しくは、「[&#x200B; オーディエンスソースからの認証済みオーディエンスのアクティブ化について](https://experienceleague.adobe.com/docs/advertising-cloud/dsp/audiences/sources/source-about.html)」を参照してください。

>[!IMPORTANT]
>
>このページはDSPチームによって作成されました。 お問い合わせやアップデートのリクエストについては、`adcloud_support@adobe.com`から直接Advertising Cloud サポートにお問い合わせください。

## ユースケース {#use-cases}

Advertising Cloud DSPの宛先を使用する方法とタイミングをより正確に把握するために、この宛先を使用して[!DNL Adobe Experience Platform]のお客様が解決できるユースケースの例を次に示します。

### ブランド広告のユースケース {#brand-advertising}

オンラインのretailerでは、ターゲティングにCookieを使用することなく、ディスプレイキャンペーンを通じて価値の高い顧客をリターゲティングしたいと考えています。 Retailerは、Adobe [!DNL Real-Time Customer Data Platform] （[!DNL Real-Time CDP]）アカウントからDSP アカウントに対して、その価値の高い顧客のハッシュ化された電子メール IDで構成されるオーディエンスを共有します。 その後、DSPは、DSPとLiveRampのパートナーシップを通じて、ハッシュ化されたメール IDを認証済み[!DNL RampIDs]に変換します。 結果の[!DNL RampIDs]は、表示キャンペーンでオーディエンスをターゲットにするのに使用できます。

### 代理店のユースケース {#agency-use-case}

DSPのアカウントを持つメディア代理店が、ホスピタリティ業界のトップブランドである顧客の代理としてリターゲティングキャンペーンを実施しています。 同社は、過去1年間のすべての顧客に対して、新しいプロモーションオファーを提供することを望んでいます。 ブランドは[!DNL Real-Time CDP]のすべてのゲスト情報をホストします。 [!DNL Real-Time CDP] アカウントのゲストのハッシュ化された電子メール IDで構成されるオーディエンスをメディア エージェンシーのDSP アカウントと共有して、メディア キャンペーンを通じてゲストをリターゲティングできます。

## 前提条件 {#prerequisites}

* DSPのアカウントレベルおよびキャンペーンレベルの設定により、[!DNL LiveRamp RampID]とのオーディエンス共有が可能になります。これにより、顧客データが[!DNL RampIDs]に変換され、ターゲットを絞ったセグメントが作成されます。 DSPのアカウントチームが、この設定を実行します。 [!DNL RampID]はDSPと[!DNL LiveRamp]のパートナーシップを通じて利用でき、ご自身の[!DNL LiveRamp] メンバーシップは必要ありません。
* Experience Platform アカウントのExperience Cloud Organization ID。 IDは、[!DNL Real-Time CDP] ユーザープロファイルページで確認できます。
* キャンペーンのアクティベーション用のオーディエンスを受け取るDSP[[!DNL Real-Time CDP] の](https://experienceleague.adobe.com/docs/advertising-cloud/dsp/audiences/sources/source-create.html) ソース。 DSP アカウントチームは、Experience Cloud組織IDを使用してソースを作成します。
* DSP アカウントまたは広告主のソースキー。DSP[[!DNL Real-Time CDP] で](https://experienceleague.adobe.com/docs/advertising-cloud/dsp/audiences/sources/source-create.html) ソースが作成されたときに生成されます。 DSP アカウントチームがこのキーを共有します。 Experience Platform内で使用して、次の[に示すように、Advertising Cloud DSPの宛先への宛先接続を作成します](#authenticate)。
* 電子メールやハッシュ化された電子メールから構成される顧客データ。

## サポートされている ID {#supported-identities}

[!DNL Adobe Advertising Cloud] DSPの宛先では、次の表に示すIDのアクティベーションがサポートされています。 [ID](/help/identity-service/features/namespaces.md) についての詳細情報。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| email_lc_sha256 | SHA256 アルゴリズムでハッシュ化されたメールアドレス | Experience Platformは、プレーンテキストとSHA256 ハッシュ化されたメールアドレスの両方をサポートしています。 ソースフィールドにハッシュ化されていない属性が含まれている場合は、**[!UICONTROL Apply transformation]** オプションをオンにして、Experience Platformがアクティベーション時にデータを自動的にハッシュします。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しタイプと頻度については、次の表を参照してください。

| 項目 | タイプ | メモ |
|---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL Audience export]** | Advertising Cloud DSPの宛先で使用されている識別子（電子メールまたはハッシュ化された電子メール）を使用して、オーディエンスのすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL Streaming]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいてExperience Platformでプロファイルが更新されると、コネクターは更新をダウンストリームの宛先プラットフォームに送信します。 詳しくは、[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)を参照してください。 |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

>[!IMPORTANT]
>
>宛先に接続するには、Experience Platformの&#x200B;**[!UICONTROL View Destinations]**&#x200B;および&#x200B;**[!UICONTROL Manage Destinations]** [&#x200B; アクセス制御権限](/help/access-control/home.md#permissions)が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

宛先に接続するには、[Experience Platform ユーザーインターフェイスを使用して宛先接続](/help/destinations/ui/connect-destination.md)を作成する手順に従います。 宛先の設定ワークフローで、以下の 2 つの節でリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

宛先に接続するには、[!UICONTROL Connection type] セクションに次のパラメーターを指定し、**[!UICONTROL Connect to destination]**&#x200B;を選択します。

* **[!UICONTROL Account or Advertiser Key]**：この[!UICONTROL Source Key]は、[[!DNL Real-Time CDP]  ソースがDSP ユーザーインターフェイス &#x200B;](https://experienceleague.adobe.com/docs/advertising-cloud/dsp/audiences/sources/source-create.html)で作成されたときに生成されます。 DSP アカウントチームは、ソースを作成した後、このキーを共有します。

![接続タイプ フィールド &#x200B;](/help/destinations/assets/catalog/advertising/adobe-advertising-cloud-connection/authenticate-destination.png)

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

* **[!UICONTROL Name]**：今後この宛先を認識する際に使用する名前。
* **[!UICONTROL Description]**：今後この宛先を特定するのに役立つ説明です。

![宛先の詳細フィールド &#x200B;](/help/destinations/assets/catalog/advertising/adobe-advertising-cloud-connection/destination-details.png)

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の提供が完了したら、**[!UICONTROL Next]**&#x200B;を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
>
>* データをアクティブ化するには、**[!UICONTROL View Destinations]**、**[!UICONTROL Activate Destinations]**、**[!UICONTROL View Profiles]**&#x200B;および&#x200B;**[!UICONTROL View Segments]** [&#x200B; アクセス制御権限](/help/access-control/home.md#permissions)が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* *ID*&#x200B;をエクスポートするには、**[!UICONTROL View Identity Graph]** [&#x200B; アクセス制御権限](/help/access-control/home.md#permissions)が必要です。<br> ![&#x200B; ワークフローで強調表示されているID名前空間を選択して、オーディエンスを宛先にアクティブ化します。](/help/destinations/assets/overview/export-identities-to-destination.png " ワークフローで強調表示されたID名前空間を選択して、オーディエンスを宛先にアクティブ化します。"){width="100" zoomable="yes"}

この宛先にオーディエンスをアクティベートする手順は、[ストリーミングオーディエンスの書き出し宛先へのプロファイルとオーディエンスのアクティベート](/help/destinations/ui/activate-segment-streaming-destinations.md)を参照してください。

## データの書き出しを検証する {#exported-data}

データオーディエンスがAdvertising Cloudと共有されていることを確認するには、次の点を確認します。

* [!DNL Real-Time CDP]宛先のデータ フローが成功しました。

* DSPでは、オーディエンスは、[!UICONTROL Audiences] > [!UICONTROL All Audiences]またはプレースメント設定の[!UICONTROL Audience Targeting] セクション内からオーディエンスを作成または編集する際に使用できます。 オーディエンスは、[!UICONTROL Adobe Segments] フォルダーの下の[!UICONTROL Real-Time CDP] タブに表示されます。

DSP オーディエンス設定の![Real-Time CDP オーディエンス &#x200B;](/help/destinations/assets/catalog/advertising/adobe-advertising-cloud-connection/segments-in-dsp.png)

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform]がデータガバナンスを適用する方法について詳しくは、[&#x200B; データガバナンスの概要](/help/data-governance/home.md)を参照してください。
