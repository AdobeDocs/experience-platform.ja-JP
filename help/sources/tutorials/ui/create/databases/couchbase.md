---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: UIでCouchbaseソースコネクタを作成する
topic: overview
translation-type: tm+mt
source-git-commit: d3c725c4760acb3857a67d0d30b24732c963a030
workflow-type: tm+mt
source-wordcount: '466'
ht-degree: 1%

---


# UIで [!DNL Couchbase] ソースコネクタを作成する

> [!NOTE]
> コネクタ [!DNL Couchbase] はベータ版です。 ベータラベル付きのコネクタの使用について詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) 「」を参照してください。

のソースコネクタは、外部ソースのデータをスケジュールに基づいて取り込む機能を [!DNL Adobe Experience Platform] 提供します。 このチュートリアルでは、ユー [!DNL Couchbase] ザインタフェースを使用して [!DNL Platform] ソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルでは、次のコンポーネントについて十分に理解している必要があり [!DNL Platform]ます。

* [Experience Data Model(XDM)System](../../../../../xdm/home.md): 顧客体験データを [!DNL Experience Platform] 整理するための標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md): XDMスキーマの基本構成要素について説明します。この基本構成要素には、スキーマ構成の主な原則とベストプラクティスが含まれます。
   * [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md): スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
* [リアルタイム顧客プロファイル](../../../../../profile/home.md): 複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

既に有効な [!DNL Couchbase] 接続がある場合は、このドキュメントの残りの部分をスキップして、データフローの [設定に関するチュートリアルに進むことができます](../../dataflow/databases.md)。

### 必要な資格情報の収集

ソースコネクタを認証するには、次の接続プロパティの値を指定する必要があり [!DNL Couchbase] ます。

| Credential | 説明 |
| ---------- | ----------- |
| `connectionString` | インスタンスへの接続に使用する接続文字列 [!DNL Couchbase] です。 の接続文字列パターン [!DNL Couchbase] は、 `Server={SERVER}; Port={PORT};AuthMech=1;CredString=[{\"user\": \"{USER}\", \"pass\":\"{PASS}\"}];`です。 接続文字列の取得について詳しくは、 [Couchbase接続に関するドキュメントを参照してください](https://docs.Couchbase.com/c-sdk/2.10/client-settings.html#configuring-overview)。 |

## アカウントに接続 [!DNL Couchbase] する

必要な資格情報を収集したら、次の手順に従って、接続する新しい [!DNL Couchbase] アカウントを作成でき [!DNL Platform]ます。

「 [Adobe Experience Platform](https://platform.adobe.com) 」にログインし、左のナビゲーションバーで「 **[!UICONTROL ソース]** 」を選択して「 *[!UICONTROL ソース]* 」ワークスペースにアクセスします。 カ *[!UICONTROL タログ]* 画面には様々なソースが表示され、このソースを使用してインバウンドアカウントを作成できます。各ソースには既存のアカウントの数と関連するデータセットフローが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索オプションを使用して、使用する特定のソースを見つけることもできます。

「 *[!UICONTROL Databases]* 」 **[!UICONTROL カテゴリで、「]** Couchbase **」を選択し、「+」アイコン(+)** をクリックして新しい [!DNL Couchbase] コネクタを作成します。

![カタログ](../../../../images/tutorials/create/couchbase/catalog.png)

Couchbase *[!UICONTROL に接続]* ページが表示されます。 このページでは、新しい秘密鍵証明書または既存の秘密鍵証明書を使用できます。

### 新しいアカウント

新しい資格情報を使用する場合は、「 **[!UICONTROL 新規アカウント]**」を選択します。 表示される入力フォームで、接続に名前、オプションの説明および [!DNL Couchbase] 資格情報を入力します。 完了したら、「 **[!UICONTROL Connect to source]** 」を選択し、新しいアカウントが確立されるまでの時間を許可します。

![connect](../../../../images/tutorials/create/couchbase/new.png)

### 既存のアカウント

既存のアカウントに接続するには、接続する [!DNL Couchbase] アカウントを選択し、右上隅の「 **[!UICONTROL 次へ]** 」を選択して次に進みます。

![既存の](../../../../images/tutorials/create/couchbase/existing.png)

## 次の手順

このチュートリアルに従って、ア [!DNL Couchbase] カウントへの接続を確立しました。 次のチュートリアルに進み、データをPlatformに取り込むようにデータフローを [設定できます](../../dataflow/databases.md)。