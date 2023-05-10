---
title: ID サービスでの削除
description: このドキュメントでは、Experience Platform での ID データの削除や、ID グラフがどのような影響を受ける可能性があるかを明確にするために使用できる様々なメカニズムの概要を説明します。
exl-id: 0619d845-71c1-4699-82aa-c6436815d5b3
source-git-commit: 05a7b73da610a30119b4719ae6b6d85f93cdc2ae
workflow-type: tm+mt
source-wordcount: '1198'
ht-degree: 100%

---

# ID サービスでの削除

Adobe Experience Platform ID サービスでは、個々のユーザーのすべてのデバイスやシステムにわたって ID を確定的にリンクすることで ID グラフを生成します。ID グラフのリンクは、マークされた 2 つ以上の ID がデータの同じ行内で受信されると確立されます。

リアルタイム顧客プロファイルで ID グラフを活用して、顧客の属性や行動の包括的な単一ビューを作成することで、インパクトの強い個人的なデジタルエクスペリエンスをデバイスではなく人物にリアルタイムで提供できるようになります。

このドキュメントでは、Experience Platform での ID データの削除や、ID グラフがどのような影響を受ける可能性があるかを明確にするために使用できる様々なメカニズムの概要を説明します。

## はじめに

以下のドキュメントでは、Experience Platform の次の機能について説明しています。

* [ID サービス](home.md)：デバイスやシステム間で ID を橋渡しすることで、個々の顧客とその行動をより確実に把握することができます。
   * [ID グラフ](./ui/identity-graph-viewer.md)：ID グラフは、特定の顧客の異なる ID 間の関係のマップで、顧客が様々なチャネルでブランドとどのようにやり取りするかを視覚的に表現します。
   * [ID 名前空間](namespaces.md)：ID 名前空間は ID サービスのコンポーネントで、ID の関連先コンテキストのインジケーターとして機能します。例えば、「name<span>@email.com」の値をメールアドレスとして識別したり、「443522」を数値 CRM ID として識別したりします。
* [カタログサービス](../catalog/home.md)：データレイク内のデータ系列、メタデータ、ファイル記述、ディレクトリおよびデータセットを調べます。
* [データハイジーン](../hygiene/home.md)：データセットの自動的な有効期限切れをスケジュールするか、1 つのデータセットまたはすべてのデータセットから個々のレコードを削除することで、保存されている消費者データを管理します。
* [Adobe Experience Platform Privacy Service](../privacy-service/home.md)：すべての Adobe Experience Cloud アプリケーションにわたって、顧客自身の個人データのアクセス、販売のオプトアウトまたは削除を求める顧客リクエストを管理します。
* [リアルタイム顧客プロファイル](../profile/home.md)：複数のソースから集約されたデータに基づいて、統合された顧客プロファイルをリアルタイムに提供します。

## 単一 ID の削除

単一 ID の削除リクエストでは、グラフ内の ID を削除できるので、ID 名前空間に関連付けられている単一のユーザー ID に結びついているリンクが削除されます。[Privacy Service](../privacy-service/home.md) から提供されるメカニズムは、顧客からのデータ削除リクエストや、EU 一般データ保護規則（GDPR）などのプライバシー規制への準拠といったユースケースに使用することができます。

以下の節では、Experience Platform で単一の ID 削除リクエストに使用できるメカニズムの概要を説明します。

### Privacy Service での単一 ID 削除

 Privacy Service では、EU 一般データ保護規則（GDPR）やカリフォルニア州消費者プライバシー法（CCPA）などのプライバシー規制で規定されている個人データのアクセス、販売のオプトアウトまたは削除を求める顧客のリクエストを処理します。Privacy Service を使用すると、API または UI を使用してジョブリクエストを送信できます。Experience Platform が Privacy Service から削除リクエストを受信すると、リクエストが受信され、影響を受けるデータに削除マークが付けられた旨の確認を Platform が Privacy Service に送信します。個々 ID の削除は、指定した名前空間や ID 値に基づいて行われます。 さらに、指定した組織に関連付けられているすべてのサンドボックスに対して削除が行われます。詳しくは、[ID サービスでのプライバシーリクエストの処理](privacy.md)に関するガイドを参照してください。

Privacy Service での単一 ID 削除の分類を次の表に示します。

| 単一 ID 削除 | Privacy Service |
| --- | --- |
| 許可されているユースケース | データプライバシーリクエスト（GDPR、CCPA）のみ。 |
| 推定待ち時間 | 数日から数週間 |
| 影響を受けるサービス | Privacy Service での単一 ID 削除では、データを ID サービス、リアルタイム顧客プロファイル、データレイクのどれから削除するかを選択できます。 |
| 削除パターン | ID サービスから ID を削除します。 |

{style="table-layout:auto"}

## データセットの削除

以下の節では、Experience Platform 内のデータセットおよび関連する ID リンクの削除に使用できるメカニズムの概要を説明します。

### カタログサービスでのデータセットの削除

カタログサービスを使用すると、データセット削除のリクエストを送信できます。カタログサービスを使用してデータセットを削除する方法について詳しくは、[カタログサービス API を使用したオブジェクトの削除](../catalog/api/delete-object.md)に関するガイドを参照してください。また、Platform UI を使用して、データセット削除のリクエストを送信することもできます。 詳しくは、[データセットユーザーガイド](../catalog/datasets/user-guide.md#delete-a-dataset)を参照してください。

### データハイジーンにおけるデータセットの有効期限

Adobe Experience Platform UI の[[!UICONTROL データハイジーン]ワークスペース](../hygiene/ui/overview.md)では、データセットの有効期限をスケジュールできます。データセットが有効期限に達すると、データレイク、ID サービスおよびリアルタイム顧客プロファイルは別個のプロセスを開始して、それぞれのサービスからデータセットの内容を削除します。詳しくは、[[!UICONTROL データハイジーン]ワークスペースを使用したデータセットの有効期限の管理](../hygiene/ui/dataset-expiration.md)に関するガイドを参照してください。

カタログサービスとデータハイジーンにおけるデータセット削除の違いの分類を次の表に示します。

| データセットの削除 | カタログサービス | データハイジーン |
| --- | --- | --- |
| 許可されているユースケース | Platform における完全なデータセットとそれらに関連する ID 情報を削除します。 | Experience Platform に保存されているデータの管理。 |
| 推定待ち時間 | 数日 | 数日 |
| 影響を受けるサービス | カタログサービスによるデータセット削除では、ID サービス、リアルタイム顧客プロファイルおよびデータレイクからデータが削除されます。 | データハイジーンによるデータセット削除では、ID サービス、リアルタイム顧客プロファイルおよびデータ レイクからデータが削除されます。 |
| 削除パターン | 特定のデータセットによって確立されたリンク済み ID を ID サービスから削除します。 | 有効期限スケジュールに基づいて、特定のデータセットによって確立されたリンク済み ID を ID サービスから削除します。 |

{style="table-layout:auto"}

## 削除後の ID グラフの様々な状態

すべての ID グラフ削除の結果、削除リクエストで指定しているとおりに、2 つ以上の ID 間のリンクが削除されます。データセット削除リクエストの場合、指定したデータセットによって確立されたすべての ID リンクが削除され、ID はグラフから削除される場合と削除されない場合があります。単一 ID 削除リクエストの場合、指定した ID の ID リンクが削除されるので、ID 値そのものがすべての ID グラフから削除されます。別の ID へのリンクが 1 つもない ID は、ID サービスには保存されません。

削除が ID グラフの状態に与える可能性のある影響の概要を以下に示します。

| ID グラフの状態 | 説明 |
| --- | --- |
| 部分更新 | グラフの部分更新は、削除リクエストが正常に処理された後、少なくとも 2 つの ID がグラフ内でリンクされたままになっている場合に行われます。削除後、残りの ID リンクは相互に接続されたままになるか、削除された ID によっては 2 つ以上の別個のグラフに分割される可能性があります。 |
| 完全削除 | グラフが存在するには、リンクされた ID が少なくとも 2 つ必要です。したがって、削除リクエストの結果、グラフ内の既存のリンクがすべて削除される場合、グラフは完全に削除されます。 |
| 変更なし | グラフのどのメンバーにも関連付けられていない ID またはデータセットが特定の削除リクエストに含まれている場合、グラフは影響を受けません。また、削除されなかった別のリンクによってリンクが確立された場合、削除リクエストによってデータセットまたは ID とデータセットの組み合わせ間のリンクが削除されても、グラフは更新されません。つまり、2 つの異なるデータセットにリンクが存在する場合、一方のデータセットのみ削除されるので、グラフは更新されません。 |

{style="table-layout:auto"}

## 次の手順

このドキュメントでは、Experience Platform での ID とデータセットの削除に使用できる様々なメカニズムについて説明しました。このドキュメントでは、ID とデータセットの削除が ID グラフに与える可能性のある影響についても説明しました。ID サービスについて詳しくは、[ID サービスの概要](home.md)を参照してください。

<!--

You can use [Data hygiene](../hygiene/home.md) for data cleansing, removing anonymous data, or data minimization for the data that you have collected.

### Single identity deletion in the [!UICONTROL Data Hygiene] workspace

The [[!UICONTROL Data Hygiene] workspace](../hygiene/ui/overview.md) in the Platform UI allows you to delete consumer records that are participating in Identity Service and Real-Time Customer Profile. For a comprehensive guide on using the [!UICONTROL Data Hygiene] workspace, see the tutorial on [deleting consumer records](../hygiene/ui/record-delete.md).

The table below provides a breakdown of differences between single identity deletion in Privacy Service and Data hygiene:

| Single identity deletion | Privacy Service | Data hygiene |
| --- | --- | --- |
| Accepted use cases | Data privacy requests (GDPR, CCPA) only. | Management of data stored in Experience Platform. |
| Estimated latency | Days to weeks | Days |
| Services impacted | Single identity deletion in Privacy Service allows you to select whether data will be deleted from Identity Service, Real-Time Customer Profile, or data lake. | Single identity deletion in Data hygiene deletes the selected data across Identity Service, Real-Time Customer Profile, and data lake. |
| Deletion patterns | Delete an identity from Identity Service. | Delete an identity from Identity Service. |

-->
