---
title: ID グラフリンクルール設定ガイド
description: ID グラフリンクルール設定を使用してデータを実装する際に従うべき推奨手順を説明します。
badge: ベータ版
source-git-commit: d8a36650b2cd3ec9683763f536ea5c2c2e27455c
workflow-type: tm+mt
source-wordcount: '696'
ht-degree: 5%

---

# ID グラフリンクルール設定ガイド

Adobe Experience Platform ID サービスを使用してデータを実装する際に従うことができる、ステップバイステップのガイドについては、このドキュメントを参照してください。

操作手順の概要：

1. [必要な ID 名前空間の作成](#namespace)
2. [グラフシミュレーションツールを使用して、ID 最適化アルゴリズムを理解します](#graph-simulation)
3. [ID 設定ツールを使用して、一意の名前空間を指定し、名前空間の優先度ランキングを設定します](#identity-settings)
4. [エクスペリエンスデータモデル（XDM）スキーマの作成](#schema)
5. [データセットの作成](#dataset)
6. [Experience Platformへのデータの取り込み](#ingest)

## 実装前の前提条件

開始する前に、まず、システム内の認証済みイベントにユーザー識別子が常に含まれていることを確認する必要があります。

<!-- ## Set permissions {#set-permissions}

The first step in the implementation process for Identity Service is to ensure that your Experience Platform account is added to a role that is provisioned with the necessary permissions. Your administrator can configure permissions for your account by navigating to the Permissions UI in Adobe Experience Cloud. From there, your account must be added to a role with the following permissions:

* manage-identity-settings
* view-identity-dashboard
* view-identity-simulation

For more information on permissions, read the [permissions guide](../../access-control/abac/ui/permissions.md). -->

## ID 名前空間の作成 {#namespace}

データで名前空間が必要な場合、まず組織に適した名前空間を作成する必要があります。 カスタム名前空間の作成手順については、のガイドを参照してください。 [ui でのカスタム名前空間の作成](../features/namespaces.md#create-custom-namespaces).

## グラフシミュレーションツールの使用 {#graph-simulation}

次に、に移動します [グラフシミュレーションツール](./graph-simulation.md) ID サービス UI ワークスペースで変更します。 グラフシミュレーションツールを使用して、様々な一意の名前空間や名前空間の優先度設定で作成された ID グラフをシミュレートできます。

様々な設定を作成することで、グラフシミュレーションツールを使用して、ID 最適化アルゴリズムや特定の設定が、グラフの動作に与える影響をより深く理解できます。

## ID 設定の指定 {#identity-settings}

グラフがどのように動作するかを把握したら、 [id 設定ツール](./identity-settings-ui.md) ID サービス UI ワークスペースで変更します。

ID 設定ツールを使用して、一意の名前空間を指定し、優先順に名前空間を設定します。 設定の適用が完了したら、新しい設定が ID サービスに反映されるまで少なくとも 6 時間かかるので、データの取り込みに進むまでに少なくとも 6 時間待つ必要があります。

## XDM スキーマの作成 {#schema}

独自の名前空間と名前空間の優先順位が確立されたので、データを取り込むために必要な設定に進むことができます。 まず、XDM スキーマを作成する必要があります。 データに応じて、XDM 個人プロファイルと XDM ExperienceEvent の両方のスキーマを作成する必要が生じる場合があります。

リアルタイム顧客プロファイルにデータを取り込むには、スキーマに、プライマリ ID として指定されたフィールドが少なくとも 1 つ含まれていることを確認する必要があります。 プライマリ ID を設定することで、特定のスキーマをプロファイル取り込みに対して有効にできます。

スキーマの作成方法については、に関するガイドを参照してください。 [ui での XDM スキーマの作成](../../xdm/tutorials/create-schema-ui.md).

## データセットの作成 {#dataset}

次に、取り込むデータの構造を提供するデータセットを作成します。 データセットは、スキーマ（列）とフィールド（行）で構成されるデータコレクション（通常はテーブル）を格納し管理するための構造です。データセットはスキーマと連携し、リアルタイム顧客プロファイルにデータを取り込むには、データセットがプロファイル取り込みに対して有効になっている必要があります。 データセットをプロファイルで有効にするには、プロファイルの取り込みが有効になっているスキーマを参照する必要があります。

データセットの作成方法については、次を参照してください [データセット UI ガイド](../../catalog/datasets/user-guide.md).

## データの取り込み {#ingest}

この時点で、次の情報が得られます。

* ID サービス機能にアクセスするために必要な権限。
* データの名前空間。
* 名前空間に一意の名前空間を指定し、優先順位を設定します。
* 1 つ以上の XDM スキーマ。 （データと特定のユースケースに応じて、プロファイルスキーマとエクスペリエンスイベントスキーマの両方を作成する必要がある場合があります）。
* スキーマに基づくデータセット。

上記のすべての項目を完了したら、データをExperience Platformに取り込み始めることができます。 様々な方法でデータ取り込みを実行できます。 次のサービスを使用してデータをExperience Platformに取り込むことができます。

* [バッチおよびストリーミング取得](../../ingestion/home.md)
* [Experience Platformでのデータ収集](../../collection/home.md)
* [Experience Platformソース](../../sources/home.md)

>[!TIP]
>
>データが取り込まれても、XDM 生データペイロードは変更されません。 プライマリ ID 設定は、UI に表示される場合があります。 ただし、これらの設定は ID 設定によって上書きされます。

フィードバックには、を使用します **[!UICONTROL Betaのフィードバック]** id サービス UI ワークスペースの「」オプション。