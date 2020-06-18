---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: UIで汎用ODataソースコネクタを作成する
topic: overview
translation-type: tm+mt
source-git-commit: 7328226b8349ffcdddadbd27b74fc54328b78dc5
workflow-type: tm+mt
source-wordcount: '515'
ht-degree: 0%

---


# UIで汎用ODataソースコネクタを作成する

> [!NOTE]
> 汎用ODataコネクタはベータ版です。 ベータラベル付きのコネクタの使用について詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) 「」を参照してください。

Adobe Experience Platformのソースコネクタは、外部ソースのデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、Platformユーザーインターフェイスを使用してGeneric Open Data Protocol（以下「OData」という）ソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルでは、次のAdobe Experience Platformのコンポーネントについて十分に理解している必要があります。

* [Experience Data Model(XDM)System](../../../../../xdm/home.md): Experience Platformが顧客体験データを編成する際に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md): XDMスキーマの基本構成要素について説明します。この基本構成要素には、スキーマ構成における主な原則とベストプラクティスが含まれます。
   * [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md): スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
* [リアルタイム顧客プロファイル](../../../../../profile/home.md): 複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

既に有効なOData接続がある場合は、このドキュメントの残りの部分をスキップし、プロトコルデータセットフローの [設定に関するチュートリアルに進むことができます](../../dataflow/protocols.md)

### 必要な資格情報の収集

PlatformでODataアカウントにアクセスするには、次の値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `url` | ODataサービスのルートURLです。 |

使い始める前に詳しくは、 [このODataドキュメントを参照してください](https://www.odata.org/getting-started/basic-tutorial/)。

## ODataアカウントの接続

必要な資格情報を収集したら、次の手順に従って新しいODataアカウントを作成し、Platformに接続します。

「 <a href="https://platform.adobe.com" target="_blank">Adobe Experience Platform</a> 」にログインし、左のナビゲーションバーで「 **ソース** 」を選択して「 *ソース* 」ワークスペースにアクセスします。 「 *カタログ* 」画面には様々なソースが表示され、このソースから受信アカウントを作成できます。 各ソースには、既存のアカウントの数と、それらに関連付けられたデータセットフローが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索オプションを使用して、使用する特定のソースを見つけることもできます。

「 *プロトコル* 」カテゴリの下で、「 **汎用OData** 」を選択して、情報バーを画面の右側に表示します。 情報バーには、選択したソースの簡単な説明と、ソースまたは表示のドキュメントに接続するためのオプションが表示されます。 新しい受信接続を作成するには、「 **接続ソース**」を選択します。

![カタログ](../../../../images/tutorials/create/odata/catalog.png)

「 *汎用ODataに* 接続」ページが表示されます。 このページでは、新しい秘密鍵証明書または既存の秘密鍵証明書を使用できます。

### 新しいアカウント

新しい資格情報を使用する場合は、「 **新規アカウント**」を選択します。 表示される入力フォームで、接続に名前、オプションの説明、OData資格情報を入力します。 完了したら、[ **接続** ]を選択し、新しいアカウントが確立されるまでの時間を許可します。

![connect](../../../../images/tutorials/create/odata/connect.png)

### 既存のアカウント

既存のアカウントに接続するには、接続するODataアカウントを選択し、「 **次へ** 」を選択して次に進みます。

![既存の](../../../../images/tutorials/create/odata/existing.png)

## 次の手順

このチュートリアルに従うことで、ODataアカウントへの接続を確立できました。 次のチュートリアルに進み、プロトコルのデータをPlatformに送り込むためのデータセットフローを [設定できるようになりました](../../dataflow/protocols.md)。