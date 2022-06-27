---
keywords: 宛先；adobe experience platform；プラットフォーム；宛先の概要；データのアクティブ化；アクティブ化；
title: 宛先の概要
description: 宛先は、Adobe Experience Platform からのデータの円滑なアクティベーションを可能にする、宛先プラットフォームとの事前定義済みの統合です。Adobe Experience Platformの宛先を使用して、クロスチャネルマーケティングキャンペーン、電子メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。
exl-id: afd07ddc-652e-4e22-b298-feba27332462
source-git-commit: ad0d38cbd249642d582a807c5679065827f57717
workflow-type: tm+mt
source-wordcount: '489'
ht-degree: 47%

---

# [!DNL Destinations] の概要 {#overview}

![宛先の概要バナー](./assets/overview/destinations-overview-banner.png)

**[!DNL Destinations]** は、Adobe Experience Platform からのデータの円滑なアクティベーションを可能にする、事前定義済みの出力先プラットフォームとの統合です。宛先を使用して、クロスチャネルマーケティングキャンペーン、電子メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

## 宛先とソース {#destinations-and-sources}

Platform の主な機能の 1 つは、ファーストパーティデータを取り込み、ビジネスニーズに合わせてアクティブ化することです。 用途 [ソース](../sources/home.md) を使用して、Platform と宛先にデータを取り込み、Platform からデータを書き出します。

## 宛先の手順 {#steps}

* 次から選択： [セルフサービスカタログ](./catalog/overview.md) を含む ) を含める必要があります。
* 宛先を使用して、プロファイルやセグメントをマーケティング自動化プラットフォームやデジタル広告プラットフォームに送信します。
* データを希望の宛先に定期的に書き出しするようにスケジュールします。

## コントロール {#controls}

「[宛先](./ui/destinations-workspace.md)」ワークスペースのコントロールを使用すると、次のことができます。

* データをアクティブ化できる宛先プラットフォームのカタログを参照する。
* カタログ内の宛先へのデータフローを作成、編集、アクティブ化、無効化する。
* ストレージの場所にアカウントを作成するか、Platform を宛先プラットフォームのアカウントにリンクする。
* 宛先に対してアクティブにするセグメントを選択する。
* 電子メールマーケティングの宛先にセグメントをアクティブ化する際に書き出す[エクスペリエンスデータモデル（XDM）フィールド](../xdm/home.md)を選択します。

## 宛先のタイプとカテゴリ {#types-and-categories}

詳しくは、「[宛先のタイプとカテゴリ](./destination-types.md)」の概要を参照してください。

## 宛先とアクセス制御 {#access-controls}

Platform の宛先機能は、Adobe Experience Platformのアクセス制御権限と連携します。 ユーザーの権限レベルに応じて、宛先を表示、管理、アクティブ化できます。個々の権限について詳しくは、「[Adobe Experience Platform でのアクセス制御](../access-control/home.md)」を参照して、ページの下部まで下にスクロールします。

アクセス制御の詳細については、「[アクセス制御ユーザーガイド](../access-control/ui/overview.md)」を参照してください。

## 宛先へのデータのアクティブ化に関するデータガバナンス制限 {#data-governance}

データガバナンスは、次の方法で Platform の宛先に適用されます。

* *マーケティングアクション* 宛先の作成ワークフローで選択できるもの。
* *データ使用ポリシー* 特定の使用ラベルを含むデータを、特定のマーケティングアクションを持つ宛先に対してアクティブ化しないように制限する機能を追加しました。

詳しくは、Platform のドキュメントの「データガバナンス」を参照してください。 [マーケティングアクション](../data-governance/policies/overview.md) および [データポリシー違反の解決](../data-governance/enforcement/auto-enforcement.md).

宛先の作成ワークフローでマーケティングアクションを選択する方法について詳しくは、Platform の様々な宛先タイプについて、次のページを参照してください。

* [広告の宛先 — Google Ad Manager ](./catalog/advertising/google-ad-manager.md)
* [広告の宛先 — Google Ads](./catalog/advertising/google-ads-destination.md)
* [広告の宛先 — Google Display &amp; Video 360 ](./catalog/advertising/google-dv360.md)
* [クラウドストレージの宛先](./catalog/cloud-storage/overview.md)
* [電子メールマーケティングの宛先](./catalog/email-marketing/overview.md)
* [ソーシャルの宛先 ](./catalog/social/overview.md)

セグメントのアクティベーションワークフローでのデータポリシー違反について詳しくは、次のガイドの「確認」の手順を参照してください。

* [ストリーミングセグメント書き出し宛先に対するオーディエンスデータの有効化](./ui/activate-segment-streaming-destinations.md#review)
* [ストリーミングプロファイル書き出し宛先に対するオーディエンスデータの有効化](./ui/activate-streaming-profile-destinations.md#review)
* [プロファイル書き出しのバッチ宛先に対するオーディエンスデータの有効化](./ui/activate-batch-profile-destinations.md#review)
