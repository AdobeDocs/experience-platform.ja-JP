---
keywords: 宛先；adobe experience platform；プラットフォーム；宛先の概要；データのアクティブ化；アクティブ化；
title: Destinations overview
seo-title: 宛先の概要
description: クロスチャネルマーケティングキャンペーン、電子メール、ターゲット広告などの宛先に対してAdobe Experience Platformデータをアクティブ化する方法を説明します。
seo-description: 宛先は、Adobe Experience Platformからのデータのシームレスなアクティブ化を可能にする、宛先プラットフォームとの事前定義済みの統合です。 Adobe Experience Platformの宛先を使用して、クロスチャネルマーケティングキャンペーン、電子メールキャンペーン、ターゲット広告、その他多くの使用例に関する既知および不明なデータをアクティブ化できます。
exl-id: afd07ddc-652e-4e22-b298-feba27332462
source-git-commit: b7392596c7ed96032dc8ad6bb8e423640f562394
workflow-type: tm+mt
source-wordcount: '507'
ht-degree: 37%

---

# [!DNL Destinations] の概要 {#overview}

![宛先の概要バナー](./assets/overview/destinations-overview-banner.png)

**[!DNL Destinations]** は、Adobe Experience Platform からのデータの円滑なアクティベーションを可能にする、事前定義済みの出力先プラットフォームとの統合です。宛先を使用して、クロスチャネルマーケティングキャンペーン、電子メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

## 宛先とソース {#destinations-and-sources}

Platformの主な機能の1つは、ファーストパーティデータを取り込み、ビジネスニーズに合わせてアクティブ化することです。 [sources](../sources/home.md)を使用して、データをPlatformおよび宛先に取り込み、Platformからデータを書き出します。

## 宛先の手順 {#steps}

* Platformで使用可能なすべての宛先の[セルフサービスカタログ](./catalog/overview.md)から選択します。
* 宛先を使用して、プロファイルやセグメントをマーケティング自動化プラットフォームやデジタル広告プラットフォームに送信します。
* データを希望の宛先に定期的に書き出しするようにスケジュールします。

## コントロール {#controls}

「[宛先](./ui/destinations-workspace.md)」ワークスペースのコントロールを使用すると、次のことができます。

* データをアクティブ化できる宛先プラットフォームのカタログを参照する。
* カタログ内の宛先へのデータフローを作成、編集、アクティブ化、無効化する。
* ストレージの場所にアカウントを作成するか、Platformを宛先プラットフォームのアカウントにリンクする。
* 宛先に対してアクティブにするセグメントを選択する。
* 電子メールマーケティングの宛先にセグメントをアクティブ化する際に書き出す[エクスペリエンスデータモデル（XDM）フィールド](../xdm/home.md)を選択します。

## 宛先のタイプとカテゴリ {#types-and-categories}

詳しくは、「[宛先のタイプとカテゴリ](./destination-types.md)」の概要を参照してください。

## 宛先とアクセス制御 {#access-controls}

Platformの宛先機能は、Adobe Experience Platformのアクセス制御権限と連携します。 ユーザーの権限レベルに応じて、宛先を表示、管理、アクティブ化できます。個々の権限について詳しくは、「[Adobe Experience Platform でのアクセス制御](../access-control/home.md)」を参照して、ページの下部まで下にスクロールします。

アクセス制御の詳細については、「[アクセス制御ユーザーガイド](../access-control/ui/overview.md)」を参照してください。

## [!DNL Data Governance] 宛先へのデータのアクティブ化の制限 {#data-governance}

データガバナンスは、次の方法でPlatformの宛先に適用されます。

* *宛先* の作成ワークフローで選択できるマーケティングアクション。
* *特定の使用ラベルを含むデータを制限するデータ使用ポリシー* は、特定のマーケティングアクションを持つ宛先に対して、アクティブ化されません。

[マーケティングアクション](../data-governance/policies/overview.md)と[データポリシー違反の解決](../data-governance/enforcement/auto-enforcement.md)について詳しくは、Platformドキュメントの[!DNL Data Governance]を参照してください。

宛先の作成ワークフローでのマーケティングアクションの選択について詳しくは、Platformの様々な宛先タイプに関する次のページを参照してください。

* [広告の宛先 — Google Ad Manager ](./catalog/advertising/google-ad-manager.md)
* [広告の宛先 — Google Ads](./catalog/advertising/google-ads-destination.md)
* [広告の宛先 — Google Display &amp; Video 360 ](./catalog/advertising/google-dv360.md)
* [クラウドストレージの宛先](./catalog/cloud-storage/overview.md)
* [電子メールマーケティングの宛先](./catalog/email-marketing/overview.md)
* [ソーシャルの宛先](./catalog/social/overview.md)

セグメントのアクティベーションワークフローでのデータポリシー違反について詳しくは、次のガイドの「確認」の手順を参照してください。

* [ストリーミングセグメントの書き出し先に対するオーディエンスデータのアクティブ化](./ui/activate-segment-streaming-destinations.md#review)
* [ストリーミングプロファイルの書き出し先に対するオーディエンスデータのアクティブ化](./ui/activate-streaming-profile-destinations.md#review)
* [プロファイルの一括書き出し先へのオーディエンスデータのアクティブ化](./ui/activate-batch-profile-destinations.md#review)
