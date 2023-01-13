---
title: ID サービスの削除
description: このドキュメントでは、Experience Platformで ID データを削除するために使用できる様々なメカニズムの概要と、ID グラフがどのように影響を受けるかの明確な説明を提供します。
source-git-commit: da1ce4560d28d43db47318883f9656cebb2eb487
workflow-type: tm+mt
source-wordcount: '1207'
ht-degree: 13%

---

# ID サービスの削除

Adobe Experience Platform ID サービスは、個人のデバイスやシステム間で ID を決定的にリンクすることで、ID グラフを生成します。 ID グラフのリンクは、同じデータ行内で 2 つ以上のマークされた ID を受け取った場合に確立されます。

リアルタイム顧客プロファイルで ID グラフを活用して、顧客の属性と行動を包括的で単数のビューで作成し、デバイスではなく、効果的な個人のデジタルエクスペリエンスをリアルタイムでユーザーに提供できます。

このドキュメントでは、Experience Platformで ID データを削除するために使用できる様々なメカニズムの概要と、ID グラフがどのように影響を受けるかの明確な説明を提供します。

## はじめに

以下のドキュメントでは、Experience Platformの次の機能を参照します。

* [ID サービス](home.md)：デバイスやシステム間で ID を関連付けることで、個々の顧客とその行動への理解を深めることができます。
   * [ID グラフ](./ui/identity-graph-viewer.md):ID グラフは、特定の顧客の異なる ID 間の関係のマップで、顧客が様々なチャネルを通じてブランドとどのようにやり取りするかを視覚的に示します。
   * [ID 名前空間](namespaces.md):ID 名前空間は、ID が関連するコンテキストのインジケーターとして機能する ID サービスのコンポーネントです。 例えば、「name<span>@email.com」の値を電子メールアドレスとして、または「443522」を数値 CRM ID として区別します。
* [カタログサービス](../catalog/home.md):データレイク内でデータ系列、メタデータ、ファイル記述、ディレクトリ、データセットを調べます。
* [データの衛生状態](../hygiene/home.md):自動データセット有効期限をスケジュールしたり、1 つのデータセットまたはすべてのデータセットから個々のレコードを削除したりして、保存したコンシューマーデータを管理します。
* [Adobe Experience Platform Privacy Service](../privacy-service/home.md):Adobe Experience Cloudアプリケーションをまたいで、自分の個人データへのアクセス、販売のオプトアウト、または削除に対する顧客リクエストを管理します。
* [リアルタイム顧客プロファイル](../profile/home.md):複数のソースからの集計データに基づいて、統合された顧客プロファイルをリアルタイムで提供します。

## 単一の ID の削除

単一の ID 削除リクエストを使用すると、グラフ内の ID を削除できるので、ID 名前空間に関連付けられた単一のユーザー ID に関連付けられたリンクが削除されます。 UOU は次のメカニズムを使用できます： [Privacy Service](../privacy-service/home.md) を参照してください。

以下の節では、Experience Platformで単一の ID 削除リクエストに使用できるメカニズムの概要を説明します。

### Privacy Serviceでの単一の ID の削除

 Privacy Service は、EU 一般データ保護規則（GDPR）やカリフォルニア州消費者プライバシー法（CCPA）などのプライバシー規制に基づいて定義された個人データのアクセス、販売、または削除を求める顧客のリクエストを処理します。Privacy Serviceを使用すると、API または UI を使用してジョブリクエストを送信できます。 Experience Platform が Privacy Service から削除リクエストを受信すると、Platform は、Privacy Service に対し、リクエストを受信し、影響を受けるデータが削除用にマークされている旨の確認を送信します。各 ID の削除は、指定した名前空間または ID の値に基づいて行われます。 さらに、特定の組織に関連付けられているすべてのサンドボックスに対して、削除が実行されます。 詳しくは、 [ID サービスでのプライバシーリクエストの処理](privacy.md).

次の表に、Privacy Serviceでの単一の ID 削除の分類を示します。

| 単一の ID の削除 | Privacy Service |
| --- | --- |
| 受け入れられた使用例 | データプライバシーリクエスト (GDPR、CCPA) のみ。 |
| 推定遅延 | 数日から数週間 |
| 影響を受けるサービス | Privacy Serviceでの単一の ID 削除を使用すると、データを ID サービス、リアルタイム顧客プロファイル、データレイクのどれから削除するかを選択できます。 |
| 削除パターン | ID サービスから ID を削除します。 |

{style=&quot;table-layout:auto&quot;}

## データセットの削除

次の節では、Experience Platform内のデータセットと関連する ID リンクを削除するために使用できるメカニズムの概要を説明します。

### カタログサービスでのデータセットの削除

カタログサービスを使用して、データセット削除のリクエストを送信できます。 カタログサービスを使用してデータセットを削除する方法の詳細については、 [カタログサービス API を使用したオブジェクトの削除](../catalog/api/delete-object.md). また、Platform UI を使用して、データセット削除のリクエストを送信することもできます。 詳しくは、 [データセットユーザーガイド](../catalog/datasets/user-guide.md#delete-a-dataset).

### データの衛生状態におけるデータセットの有効期限

Adobe Experience Platform UI の[[!UICONTROL データハイジーン]ワークスペース](../hygiene/ui/overview.md)では、データセットの有効期限をスケジュールできます。データセットが有効期限に達すると、データレイク、ID サービス、リアルタイム顧客プロファイルは、別々のプロセスを開始し、各サービスからデータセットの内容を削除します。 詳しくは、 [を使用したデータセット有効期限の管理 [!UICONTROL データの衛生状態] workspace](../hygiene/ui/dataset-expiration.md).

次の表に、カタログサービスでのデータセット削除とデータの衛生状態の違いを示します。

| データセットの削除 | カタログサービス | データハイジーン |
| --- | --- | --- |
| 受け入れられた使用例 | Platform で完全なデータセットと関連する ID 情報を削除します。 | Experience Platformに保存されたデータの管理。 |
| 推定遅延 | Days | Days |
| 影響を受けるサービス | カタログサービスを通じてデータセットを削除すると、ID サービス、リアルタイム顧客プロファイル、データレイクからデータが削除されます。 | データの衛生状態を通じてデータセットを削除すると、ID サービス、リアルタイム顧客プロファイル、データレイクからデータが削除されます。 |
| 削除パターン | 特定のデータセットによって確立された ID サービスから、リンクされた ID を削除します。 | 有効期限のスケジュールに基づいて、特定のデータセットによって確立された ID サービスから、リンクされた ID を削除します。 |

{style=&quot;table-layout:auto&quot;}

## 削除後の ID グラフの様々な状態

すべての ID グラフが削除されると、削除リクエストで指定された 2 つ以上の ID 間のリンクが削除されます。 データセット削除リクエストの場合、指定したデータセットによって確立されたすべての ID リンクが削除され、グラフから ID を削除する場合と削除しない場合があります。 単一の ID 削除要求の場合、指定した ID の ID リンクは削除され、その結果、ID 値自体はすべての ID グラフから削除されます。 別の ID と単一のリンクがない ID は、ID サービスに保存されません。

ID グラフの状態に対して削除が及ぼす潜在的な影響の概要を以下に示します。

| ID グラフの状態 | 説明 |
| --- | --- |
| 部分更新 | グラフの部分的な更新は、削除リクエストが正常に処理された後、1 つのグラフ内で少なくとも 2 つの ID がリンクされたままになる場合に発生します。 削除後、残りの ID リンクは相互に接続されたままになる場合もあれば、削除された ID に応じて、2 つ以上の別々のグラフに分割することもできます。 |
| 完全削除 | グラフが存在するには、グラフに少なくとも 2 つのリンクされた ID が必要です。 したがって、削除要求によってグラフ内の既存のリンクがすべて削除された場合、グラフは完全に削除されます。 |
| 変更なし | グラフのメンバーに関連付けられていない ID またはデータセットが、特定の削除要求に含まれている場合、グラフは影響を受けません。 また、削除要求によって、削除されなかった別のリンクによってリンクが確立された場合、データセットまたは ID とデータセットの組み合わせの間のリンクが削除されても、グラフは更新されません。 つまり、2 つの異なるデータセットにリンクが存在する場合、削除されるデータセットは 1 つだけなので、グラフは更新されません。 |

{style=&quot;table-layout:auto&quot;}

## 次の手順

このドキュメントでは、Experience Platform上の ID とデータセットを削除するために使用できる様々なメカニズムについて説明しました。 このドキュメントでは、ID とデータセットの削除が ID グラフに与える影響についても説明しました。 ID サービスの詳細については、 [ID サービスの概要](home.md).

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