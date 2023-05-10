---
keywords: Experience Platform；ホーム；人気の高いトピック；Adobe Campaign Managed Cloud Services;campaign;campaign managed services
title: Adobe Campaign Managed Cloud Services
description: ユーザーインターフェイスを使用して Campaign で管理されたCloud Servicesを Platform に接続する方法を説明します
exl-id: 8f18bf73-ebf1-4b4e-a12b-964faa0e24cc
source-git-commit: 34e0381d40f884cd92157d08385d889b1739845f
workflow-type: tm+mt
source-wordcount: '663'
ht-degree: 11%

---

# Adobe Campaign Managed Cloud Services

Adobe Experience Platform を使用すると、外部ソースからデータを取り込みながら、Platform サービスを使用して受信データの構造化、ラベル付けおよび拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。

Adobe Campaign Managed Cloud Servicesは、クロスチャネルの顧客エクスペリエンスを設計するManaged Servicesプラットフォームを提供し、視覚的なキャンペーン編成、リアルタイムのインタラクション管理、クロスチャネルの実行のための環境を提供します。 次にアクセス： [Adobe Campaign v8 ドキュメント](https://experienceleague.adobe.com/docs/campaign/campaign-v8/campaign-home.html?lang=ja) を参照してください。

Adobe Campaign Managed Cloud Servicesソースを使用すると、Adobe Campaign v8 の配信ログとトラッキングログデータをAdobe Experience Platformに取り込むことができます。

## 前提条件

ソース接続を作成して Campaign v8 をExperience Platformに移行する前に、次の前提条件を満たす必要があります。

* [Adobe Campaignクライアントコンソールを使用したイベントログのインポートのセットアップ](#view-delivery-and-tracking-log-data)
* [XDM ExperienceEvent スキーマの作成](#create-a-schema)
* [データセットの作成](#create-a-dataset)

### 配信およびトラッキングログデータの表示 {#view-delivery-and-tracking-log-data}

>[!IMPORTANT]
>
>Campaign でログデータを表示するには、Adobe Campaign v8 クライアントコンソールへのアクセス権が必要です。 次にアクセス： [Campaign v8 ドキュメント](https://experienceleague.adobe.com/docs/campaign/campaign-v8/deploy/connect.html?lang=en) クライアントコンソールをダウンロードしてインストールする方法については、を参照してください。

クライアントコンソールから Campaign v8 インスタンスにログインします。 以下 [!DNL Explorer] タブ、選択 [!DNL Administration] 次に、 [!DNL Configuration]. 次に、 [!DNL Data schemas] 次に、 `broadLog` 名前またはラベルのフィルター。 表示されるリストで、次の名前の受信者配信ログのソーススキーマを選択します。 `broadLogRcp`.

![「エクスプローラー」タブが選択されたAdobe Campaign v8 クライアントコンソール、「管理」、「設定」、「データスキーマ」の各ノードが展開され、フィルタリングが「broad」に設定されます。](./images/campaign/explorer.png)

次に、 **データ** タブをクリックします。

![「データ」タブが選択されたAdobe Campaign v8 クライアントコンソール。](./images/campaign/data.png)

データパネルで右クリックまたはキー操作を行うと、コンテキストメニューが開きます。 ここからを選択します。 **リストを設定…**

![コンテキストメニューが開き、「リストを設定」オプションが選択されたAdobe Campaign v8 クライアントコンソール。](./images/campaign/configure.png)

リスト設定ウィンドウが表示され、既存のリストに目的のフィールドを追加してデータパネルにデータを表示できるインターフェイスが表示されます。

![表示用に追加できる受信者配信ログの設定のリストです。](./images/campaign/list-configuration.png)

これで、前の手順で追加した設定フィールドを含む受信者配信ログを表示できます。

>[!TIP]
>
>同じ手順を繰り返すことができますが、次の手順でフィルターを適用します。 `tracking` をクリックして、トラッキングログデータを表示します。

![受信者配信ログには、最終変更日の名前、配信チャネル、内部配信名、ラベルに関する情報が表示されます。](./images/campaign/recipient-delivery-logs.png)

### スキーマの作成 {#create-a-schema}

次に、配信ログとトラッキングログの両方に XDM ExperienceEvent スキーマを作成します。 「キャンペーン配信ログ」フィールドグループを配信ログスキーマに、「キャンペーントラッキングログ」フィールドグループをトラッキングログスキーマに適用する必要があります。 また、 `externalID` フィールドをスキーマのプライマリ ID に設定します。

>[!NOTE]
>
>Campaign データをに取り込むには、XDM ExperienceEvent スキーマでプロファイルを有効にする必要があります。 [!DNL Real-Time Customer Profile].

スキーマの作成方法について詳しくは、 [UI での XDM スキーマの作成](../../../xdm/tutorials/create-schema-ui.md).

### データセットの作成 {#create-a-dataset}

最後に、スキーマのデータセットを作成する必要があります。 データセットの作成方法について詳しくは、 [UI でのデータセットの作成](../../../catalog/datasets/user-guide.md).

## Platform UI を使用したAdobe Campaign Managed Cloud Servicesソース接続の作成

これで、Campaign クライアントコンソールでデータログにアクセスし、スキーマとデータセットを作成したので、ソース接続を作成して、Campaign Managed Servicesデータを Platform に取り込むことができます。

Campaign v8 の配信ログとトラッキングログデータを Experience Platform に取り込む方法について詳しくは、 [UI での Campaigned Managed Servicesソース接続の作成](../../tutorials/ui/create/adobe-applications/campaign.md).
