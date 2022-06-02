---
title: Adobe Advertising Cloud DSP接続
description: Adobe Advertising Cloud DSPは、 [!DNL Adobe Real-time Customer Data Profile]：認証済みのファーストパーティセグメントを承認済みの広告主やユーザーと共有して、キャンペーンをアクティベートできます。
exl-id: 11ff7797-a9c6-4334-b843-ae9df9a48e54
source-git-commit: 2b8c9d81b7d9eddbbed3119a496e9c8d37e6c415
workflow-type: tm+mt
source-wordcount: '954'
ht-degree: 4%

---

# Adobe Advertising Cloud DSP接続

## 概要 {#overview}

ザAdobe Advertising Cloud [!DNL Demand-Side Platform] (DSP) の宛先を使用すると、認証済みのファーストパーティセグメントを承認済みの広告主やユーザーと共有し、DSPとキャンペーンのアクティベーション用に共有できます。 DSPとのReal-Time CDP統合について詳しくは、 [オーディエンスソースからの認証済みセグメントのアクティブ化について](https://experienceleague.adobe.com/docs/advertising-cloud/dsp/audiences/sources/source-about.html).

>[!IMPORTANT]
>
>このページはDSPチームが作成しました。 お問い合わせや更新のご依頼については、 Advertising Cloudサポートに直接お問い合わせください。 `adcloud_support@adobe.com`.

## ユースケース {#use-cases}

Advertising Cloud DSPの宛先をいつどのように使用するかを理解しやすくするために、Adobe Experience Platformのお客様がこの宛先を使用して解決できる使用例を以下に示します。

### ブランド広告の使用例

あるオンライン小売業者は、ターゲティングに cookie を使用せずに、ディスプレイキャンペーンを通じて価値の高い顧客を再ターゲット化したいと考えています。 小売業者は、高価値顧客のハッシュ化された電子メール ID から成るセグメントを、その [!DNL Real-Time CDP] アカウントをDSPアカウントに追加します。 DSPは、ハッシュ化された電子メール ID を認証済みに変換します [!DNL RampIDs] DSPと LiveRamp のパートナーシップを通じて 結果 [!DNL RampIDs] を使用して、オーディエンスをターゲット設定するディスプレイキャンペーンで使用できます。

### エージェンシーの使用例

DSPアカウントを持つメディアエージェンシーは、接客業のトップブランドである顧客に代わってリターゲティングキャンペーンを実施しています。 同ブランドは、昨年にすべてのゲストを再ターゲット化し、新しいプロモーションオファーを提供したいと考えています。 ブランドは、のすべてのゲスト情報をホストします。 [!DNL Real-Time CDP]. ブランドは、ゲストのハッシュ化された電子メール ID で構成されるセグメントを共有できます [!DNL Real-Time CDP] メディアエージェンシーのDSPアカウントにアカウントし、メディアキャンペーンを通じてゲストをリターゲティングします。

## 前提条件 {#prerequisites}

* DSPのアカウントレベルとキャンペーンレベルの設定で、 [!DNL LiveRamp RampID]（顧客データをに変換します） [!DNL RampIDs] をクリックして、ターゲット設定可能なセグメントを作成します。 この設定は、DSPアカウントチームが実行します。
* Experience CloudアカウントのExperience Platform組織 ID。 ID は、 [!DNL Real-Time CDP] ユーザープロファイルページ
* A [[!DNL Real-Time CDP] DSP内のソース](https://experienceleague.adobe.com/docs/advertising-cloud/dsp/audiences/sources/source-create.html) をクリックして、campaign のアクティベーション用のセグメントを受け取ります。 DSPアカウントチームは、組織 ID を使用してExperience Cloudを作成します。
* DSPアカウントまたは広告主のソースキー。 [[!DNL Real-Time CDP] ソースがDSPで作成されました](https://experienceleague.adobe.com/docs/advertising-cloud/dsp/audiences/sources/source-create.html). DSPアカウントチームがこのキーを共有します。 Experience Platform内で使用して、Advertising Cloud DSPの宛先への宛先接続を作成します。 [以下に説明する](#authenticate).
* 電子メールまたはハッシュ化された電子メールで構成される顧客データ。

## サポートされる ID {#supported-identities}

Adobe Advertising Cloud DSPの宛先では、以下の表で説明する ID のアクティブ化をサポートしています。 詳細情報： [id](/help/identity-service/namespaces.md).

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| email_lc_sha256 | SHA256 アルゴリズムでハッシュ化された電子メールアドレス | Experience Platformは、プレーンテキストと SHA256 ハッシュ化された電子メールアドレスの両方をサポートしています。 ソースフィールドにハッシュ化されていない属性が含まれている場合は、 **[!UICONTROL 変換を適用]** オプションを使用して、Experience Platformがアクティベーション時にデータを自動的にハッシュ化するように設定する必要があります。 |

{style=&quot;table-layout:auto&quot;}

## エクスポートのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、次の表を参照してください。

| 項目 | タイプ | 備考 |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL セグメントエクスポート]** | Advertising Cloud DSPの宛先で使用される識別子（電子メールまたはハッシュ化された電子メール）を使用して、セグメント（オーディエンス）のすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は、API ベースの接続です。 セグメントの評価に基づいてExperience Platformでプロファイルが更新されると、コネクタは更新を宛先プラットフォームに送信します。 詳細を表示 [ストリーミング先](/help/destinations/destination-types.md#streaming-destinations). |

{style=&quot;table-layout:auto&quot;}

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、 **[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions) Experience Platform 詳しくは、 [アクセス制御の概要](/help/access-control/ui/overview.md) または製品管理者に問い合わせて、必要な権限を取得してください。

宛先に接続するには、次の手順に従います： [宛先接続の作成](/help/destinations/ui/connect-destination.md) を使用します。 宛先設定ワークフローで、以下の 2 つのセクションにリストするフィールドに入力します。

### 宛先に対する認証 {#authenticate}

宛先に接続するには、 [!UICONTROL 接続タイプ] セクションで、 **[!UICONTROL 宛先に接続]**.:

* **[!UICONTROL アカウントまたは広告主キー]**:この [!UICONTROL ソースキー] が [[!DNL Real-Time CDP] ソースはDSPユーザーインターフェイスで作成されます。](https://experienceleague.adobe.com/docs/advertising-cloud/dsp/audiences/sources/source-create.html). ソースを作成した後、DSPアカウントチームがこのキーを共有します。

![接続タイプフィールド](/help/destinations/assets/catalog/advertising/adobe-advertising-cloud-connection/authenticate-destination.png)

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、 [!UICONTROL 宛先の詳細] セクションで、 **[!UICONTROL 次へ]**.

* **[!UICONTROL 名前]**:将来この宛先を認識するための名前。
* **[!UICONTROL 説明]**:今後この宛先を識別するのに役立つ説明。

![宛先の詳細フィールド](/help/destinations/assets/catalog/advertising/adobe-advertising-cloud-connection/destination-details.png)

## この宛先に対してセグメントをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>データをアクティブ化するには、 **[!UICONTROL 宛先の管理]**, **[!UICONTROL 宛先のアクティブ化]**, **[!UICONTROL プロファイルの表示]**、および **[!UICONTROL セグメントを表示]** [アクセス制御権限](/help/access-control/home.md#permissions). 詳しくは、 [アクセス制御の概要](/help/access-control/ui/overview.md) または製品管理者に問い合わせて、必要な権限を取得してください。

読み取り [ストリーミングセグメントの書き出し先に対するプロファイルとセグメントのアクティブ化](/help/destinations/ui/activate-segment-streaming-destinations.md) を参照してください。

## データの書き出しを検証する {#exported-data}

データセグメントがAdvertising Cloudと共有されていることを確認するには、以下を確認します。

* のデータフロー [!DNL Real-Time CDP] の宛先が正常に作成されました。

* DSPでは、 [!UICONTROL オーディエンス] > [!UICONTROL すべてのオーディエンス] または、 [!UICONTROL オーディエンスのターゲティング] 配置設定のセクション。 セグメントが [!UICONTROL Adobeセグメント] タブを [!UICONTROL Real-Time CDP] フォルダー。

![DSPオーディエンス設定のReal-Time CDPセグメント](/help/destinations/assets/catalog/advertising/adobe-advertising-cloud-connection/segments-in-dsp.png)

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。詳しくは、 [!DNL Adobe Experience Platform] データガバナンスを強制します。詳しくは、 [データガバナンスの概要](/help/data-governance/home.md).
