---
title: Demandbase 人物接続
description: この宛先を使用してオーディエンスをアクティブ化し、Demandbase のサードパーティデータで強化することで、マーケティングや販売におけるその他のダウンストリームのユースケースに対応できます。
source-git-commit: df2cb1edbf998082fca961e6d9bb567a1ad3b7e6
workflow-type: tm+mt
source-wordcount: '745'
ht-degree: 33%

---


# Demandbase 人物接続 {#demandbase-people}

オーディエンスのターゲティング、パーソナライゼーションおよび抑制のために、Demandbase キャンペーン用のプロファイルをアクティブ化します。

>[!IMPORTANT]
>
>[ アカウントオーディエンスをアクティブ化 ](../../ui/activate-account-audiences.md) する必要がある B2B のユースケースについては、代わりに [Demandbase](demandbase.md) 宛先コネクタを使用します。

## ユースケース {#use-case}

マーケターは、Adobe Real-Time CDPを使用してファーストパーティの連絡先の人物リストを作成し、Demandbase でアクティブ化して、デマンドサイドプラットフォーム（DSP）や LinkedIn などのその他のチャネル全体で最適化および調整されたエンゲージメントを行うことができます。

このアプローチを使用すると、マーケターは CRM またはマーケティング自動化システムをソースとする既知の個人に対してキャンペーン支出の優先順位を付け、マーケティング活動が高価値の見込み客に集中できるようにします。

アクティブ化すると、Demandbase は広告配信を最適化し、ターゲティング戦略を絞り込んで、エンゲージメント、リーチ、コンバージョン率を最大化し、最終的にキャンペーンの効率を向上させます。

## サポートされている ID {#supported-identities}

[!DNL Demandbase People] 接続では、以下の表で説明する ID のアクティブ化をサポートしています。 [ID](/help/identity-service/features/namespaces.md) についての詳細情報。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| メール | プレーンテキストのメールアドレス | [!DNL Demandbase People] 接続では、プレーンテキストのメールアドレスのみがサポートされます。 |

{style="table-layout:auto"}

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスオリジン | サポートあり | 説明 |
---------|----------|----------|
| [!DNL Segmentation Service] | ✓ | Experience Platform [ セグメント化サービス ](../../../segmentation/home.md) を通じて生成されたオーディエンス。 |
| カスタムアップロード | X | CSV ファイルから Experience Platform に[読み込まれた](../../../segmentation/ui/overview.md#import-audience)オーディエンス。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-and-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
|--------------|-----------|---------------------------|
| 書き出しタイプ | オーディエンスのエクスポート | *Demandbase* 宛先で使用される識別子（名前、電話番号など）を使用して、オーディエンスのすべてのメンバーを書き出します。 |
| 頻度 | ストリーミング | ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)の詳細についてはこちらを参照してください。 |

{style="table-layout:auto"}

## 前提条件 {#prerequisites}

オーディエンスを Demandbase に書き出すには、次が必要です。

1. Demandbase アカウント。
2. Demandbase API トークン。 Demandbase でユーザーと API トークンを生成できます。 トークンを生成するには、Demandbase アカウントにログインした後、[ マイプロファイル/API トークン ](https://web.demandbase.com/o/ad/at) に移動します。

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL 宛先の表示]** および **[!UICONTROL 宛先の管理]**[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

宛先に対して認証するには、必須フィールドに入力し、「**[!UICONTROL 宛先に接続]**」を選択します。

![ ベアラートークンの追加 ](../../assets/catalog/advertising/demandbase-people/bearer-token.png)

* **[!UICONTROL ベアラートークン]**：宛先を認証するためのベアラートークンを入力します。 トークンの取得方法については、[ 前提条件 ](#prerequisites) を参照してください。

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

![ 宛先接続に関する情報の追加 ](../../assets/catalog/advertising/demandbase-people/name-and-description.png)

* **[!UICONTROL 名前]**：今後この宛先を認識するための名前。
* **[!UICONTROL 説明]**：今後この宛先を識別するのに役立つ説明。

これで、Demandbase People 内でオーディエンスをアクティブ化する準備が整いました。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>* データをアクティブ化するには、**[!UICONTROL 宛先の表示]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]** および **[!UICONTROL セグメントの表示]**[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* *ID* を書き出すには、**[!UICONTROL ID グラフの表示]**[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。<br> ![ 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択します。](/help/destinations/assets/overview/export-identities-to-destination.png " 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択 "){width="100" zoomable="yes"}

この宛先にオーディエンスをアクティベートする手順は、[ストリーミングオーディエンスの書き出し宛先へのプロファイルとオーディエンスのアクティベート](/help/destinations/ui/activate-segment-streaming-destinations.md)を参照してください。

## 追加のメモと重要な引き出し {#additional-notes}

* **Demandbase API ガードレール**:Demandbase にオーディエンスを書き出し、Experience Platformで書き出しが成功しても、すべてのデータが Demandbase に到達するわけではない場合、Demandbase 側で API スロットルに遭遇した可能性があります。 明確にするために彼らに連絡してください。
* **リスト削除**：人物リストは一意なので、既に使用されている名前で新しいリストを再作成することはできません。 ユーザーをリストから削除すると、そのユーザーは使用できなくなりますが、削除はされません。
* **アクティベーション時間**:Demandbase へのデータ読み込みは、一夜で処理される可能性があります。
* **オーディエンスの命名**：同じ名前のアカウントオーディエンスが Demandbase に対して以前にアクティブ化された場合、Demandbase の宛先に対して別のデータフローを介して再度アクティブ化することはできません。
