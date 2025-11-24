---
title: Demandbase 人物接続
description: この宛先を使用してオーディエンスをアクティブ化し、Demandbase のサードパーティデータで強化することで、マーケティングや販売におけるその他のダウンストリームのユースケースに対応できます。
exl-id: 748f5518-7cc1-4d65-ab70-4a129d9e2066
source-git-commit: cc05ca282cdfd012366e3deccddcae92a29fef1c
workflow-type: tm+mt
source-wordcount: '891'
ht-degree: 24%

---

# Demandbase 人物接続 {#demandbase-people}

オーディエンスのターゲティング、パーソナライゼーションおよび抑制のために、Demandbase キャンペーン用のプロファイルをアクティブ化します。

>[!IMPORTANT]
>
>[&#x200B; アカウントオーディエンスをアクティブ化 &#x200B;](../../ui/activate-account-audiences.md) する必要がある B2B のユースケースについては、代わりに [Demandbase](demandbase.md) 宛先コネクタを使用します。

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
|---------|----------|----------|
| [!DNL Segmentation Service] | ✓ | Experience Platform [&#x200B; セグメント化サービス &#x200B;](../../../segmentation/home.md) を通じて生成されたオーディエンス。 |
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
2. Demandbase API トークン。 Demandbase でユーザーと API トークンを生成できます。 トークンを生成するには、Demandbase アカウントにログインした後、[&#x200B; マイプロファイル/API トークン &#x200B;](https://web.demandbase.com/o/ad/at) に移動します。

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL View Destinations]** および **[!UICONTROL Manage Destinations]**&#x200B;[&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

宛先に対する認証を行うには、必須フィールドに入力し、「**[!UICONTROL Connect to destination]**」を選択します。

![&#x200B; ベアラートークンの追加 &#x200B;](../../assets/catalog/advertising/demandbase-people/bearer-token.png)

* **[!UICONTROL Bearer token]**：宛先を認証するためのベアラートークンを入力します。 トークンの取得方法については、[&#x200B; 前提条件 &#x200B;](#prerequisites) を参照してください。

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

![&#x200B; 宛先接続に関する情報の追加 &#x200B;](../../assets/catalog/advertising/demandbase-people/name-and-description.png)

* **[!UICONTROL Name]**：今後この宛先を認識するための名前。
* **[!UICONTROL Description]**：今後この宛先を識別するのに役立つ説明。

これで、Demandbase People 内でオーディエンスをアクティブ化する準備が整いました。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>* データをアクティブ化するには、**[!UICONTROL View Destinations]**、**[!UICONTROL Activate Destinations]**、**[!UICONTROL View Profiles]**、**[!UICONTROL View Segments]** [&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* *ID* を書き出すには、**[!UICONTROL View Identity Graph]** [&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。<br> ![&#x200B; 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択します。](/help/destinations/assets/overview/export-identities-to-destination.png " 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択 "){width="100" zoomable="yes"}

この宛先にオーディエンスをアクティベートする手順は、[ストリーミングオーディエンスの書き出し宛先へのプロファイルとオーディエンスのアクティベート](/help/destinations/ui/activate-segment-streaming-destinations.md)を参照してください。

### 必須のマッピング {#mandatory-mappings}

[!DNL Demandbase People] の宛先に対してオーディエンスをアクティブ化する場合、マッピング手順で次の必須のフィールドマッピングを設定する必要があります。

| ソースフィールド | ターゲットフィールド | 説明 |
|--------------|--------------|-------------|
| `xdm: workEmail.address` | `Identity: email` | 人物の仕事用電子メールアドレス |

### 推奨されるマッピング {#recommended-mappings}

最適なマッチング精度を得るには、上記の [&#x200B; 必須マッピング &#x200B;](#mandatory-mappings) に加えて、アクティベーションフローに次のオプションマッピングを含めます。

| ソースフィールド | ターゲットフィールド | 説明 |
|--------------|--------------|-------------|
| `xdm: b2b.personKey.sourceKey` | `xdm: externalPersonId` | 人物の一意の ID |
| `xdm: person.name.lastName` | `xdm: lastName` | 人物の姓 |
| `xdm: person.name.firstName` | `xdm: firstName` | 人物の名 |

### マッピングのベストプラクティス {#mapping-best-practices}

フィールドを [!DNL Demandbase People] にマッピングする際は、次の一致動作を考慮してください。

* **プライマリ一致**: `externalPersonId` が存在する場合、Demandbase はこれを人物の一致のプライマリ ID として使用します。
* **フォールバックマッチング**:`externalPersonId` が利用できない場合、Demandbase は識別に `email` フィールドを使用します。
* Adobe **必須と推奨**:Demandbase で必要なのは `email` だけですが、マッチングの精度とキャンペーンのパフォーマンスを向上させるために、上記の推奨マッピング表から使用可能なすべてのフィールドをマッピングすることをお勧めします。

![Demandbase 人物マッピング &#x200B;](/help/destinations/assets/catalog/advertising/demandbase-people/demandbase-people-mapping.png)

これらのマッピングは、宛先が正しく機能するために必要であり、アクティベーションワークフローを続行する前に設定する必要があります。

## 追加のメモと重要な引き出し {#additional-notes}

* **Demandbase API ガードレール**:Demandbase にオーディエンスを書き出し、Experience Platformで書き出しが成功しても、すべてのデータが Demandbase に到達するわけではない場合、Demandbase 側で API スロットルに遭遇した可能性があります。 明確にするために彼らに連絡してください。
* **リスト削除**：人物リストは一意なので、既に使用されている名前で新しいリストを再作成することはできません。 ユーザーをリストから削除すると、そのユーザーは使用できなくなりますが、削除はされません。
* **アクティベーション時間**:Demandbase へのデータ読み込みは、一夜で処理される可能性があります。
* **オーディエンスの命名**：同じ名前の人物オーディエンスが Demandbase に対して以前にアクティブ化された場合、Demandbase の宛先に対して別のデータフローを介して再度アクティブ化することはできません。
