---
title: Demandbase接続
description: この宛先を使用して、Account-Based Marketing（ABM）ユースケースのアカウントオーディエンスをアクティブ化します。 DemandBaseのB2B Demand Side Platform（DSP）を介して、ターゲットアカウント内の関連するペルソナや役割に広告を表示します。 ターゲットアカウントは、マーケティングやセールスにおけるその他の下流のユースケースのために、Demandbaseのサードパーティデータで強化することもできます。
last-substantial-update: 2024-09-30T00:00:00Z
exl-id: a84609a2-f1d3-4998-9db4-ad59c0a0b631
source-git-commit: 58f69a78fb3c622c8741d7a1618f15509c160a5b
workflow-type: tm+mt
source-wordcount: '904'
ht-degree: 16%

---


# Demandbase接続 {#demandbase}

>[!AVAILABILITY]
>
>デマンドベース宛先にアカウントオーディエンスをアクティブ化する機能は、[の](/help/rtcdp/overview.md#rtcdp-b2b)Business-to-Business[および](/help/rtcdp/overview.md#rtcdp-b2p)Business-to-Person[!DNL Real-Time Customer Data Platform] エディションを購入する企業で使用できます。

[&#x200B; アカウントオーディエンス &#x200B;](/help/segmentation/types/account-audiences.md)に基づいて、オーディエンスのターゲティング、パーソナライゼーション、抑制のために、デマンドベースキャンペーンのプロファイルをアクティブ化します。

## ユースケース {#use-case}

この宛先を使用して、Account-Based Marketing（ABM）ユースケースのアカウントオーディエンスをアクティブ化します。 DemandBaseのB2B Demand Side Platform（DSP）を介して、ターゲットアカウント内の関連するペルソナや役割に広告を表示します。 ターゲットアカウントは、マーケティングやセールスにおけるその他の下流のユースケースのために、Demandbaseのサードパーティデータで強化することもできます。

たとえば、DemandbaseのアドテクDSPを活用して、主要アカウント内の特定のペルソナや役割をターゲットにしたり、funnel上部でのリードジェネレーションを実現したり、購買グループを作成して成長させたりできます。 デマンドベースの宛先を使用して、アカウントを効果的にターゲティングする他のユースケースを検討します。

また、リアルタイムのアカウント情報検索を使用して、web サイト体験をパーソナライズし、エンゲージメントを最適化できます。

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスの由来 | サポートあり | 説明 |
|---------|----------|----------|
| [!DNL Segmentation Service] | ○ | Experience Platform [&#x200B; セグメント化サービス &#x200B;](../../../segmentation/home.md)を通じて生成されたオーディエンス。 |
| その他すべてのオーディエンスの生成元 | ○ | このカテゴリには、[!DNL Segmentation Service]を通じて生成されたオーディエンス以外のすべてのオーディエンスのオリジンが含まれます。 [様々なオーディエンスの起源](/help/segmentation/ui/audience-portal.md#customize)について読みます。 次に例を示します。 <ul><li> カスタムアップロードオーディエンス [がCSV ファイルからExperience Platformに](../../../segmentation/ui/audience-portal.md#import-audience)をインポートしました。</li><li> 類似オーディエンス， </li><li> 連合オーディエンス， </li><li> [!DNL Adobe Journey Optimizer]などの他のExperience Platform アプリで生成されたオーディエンス </li><li> その他。 </li></ul> |

{style="table-layout:auto"}

オーディエンスのデータタイプ別にサポートされるオーディエンス：

| オーディエンスのデータタイプ | サポートあり | 説明 | ユースケース |
|--------------------|-----------|-------------|-----------|
| [人物オーディエンス &#x200B;](/help/segmentation/types/people-audiences.md) | ○ | 顧客プロファイルにもとづいて、マーケティング施策の特定のグループをターゲットにすることができます。 | 買い物客やカートの放棄が多い |
| [&#x200B; アカウントオーディエンス &#x200B;](/help/segmentation/types/account-audiences.md) | ○ | アカウントベースドマーケティング戦略のために、特定の組織内の個人をターゲットにします。 | B2B マーケティング |
| [見込みオーディエンス &#x200B;](/help/segmentation/types/prospect-audiences.md) | × | まだ顧客ではないが、ターゲットオーディエンスと特徴を共有する個人をターゲットにします。 | サードパーティデータによる見込み顧客の開拓 |
| [&#x200B; データセットの書き出し](/help/catalog/datasets/overview.md) | × | [!DNL Adobe Experience Platform] データ レイクに保存されている構造化データのコレクション。 | レポート，データサイエンスワークフロー |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-and-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
|--------------|-----------|---------------------------|
| 書き出しタイプ | オーディエンスの書き出し | 名前や電話番号などの主要な識別子を使用して、あらゆるオーディエンスメンバーを書き出すことができます。 |
| 頻度 | ストリーミング | &quot;Always-on&quot; API ベースの接続。 更新は、プロファイルが変更された直後にダウンストリームに送信されます。 |

{style="table-layout:auto"}

## 前提条件 {#prerequisites}

アカウントのオーディエンスをDemandbaseに書き出すには、次の操作が必要です。

1. Demandbase アカウント。
2. Demandbase API トークン。 DemandbaseでユーザーとAPI トークンを生成できます。 トークンを生成するには、Demandbase アカウントにログインした後、[&#x200B; マイプロファイル/API トークン &#x200B;](https://web.demandbase.com/o/ad/at)に移動します。

## 宛先への接続 {#connect}

>[!IMPORTANT]
>
>宛先に接続するには、**[!UICONTROL View Destinations]**&#x200B;および&#x200B;**[!UICONTROL Manage Destinations]** [&#x200B; アクセス制御権限](/help/access-control/home.md#permissions)が必要です。 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

宛先に対して認証を行うには、必須フィールドに入力し、**[!UICONTROL Connect to destination]**&#x200B;を選択します。

![&#x200B; ベアラートークンを追加](/help/destinations/assets/catalog/advertising/demandbase/add-bearer-token.png)

* **[!UICONTROL Bearer token]**: ベアラートークンを入力して、宛先に対する認証を行います。 トークンの取得方法について詳しくは、[前提条件](#prerequisites)を参照してください。

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

![宛先接続に関する情報を追加](/help/destinations/assets/catalog/advertising/demandbase/name-and-description.png)

* **[!UICONTROL Name]**：今後この宛先を認識する際に使用する名前。
* **[!UICONTROL Description]**：今後この宛先を特定するのに役立つ説明です。
* **[!UICONTROL Entity type]**: エンティティの種類として&#x200B;**[!UICONTROL Account]**&#x200B;を選択します。

これで、Demandbase内でオーディエンスをアクティブ化する準備が整いました。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
>
>* データをアクティブ化するには、**[!UICONTROL View Destinations]**、**[!UICONTROL Activate Destinations]**、**[!UICONTROL View Profiles]**&#x200B;および&#x200B;**[!UICONTROL View Segments]** [&#x200B; アクセス制御権限](/help/access-control/home.md#permissions)が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* *ID*&#x200B;をエクスポートするには、**[!UICONTROL View Identity Graph]** [&#x200B; アクセス制御権限](/help/access-control/home.md#permissions)が必要です。<br> ![&#x200B; ワークフローで強調表示されているID名前空間を選択して、オーディエンスを宛先にアクティブ化します。](/help/destinations/assets/overview/export-identities-to-destination.png " ワークフローで強調表示されたID名前空間を選択して、オーディエンスを宛先にアクティブ化します。"){width="100" zoomable="yes"}

この宛先に対してアカウントオーディエンスをアクティブ化する手順については、[&#x200B; アカウントオーディエンスをアクティブ化](/help/destinations/ui/activate-account-audiences.md)を参照してください。

### 必須マッピング {#mandatory-mappings}

[!DNL Demandbase]宛先に対してオーディエンスをアクティブ化する場合は、マッピング手順で次の必須フィールドマッピングを設定する必要があります。

| ソースフィールド | ターゲットフィールド | 説明 |
|--------------|--------------|-------------|
| `xdm: accountName` | `xdm: accountName` | アカウントの名前 |
| `xdm: accountOrganization.domain` | `xdm: accountEmailDomain` | アカウント組織のメールドメイン |
| `xdm: accountKey.sourceKey` | `Identity: primaryId` | アカウントのプライマリ ID |

![&#x200B; デマンドベースマッピング &#x200B;](/help/destinations/assets/catalog/advertising/demandbase/demandbase-mapping.png)

宛先が正しく機能するためには、これらのマッピングが必要であり、アクティベーションワークフローを続行する前に設定する必要があります。

## その他のメモと重要なコールアウト {#additional-notes}

* **オーディエンスの命名**：同じ名前のアカウントオーディエンスがDemandbaseに対して以前にアクティブ化された場合、Demandbaseの宛先に対して別のデータフローを介して再びアクティブ化することはできません。
* **Demandbase APIのガードレール**: Experience PlatformでオーディエンスをDemandbaseに書き出したが、すべてのデータがDemandbaseに到達しない場合、Demandbase側でAPIのスロットリングが発生した可能性があります。 明確にするために、顧客にリーチする：
* **リストの削除**：アカウントリストは一意であるため、既に使用されている名前で新しいリストを再作成することはできません。 リストからアカウントを削除すると、そのアカウントは使用できなくなります。ただし、削除されることはありません。
* **アクティベーション時間**: Demandbaseでのデータの読み込みは一晩で処理される可能性があります。
