---
keywords: Experience Platform；ホーム；人気のトピック；Audience Manager コネクタ；Audience Manager;audience manager
solution: Experience Platform
title: Audience Manager Sourceの概要
description: Adobe Audience Manager ソースは、Audience Managerで収集されたファーストパーティデータをAdobe Experience Platformにストリーミングします。
exl-id: be90db33-69e1-4f42-9d1a-4f8f26405f0f
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '1137'
ht-degree: 13%

---

# Audience Manager ソース

>[!IMPORTANT]
>
>初期設定時に、特定の `namespaceCode={VALUE}` を持つ ID 名前空間が存在しないことを示すエラーメッセージがAdobe Audience Manager ソースから返されます。 **メモ**：バックエンドでは、ID シンボルを参照するために `namespaceCode` が使用されます。 統合を完了するには、次の操作を行う必要があります。
>
>- 指定した ID シンボル（`VALUE`）を使用して ](../../../identity-service/features/namespaces.md#create-custom-namespaces)ID サービスにカスタム名前空間を作成 [ します。
>- データを再度取り込みます。

Adobe Audience Manager ソースは、Adobe Experience Platformでのアクティベーション用に、Adobe Audience Managerで収集されたファーストパーティデータをストリーミングします。 Audience Manager ソースは、次の 2 種類のデータをExperience Platformに取り込みます。

- **リアルタイムデータ：** Audience Managerのデータ収集サーバーでリアルタイムにキャプチャされたデータ。 このデータは、Audience Managerでルールベースの特性を入力するために使用され、Experience Platformでは最短時間で表示されます。
- **プロファイルデータ：** Audience Managerでは、リアルタイムのデータとオンボードされたデータを使用して、顧客プロファイルを取得します。 これらのプロファイルは、セグメント認識で ID グラフと特性への入力に使用されます。

Audience Manager ソースは、これらのデータタイプをエクスペリエンスデータモデル（XDM）スキーマにマッピングしてから、Experience Platformに送信します。 リアルタイムデータは XDM ExperienceEvent データとして送信され、プロファイルデータは XDM 個人プロファイルデータとして送信されます。

詳しくは、[UI でのAudience Manager ソース接続の作成 ](../../tutorials/ui/create/adobe-applications/audience-manager.md) に関するガイドを参照してください。

## エクスペリエンスデータモデル（XDM）とは

XDM は、Experience Platformがカスタマーエクスペリエンスデータを整理するための標準化されたフレームワークを提供する、公開ドキュメント化された仕様です。

XDM 標準に準拠することで、カスタマーエクスペリエンスデータを一律に取り込むことができ、データの配信と情報の収集が容易になります。

Experience Platformでの XDM の使用方法について詳しくは、[XDM システムの概要 ](../../../xdm/home.md) を参照してください。 プロファイルとイベントの間で XDM スキーマがどのように構造化されるかについて詳しくは、[ スキーマ構成の基本 ](../../../xdm/schema/composition.md) を参照してください。

## XDM スキーマの例

Experience Platformで XDM ExperienceEvent および XDM 個人プロファイルにマッピングされるAudience Manager構造の例を以下に示します。

### ExperienceEvent - リアルタイムデータとオンボーディングデータの場合

![](images/aam-experience-events-for-dcs-and-onboarding-data.png)

### XDM 個人プロファイル – プロファイルデータ用

![](images/aam-profile-xdm-for-profile-data.png)

フィールドがAudience Managerから XDM にマッピングされる方法について詳しくは、[Audience Manager マッピングフィールド ](./mapping/audience-manager.md) に関するドキュメントを参照してください。

## Experience Platformのデータ管理

### データセット

データセットは、スキーマ（列）とフィールド（行）を含み、データ接続によって使用可能になるデータのコレクション（通常はテーブル）のストレージおよび管理用の構成体です。 Audience Manager データは、リアルタイムデータ、受信データ、プロファイルデータで構成されます。 Audience Manager のデータセットを検索するには、UI の検索機能を使用し、各データタイプの命名規則を指定します。

Audience Manager データセットはデフォルトでプロファイルに対して無効になっており、ユーザーは、ユースケースに基づいてデータセットを有効または無効にできます。 プロファイルのセグメントメンバーシップに使用されるデータセットを無効にすることは推奨されません。

>[!NOTE]
>
>AAM リアルタイムは、データレイクに送られる唯一のデータセットです。 [!DNL Profile] で有効になっている場合、他のすべてのAudience Manager データセットは [!DNL Profile] に移動します。 [!DNL Profile] に対して有効になっていない場合、データは受け取られず、空として表示されます。

| データセット名 | 説明 | クラス |
| --- | --- | --- |
| AAMリアルタイム | このデータセットには、Audience Manager DCS エンドポイントで直接ヒットによって収集されたデータと、Audience Manager プロファイルの ID マップが含まれています。このデータセットを有効な状態にして、プロファイルの取り込みをおこないます。 | エクスペリエンスイベント |
| AAM リアルタイムプロファイルのアップデート | このデータセットにより、Audience Managerの特性とセグメントのリアルタイムターゲティングが可能になります。 これには、エッジ地域ルーティング、特性、セグメントメンバーシップに関する情報が含まれています。このデータセットをプロファイル取り込み用に有効にしておきます。 データは、データセット内のバッチとして表示されません。 「**[!UICONTROL プロファイル]**」切替スイッチを有効にして、データをプロファイルに直接取り込むことができます。 | レコード |
| AAM デバイスのデータ | ECID と、対応するセグメント適合がAudience Managerに集計されたデバイスデータ。 データは、データセット内のバッチとして表示されません。 「**[!UICONTROL プロファイル]**」切替スイッチを有効にして、データをプロファイルに直接取り込むことができます。 | レコード |
| AAM デバイスプロファイルデータ | Audience Manager コネクタの診断に使用されます。データは、データセット内のバッチとして表示されません。 「**[!UICONTROL プロファイル]**」切替スイッチを有効にして、データをプロファイルに直接取り込むことができます。 | レコード |
| AAM認証済みプロファイル | このデータセットには、Audience Manager認証済みプロファイルが含まれています。 データは、データセット内のバッチとして表示されません。 「**[!UICONTROL プロファイル]**」切替スイッチを有効にして、データをプロファイルに直接取り込むことができます。 | レコード |
| AAM認証済みプロファイルのメタデータ | Audience Manager Connector の診断に使用されます。 データは、データセット内のバッチとして表示されません。 「**[!UICONTROL プロファイル]**」切替スイッチを有効にして、データをプロファイルに直接取り込むことができます。 | レコード |
| AAM デバイスのデータバックフィル | 過去のデバイスデータを取り込んだデータセット。 これには、ECID と、対応するセグメント適合がAudience Managerに集約されて含まれます。 データは、データセット内のバッチとして表示されません。 「**[!UICONTROL プロファイル]**」切替スイッチを有効にして、データをプロファイルに直接取り込むことができます。 | レコード |
| AAM認証済みプロファイルのバックフィル | 過去の認証済みデータを取り込んだデータセット。 これには、Audience Manager認証済みプロファイルが含まれます。 データは、データセット内のバッチとして表示されません。 「**[!UICONTROL プロファイル]**」切替スイッチを有効にして、データをプロファイルに直接取り込むことができます。 | レコード |

### 接続

Adobe Audience Managerは、カタログに、Audience Manager連携の 1 つの連携を作成します。 カタログは、Adobe Experience Platform におけるデータの場所と系列のレコード体系です。接続は、顧客固有のコネクタのインスタンスである Catalog オブジェクトです。 カタログ、接続、コネクタについて詳しくは、[ カタログサービスの概要 ](../../../catalog/home.md) を参照してください。

### セグメント母集団からプロファイルへの影響

セグメント母集団のサイズは、最初にAudience Manager セグメントをExperience Platformに送信する際に、プロファイル番号に直接影響します。 つまり、すべてのセグメントを選択すると、プロファイルの超過がライセンス使用権限を超える可能性があります。 また、Experience Platformは、プロファイル取り込み履歴データから新しいデータを区別します。 ファーストパーティベースの ID が 100 あるセグメントは、100 個のプロファイルを作成します。 ただし、その同じセグメントの母集団が 150 に増やしてExperience Platformに取り込まれた場合、新しいプロファイルは 50 個しかないので、プロファイルの数は 50 増加するだけです。

また、[ ライセンス使用状況ダッシュボード ](../../../dashboards/guides/license-usage.md) から、アカウントのプロファイル使用状況を確認することもできます。

## Experience Platform上のAudience Manager データでは、どの程度の待ち時間が予想されますか？

| Audience Manager データ | タイプ | 遅延 | 備考 |
| --- | --- | --- | --- |
| リアルタイムデータ | イベント | &lt;25 分 | Audience Manager Edge ノードでキャプチャされてから、データレイクに表示されるまでの時間。 |
| リアルタイムデータ | プロファイルの更新 | 10 分未満 | リアルタイム顧客プロファイルへの応答時間。 |
| リアルタイムのオンボーディングデータ | プロファイルの更新 | 24 ～ 36 時間 | DCS/PCS Edge データと転送されたデータを介してキャプチャされてから、ユーザープロファイルに処理されてリアルタイム顧客プロファイルに表示されるまでの時間。 現在、このデータはデータレイクに直接到達しません。 Audience Manager プロファイルデータセットでプロファイルの切り替えを有効にして、このデータをリアルタイム顧客プロファイルに直接取り込むことができます。 |
