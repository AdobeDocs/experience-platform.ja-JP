---
title: Marketo Measure Ultimate の宛先
description: Marketo Measure Ultimate の宛先にデータを接続してアクティブ化する方法を説明します。
last-substantial-update: 2023-03-07T00:00:00Z
exl-id: b4220841-8908-41ff-b977-dbeebfa787c8
source-git-commit: c3ef732ee82f6c0d56e89e421da0efc4fbea2c17
workflow-type: tm+mt
source-wordcount: '643'
ht-degree: 36%

---

# Marketo Measure Ultimate の宛先 {#mmu-destination}

## 概要 {#overview}

Marketo Measure（旧称 Bizible）を使用すると、マーケターは、会社の収益を増やし投資回収率を最大限に高めるのに最も効果的なマーケティング活動に関するインサイトを得ることができます。 Marketo Measureは、チャネルパフォーマンスを自動的に追跡およびレポートするマーケティングアトリビューションソリューションです。最も顧客エンゲージメントを促進しているチャネルを可視化し、それに応じてマーケティング費用を最適化できます。

宛先により、Adobe Experience PlatformからMarketo Measureへの B2B データフローが可能になります。 このカードを使用できるのは、Marketo Measure Ultimate のお客様のみです。

## ユースケース {#use-cases}

Marketo Measureの宛先を使用する方法とタイミングをより深く理解するために、Adobe Experience Platformのお客様がこの宛先を使用して解決できるサンプルユースケースを以下に示します。 この統合：

* 大企業の複雑なデータおよびパフォーマンス・レポートの要件を満たす
* 複数の CRM およびマーケティング自動化システムによる B2B アトリビューションレポートを有効にします。
* サードパーティのオフラインのタッチポイントデータを取り込みやすくなりました。

## 前提条件 {#prerequisites}

Marketo Measureの宛先については、次の前提条件に注意してください。

* Experience Platformサンドボックスマッピングは、管理者がMarketo Measure設定ページで行う必要があります。 サンドボックスマッピングがないと、宛先に接続してデータを保存してアクティブ化するワークフローを完了できません。
* B2B XDM クラスのデータセットのみを書き出すことができます（例えば、XDM Business Account クラスや XDM Business Opportunity クラスを参照）。 指定されたデータソースに、同じ B2B XDM クラスの複数のデータセットを取り込むことはできません。
* 各データセットは、Marketo Measure宛先への 1 つのデータフローにのみ含めることができます。

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL データセットの書き出]** | オーディエンスの関心や選定別にグループ化または構造化されていない未加工のデータセットを書き出しています。 詳しくは、[ データセット書き出し ](/help/destinations/destination-types.md#dataset-export-destinations) を参照してください。 |
| 書き出し頻度 | **[!UICONTROL バッチ]** | このバッチ宛先では、ファイルを 2 時間ごとにMarketo Measure プラットフォームに書き出します。 詳しくは、[ データセット書き出しのスケジュール設定 ](/help/destinations/ui/export-datasets.md#scheduling) を参照してください。 |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL 宛先の表示]** および **[!UICONTROL データセット宛先の管理とアクティブ化]**&#x200B;[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下のセクションにリストされているフィールドに入力します。

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

* **[!UICONTROL 名前]**：今後この宛先を認識するための名前。
* **[!UICONTROL 説明]**：今後この宛先を識別するのに役立つ説明。

![Marketo Measure宛先の宛先に接続ワークフロー ](/help/destinations/assets/catalog/adobe/marketo-measure-ultimate/marketo-measure-connect-to-destination.png)

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続への詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先へのデータセットの書き出し {#export-datasets}

>[!IMPORTANT]
> 
>データをアクティブ化するには、**[!UICONTROL 宛先の表示]** および **[!UICONTROL データセット宛先の管理とアクティブ化]**&#x200B;[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

この宛先にデータセットを書き出す詳細な手順については、[ データセットの書き出し ](/help/destinations/ui/export-datasets.md) チュートリアルをお読みください。

## データの書き出しを検証する {#exported-data}

データセットの書き出しが正常に行われたことを検証するには、[Snowflakeデータウェアハウス ](https://experienceleague.adobe.com/docs/marketo-measure/using/marketo-measure-data-warehouse/data-warehouse-access-reader-account.html) に対してデータセットが正常に送信されたことを確認します。

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのように データガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。
