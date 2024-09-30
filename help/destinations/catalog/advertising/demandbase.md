---
title: Demandbase 接続
description: この宛先を使用して、Account-Based Marketing（ABM）のユースケースのアカウントオーディエンスをアクティベートします。 DemandBase の B2BDemand Side Platform（DSP）を使用して、ターゲットアカウント内の関連するペルソナやロールにアドバタイズします。 Target アカウントは、マーケティングや販売におけるその他のダウンストリームのユースケース向けに、Demandbase のサードパーティデータを使用して強化することもできます。
badgeB2B: label="B2B エディション" type="Informative" url=" https://experienceleague.adobe.com/docs/experience-platform/rtcdp/intro/rtcdp-intro/overview.html?lang=en#rtcdp-editions newtab=true"
badgeB2P: label="B2P エディション" type="Positive" url=" https://experienceleague.adobe.com/docs/experience-platform/rtcdp/intro/rtcdp-intro/overview.html?lang=en#rtcdp-editions newtab=true"
source-git-commit: 5c694dfab7e53cf8a78500e3ecd086a3986b9b91
workflow-type: tm+mt
source-wordcount: '696'
ht-degree: 26%

---


# Demandbase 接続 {#demandbase}

>[!AVAILABILITY]
>
>>Demandbase の宛先に対してアカウントオーディエンスをアクティブ化する機能は、Real-time Customer Data Platformの [B2B](/help/rtcdp/overview.md#rtcdp-b2b) エディションと [B2Person](/help/rtcdp/overview.md#rtcdp-b2p) エディションを購入する企業が利用できます。

[ アカウントオーディエンス ](/help/segmentation/ui/account-audiences.md) に基づいて、オーディエンスのターゲティング、パーソナライゼーションおよび抑制のために、Demandbase キャンペーン用のプロファイルをアクティブ化します。

## ユースケース {#use-case}

この宛先を使用して、Account-Based Marketing（ABM）のユースケースのアカウントオーディエンスをアクティベートします。 DemandBase の B2BDemand Side Platform（DSP）を使用して、ターゲットアカウント内の関連するペルソナやロールにアドバタイズします。 Target アカウントは、マーケティングや販売におけるその他のダウンストリームのユースケース向けに、Demandbase のサードパーティデータを使用して強化することもできます。

例えば、Demandbase のアドテック DSPを活用して、ファネルのトップのリードジェネレーション向けに特定のペルソナや主要アカウント内の役割をターゲットにしたり、購入グループを作成して成長させたりします。 Demandbase の宛先を使用して、アカウントを効果的にターゲットにする他のユースケースを検討します。

この統合を使用すると、リアルタイムのアカウント情報検索を使用して web サイトエクスペリエンスをパーソナライズし、エンゲージメントを最適化することもできます。

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスオリジン | サポートあり | 説明 |
---------|----------|----------|
| [!DNL Segmentation Service] | ✓ | Experience Platform[ セグメント化サービス ](../../../segmentation/home.md) を通じて生成されたオーディエンス。 |
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
2. Demandbase API トークン。 Demandbase でユーザーと API トークンを生成できます。 トークンを生成するには、Demandbase アカウントにログインした後、[ マイプロファイル/API トークン ](https://web.demandbase.com/o/ad/at) に移動します。

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL 宛先の表示]** および **[!UICONTROL 宛先の管理]**[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

宛先に対して認証するには、必須フィールドに入力し、「**[!UICONTROL 宛先に接続]**」を選択します。

![ ベアラートークンの追加 ](/help/destinations/assets/catalog/advertising/demandbase/add-bearer-token.png)

* **[!UICONTROL ベアラートークン]**：宛先を認証するためのベアラートークンを入力します。 トークンの取得方法については、[ 前提条件 ](#prerequisites) を参照してください。

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

![ 宛先接続に関する情報の追加 ](/help/destinations/assets/catalog/advertising/demandbase/name-and-description.png)

* **[!UICONTROL 名前]**：今後この宛先を認識するための名前。
* **[!UICONTROL 説明]**：今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL エンティティタイプ]**: エンティティタイプとして **[!UICONTROL Account]** を選択します。

これで、Demandbase 内でオーディエンスをアクティブ化する準備が整いました。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>* データをアクティブ化するには、**[!UICONTROL 宛先の表示]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]** および **[!UICONTROL セグメントの表示]**[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* *ID* を書き出すには、**[!UICONTROL ID グラフの表示]**[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。<br> ![ 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択します。](/help/destinations/assets/overview/export-identities-to-destination.png " 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択 "){width="100" zoomable="yes"}

この宛先にアカウントオーディエンスをアクティブ化する手順については、[ アカウントオーディエンスのアクティブ化 ](/help/destinations/ui/activate-account-audiences.md) をお読みください。

## 追加のメモと重要な引き出し {#additional-notes}

* 同じ名前のアカウントオーディエンスが Demandbase に対して以前にアクティブ化された場合、Demandbase の宛先に対する別のデータフローを通じて再度アクティブ化することはできません。
* Demandbase にオーディエンスを書き出し、書き出しがExperience Platformに成功しても、すべてのデータが Demandbase に到達するわけではない場合は、Demandbase 側で API スロットルに遭遇した可能性があります。 明確にするために彼らに連絡してください。
