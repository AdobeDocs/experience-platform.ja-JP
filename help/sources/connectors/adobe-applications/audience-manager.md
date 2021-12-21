---
keywords: Experience Platform；ホーム；人気の高いトピック；Audience Managerコネクタ；Audience Manager;Audience Manager
solution: Experience Platform
title: Audience Managerソースコネクタの概要
topic-legacy: overview
description: Adobe Audience Managerソースコネクタは、Audience Managerで収集されたファーストパーティデータをAdobe Experience Platformにストリーミングします。
exl-id: be90db33-69e1-4f42-9d1a-4f8f26405f0f
source-git-commit: d0b6885b6e8606692cfe1173b75c7d3537800a5f
workflow-type: tm+mt
source-wordcount: '927'
ht-degree: 43%

---

# Audience Manager コネクタ

Adobe Audience Manager Data Connector は、Adobe Audience Managerで収集されたファーストパーティデータをAdobe Experience Platformにストリーミングします。 Audience Managerコネクタは、次の 2 つのカテゴリのデータを Platform に取り込みます。

- **リアルタイムデータ：** データ収集サーバーでリアルタイムにAudience Managerされるデータ。 このデータは、Audience Manager でルールベースの特性に入力するために使用され、最短の待ち時間で Platform に表示されます。
- **プロファイルデータ：** Audience Managerは、リアルタイムのオンボードデータを使用して、顧客プロファイルを導き出します。 これらのプロファイルは、セグメント認識で ID グラフと特性への入力に使用されます。

Audience Manager コネクタは、これらのデータカテゴリをエクスペリエンスデータモデル（XDM）スキーマにマッピングし、プラットフォームに送信します。リアルタイムデータは XDM ExperienceEvent データとして送信され、プロファイルデータは XDM 個別プロファイルとして送信されます。

Platform UI を使用して Adobe Platform Manager との接続を作成する手順については、[Audience Manager コネクタのチュートリアル](../../tutorials/ui/create/adobe-applications/audience-manager.md)を参照してください。

## エクスペリエンスデータモデル（XDM）とは

XDM は公式に文書化された仕様で、Platform がカスタマーエクスペリエンスデータを体系化する際に使用する標準化されたフレームワークとなるものです。

XDM 標準に準拠することで、カスタマーエクスペリエンスデータを一律に取り込むことができ、データの配信と情報の収集が容易になります。

XDM の使用方法について詳しくは、Experience Platformで [XDM システムの概要](../../../xdm/home.md). プロファイルや ExperienceEvent などの XDM スキーマの構造について詳しくは、[スキーマ構成の基礎](../../../xdm/schema/composition.md)を参照してください。

## XDM スキーマの例

Platform で XDM ExperienceEvent および XDM 個別プロファイルにマッピングされる Audience Manager 構造の例を以下に示します。

### ExperienceEvent — リアルタイムデータおよびオンボードデータの場合

![](images/aam-experience-events-for-dcs-and-onboarding-data.png)

### XDM 個別プロファイル - プロファイルデータの場合

![](images/aam-profile-xdm-for-profile-data.png)

## フィールドは Adobe Audience Manager から XDM にどのようにマッピングされますか？

詳しくは、[Audience Manager のフィールドマッピング](./mapping/audience-manager.md)のドキュメントを参照してください。

## Platform でのデータ管理

### データセット

データセットは、スキーマ（列）とフィールド（行）を含み、データ接続で使用できるデータのコレクション（通常はテーブル）のストレージと管理の構成体です。Audience Managerデータは、リアルタイムデータ、受信データ、プロファイルデータで構成されます。 Audience Manager のデータセットを検索するには、UI の検索機能を使用し、各データタイプの命名規則を指定します。

Audience Managerデータセットは、プロファイルに対してデフォルトで無効になっており、ユーザーは、使用例に基づいてデータセットを有効または無効にできます。 プロファイルのセグメントメンバーシップに使用されるデータセットを無効にすることはお勧めしません。

>[!NOTE]
>
>AAM Real-time は、 [!DNL Data Lake]. その他のすべてのAudience Managerデータセットは、 [!DNL Profile]を有効にした場合は、 [!DNL Profile]. これらが [!DNL Profile]を含めないと、データを受け取らず、空として表示されます。

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

Adobe Audience Manager では、Audience Manager 接続という 1 つの接続をカタログに作成します。カタログは、Adobe Experience Platform におけるデータの場所と系列のレコード体系です。接続は、コネクタの顧客固有のインスタンスであるカタログオブジェクトです。カタログ、接続、コネクタについて詳しくは、[カタログサービスの概要](../../../catalog/home.md)を参照してください。

## Platform 上で Audience Manager データに予想される遅延はどの程度ですか？

| Audience Manager データ | 遅延 | 備考 |
| --- | --- | --- |
| リアルタイムデータ | 35 分未満 | Audience ManagerEdge ノードでキャプチャされてから Platform データレイクに表示されるまでの時間です。 |
| プロファイルデータ | 2 日未満 | DCS/PCS Edge データおよびオンボーディングされたデータが取得されてから、ユーザープロファイルに対して処理されてから、プロファイルに表示されるまでの時間です。 このデータは、今日、Platform データレイクに直接送信されるわけではありません。 プロファイルの切り替えを有効にして、Audience Managerプロファイルデータセットでこのデータをプロファイルに直接取り込むことができます。 |
