---
keywords: Experience Platform；ホーム；人気のトピック；Adobe Campaign Managed Cloud Services;campaign;campaign managed services
title: Adobe Campaign Managed Cloud Services
description: ユーザーインターフェイスを使用して Campaign Managed Cloud Services をExperience Platformに接続する方法について説明します
exl-id: 8f18bf73-ebf1-4b4e-a12b-964faa0e24cc
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '747'
ht-degree: 5%

---

# Adobe Campaign Managed Cloud Services

Adobe Experience Platformを使用すると、データを外部ソースから取得しながら、Experience Platform サービスを使用して、受信データの構造化、ラベル付け、拡張を行うことができます。 アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。

Adobe Campaign Managed Cloud Servicesは、クロスチャネルのカスタマーエクスペリエンスを設計するためのManaged Services プラットフォームであり、視覚的なキャンペーンオーケストレーション、リアルタイムインタラクション管理およびクロスチャネル実行のための環境となります。 詳しくは、[Adobe Campaign v8 ドキュメント ](https://experienceleague.adobe.com/docs/campaign/campaign-v8/campaign-home.html?lang=ja) を参照してください。

Adobe Campaign Managed Cloud Services ソースを使用すると、Adobe Campaign v8 の配信ログとトラッキングログデータをAdobe Experience Platformに取り込むことができます。

## 前提条件

ソース接続を作成して Campaign v8 をExperience Platformに取り込む前に、まず次の前提条件を満たす必要があります。

* [Adobe Campaign クライアントコンソールを使用してイベントログの読み込みを設定します](#view-delivery-and-tracking-log-data)
* [XDM ExperienceEvent スキーマの作成](#create-a-schema)
* [データセットの作成](#create-a-dataset)

### 配信およびトラッキングログデータの表示 {#view-delivery-and-tracking-log-data}

>[!IMPORTANT]
>
>Campaign でログデータを表示するには、Adobe Campaign v8 クライアントコンソールにアクセスできる必要があります。 クライアントコンソールのダウンロードおよびインストール方法については、[Campaign v8 ドキュメント ](https://experienceleague.adobe.com/docs/campaign/campaign-v8/deploy/connect.html?lang=ja) を参照してください。

クライアントコンソールから Campaign v8 インスタンスにログインします。 「[!DNL Explorer]」タブで、「[!DNL Administration]」を選択し、「[!DNL Configuration]」を選択します。 次に、「[!DNL Data schemas]」を選択して、名前またはラベルの `broadLog` フィルターを適用します。 表示されるリストで、`broadLogRcp` という名前の受信者配信ログソーススキーマを選択します。

![ エクスプローラタブが選択されたAdobe Campaign v8 クライアントコンソールでは、管理、設定、データスキーマノードが展開され、フィルタリングが「broad」に設定されています。](./images/campaign/explorer.png)

次に、「**データ**」タブを選択します。

![ 「データ」タブが選択されたAdobe Campaign v8 クライアントコンソール ](./images/campaign/data.png)

データパネルで右クリックまたはキーストロークして、コンテキストメニューを開きます。 ここから **リストを設定…** を選択します

![ コンテキストメニューが開き、「リストを設定」オプションが選択されているAdobe Campaign v8 クライアントコンソール ](./images/campaign/configure.png)

リスト設定ウィンドウが開き、目的のフィールドを既存のリストに追加して、データパネルにデータを表示できるインターフェイスが表示されます。

![ 表示可能な受信者配信ログの設定のリスト ](./images/campaign/list-configuration.png)

これで、前の手順で追加された設定フィールドを含む、受信者配信ログを表示できます。

>[!TIP]
>
>同じ手順を繰り返しますが、トラッキングログデータを表示す `tracking` にはフィルターを適用します。

![ 最終変更名、配信チャネル、内部配信名、ラベルの情報と共に表示される受信者配信ログ ](./images/campaign/recipient-delivery-logs.png)

### スキーマの作成 {#create-a-schema}

次に、配信ログとトラッキングログの両方に XDM ExperienceEvent スキーマを作成します。 「キャンペーン配信ログ」フィールドグループを配信ログスキーマに適用し、「キャンペーントラッキングログ」フィールドグループをトラッキングログスキーマに適用する必要があります。 また、`externalID` フィールドをスキーマのプライマリ ID として定義する必要があります。

>[!NOTE]
>
>Campaign データを [!DNL Real-Time Customer Profile] に取り込むには、XDM ExperienceEvent スキーマがプロファイル対応である必要があります。

スキーマの作成方法に関する詳細な手順については、[UI での XDM スキーマの作成 ](../../../xdm/tutorials/create-schema-ui.md) に関するガイドを参照してください。

### データセットの作成 {#create-a-dataset}

最後に、スキーマのデータセットを作成する必要があります。 データセットの作成方法に関する詳細な手順については、[UI でのデータセットの作成 ](../../../catalog/datasets/user-guide.md) に関するガイドを参照してください。

## Experience Platform UI を使用したAdobe Campaign Managed Cloud Services ソース接続の作成

Campaign クライアントコンソールでデータログにアクセスし、スキーマとデータセットを作成したら、ソース接続を作成して、Campaign Managed Services データをExperience Platformに取り込みます。

Campaign v8 の配信ログおよびトラッキングログデータを Experience Platfrom に取り込む方法について詳しくは、[UI での Campaigned Managed Services ソース接続の作成 ](../../tutorials/ui/create/adobe-applications/campaign.md) に関するガイドを参照してください。

>[!IMPORTANT]
>
>最近削除されたメール受信者とメールのやり取りが、個人情報をExperience Platformに再度取り込む可能性があるエッジケースがあります。 場合によっては、これにより、そのユーザーに対するマーケティングが再び有効になることがあります。
>
>* このシナリオは、Experience Platformでプライバシーリクエストが実行されてからAdobe Campaign Classicで実行されるまでの間にのみアクティブになります。 Campaign でリクエストが実行された後、レコードが Campaign に書き出されていないことを確認するチェックがあります。 これを解決するには、実行 72 時間後に GDPR リクエストを再発行してください。