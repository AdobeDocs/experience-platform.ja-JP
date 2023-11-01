---
title: Marketo Measure Ultimate の宛先
description: Marketo Measure Ultimate の宛先にデータを接続し、アクティブ化する方法を説明します。
last-substantial-update: 2023-03-07T00:00:00Z
exl-id: b4220841-8908-41ff-b977-dbeebfa787c8
source-git-commit: e300e57df998836a8c388511b446e90499185705
workflow-type: tm+mt
source-wordcount: '652'
ht-degree: 37%

---

# Marketo Measure Ultimate Destination {#mmu-destination}

## 概要 {#overview}

Marketo Measure（旧称 Bizible）は、マーケターに、売上高の促進と、会社の投資回収率の最大化に最も効果的なマーケティング活動に関するインサイトを提供します。 Marketo Measureは、チャネルパフォーマンスを自動的に追跡およびレポートするマーケティングアトリビューションソリューションで、どのチャネルが最も顧客エンゲージメントを促進しているかを明確に示し、それに応じてマーケティング費用を最適化できます。

宛先により、Adobe Experience PlatformからMarketo Measureへの B2B(B2B) データのフローが可能になります。 このカードは、Marketo Measure Ultimate のお客様のみご利用いただけます。

## ユースケース {#use-cases}

Marketo Measureの宛先をいつどのように使用するかを理解しやすくするために、Adobe Experience Platformのお客様がこの宛先を使用して解決できる使用例を以下に示します。 この統合：

* 大規模企業の複雑なデータとパフォーマンスのレポート要件を満たします。
* 複数の CRM およびマーケティング自動化システムで B2B 属性レポートを有効にします。
* サードパーティのオフラインタッチポイントデータを簡単に取り込むことができます。

## 前提条件 {#prerequisites}

Marketo Measureの宛先に関する次の前提条件に注意します。

* Experience Platformサンドボックスマッピングは、管理者がMarketo Measure設定ページで入力する必要があります。 サンドボックスマッピングがないと、宛先への接続にデータを保存してアクティブ化するワークフローを完了できません。
* B2B XDM クラスのデータセットのみをエクスポートできます（例えば、XDM ビジネスアカウントクラスと XDM ビジネスオポチュニティクラスを参照）。 特定のデータソースに対して、同じ B2B XDM クラスの複数のデータセットを取り込むことはできません。
* 各データセットは、Marketo Measureの宛先に対して 1 つのデータフローにのみ含めることができます。

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL データセットの書き出し]** | オーディエンスの関心や資格でグループ化または構造化されていない未加工のデータセットを書き出します。 詳細を表示： [データセットの書き出し](/help/destinations/destination-types.md#dataset-export-destinations). |
| 書き出し頻度 | **[!UICONTROL バッチ]** | このバッチ宛先では、2 時間ごとにMarketo Measureプラットフォームにファイルを書き出します。 詳細を表示： [データセットの書き出しをスケジュール](/help/destinations/ui/export-datasets.md#scheduling). |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の節で示すフィールドに入力します。

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

* **[!UICONTROL 名前]**：今後この宛先を認識するための名前。
* **[!UICONTROL 説明]**：今後この宛先を識別するのに役立つ説明。

![Marketo Measureの宛先の宛先への接続ワークフロー。](/help/destinations/assets/catalog/adobe/marketo-measure-ultimate/marketo-measure-connect-to-destination.png)

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続への詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先にデータセットを書き出す {#export-datasets}

>[!IMPORTANT]
> 
>データをアクティブ化するには、 **[!UICONTROL 宛先の管理]**, **[!UICONTROL 宛先のアクティブ化]**, **[!UICONTROL プロファイルの表示]**、および **[!UICONTROL データセットの宛先の管理とアクティブ化]** [アクセス制御権限](/help/access-control/home.md#permissions). [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

詳しくは、 [（ベータ版）データセットの書き出し](/help/destinations/ui/export-datasets.md) この宛先にデータセットを書き出す詳しい手順については、こちらのチュートリアルを参照してください。

## データの書き出しを検証する {#exported-data}

データセットの書き出しが正常におこなわれたことを検証するには、データセットが [Snowflakeデータウェアハウス](https://experienceleague.adobe.com/docs/marketo-measure/using/marketo-measure-data-warehouse/data-warehouse-access-reader-account.html).

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのように データガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。
