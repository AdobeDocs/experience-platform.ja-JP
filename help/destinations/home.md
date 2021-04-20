---
keywords: 宛先；adobe experience platform;platform;destinations overview;destinations overview;activate data;activate;
title: Destinations overview
seo-title: 宛先の概要
description: チャネル間のマーケティングキャンペーン、電子メール、ターゲットを絞った広告などの目的地に対して、Adobe Experience Platformデータをアクティブ化する方法を説明します。
seo-description: 宛先は、宛先プラットフォームとの統合が事前に構築されており、Adobe Experience Platformからのデータをシームレスにアクティベーションできます。 Adobe Experience Platformの宛先を使用すると、クロスチャネルマーケティングキャンペーン、電子メールキャンペーン、ターゲット広告など、様々な使用例に関する既知および不明なデータをアクティブ化できます。
exl-id: afd07ddc-652e-4e22-b298-feba27332462
translation-type: tm+mt
source-git-commit: 805cb72e91e6446f74cc3461d39841740eb576c7
workflow-type: tm+mt
source-wordcount: '488'
ht-degree: 35%

---

# [!DNL Destinations] 概要 {#overview}

![宛先の概要バナー](./assets/overview/destinations-overview-banner.png)

**[!DNL Destinations]** は、目的のプラットフォームと事前にビルドされた統合機能で、Adobe Experience Platformからのデータをシームレスにアクティベーションできます。宛先を使用して、クロスチャネルマーケティングキャンペーン、電子メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

## 宛先とソース{#destinations-and-sources}

プラットフォームの主な機能の1つは、ファーストパーティのデータを取り込み、ビジネスニーズに合わせてアクティブ化することです。 [ソース](../sources/home.md)を使用して、プラットフォームにデータを取り込み、プラットフォームからデータをエクスポートする宛先にデータを取り込みます。

## 宛先の手順 {#steps}

* プラットフォームで使用可能なすべての宛先の[セルフサービスカタログ](./catalog/overview.md)から選択します。
* 宛先を使用して[アクティブ化](./ui/activate-destinations.md)し、プロファイルやセグメントをマーケティング自動化プラットフォーム、デジタル広告プラットフォームなどに送信します。
* データを希望の宛先に定期的に書き出しするようにスケジュールします。

## コントロール  {#controls}

「[宛先](./ui/destinations-workspace.md)」ワークスペースのコントロールを使用すると、次のことができます。

* データをアクティブ化できる宛先プラットフォームのカタログを参照する。
* カタログ内の宛先へのデータフローを作成、編集、アクティブ化、無効化する。
* ストレージーの場所でアカウントを作成するか、宛先プラットフォームのアカウントにPlatformをリンクします。
* 宛先に対してアクティブにするセグメントを選択する。
* 電子メールマーケティングの宛先にセグメントをアクティブ化する際に書き出す[エクスペリエンスデータモデル（XDM）フィールド](../xdm/home.md)を選択します。

## 宛先のタイプとカテゴリ  {#types-and-categories}

詳しくは、「[宛先のタイプとカテゴリ](./destination-types.md)」の概要を参照してください。

## 宛先とアクセス制御{#access-controls}

プラットフォームの宛先機能は、Adobe Experience Platformアクセス制御権限と連携します。 ユーザーの権限レベルに応じて、宛先を表示、管理、アクティブ化できます。個々の権限について詳しくは、「[Adobe Experience Platform でのアクセス制御](../access-control/home.md)」を参照して、ページの下部まで下にスクロールします。

アクセス制御の詳細については、「[アクセス制御ユーザーガイド](../access-control/ui/overview.md)」を参照してください。

## [!DNL Data Governance] 宛先へのデータのアクティブ化に関する制限  {#data-governance}

データ・ガバナンスは、次を通じて、プラットフォームの宛先に適用されます。

* *宛先の作成ワークフローで選択できるマーケティ* ングアクション。
* *特定の使用状況ラベルを含むデータが、特定のマーケティングアクションを持つ宛先に対してアクティブ化されるのを制限するデータ使用* ポリシー。

[marketing actions](../data-governance/policies/overview.md)と[resolving data policy violations](../data-governance/enforcement/auto-enforcement.md)の詳細については、プラットフォームドキュメントの[!DNL Data Governance]を参照してください。

宛先を作成ワークフローでのマーケティングアクションの選択について詳しくは、プラットフォームの様々な宛先タイプに関する次のページを参照してください。

* [広告の表示先 — Google Ad Manager  ](./catalog/advertising/google-ad-manager.md)
* [広告の表示先 — Google Ads](./catalog/advertising/google-ads-destination.md)
* [広告の表示先 — Google Display &amp; Video 360  ](./catalog/advertising/google-dv360.md)
* [クラウドストレージの宛先](./catalog/cloud-storage/workflow.md)
* [電子メールマーケティングの宛先](./catalog/email-marketing/overview.md)
* [ソーシャルの宛先](./catalog/social/workflow.md)

セグメントアクティベーションワークフローでのデータポリシー違反の詳細については、[宛先へのプロファイルとセグメントのアクティブ化](./ui/activate-destinations.md#review)のレビュー手順を参照してください。
