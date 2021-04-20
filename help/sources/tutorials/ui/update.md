---
keywords: Experience Platform；ホーム；人気の高いトピック；アカウントの更新
description: 状況によっては、既存のソースアカウントの詳細の更新が必要になる場合があります。 ソースワークスペースでは、既存のバッチ接続またはストリーミング接続（名前、説明、秘密鍵証明書など）の詳細を追加、編集および削除できます。
solution: Experience Platform
title: UIでのソース接続アカウントの詳細の更新
topic: overview
type: Tutorial
translation-type: tm+mt
source-git-commit: 4a7405e2c8c97442d2781295dd827c6940aa33eb
workflow-type: tm+mt
source-wordcount: '455'
ht-degree: 10%

---


# UIでのアカウントの詳細の更新

状況によっては、既存のソースアカウントの詳細の更新が必要になる場合があります。 [!UICONTROL ソース]ワークスペースでは、既存のバッチ接続またはストリーミング接続の詳細（名前、説明、秘密鍵証明書など）を追加、編集および削除できます。

このチュートリアルでは、[!UICONTROL ソース]ワークスペースから既存のアカウントの詳細と秘密鍵証明書を更新する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

- [ソース](../../home.md):Experience Platformを使用すると、様々なソースからデータを取り込むことができ、Platform Servicesを使用して、データの構造化、ラベル付け、および入力データの拡張を行うことができます。
- [サンドボックス](../../../sandboxes/home.md)：Experience Platform は、単一の Platform インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

## アカウントの更新

[Experience PlatformUI](https://platform.adobe.com)にログインし、左側のナビゲーションから「**[!UICONTROL ソース]**」を選択して[!UICONTROL ソース]ワークスペースにアクセスします。 上部のヘッダーから「**[!UICONTROL アカウント]**」を選択して、既存のアカウントを表示します。

![カタログ](../../images/tutorials/update/catalog.png)

**[!UICONTROL アカウント]**&#x200B;ページが表示されます。 このページには、ソース、ユーザー名、データ・フロー数、作成日など、表示可能なアカウントのリストが表示されます。

左上のフィルターアイコン![フィルター](../../images/tutorials/update/filter.png)を選択して、並べ替えパネルを起動します。

![アカウントリスト](../../images/tutorials/update/accounts-list.png)

並べ替えパネルには、すべてのソースのリストが表示されます。 リストから複数のソースを選択して、様々なソースに関連付けられたアカウントのフィルタされた選択範囲にアクセスできます。

既存のアカウントのリストを表示するために操作するソースを選択します。 更新するアカウントを特定したら、アカウント名の横にある三点リーダー(`...`)を選択します。

![アカウントの並べ替え](../../images/tutorials/update/accounts-sort.png)

ドロップダウンメニューが表示され、**[!UICONTROL 追加データ]**、**[!UICONTROL 詳細を編集]**、**[!UICONTROL 削除]**&#x200B;の各オプションが表示されます。 メニューから[**[!UICONTROL 詳細を編集]**]を選択して、アカウントを更新します。

![update](../../images/tutorials/update/update.png)

**[!UICONTROL アカウントの詳細を編集]**&#x200B;ダイアログボックスを使用すると、アカウントの名前、説明、認証資格情報を更新できます。 必要な情報を更新したら、「**[!UICONTROL 保存]**」を選択します。

![edit-account-details](../../images/tutorials/update/edit-account-details.png)

しばらくすると、画面下部に、更新が成功したことを確認する確認ボックスが表示されます。

![更新確認済み](../../images/tutorials/update/update-confirmed.png)

## 次の手順

このチュートリアルに従うと、[!UICONTROL ソース]ワークスペースを使用して、既存のソースアカウントの情報を更新できます。

[!DNL Flow Service] APIを使用してプログラムでこれらの操作を実行する手順については、[Flow Service API](../../tutorials/api/update.md)を使用した接続情報の更新に関するチュートリアルを参照してください。