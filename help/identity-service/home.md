---
keywords: Experience Platform;ホーム;人気のトピック;ID;ID;XDM グラフ;ID サービス;ID サービス
solution: Experience Platform
title: ID サービスの概要
description: Adobe Experience Platform ID サービスを利用すると、デバイスやシステム間で ID を橋渡しすることで、顧客とその行動をよりよく把握できます。これによって、インパクトのある個人的なデジタルエクスペリエンスをリアルタイムで提供できます。
exl-id: a22dc3f0-3b7d-4060-af3f-fe4963b45f18
source-git-commit: 16e49628df73d5ce97ef890dbc0a6f2c8e7de346
workflow-type: tm+mt
source-wordcount: '1560'
ht-degree: 9%

---

# Adobe Experience Platform ID サービス

関連するデジタルエクスペリエンスを提供するには、顧客ベースを構成する実際のエンティティを包括的で正確に表現する必要があります。

組織や企業は、現在、大量の異なるデータセットに直面しています。個々の顧客は、様々な識別子で表されます。 顧客は、様々な Web ブラウザー (Safari、Google Chrome)、ハードウェアデバイス（携帯電話、ノートパソコン）およびその他の個人 ID（CRM ID、電子メールアカウント）にリンクできます。 これにより、顧客の不統一な表示が作成されます。

Adobe Experience Platform Identity Service とその機能を使用して、これらの課題を解決できます。

* を生成 **ID グラフ** は異なる id を相互にリンクするので、顧客が様々なチャネルをまたいでブランドとどのようにやり取りするかを視覚的に示します。
* リアルタイム顧客プロファイルのグラフを作成し、属性と行動を結合して、顧客の包括的なビューを作成するために使用します。
* 様々なツールを使用して検証およびデバッグを実行します。

このドキュメントでは、ID サービスの概要と、Experience Platformのコンテキスト内で ID サービスの機能を使用する方法を説明します。

## 用語 {#terminology}

ID サービスの詳細を確認する前に、次の表を読んで、主な用語の概要を確認してください。

| 用語 | 定義 |
| --- | --- |
| ID | ID は、エンティティに固有のデータです。 通常、これは個人、ハードウェアデバイス、Web ブラウザー（Cookie で表される）など、実際のオブジェクトです。 完全修飾 ID は、次の 2 つの要素で構成されます。 **ID 名前空間** および **ID 値**. |
| ID 名前空間 | ID 名前空間は、特定の ID のコンテキストです。例えば、 `Email` は、次の id 値に対応します。 **julien<span>@acme.com**. 同様に、 `Phone` は、次の id 値に対応します。 `555-555-1234`. 詳しくは、 [ID 名前空間の概要](./features/namespaces.md). |
| ID 値 | ID 値は、実際のエンティティを表す文字列で、名前空間を通じて ID サービス内に分類されます。 例えば、ID 値（文字列） **julien<span>@acme.com** は、次のように分類できます `Email` 名前空間。 |
| ID タイプ | ID タイプは、ID 名前空間のコンポーネントです。 ID タイプは、ID データが ID グラフでリンクされているかどうかを示します。 |
| リンク | リンクやリンケージは、2 つの異なる ID が同じエンティティを表すことを確立するメソッドです。 例えば、「`Email` = julien<span>@acme.com」および「`Phone` = 555-555-1234」は、両方の ID が同じエンティティを表すことを意味します。 これは、julien の E メールアドレスを使用してブランドとやり取りした顧客を示しています。<span>@acme.comと555-555-1234の電話番号は同じです。 |
| ID サービス | ID サービスは、ID をリンク（またはリンク解除）して ID グラフを維持する、Experience Platform内のサービスです。 |
| ID グラフ | ID グラフは、単一の顧客を表す ID のコレクションです。 詳しくは、 [id グラフビューアの使用](./features/identity-graph-viewer.md). |
| リアルタイム顧客プロファイル | リアルタイム顧客プロファイルは、Adobe Experience Platform内の次の機能を持つサービスです。 <ul><li>プロファイルフラグメントを結合して、ID グラフに基づいてプロファイルを作成します。</li><li>プロファイルをセグメント化して、アクティベーション用に宛先に送信できるようにします。</li></ul> |
| プロファイル | プロファイルとは、主体、組織、個人を表すものです。 プロファイルは、次の 4 つの要素で構成されます。 <ul><li>属性：属性は、名前、年齢、性別などの情報を提供します。</li><li>行動：行動は、特定のプロファイルのアクティビティに関する情報を提供します。 例えば、プロファイルの動作では、特定のプロファイルが「サンダルの検索」と「T シャツの注文」のどちらであったかを判別できます。</li><li>ID：結合されたプロファイルの場合、個人に関連付けられたすべての ID の情報が提供されます。 ID は、人物（CRMID、E メール、電話）、デバイス (IDFA、GAID)、cookie(ECID、AAID) の 3 つのカテゴリに分類できます。</li><li>オーディエンスのメンバーシップ：プロファイルが属するグループ（常連ユーザー、カリフォルニアに住むユーザーなど）</li></ul> |

{style="table-layout:auto"}

## ID サービスとは

![Platform での ID の組み合わせ](./images/identity-service-stitching.png)

B2C(B2C) というビジネス間のコンテキストでは、顧客はビジネスとやり取りし、ブランドとの関係を確立します。 一般的なお客様は、組織のデータインフラストラクチャ内の任意の数のシステムでアクティブになる場合があります。 e コマース、ロイヤリティ、ヘルプデスクシステム内の任意の顧客がアクティブになる場合があります。 同じ顧客が、任意の数の異なるデバイスで、匿名で、または認証済みの手段の両方を通じて関与する場合もあります。

次のカスタマージャーニーについて考えてみましょう。

* Julien は e コマース Web サイトにアカウントを作成し、過去にいくつかの項目を注文しました。 Julien は通常、パソコンを使用して買い物をし、アカウントにログインし、使用時間ごとに使用します。
* 一方、サイト訪問時にはタブレットを使用してサンダルを検索していました。 このセッション中は、別のデバイスを使用していたので、ログインも注文もおこないませんでした。
* この時点で、Julien のアクティビティは、次の 2 つの異なるプロファイルで表されます。
   * 最初のプロファイルは e コマースログイン ID です。 このプロファイルは、ユーザー名とパスワードの組み合わせを使用して e コマースサイトでセッションを認証する場合に使用されます。 このプロファイルは、デバイス間の識別子で識別されます。
   * 2 つ目のプロフィールはタブレットデバイスです。 このプロファイルは、Sarah がアカウントにログインせずにタブレットを使用して匿名で e コマースサイトを閲覧した後に作成されました。 このプロファイルは、Cookie 識別子によって識別されます。
* その後、Julien はタブレットセッションを再開します。 ただし、今回は自分のアカウントにログインします。 その結果、ID サービスは、Julien のタブレットデバイスのアクティビティと e コマースのログイン ID を関連付けるようになりました。
* 今後、ターゲットコンテンツは、Julien の完全なプロファイル、購入履歴、匿名の閲覧アクティビティを反映します。

>[!IMPORTANT]
>
>ID サービスを使用して ID をリンクし、異なるシステムに分散する可能性のある顧客の全体像をまとめることができます。

## ID サービスの機能

ID サービスは、ミッションを達成するために次の操作を提供します。

* 組織のニーズに合わせてカスタム名前空間を作成します。
* ID グラフの作成、更新、表示。
* データセットに基づいて ID を削除します。
* ID を削除して規制への準拠を確保します。

## ID サービスによる ID のリンク方法

2 つの ID 間のリンクは、ID 名前空間と ID 値が一致すると確立されます。

一般的なログインイベント **2 つの ID を送信** EXPERIENCE PLATFORM:

* 認証済みユーザーを表す人物識別子（CRM ID など）。
* Web ブラウザーを表すブラウザー識別子（ECID など）。

次の例をご覧ください。


* ノート PC を使用して e コマース Web サイトにユーザー名とパスワードを組み合わせてログインします。 このイベントは、ユーザーを認証済みユーザーに認定するので、ID サービスは CRM ID を認識します。
* e コマース Web サイトにアクセスするためのブラウザーの使用も、ID サービスによってイベントとして認識されます。 このイベントは、ECID を通じて ID サービスで表されます。
* ID サービスは内部で、この 2 つのイベントを次のように処理します。 `CRM_ID:ABC, ECID:123`.
   * CRM ID: ABC は、認証済みユーザーとしてユーザーを表す名前空間と値です。
   * ECID: 123 は、ノート PC での Web ブラウザーの使用状況を表す名前空間および値です。
* 次に、同じ e コマース Web サイトに同じ資格情報でログインし、ノート PC の Web ブラウザーではなく、携帯電話で Web ブラウザーを使用すると、新しい ECID が ID サービスに登録されます。
* バックグラウンドで、ID サービスはこの新しいイベントを `{CRM_ID:ABC, ECID:456}`（ここで、 CRM_ID: ABC は認証済みの顧客 ID を表し、 ECID:456 はモバイルデバイス上の Web ブラウザーを表します）。

上記のシナリオを考慮すると、ID サービスは、 `{CRM_ID:ABC, ECID:123}`、および `{CRM_ID:ABC, ECID:456}`. その結果、ID グラフでは、ユーザー ID(CRM ID) 用に 1 つ、Cookie ID(ECID) 用に 2 つの ID を「所有」します。

詳しくは、 [ID サービスによる ID のリンク方法](./features/identity-linking-logic.md).

## ID グラフ

ID グラフは異なる ID 名前空間の関係を表すマップで、顧客 ID 同士の結び付きを視覚化し、わかりやすくします。に関するチュートリアルをお読みください。 [id グラフビューアの使用](./features/identity-graph-viewer.md) を参照してください。

次のビデオは、ID と ID のグラフについての理解をサポートすることを目的としています。

>[!VIDEO](https://video.tv.adobe.com/v/27841?quality=12&learn=on)

## Experience Platformインフラストラクチャ内での ID サービスの役割について

ID サービスは、Experience Platform内で重要な役割を果たします。 これらの主要統合の一部を次に示します。

* [スキーマ](../xdm/home.md)：特定のスキーマ内で、ID としてマークされたスキーマフィールドを使用して、ID グラフを作成できます。
* [データセット](../catalog/datasets/overview.md)：データセットのリアルタイム顧客プロファイルへの取り込みが有効になっている場合、データセットが ID としてマークされた 2 つ以上のフィールドとして存在すると、ID グラフはデータセットから生成されます。
* [Web SDK](../web-sdk/home.md):Web SDK はエクスペリエンスイベントをAdobe Experience Platformに送信し、イベントに 2 つ以上の ID が存在する場合、ID サービスはグラフを生成します。
* [リアルタイム顧客プロファイル](../profile/home.md)：特定のプロファイルの属性とイベントが結合される前に、リアルタイム顧客プロファイルは ID グラフを参照できました。 詳しくは、 [ID サービスとリアルタイム顧客プロファイルの関係について](./identity-and-profile.md).
* [宛先](../destinations/home.md)：宛先は、ハッシュ化された電子メールなどの ID 名前空間に基づいて、プロファイル情報を他のシステムに送信できます。
* [セグメントの一致](../segmentation/ui/segment-match/overview.md)：セグメントの一致は、同じ ID 名前空間および ID 値を持つ 2 つの異なるサンドボックス全体で 2 つのプロファイルと一致します。
* [Privacy Service](../privacy-service/home.md)：削除リクエストに `identity`に設定すると、指定した名前空間と ID 値の組み合わせを、プライバシーのプライバシーリクエスト処理機能を使用して ID サービスからPrivacy Serviceで削除できます。

