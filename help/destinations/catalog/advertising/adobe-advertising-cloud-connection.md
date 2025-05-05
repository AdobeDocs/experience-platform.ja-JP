---
title: Adobe Advertising Cloud DSP接続
description: Adobe Advertising Cloud DSPは、Adobe Real-time Customer Data Platformの統合先であり、認証済みのファーストパーティオーディエンスを承認済みの広告主やユーザーと共有して、キャンペーンのアクティベーションを行うことができます。
exl-id: 11ff7797-a9c6-4334-b843-ae9df9a48e54
source-git-commit: ba39f62cd77acedb7bfc0081dbb5f59906c9b287
workflow-type: tm+mt
source-wordcount: '1055'
ht-degree: 25%

---

# Adobe Advertising Cloud DSP接続

## 概要 {#overview}

Adobe Advertising Cloud [!DNL Demand-Side Platform] （DSP）の宛先を使用すると、認証済みのファーストパーティオーディエンスを、承認済みの広告主やユーザーと共有して、DSPとのキャンペーンアクティベーションを行うことができます。 DSPとのReal-Time CDP統合について詳しくは、[ オーディエンスソースからの認証済みオーディエンスのアクティブ化について ](https://experienceleague.adobe.com/docs/advertising-cloud/dsp/audiences/sources/source-about.html?lang=ja) を参照してください。

>[!IMPORTANT]
>
>このページはDSP チームによって作成されました。 お問い合わせや更新のリクエストについては、Advertising Cloud サポート（`adcloud_support@adobe.com`）に直接お問い合わせください。

## ユースケース {#use-cases}

Advertising Cloud DSPの宛先を使用する方法とタイミングをより深く理解するために、Adobe Experience Platformのお客様がこの宛先を使用して解決できるサンプルユースケースを以下に示します。

### Brand Advertising の使用例

オンライン小売業者は、ターゲティングに Cookie を使用しない表示キャンペーンを通じて、価値の高い顧客をリターゲットしたいと考えています。 小売業者は、価値の高い顧客のハッシュ化されたメール ID で構成されるオーディエンスを、Adobe Real-time Customer Data Platform（Real-Time CDP）アカウントからDSP アカウントに共有します。 次に、DSPは、DSPと LiveRamp の間のパートナーシップを通じて、ハッシュ化されたメール ID を認証済み [!DNL RampIDs] に変換します。 結果の [!DNL RampIDs] は、オーディエンスをターゲットにするディスプレイキャンペーンで使用できます。

### 代理店のユースケース

DSP アカウントを持つメディアエージェンシーは、接客業のトップブランドであるお客様に代わってリターゲティングキャンペーンを実施しています。 このブランドは、昨年、すべてのゲストを新しいプロモーションオファーでリターゲティングしたいと考えています。 ブランドは、すべてのゲスト情報を [!DNL Real-Time CDP] でホストします。 メディアキャンペーンを通じてゲストを再ターゲットするために、ブランドは、[!DNL Real-Time CDP] アカウントからメディアエージェンシーのDSP アカウントに、ゲストのハッシュ化されたメール ID で構成されるオーディエンスを共有できます。

## 前提条件 {#prerequisites}

* DSPのアカウントレベルとキャンペーンレベルの設定によって、[!DNL LiveRamp RampID] とのオーディエンス共有が可能になります。これにより、顧客データが [!DNL RampIDs] に変換され、ターゲティング可能なセグメントが作成されます。 この設定は、担当のDSP アカウントチームが行います。 [!DNL RampID] は、DSPと [!DNL LiveRamp] の間のパートナーシップを通じて利用でき、使用するために独自の [!DNL LiveRamp] メンバーシップは必要ありません。
* Experience PlatformアカウントのExperience Cloud組織 ID。 お使いの ID は、[!DNL Real-Time CDP] ユーザープロファイルページで確認できます。
* キャンペーンアクティベーション用のオーディエンスを受け取る [[!DNL Real-Time CDP] DSP内のソース ](https://experienceleague.adobe.com/docs/advertising-cloud/dsp/audiences/sources/source-create.html?lang=ja)。 DSP アカウントチームは、Experience Cloud組織 ID を使用してソースを作成します。
* DSP アカウントまたは広告主のソースキー。[[!DNL Real-Time CDP]  ソースがDSPで作成される ](https://experienceleague.adobe.com/docs/advertising-cloud/dsp/audiences/sources/source-create.html?lang=ja) ときに生成されます。 DSP アカウントチームがこのキーを共有します。 [ 後述 ](#authenticate) のように、Experience Platform内で使用して、Advertising Cloud DSPの宛先への宛先接続を作成します。
* メールまたはハッシュ化されたメールで構成される顧客データ。

## サポートされている ID {#supported-identities}

Adobe Advertising Cloud DSPの宛先では、以下の表に示す ID のアクティブ化をサポートしています。 [ID](/help/identity-service/features/namespaces.md) についての詳細情報。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| email_lc_sha256 | SHA256 アルゴリズムでハッシュ化されたメールアドレス | Experience Platformでは、プレーンテキストと SHA256 でハッシュ化されたメールアドレスの両方をサポートしています。 ハッシュ化されていない属性がソースフィールドに含まれている場合は、「**[!UICONTROL 変換を適用]**」オプションをオンにして、アクティブ化時にExperience Platformでデータを自動的にハッシュ化するように設定します。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、次の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL オーディエンスの書き出し]** | Advertising Cloud DSPの宛先で使用される識別子（メールまたはハッシュ化されたメール）を使用して、オーディエンスのすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンスの評価に基づいてExperience Platform内でプロファイルが更新されると、コネクタは更新を宛先プラットフォームに送信します。 詳しくは、[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)を参照してください。 |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先にExperience Platformするには、接続に **[!UICONTROL 宛先の表示]** および **[!UICONTROL 宛先の管理]**&#x200B;[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

宛先に接続するには、Experience Platformユーザーインターフェイスを使用して [ 宛先接続の作成 ](/help/destinations/ui/connect-destination.md) の手順に従います。 宛先の設定ワークフローで、以下の 2 つの節でリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

宛先に接続するには、「接続タイプ [!UICONTROL 」セクションで以下のパラメーターを指定し &#x200B;] 「**[!UICONTROL 宛先に接続]**」を選択します。

* **[!UICONTROL アカウントまたは広告主キー]**：この [!UICONTROL Source キー &#x200B;] は、[[!DNL Real-Time CDP]  ソースがDSP ユーザーインターフェイスで作成 ](https://experienceleague.adobe.com/docs/advertising-cloud/dsp/audiences/sources/source-create.html?lang=ja) されたときに生成されます。 DSP アカウントチームがソースを作成した後、このキーを共有します。

![ 接続タイプフィールド ](/help/destinations/assets/catalog/advertising/adobe-advertising-cloud-connection/authenticate-destination.png)

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

* **[!UICONTROL 名前]**：今後この宛先を認識するための名前。
* **[!UICONTROL 説明]**：今後この宛先を識別するのに役立つ説明。

![ 宛先の詳細フィールド ](/help/destinations/assets/catalog/advertising/adobe-advertising-cloud-connection/destination-details.png)

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>* データをアクティブ化するには、**[!UICONTROL 宛先の表示]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]** および **[!UICONTROL セグメントの表示]**&#x200B;[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* *ID* を書き出すには、**[!UICONTROL ID グラフの表示]**&#x200B;[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。<br> ![ 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択します。](/help/destinations/assets/overview/export-identities-to-destination.png " 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択 "){width="100" zoomable="yes"}

この宛先にオーディエンスをアクティベートする手順は、[ストリーミングオーディエンスの書き出し宛先へのプロファイルとオーディエンスのアクティベート](/help/destinations/ui/activate-segment-streaming-destinations.md)を参照してください。

## データの書き出しを検証する {#exported-data}

データオーディエンスがAdvertising Cloudと共有されたことを確認するには、次の点を確認します。

* [!DNL Real-Time CDP] 宛先のデータフローが成功しました。

* DSPでは、オーディエンスは、[!UICONTROL &#x200B; オーディエンス &#x200B;]/[!UICONTROL &#x200B; すべてのオーディエンス &#x200B;] またはプレースメント設定の [!UICONTROL &#x200B; オーディエンスターゲティング &#x200B;] セクション内からオーディエンスを作成または編集する際に使用できます。 オーディエンスは、[!UICONTROL Real-Time CDP] フォルダーの下の「[!UICONTROL Adobeセグメント &#x200B;]」タブに表示されます。

![DSP audience settings のReal-Time CDP オーディエンス ](/help/destinations/assets/catalog/advertising/adobe-advertising-cloud-connection/segments-in-dsp.png)

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのようにデータガバナンスを実施するかについて詳しくは、[ データガバナンスの概要 ](/help/data-governance/home.md) を参照してください。
