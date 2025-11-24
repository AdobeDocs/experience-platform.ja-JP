---
title: Demandbase 接続
description: この宛先を使用して、Account-Based Marketing（ABM）のユースケースのアカウントオーディエンスをアクティベートします。 DemandBase の B2B Demand Side Platform（DSP）を使用して、ターゲットアカウント内の関連するペルソナやロールにアドバタイズします。 Target アカウントは、マーケティングや販売におけるその他のダウンストリームのユースケース向けに、Demandbase のサードパーティデータを使用して強化することもできます。
badgeB2B: label="B2B edition" type="Informative" url=" https://experienceleague.adobe.com/docs/experience-platform/rtcdp/intro/rtcdp-intro/overview.html?lang=ja#rtcdp-editions newtab=true"
badgeB2P: label="B2P エディション" type="Positive" url=" https://experienceleague.adobe.com/docs/experience-platform/rtcdp/intro/rtcdp-intro/overview.html?lang=ja#rtcdp-editions newtab=true"
last-substantial-update: 2024-09-30T00:00:00Z
exl-id: a84609a2-f1d3-4998-9db4-ad59c0a0b631
source-git-commit: cc05ca282cdfd012366e3deccddcae92a29fef1c
workflow-type: tm+mt
source-wordcount: '785'
ht-degree: 19%

---

# Demandbase 接続 {#demandbase}

>[!AVAILABILITY]
>
>Demandbase の宛先に対してアカウントオーディエンスをアクティブ化する機能は、Real-Time Customer Data Platformの [B2B](/help/rtcdp/overview.md#rtcdp-b2b) エディションと [B2Person](/help/rtcdp/overview.md#rtcdp-b2p) エディションを購入する企業が利用できます。

[&#x200B; アカウントオーディエンス &#x200B;](/help/segmentation/types/account-audiences.md) に基づいて、オーディエンスのターゲティング、パーソナライゼーションおよび抑制のために、Demandbase キャンペーン用のプロファイルをアクティブ化します。

## ユースケース {#use-case}

この宛先を使用して、Account-Based Marketing（ABM）のユースケースのアカウントオーディエンスをアクティベートします。 DemandBase の B2B Demand Side Platform（DSP）を使用して、ターゲットアカウント内の関連するペルソナやロールにアドバタイズします。 Target アカウントは、マーケティングや販売におけるその他のダウンストリームのユースケース向けに、Demandbase のサードパーティデータを使用して強化することもできます。

例えば、Demandbase のアドテックDSPを活用して、funnelトップのリードジェネレーション向けの特定のペルソナや主要アカウント内の役割をターゲットにしたり、購入グループを作成して成長させたりします。 Demandbase の宛先を使用して、アカウントを効果的にターゲットにする他のユースケースを検討します。

この統合を使用すると、リアルタイムのアカウント情報検索を使用して web サイトエクスペリエンスをパーソナライズし、エンゲージメントを最適化することもできます。

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスオリジン | サポートあり | 説明 |
|---------|----------|----------|
| [!DNL Segmentation Service] | ✓ | Experience Platform [&#x200B; セグメント化サービス &#x200B;](../../../segmentation/home.md) を通じて生成されたオーディエンス。 |
| カスタムアップロード | X | CSV ファイルから Experience Platform に[読み込まれた](../../../segmentation/ui/overview.md#import-audience)オーディエンス。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-and-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
|--------------|-----------|---------------------------|
| 書き出しタイプ | オーディエンスのエクスポート | すべてのオーディエンスメンバーは、名前、電話番号などの主要な識別子で書き出されます。 |
| 頻度 | ストリーミング | 「常時」 API ベースの接続。 アップデートは、プロファイルの変更直後にダウンストリームに送信されます。 |

{style="table-layout:auto"}

## 前提条件 {#prerequisites}

アカウントオーディエンスを Demandbase に書き出すには、次が必要です。

1. Demandbase アカウント。
2. Demandbase API トークン。 Demandbase でユーザーと API トークンを生成できます。 トークンを生成するには、Demandbase アカウントにログインした後、[&#x200B; マイプロファイル/API トークン &#x200B;](https://web.demandbase.com/o/ad/at) に移動します。

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL View Destinations]** および **[!UICONTROL Manage Destinations]**&#x200B;[&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

宛先に対する認証を行うには、必須フィールドに入力し、「**[!UICONTROL Connect to destination]**」を選択します。

![&#x200B; ベアラートークンの追加 &#x200B;](/help/destinations/assets/catalog/advertising/demandbase/add-bearer-token.png)

* **[!UICONTROL Bearer token]**：宛先を認証するためのベアラートークンを入力します。 トークンの取得方法については、[&#x200B; 前提条件 &#x200B;](#prerequisites) を参照してください。

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

![&#x200B; 宛先接続に関する情報の追加 &#x200B;](/help/destinations/assets/catalog/advertising/demandbase/name-and-description.png)

* **[!UICONTROL Name]**：今後この宛先を認識するための名前。
* **[!UICONTROL Description]**：今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL Entity type]**: エンティティタイプとして **[!UICONTROL Account]** を選択します。

これで、Demandbase 内でオーディエンスをアクティブ化する準備が整いました。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>* データをアクティブ化するには、**[!UICONTROL View Destinations]**、**[!UICONTROL Activate Destinations]**、**[!UICONTROL View Profiles]**、**[!UICONTROL View Segments]** [&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* *ID* を書き出すには、**[!UICONTROL View Identity Graph]** [&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。<br> ![&#x200B; 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択します。](/help/destinations/assets/overview/export-identities-to-destination.png " 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択 "){width="100" zoomable="yes"}

この宛先にアカウントオーディエンスをアクティブ化する手順については、[&#x200B; アカウントオーディエンスのアクティブ化 &#x200B;](/help/destinations/ui/activate-account-audiences.md) をお読みください。

### 必須のマッピング {#mandatory-mappings}

[!DNL Demandbase] の宛先に対してオーディエンスをアクティブ化する場合は、マッピング手順で次の必須のフィールドマッピングを設定する必要があります。

| ソースフィールド | ターゲットフィールド | 説明 |
|--------------|--------------|-------------|
| `xdm: accountName` | `xdm: accountName` | アカウントの名前 |
| `xdm: accountOrganization.domain` | `xdm: accountEmailDomain` | アカウント組織の電子メールドメイン |
| `xdm: accountKey.sourceKey` | `Identity: primaryId` | アカウントのプライマリ識別子 |

![Demandbase マッピング &#x200B;](/help/destinations/assets/catalog/advertising/demandbase/demandbase-mapping.png)

これらのマッピングは、宛先が正しく機能するために必要であり、アクティベーションワークフローを続行する前に設定する必要があります。

## 追加のメモと重要な引き出し {#additional-notes}

* **オーディエンスの命名**：同じ名前のアカウントオーディエンスが Demandbase に対して以前にアクティブ化された場合、Demandbase の宛先に対して別のデータフローを介して再度アクティブ化することはできません。
* **Demandbase API ガードレール**:Demandbase にオーディエンスを書き出し、Experience Platformで書き出しが成功しても、すべてのデータが Demandbase に到達するわけではない場合、Demandbase 側で API スロットルに遭遇した可能性があります。 明確にするために彼らに連絡してください。
* **リストの削除**：アカウントリストは一意なので、既に使用されている名前で新しいリストを再作成することはできません。 リストからアカウントを削除すると、そのアカウントは使用できなくなりますが、削除はされません。
* **アクティベーション時間**:Demandbase へのデータ読み込みは、一夜で処理される可能性があります。
