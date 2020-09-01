---
keywords: Experience Platform;home;popular topics;Audience Manager connector;Audience manager;audience manager
solution: Experience Platform
title: Audience Manager コネクタ
topic: overview
description: Adobe Audience Manager データコネクタは、Adobe Audience Manager で収集されたファーストパーティデータを Adobe Experience Platform にストリーミングします。Audience Managerコネクタは、3つのカテゴリのデータをプラットフォームに取り込みます。
translation-type: tm+mt
source-git-commit: 6934bfeee84f542558894bbd4ba5759891cd17f3
workflow-type: tm+mt
source-wordcount: '673'
ht-degree: 87%

---


# Audience Manager コネクタ

Adobe Audience Manager データコネクタは、Adobe Audience Manager で収集されたファーストパーティデータを Adobe Experience Platform にストリーミングします。Audience Manager コネクタは、次の 3 つのカテゴリのデータを Platform に取り込みます。

- **リアルタイムデータ**：Audience Manager のデータ収集サーバーでリアルタイムにキャプチャされるデータです。このデータは、Audience Manager でルールベースの特性に入力するために使用され、最短の待ち時間で Platform に表示されます。
- **プロファイルデータ**：Audience Manager では、リアルタイムのオンボードデータを使用して、顧客プロファイルを導き出します。これらのプロファイルは、セグメント認識で ID グラフと特性への入力に使用されます。

Audience Manager コネクタは、これらのデータカテゴリをエクスペリエンスデータモデル（XDM）スキーマにマッピングし、プラットフォームに送信します。リアルタイムデータはXDM ExperienceEventデータとして送信され、プロファイルデータはXDM Individualプロファイルとして送信されます。

Platform UI を使用して Adobe Platform Manager との接続を作成する手順については、[Audience Manager コネクタのチュートリアル](../../tutorials/ui/create/adobe-applications/audience-manager.md)を参照してください。

## エクスペリエンスデータモデル（XDM）とは

XDM は公式に文書化された仕様で、Platform がカスタマーエクスペリエンスデータを体系化する際に使用する標準化されたフレームワークとなるものです。

XDM 標準に準拠することで、カスタマーエクスペリエンスデータを一律に取り込むことができ、データの配信と情報の収集が容易になります。

For more information about how XDM is used in Experience Platform, see the [XDM System overview](../../../xdm/home.md). プロファイルや ExperienceEvent などの XDM スキーマの構造について詳しくは、[スキーマ構成の基礎](../../../xdm/schema/composition.md)を参照してください。

## XDM スキーマの例

Platform で XDM ExperienceEvent および XDM 個別プロファイルにマッピングされる Audience Manager 構造の例を以下に示します。

### ExperienceEvent - リアルタイムデータおよびオンボードデータの場合

![](images/aam-experience-events-for-dcs-and-onboarding-data.png)

### XDM 個別プロファイル - プロファイルデータの場合

![](images/aam-profile-xdm-for-profile-data.png)

## フィールドは Adobe Audience Manager から XDM にどのようにマッピングされますか？

詳しくは、[Audience Manager のフィールドマッピング](./mapping/audience-manager.md)のドキュメントを参照してください。

## Platform でのデータ管理

### データセット

データセットは、スキーマ（列）とフィールド（行）を含み、データ接続で使用できるデータのコレクション（通常はテーブル）のストレージと管理の構成体です。Audience Manager のデータは、リアルタイムデータ、インバウンドデータ、プロファイルデータで構成されます。Audience Manager のデータセットを検索するには、UI の検索機能を使用し、各データタイプの命名規則を指定します。

Audience Managerデータセットはデフォルトでプロファイルに対して無効になっており、ユーザーは使用事例に基づいてデータセットを有効または無効にすることができます。 プロファイルのセグメントメンバーシップに使用するデータセットを無効にすることはお勧めしません。

| データセット名 | 説明 |
| ------------ | ----------- |
| Audience Manager リアルタイム | このデータセットには、Audience Manager DCS エンドポイントで直接ヒットによって収集されたデータと、Audience Manager プロファイルの ID マップが含まれています。このデータセットを有効な状態にして、プロファイルの取り込みをおこないます。 |
| Audience Manager リアルタイムプロファイル更新 | このデータセットを使用すると、Audience Manager の特性とセグメントをリアルタイムでターゲティングできます。これには、エッジ地域ルーティング、特性、セグメントメンバーシップに関する情報が含まれています。このデータセットを有効な状態にして、プロファイルの取り込みをおこないます。 |
| Audience Manager デバイスデータ | ECID を持つデバイスデータと対応するセグメント認識を Audience Manager で集計したもの。 |
| Audience Manager デバイスプロファイルデータ | Audience Manager コネクタの診断に使用されます。 |
| Audience Manager 認証済みプロファイル | このデータセットには、Audience Manager で認証されたプロファイルが含まれています。 |
| Audience Manager 認証済みプロファイルメタデータ | Audience Manager コネクタの診断に使用されます。 |

### 接続

Adobe Audience Manager では、**Audience Manager 接続**&#x200B;という 1 つの接続をカタログに作成します。カタログは、Adobe Experience Platform におけるデータの場所と系列のレコード体系です。接続は、コネクタの顧客固有のインスタンスであるカタログオブジェクトです。カタログ、接続、コネクタについて詳しくは、[カタログサービスの概要](../../../catalog/home.md)を参照してください。

## Platform 上で Audience Manager データに予想される遅延はどの程度ですか？

| Audience Manager データ | 遅延 | 備考 |
| --- | --- | --- |
| リアルタイムデータ | 35 分未満 | リアルタイムノードでキャプチャされてから Platform データレイクに表示されるまでの時間です。 |
| インバウンドデータ | 13 時間未満 | S3 バケットでキャプチャされてから Platform データレイクに表示されるまでの時間です。 |
| プロファイルデータ | 2 日未満 | リアルタイム／インバウンドデータから取り込まれてから、ユーザープロファイルに追加されて最終的に Platform データレイクに表示されるまでの時間です。 |