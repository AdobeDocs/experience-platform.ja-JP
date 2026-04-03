---
title: Marketo Measure Ultimateの目的地
description: Marketo Measure Ultimateの宛先にデータを接続してアクティベートする方法について説明します。
last-substantial-update: 2023-03-07T00:00:00Z
exl-id: b4220841-8908-41ff-b977-dbeebfa787c8
source-git-commit: 58f69a78fb3c622c8741d7a1618f15509c160a5b
workflow-type: tm+mt
source-wordcount: '782'
ht-degree: 26%

---

# Marketo Measure Ultimate Destination {#mmu-destination}

## 概要 {#overview}

Marketo Measure（旧Bizible）は、insightをマーケターに提供し、自社の売上を促進し、投資収益率を最大化する上で、最も効果的なマーケティング活動を実現します。 Marketo Measureは、チャネルのパフォーマンスを自動的に追跡およびレポートし、最も顧客エンゲージメントを促進しているチャネルを可視化することで、マーケティング費用を最適化できるマーケティングアトリビューションソリューションです。

宛先は、[!DNL Adobe Experience Platform]からMarketo Measureへの企業間（B2B）データフローを有効にします。 Marketo Measure Ultimateをご利用のお客様のみが利用できます。

## ユースケース {#use-cases}

Marketo Measureの宛先を使用する方法とタイミングをより正確に把握するために、この宛先を使用して[!DNL Adobe Experience Platform]のお客様が解決できるユースケースの例を次に示します。 この統合：

* 大企業の複雑なデータとパフォーマンスのレポート要件を満たします。
* 複数のCRMおよびマーケティングオートメーションシステムを使用して、B2B アトリビューションレポートを作成できます。
* サードパーティのオフラインタッチポイントデータを簡単に取り込むことができます。

## 前提条件 {#prerequisites}

Marketo Measureの宛先に関する次の前提条件に注意してください。

* Experience Platform サンドボックスマッピングは、管理者がMarketo Measure設定ページで行う必要があります。 サンドボックスマッピングがなければ、宛先に接続してデータを保存してアクティブ化するワークフローを完了できません。
* B2B XDM クラスのデータセットのみをエクスポートできます（例えば、XDM ビジネスアカウントおよびXDM ビジネスオポチュニティクラスを参照）。 特定のデータソースに対して、同じB2B XDM クラスの複数のデータセットを取り込むことはできません。
* 各データセットは、Marketo Measureの宛先に対する1つのデータフローにのみ含めることができます。

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスの由来 | サポートあり | 説明 |
|---------|----------|----------|
| [!DNL Segmentation Service] | ○ | Experience Platform [ セグメント化サービス ](../../../segmentation/home.md)を通じて生成されたオーディエンス。 |
| その他すべてのオーディエンスの生成元 | × | このカテゴリには、[!DNL Segmentation Service]を通じて生成されたオーディエンス以外のすべてのオーディエンスのオリジンが含まれます。 [様々なオーディエンスの起源](/help/segmentation/ui/audience-portal.md#customize)について読みます。 次に例を示します。 <ul><li> カスタムアップロードオーディエンス [がCSV ファイルからExperience Platformに](../../../segmentation/ui/audience-portal.md#import-audience)をインポートしました。</li><li> 類似オーディエンス， </li><li> 連合オーディエンス， </li><li> [!DNL Adobe Journey Optimizer]などの他のExperience Platform アプリで生成されたオーディエンス </li><li> その他。 </li></ul> |

{style="table-layout:auto"}



オーディエンスのデータタイプ別にサポートされるオーディエンス：

| オーディエンスのデータタイプ | サポートあり | 説明 | ユースケース |
|--------------------|-----------|-------------|-----------|
| [人物オーディエンス ](/help/segmentation/types/people-audiences.md) | × | 顧客プロファイルにもとづいて、マーケティング施策の特定のグループをターゲットにすることができます。 | 買い物客やカートの放棄が多い |
| [ アカウントオーディエンス ](/help/segmentation/types/account-audiences.md) | × | アカウントベースドマーケティング戦略のために、特定の組織内の個人をターゲットにします。 | B2B マーケティング |
| [見込みオーディエンス ](/help/segmentation/types/prospect-audiences.md) | × | まだ顧客ではないが、ターゲットオーディエンスと特徴を共有する個人をターゲットにします。 | サードパーティデータによる見込み顧客の開拓 |
| [ データセットの書き出し](/help/catalog/datasets/overview.md) | ○ | [!DNL Adobe Experience Platform] データ レイクに保存されている構造化データのコレクション。 | レポート，データサイエンスワークフロー |

{style="table-layout:auto"}


## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
|---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL Dataset export]** | 生のデータセットを書き出します。これらのデータセットは、オーディエンスの興味や資格によってグループ化または構造化されていません。 [ データセットの書き出し](/help/destinations/destination-types.md#dataset-export-destinations)について詳しく説明します。 |
| 書き出し頻度 | **[!UICONTROL Batch]** | このバッチ宛先は、2時間ごとにファイルをMarketo Measure プラットフォームに書き出します。 詳しくは、[ データセットの書き出しのスケジュール ](/help/destinations/ui/export-datasets.md#scheduling)を参照してください。 |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

>[!IMPORTANT]
>
>宛先に接続するには、**[!UICONTROL View Destinations]**&#x200B;および&#x200B;**[!UICONTROL Manage and Activate Dataset Destinations]** [ アクセス制御権限](/help/access-control/home.md#permissions)が必要です。 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下のセクションに記載されているフィールドに入力します。

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

* **[!UICONTROL Name]**：今後この宛先を認識する際に使用する名前。
* **[!UICONTROL Description]**：今後この宛先を特定するのに役立つ説明です。

![Marketo Measureの宛先への接続ワークフロー。](/help/destinations/assets/catalog/adobe/marketo-measure-ultimate/marketo-measure-connect-to-destination.png)

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の提供が完了したら、**[!UICONTROL Next]**&#x200B;を選択します。

## データセットをこの宛先に書き出す {#export-datasets}

>[!IMPORTANT]
>
>データをアクティブ化するには、**[!UICONTROL View Destinations]**&#x200B;および&#x200B;**[!UICONTROL Manage and Activate Dataset Destinations]** [ アクセス制御権限](/help/access-control/home.md#permissions)が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

データセットをこの宛先に書き出す詳細な手順については、[ データセットの書き出し](/help/destinations/ui/export-datasets.md) チュートリアルを参照してください。

## データの書き出しを検証する {#exported-data}

正常なデータセット書き出しを検証するには、データセットが[Snowflake データウェアハウス ](https://experienceleague.adobe.com/docs/marketo-measure/using/marketo-measure-data-warehouse/data-warehouse-access-reader-account.html)に正常に送信されたことを確認します。

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのように データガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。
