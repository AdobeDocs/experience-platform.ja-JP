---
keywords: Experience Platform；ホーム；人気の高いトピック；Audience Managerコネクタ；オーディエンスマネージャ；オーディエンスマネージャ
solution: Experience Platform
title: Audience Managerソースコネクタの概要
topic-legacy: overview
description: Adobe Audience Managerソースコネクタは、Audience Managerに収集されたファーストパーティデータをAdobe Experience Platformにストリーミングします。
exl-id: be90db33-69e1-4f42-9d1a-4f8f26405f0f
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '875'
ht-degree: 45%

---

# Audience Manager コネクタ

Adobe Audience Managerデータコネクタは、Adobe Audience Managerで収集されたファーストパーティデータをAdobe Experience Platformにストリーミングします。 Audience Managerコネクタは、2つのカテゴリのデータをプラットフォームに取り込みます。

- **リアルタイムデータ：Audience Managerのデータ収集サーバーでリアルタイムに取り込まれる** データ。このデータは、Audience Manager でルールベースの特性に入力するために使用され、最短の待ち時間で Platform に表示されます。
- **プロファイルデータ：** Audience Managerは、リアルタイムでオンボードのデータを使用して顧客のプロファイルを引き出します。これらのプロファイルは、セグメント認識で ID グラフと特性への入力に使用されます。

Audience Manager コネクタは、これらのデータカテゴリをエクスペリエンスデータモデル（XDM）スキーマにマッピングし、プラットフォームに送信します。リアルタイムデータはXDM ExperienceEventデータとして送信され、プロファイルデータはXDM Individualプロファイルとして送信されます。

Platform UI を使用して Adobe Platform Manager との接続を作成する手順については、[Audience Manager コネクタのチュートリアル](../../tutorials/ui/create/adobe-applications/audience-manager.md)を参照してください。

## エクスペリエンスデータモデル（XDM）とは

XDM は公式に文書化された仕様で、Platform がカスタマーエクスペリエンスデータを体系化する際に使用する標準化されたフレームワークとなるものです。

XDM 標準に準拠することで、カスタマーエクスペリエンスデータを一律に取り込むことができ、データの配信と情報の収集が容易になります。

Experience PlatformでのXDMの使い方について詳しくは、[XDM System overview](../../../xdm/home.md)を参照してください。 プロファイルや ExperienceEvent などの XDM スキーマの構造について詳しくは、[スキーマ構成の基礎](../../../xdm/schema/composition.md)を参照してください。

## XDM スキーマの例

Platform で XDM ExperienceEvent および XDM 個別プロファイルにマッピングされる Audience Manager 構造の例を以下に示します。

### ExperienceEvent — リアルタイムデータおよびオンボードデータ用

![](images/aam-experience-events-for-dcs-and-onboarding-data.png)

### XDM 個別プロファイル - プロファイルデータの場合

![](images/aam-profile-xdm-for-profile-data.png)

## フィールドは Adobe Audience Manager から XDM にどのようにマッピングされますか？

詳しくは、[Audience Manager のフィールドマッピング](./mapping/audience-manager.md)のドキュメントを参照してください。

## Platform でのデータ管理

### データセット

データセットは、スキーマ（列）とフィールド（行）を含み、データ接続で使用できるデータのコレクション（通常はテーブル）のストレージと管理の構成体です。Audience Managerデータは、リアルタイムデータ、受信データ、プロファイルデータで構成されます。 Audience Manager のデータセットを検索するには、UI の検索機能を使用し、各データタイプの命名規則を指定します。

Audience Managerデータセットはデフォルトでプロファイルに対して無効になっており、ユーザーは使用事例に基づいてデータセットを有効または無効にすることができます。 プロファイルのセグメントメンバーシップに使用するデータセットを無効にすることはお勧めしません。

| データセット名 | 説明 |
| ------------ | ----------- |
| AAMリアルタイム | このデータセットには、Audience Manager DCS エンドポイントで直接ヒットによって収集されたデータと、Audience Manager プロファイルの ID マップが含まれています。このデータセットを有効な状態にして、プロファイルの取り込みをおこないます。 |
| AAMリアルタイムプロファイルの更新 | このデータセットは、Audience Managerの特性とセグメントをリアルタイムでターゲティングできるようにします。 これには、エッジ地域ルーティング、特性、セグメントメンバーシップに関する情報が含まれています。このデータセットを有効な状態にして、プロファイルの取り込みをおこないます。データセットには、データをバッチとして表示することはできません。 **[!UICONTROL プロファイル]**&#x200B;の切り替えを有効にして、データをプロファイルに直接取り込むことができます。 |
| AAMデバイスデータ | ECID を持つデバイスデータと対応するセグメント認識を Audience Manager で集計したもの。データセットには、データをバッチとして表示することはできません。 **[!UICONTROL プロファイル]**&#x200B;の切り替えを有効にして、データをプロファイルに直接取り込むことができます。 |
| AAMデバイスプロファイルデータ | Audience Manager コネクタの診断に使用されます。データセットには、データをバッチとして表示することはできません。 **[!UICONTROL プロファイル]**&#x200B;の切り替えを有効にして、データをプロファイルに直接取り込むことができます。 |
| AAM認証済みプロファイル | このデータセットには、Audience Manager で認証されたプロファイルが含まれています。データセットには、データをバッチとして表示することはできません。 **[!UICONTROL プロファイル]**&#x200B;の切り替えを有効にして、データをプロファイルに直接取り込むことができます。 |
| AAM認証済みプロファイルメタデータ | Audience Manager コネクタの診断に使用されます。データセットには、データをバッチとして表示することはできません。 **[!UICONTROL プロファイル]**&#x200B;の切り替えを有効にして、データをプロファイルに直接取り込むことができます。 |
| AAMデバイスのデータバックフィル | 過去のデバイスデータを取り込むデータセット。 これには、ECIDと、Audience Managerで集計された対応するセグメントの実現が含まれます。 データセットには、データをバッチとして表示することはできません。 **[!UICONTROL プロファイル]**&#x200B;トグルを有効にして、データをプロファイルに直接取り込むことができます。 |
| AAM認証済みプロファイルのバックフィル | 過去の認証済みデータを取り込まないデータセット。 Audience Managerが認証したプロファイルが含まれます。 データセットには、データをバッチとして表示することはできません。 **[!UICONTROL プロファイル]**&#x200B;トグルを有効にして、データをプロファイルに直接取り込むことができます。 |

### 接続

Adobe Audience Manager では、Audience Manager 接続という 1 つの接続をカタログに作成します。カタログは、Adobe Experience Platform におけるデータの場所と系列のレコード体系です。接続は、コネクタの顧客固有のインスタンスであるカタログオブジェクトです。カタログ、接続、コネクタについて詳しくは、[カタログサービスの概要](../../../catalog/home.md)を参照してください。

## Platform 上で Audience Manager データに予想される遅延はどの程度ですか？

| Audience Manager データ | 遅延 | 備考 |
| --- | --- | --- |
| リアルタイムデータ | 35 分未満 | Audience Managerエッジノードでキャプチャされてから、プラットフォームデータレイクに表示されるまでの時間。 |
| プロファイルデータ | 2 日未満 | DCS/PCSエッジデータとオンボードデータを介して取り込まれ、ユーザープロファイルに処理されてから、プロファイルに表示されるまでの時間。 このデータは、プラットフォームデータレイクに直接送信されるわけではありません。 プロファイルの切り替えを有効にすると、Audience Managerプロファイルデータセットでこのデータを直接プロファイルに取り込むことができます。 |
