---
keywords: Experience Platform；ホーム；人気の高いトピック；Audience Managerコネクタ；Audience Manager;Audience Manager
solution: Experience Platform
title: Audience Managerソースの概要
description: Adobe Audience Managerソースは、Audience Managerで収集されたファーストパーティデータをAdobe Experience Platformにストリーミングします。
exl-id: be90db33-69e1-4f42-9d1a-4f8f26405f0f
source-git-commit: 59dfa862388394a68630a7136dee8e8988d0368c
workflow-type: tm+mt
source-wordcount: '1059'
ht-degree: 22%

---

# Audience Manager ソース

Adobe Audience Managerソースは、Adobe Audience Managerでアクティベーション用に収集されたファーストパーティデータをAdobe Experience Platformでストリーミングします。 Audience Managerソースは、次の 2 種類のデータを Platform に取り込みます。

- **リアルタイムデータ：** データ収集サーバーでリアルタイムにAudience Managerされるデータ。 このデータは、Audience Managerでルールベースの特性に入力するために使用され、最短の待ち時間で Platform に表示されます。
- **プロファイルデータ：** Audience Managerは、リアルタイムのオンボードデータを使用して、顧客プロファイルを導き出します。 これらのプロファイルは、セグメント認識で ID グラフと特性への入力に使用されます。

Audience Managerソースは、これらのデータタイプを Experience Data Model(XDM) スキーマにマッピングし、Platform に送信します。 リアルタイムデータは XDM ExperienceEvent データとして送信され、プロファイルデータは XDM 個別プロファイルデータとして送信されます。

詳しくは、 [UI でのAudience Managerソース接続の作成](../../tutorials/ui/create/adobe-applications/audience-manager.md).

## エクスペリエンスデータモデル（XDM）とは

XDM は公式に文書化された仕様で、Platform がカスタマーエクスペリエンスデータを体系化する際に使用する標準化されたフレームワークとなるものです。

XDM 標準に準拠することで、カスタマーエクスペリエンスデータを一律に取り込むことができ、データの配信と情報の収集が容易になります。

XDM のExperience Platformでの使用方法について詳しくは、 [XDM システムの概要](../../../xdm/home.md). プロファイルとイベントの間で XDM スキーマが構造化されている方法について詳しくは、 [スキーマ構成の基本](../../../xdm/schema/composition.md).

## XDM スキーマの例

Platform で XDM ExperienceEvent および XDM 個別プロファイルにマッピングされる Audience Manager 構造の例を以下に示します。

### ExperienceEvent — リアルタイムデータおよびオンボードデータの場合

![](images/aam-experience-events-for-dcs-and-onboarding-data.png)

### XDM 個別プロファイル — プロファイルデータ用

![](images/aam-profile-xdm-for-profile-data.png)

フィールドがAudience Managerから XDM にマッピングされる方法について詳しくは、 [Audience Managerマッピングフィールド](./mapping/audience-manager.md).

## Platform でのデータ管理

### データセット

データセットは、スキーマ（列）とフィールド（行）を含み、データ接続で使用できるデータの集まり（通常はテーブル）のストレージと管理の構成体です。 Audience Managerデータは、リアルタイムデータ、受信データ、プロファイルデータで構成されます。 Audience Manager のデータセットを検索するには、UI の検索機能を使用し、各データタイプの命名規則を指定します。

Audience Managerデータセットは、プロファイルに対してデフォルトで無効になっており、ユーザーは、使用例に基づいてデータセットを有効または無効にできます。 プロファイルのセグメントメンバーシップに使用されるデータセットを無効にすることはお勧めしません。

>[!NOTE]
>
>AAMリアルタイムは、データレイクに送信される唯一のデータセットです。 その他のすべてのAudience Managerデータセットは、 [!DNL Profile]を有効にした場合は、 [!DNL Profile]. これらが [!DNL Profile]を含めないと、データを受け取らず、空として表示されます。

| データセット名 | 説明 | クラス |
| --- | --- | --- |
| AAMリアルタイム | このデータセットには、Audience Manager DCS エンドポイントで直接ヒットによって収集されたデータと、Audience Manager プロファイルの ID マップが含まれています。このデータセットを有効な状態にして、プロファイルの取り込みをおこないます。 | エクスペリエンスイベント |
| AAMリアルタイムプロファイル更新 | このデータセットを使用すると、Audience Manager特性とセグメントのリアルタイムターゲティングが可能になります。 これには、エッジ地域ルーティング、特性、セグメントメンバーシップに関する情報が含まれています。このデータセットを有効な状態にして、プロファイルの取り込みをおこないます。データは、データセット内のバッチとして表示されません。 次の項目を有効にすると、 **[!UICONTROL プロファイル]** を切り替えて、データをプロファイルに直接取り込みます。 | レコード |
| AAM Devices Data | ECID を持つデバイスデータと対応するセグメント認識を Audience Manager で集計したもの。データは、データセット内のバッチとして表示されません。 次の項目を有効にすると、 **[!UICONTROL プロファイル]** を切り替えて、データをプロファイルに直接取り込みます。 | レコード |
| AAM Device Profile Data | Audience Manager コネクタの診断に使用されます。データは、データセット内のバッチとして表示されません。 次の項目を有効にすると、 **[!UICONTROL プロファイル]** を切り替えて、データをプロファイルに直接取り込みます。 | レコード |
| AAM Authenticated Profiles | このデータセットには、Audience Manager で認証されたプロファイルが含まれています。データは、データセット内のバッチとして表示されません。 次の項目を有効にすると、 **[!UICONTROL プロファイル]** を切り替えて、データをプロファイルに直接取り込みます。 | レコード |
| AAM Authenticated Profiles メタデータ | Audience Manager コネクタの診断に使用されます。データは、データセット内のバッチとして表示されません。 次の項目を有効にすると、 **[!UICONTROL プロファイル]** を切り替えて、データをプロファイルに直接取り込みます。 | レコード |
| AAM Devices Data Backfill | 過去のデバイスデータを取り込むデータセット。 これには、ECID と、それに対応するセグメント適合がAudience Managerで集計されます。 データは、データセット内のバッチとして表示されません。 次の項目を有効にすると、 **[!UICONTROL プロファイル]** を切り替えて、データをプロファイルに直接取り込みます。 | レコード |
| AAM認証済みプロファイルのバックフィル | 過去の認証済みデータを取り込むデータセット。 これには、認証済みAudience Managerプロファイルが含まれます。 データは、データセット内のバッチとして表示されません。 次の項目を有効にすると、 **[!UICONTROL プロファイル]** を切り替えて、データをプロファイルに直接取り込みます。 | レコード |

### 接続

Adobe Audience Manager では、Audience Manager 接続という 1 つの接続をカタログに作成します。カタログは、Adobe Experience Platform におけるデータの場所と系列のレコード体系です。接続は、コネクタの顧客固有のインスタンスであるカタログオブジェクトです。 詳しくは、 [カタログサービスの概要](../../../catalog/home.md) カタログ、接続、コネクタについて詳しくは、を参照してください。

### セグメント母集団からプロファイルへの影響

セグメントの母集団のサイズは、最初にプラットフォームにセグメントを送信する際に、プロファイルの数に直接影響を与えるAudience Managerセグメントです。 つまり、すべてのセグメントを選択すると、プロファイルが使用権限を超過する可能性があります。 また、Platform は、新しいデータとプロファイル取り込みの履歴データを区別します。 100 個のファーストパーティベースの ID を持つセグメントは、100 個のプロファイルを作成します。 ただし、同じセグメントの母集団が 150 に増え、Platform に取り込まれた場合、新しいプロファイルは 50 個しかないので、プロファイルの数は 50 個しか増えません。

また、 [ライセンス使用状況ダッシュボード](../../../dashboards/guides/license-usage.md).

## Platform 上で Audience Manager データに予想される遅延はどの程度ですか？

| Audience Manager データ | タイプ | 遅延 | 備考 |
| --- | --- | --- | --- |
| リアルタイムデータ | イベント | &lt;25 分 | Audience Managerエッジノードでキャプチャされてからデータレイクに表示されるまでの時間です。 |
| リアルタイムデータ | プロファイルの更新 | &lt;10 分 | リアルタイム顧客プロファイルへの到達時間。 |
| リアルタイムデータとオンボードデータ | プロファイルの更新 | 24 ～ 36 時間 | DCS/PCS Edge データおよびオンボードデータを介して取得されてから、ユーザープロファイルに処理されてから、リアルタイム顧客プロファイルに表示されるまでの時間です。 現在、このデータは、データレイクに直接取り込まれるわけではありません。 プロファイルの切り替えを有効にして、Audience Managerプロファイルデータセットでこのデータをリアルタイム顧客プロファイルに直接取り込むことができます。 |
