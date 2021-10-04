---
keywords: Experience Platform;ホーム;人気のトピック
solution: Experience Platform
title: 外部オーディエンスのインポートと使用
description: このチュートリアルでは、Adobe Experience Platformで外部オーディエンスを使用する方法について説明します。
topic-legacy: tutorial
exl-id: 56fc8bd3-3e62-4a09-bb9c-6caf0523f3fe
source-git-commit: 82aa38c7bce05faeea5a9f42d0d86776737e04be
workflow-type: tm+mt
source-wordcount: '785'
ht-degree: 9%

---

# 外部オーディエンスのインポートと使用

Adobe Experience Platformは、外部オーディエンスをインポートする機能をサポートしています。これは、後で新しいセグメント定義のコンポーネントとして使用できます。 このドキュメントでは、外部オーディエンスをインポートして使用するExperience Platformを設定するためのチュートリアルを提供します。

## はじめに

このチュートリアルでは、オーディエンスセグメントの作成に関わる様々な [!DNL Adobe Experience Platform] サービスに関する十分な知識が必要です。 このチュートリアルを開始する前に、次のサービスのドキュメントを確認してください。

- [ セグメント化サービス](../home.md)：リアルタイム顧客プロファイルデータからオーディエンスセグメントを作成できます。
- [リアルタイム顧客プロファイル](../../profile/home.md)：複数のソースから集約されたデータに基づいて、統合されたリアルタイムのコンシューマープロファイルを提供します。
- [エクスペリエンスデータモデル（XDM）](../../xdm/home.md)：Platform が顧客エクスペリエンスデータを整理する際に使用する標準化されたフレームワーク。
- [データセット](../../catalog/datasets/overview.md)：Experience Platform のデータ永続化のためのストレージと管理の構成。
- [ストリーミング取り込み](../../ingestion/streaming-ingestion/overview.md):Experience Platformがクライアントサイドおよびサーバーサイドのデバイスからデータをリアルタイムで取り込み、保存する方法。

### セグメントデータとセグメントメタデータ

外部オーディエンスの読み込みと使用を開始する前に、セグメントデータとセグメントメタデータの違いを理解しておくことが重要です。

セグメントデータとは、セグメント認定条件を満たすプロファイルのことで、オーディエンスの一部です。

セグメントメタデータは、セグメント自体に関する情報で、名前、説明、式（該当する場合）、作成日、最終変更日、ID などが含まれます。 ID は、セグメントメタデータを、セグメント認定を満たし、結果として得られるオーディエンスの一部となる個々のプロファイルにリンクします。

| セグメントデータ | セグメントメタデータ |
| ------------ | ---------------- |
| セグメント認定を満たすプロファイル | セグメント自体に関する情報 |

## 外部オーディエンスの ID 名前空間の作成

外部オーディエンスを使用する最初の手順は、ID 名前空間を作成することです。 ID 名前空間を使用すると、Platform はセグメントの送信元を関連付けることができます。

ID 名前空間を作成するには、『[ID 名前空間ガイド ](../../identity-service/namespaces.md#manage-namespaces)』の手順に従います。 ID 名前空間を作成する際に、ソースの詳細を ID 名前空間に追加し、その [!UICONTROL  型 ] を **[!UICONTROL 人以外の識別子]** としてマークします。

![](../images/tutorials/external-audiences/identity-namespace-info.png)

## セグメントメタデータのスキーマの作成

ID 名前空間を作成したら、作成するセグメントの新しいスキーマを作成する必要があります。

スキーマの構成を開始するには、まず左のナビゲーションバーで「**[!UICONTROL スキーマ]**」を選択し、次に「スキーマ」ワークスペースの右上隅にある「**[!UICONTROL スキーマ]** を作成」を選択します。 ここから、「**[!UICONTROL 参照]**」を選択して、使用可能なスキーマタイプの完全な選択を確認します。

![](../images/tutorials/external-audiences/create-schema-browse.png)

セグメント定義（事前定義済みのクラス）を作成するので、「**[!UICONTROL 既存のクラスを使用]**」を選択します。 次に、**[!UICONTROL セグメント定義]** クラスを選択し、**[!UICONTROL クラスを割り当て]** を選択します。

![](../images/tutorials/external-audiences/assign-class.png)

スキーマが作成されたら、セグメント ID を含むフィールドを指定する必要があります。 このフィールドはプライマリ ID としてマークされ、以前作成した名前空間に割り当てられる必要があります。

![](../images/tutorials/external-audiences/mark-primary-identifier.png)

`_id` フィールドをプライマリ ID として指定した後、スキーマのタイトルを選択し、その後に「**[!UICONTROL Profile]**」というラベルの付いた切り替えを選択します。 **[!UICONTROL 有効]** を選択して、[!DNL Real-time Customer Profile] のスキーマを有効にします。

![](../images/tutorials/external-audiences/schema-profile.png)

これで、このスキーマがプロファイルに対して有効になり、作成した個人以外の ID 名前空間にプライマリ ID が割り当てられます。 その結果、このスキーマを使用して Platform に読み込まれたセグメントメタデータは、他のユーザー関連のプロファイルデータと結合されずに、プロファイルに取り込まれます。

## スキーマのデータセットの作成

スキーマを設定した後、セグメントメタデータのデータセットを作成する必要があります。

データセットを作成するには、『[ データセットユーザーガイド ](../../catalog/datasets/user-guide.md#create)』の手順に従ってください。 先ほど作成したスキーマを使用して、「**[!UICONTROL スキーマからデータセットを作成]**」オプションに従います。

![](../images/tutorials/external-audiences/select-schema.png)

データセットを作成したら、『[ データセットユーザーガイド ](../../catalog/datasets/user-guide.md#enable-profile)』の手順に従って、このデータセットをリアルタイム顧客プロファイルに対して有効にします。

![](../images/tutorials/external-audiences/dataset-profile.png)

## オーディエンスデータの設定とインポート

データセットを有効にした状態で、UI を使用して、またはExperience PlatformAPI を使用して、データを Platform に送信できるようになりました。 このデータを Platform に取り込むには、ストリーミング接続を作成する必要があります。

ストリーミング接続を作成するには、[API のチュートリアル ](../../sources/tutorials/api/create/streaming/http.md) または [UI のチュートリアル ](../../sources/tutorials/ui/create/streaming/http.md) の手順に従います。

ストリーミング接続を作成したら、固有のストリーミングエンドポイントにアクセスし、データを送信できます。 これらのエンドポイントにデータを送信する方法については、[ レコードデータのストリーミング ](../../ingestion/tutorials/streaming-record-data.md#ingest-data) に関するチュートリアルを参照してください。

![](../images/tutorials/external-audiences/get-streaming-endpoint.png)

## インポートしたオーディエンスを使用したセグメントの作成

インポートしたオーディエンスを設定したら、セグメント化プロセスの一部として使用できます。 外部オーディエンスを検索するには、セグメントビルダーに移動し、「**[!UICONTROL フィールド]**」セクションで「**[!UICONTROL オーディエンス]**」タブを選択します。

![](../images/tutorials/external-audiences/external-audiences.png)

## 次の手順

これで、セグメントで外部オーディエンスを使用できるようになったので、セグメントビルダーを使用してセグメントを作成できます。 セグメントの作成方法については、[ セグメントの作成に関するチュートリアル ](./create-a-segment.md) を参照してください。
