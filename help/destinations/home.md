---
title: 宛先の概要
description: 宛先は、Adobe Experience Platform からのデータの円滑なアクティベーションを可能にする、宛先プラットフォームとの事前定義済みの統合です。Adobe Experience Platform の宛先を使用して、クロスチャネルマーケティングキャンペーン、メールキャンペーン、ターゲット広告、その他多くのユースケースに関する既知および未知のデータをアクティブ化できます。
exl-id: afd07ddc-652e-4e22-b298-feba27332462
source-git-commit: 661ef040398a9e2ef8dd9cebdf7bd27d4268636b
workflow-type: tm+mt
source-wordcount: '956'
ht-degree: 96%

---

# [!DNL Destinations] の概要 {#overview}

![宛先の概要バナー](./assets/overview/destinations-overview-banner.png)

**[!DNL Destinations]** は、Adobe Experience Platform からのデータの円滑なアクティベーションを可能にする、事前定義済みの出力先プラットフォームとの統合です。宛先を使用して、クロスチャネルマーケティングキャンペーン、メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

<div id="recs-overview-body-1"></div>
<div id="recs-overview-body-2"></div>
<div id="recs-overview-body-3"></div>
<div id="recs-overview-body-4"></div>
<div id="recs-overview-body-5"></div>
<div id="recs-overview-body-6"></div>

## 宛先とソース {#destinations-and-sources}

Platform の主な機能の 1 つは、ファーストパーティデータを取り込み、ビジネスニーズに合わせてアクティブ化することです。[ソース](../sources/home.md)を使用してデータを Platform に取り込み、宛先を使用して Platform からデータを書き出します。

## 宛先の手順 {#steps}

* Platform で使用可能なすべての宛先の[セルフサービスカタログ](./catalog/overview.md)から選択します。
* 宛先を使用して、プロファイルやオーディエンスをマーケティング自動化プラットフォームやデジタル広告プラットフォームなどに送信します。
* データを希望の宛先に定期的に書き出しするようにスケジュールします。

## コントロール {#controls}

[宛先ワークスペース](./ui/destinations-workspace.md)のコントロールを使用すると、次のことができます。

* データをアクティブ化できる宛先プラットフォームのカタログを参照する。
* カタログ内の宛先へのデータフローを作成、編集、アクティブ化、無効化する。
* ストレージの場所にアカウントを作成するか、Platform を宛先プラットフォームのアカウントにリンクする。
* 宛先に対してアクティブにするオーディエンスを選択します。
* メールマーケティングの宛先にオーディエンスをアクティブ化する際に書き出す[エクスペリエンスデータモデル（XDM）フィールド](../xdm/home.md)を選択します。

## 宛先のタイプとカテゴリ {#types-and-categories}

Experience Platform を使用すると、様々なタイプの宛先に対してデータをアクティブ化して、アクティブ化のユースケースを満たすことができます。宛先は、API ベースの統合、ファイル受信システムとの統合、プロファイル参照の宛先など、様々です。 使用可能なすべての宛先について詳しくは、[宛先のタイプとカテゴリの概要](./destination-types.md)を参照してください。

## アドビが作成した宛先とパートナーが作成した宛先 {#adobe-and-partner-built-destinations}

Experience Platform 宛先カタログのコネクタには、アドビによって作成および管理されているものと、[Destination SDK](/help/destinations/destination-sdk/overview.md) を使用してパートナー企業によって作成および管理されているものがあります。宛先がパートナーによって作成および管理されている場合は、各パートナーが作成したコネクタに関するドキュメントページの上部にあるメモが呼び出されます。例えば、[Amazon S3 コネクタ](/help/destinations/catalog/cloud-storage/amazon-s3.md) はアドビが作成し、[TikTok コネクタ](/help/destinations/catalog/social/tiktok.md) は、TikTok チームが作成および管理しています。

パートナーが作成および管理するコネクタの場合、コネクタに関する問題をパートナーチームが解決する必要が生じる場合があります（ドキュメントページのメモに記載されている連絡先方法）。アドビが作成および管理するコネクタに関する問題については、アドビ担当者またはカスタマーケア担当者にお問い合わせください。

## 宛先とアクセス制御 {#access-controls}

Platform の宛先機能は、Adobe Experience Platform のアクセス制御権限と連携します。ユーザーの権限レベルに応じて、宛先を表示、管理、アクティブ化できます。個々の権限について詳しくは、「[Adobe Experience Platform でのアクセス制御](../access-control/home.md)」を参照して、ページの下部まで下にスクロールします。

宛先で特定のアクションを実行するために必要な権限と、権限の組み合わせをまとめたものを以下の表に示します。

| 権限レベル | 説明 |
| ---- | ---- |
| **[!UICONTROL 宛先の管理]** | 宛先に接続するには、**[!UICONTROL 宛先の管理]**[アクセス制御権限](/help/access-control/home.md#permissions)が必要です。 |
| **[!UICONTROL 宛先の管理]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]**&#x200B;および&#x200B;**[!UICONTROL セグメントの表示]** | 宛先に対してオーディエンスをアクティブ化し、ワークフローの[マッピングステップ](ui/activate-batch-profile-destinations.md#mapping)を有効にするには、**[!UICONTROL 宛先の管理]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]**&#x200B;および&#x200B;**[!UICONTROL セグメントの表示]**&#x200B;の各[アクセス制御権限](/help/access-control/home.md#permissions)が必要です。 |
| **[!UICONTROL 宛先の管理]**、**[!UICONTROL マッピングを使用しないセグメントのアクティブ化]**、**[!UICONTROL プロファイルの表示]**&#x200B;および&#x200B;**[!UICONTROL セグメントの表示]** | 宛先に対してオーディエンスをアクティブ化し、ワークフローの[マッピングステップ](ui/activate-batch-profile-destinations.md#mapping)を非表示にするには、**[!UICONTROL 宛先の管理]**、**[!UICONTROL マッピングを使用しないセグメントのアクティブ化]**、**[!UICONTROL プロファイルの表示]**&#x200B;および&#x200B;**[!UICONTROL セグメントの表示]**&#x200B;の各[アクセス制御権限](/help/access-control/home.md#permissions)が必要です。 |
| **[!UICONTROL ID グラフの表示]** | 書き出す *id* 宛先に対して、 **[!UICONTROL ID グラフを表示]** [アクセス制御権限](/help/access-control/home.md#permissions). <br> ![ワークフローでハイライト表示された ID 名前空間を選択して、宛先に対するオーディエンスをアクティブ化します。](/help/destinations/assets/overview/export-identities-to-destination.png "ワークフローでハイライト表示された ID 名前空間を選択して、宛先に対するオーディエンスをアクティブ化します。"){width="100" zoomable="yes"} |

{style="table-layout:auto"}

アクセス制御の詳細については、[アクセス制御ユーザーガイド](../access-control/ui/overview.md)を参照してください。

### 宛先の属性ベースのアクセス制御 {#attribute-based-access}

Adobe Experience Platform での属性ベースのアクセス制御では、管理者が属性に基づいて特定のオブジェクトや機能へのアクセスを制御できます。

属性ベースのアクセス制御を使用すると、権限を持つフィールドにマッピング設定を適用できます。さらに、データセット内のすべてのフィールドにアクセスできない場合は、宛先にデータを書き出すことはできません。

宛先が属性ベースのアクセス制御と連携する方法について詳しくは、[属性ベースのアクセス制御の概要](../access-control/abac/overview.md#destinations)を参照してください。

## 宛先の監視 {#destinations-monitoring}

宛先への接続を確立し、アクティベーションワークフローを完了した後、受信システムへのデータの書き出しを監視できます。詳しくは、[UI での宛先へのデータフローの監視に関するガイド](/help/dataflows/ui/monitor-destinations.md)を参照してください。

また、データが正常に宛先に送信されたかどうかを検証することもできます。 カタログ内のほとんどの宛先ドキュメントページには、*データ書き出しの検証*&#x200B;セクションがあり、Experience Platform からデータが正常に取り込まれていることを宛先プラットフォームで確認する方法を示しています。

## 宛先へのデータのアクティブ化に関するデータガバナンス制限 {#data-governance}

データガバナンスは、次を通じて Platform の宛先に適用されます。

* 宛先の作成ワークフローで選択できる&#x200B;*マーケティングアクション*。
* 特定の使用ラベルを含むデータが、特定のマーケティングアクションを持つ宛先に対してアクティブ化されることを制限する&#x200B;*データ使用ポリシー*。

[マーケティングアクション](../data-governance/policies/overview.md)と[データポリシー違反の解決](../data-governance/enforcement/auto-enforcement.md)について詳しくは、Platform のドキュメントのデータガバナンスを参照してください。

宛先の作成ワークフローでマーケティングアクションを選択する方法について詳しくは、Platform の様々な宛先タイプに関する次のページを参照してください。

* [広告の宛先 - Google アド マネージャー](./catalog/advertising/google-ad-manager.md)
* [広告の宛先 - Google 広告](./catalog/advertising/google-ads-destination.md)
* [広告の宛先 - Google ディスプレイ＆ビデオ 360](./catalog/advertising/google-dv360.md)
* [クラウドストレージの宛先](./catalog/cloud-storage/overview.md)
* [メールマーケティングの宛先](./catalog/email-marketing/overview.md)
* [ソーシャルの宛先](./catalog/social/overview.md)

オーディエンスのアクティベーションワークフローにおけるデータポリシー違反について詳しくは、次のガイドの&#x200B;**[!UICONTROL レビュー]**&#x200B;手順を参照してください。

* [ストリーミングオーディエンス書き出し宛先に対するオーディエンスデータの有効化](./ui/activate-segment-streaming-destinations.md#review)
* [ストリーミングプロファイル書き出し宛先に対するオーディエンスデータの有効化](./ui/activate-streaming-profile-destinations.md#review)
* [プロファイル書き出しのバッチ宛先に対するオーディエンスデータの有効化](./ui/activate-batch-profile-destinations.md#review)
