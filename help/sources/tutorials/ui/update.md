---
keywords: Experience Platform；ホーム；人気の高いトピック；アカウントの更新
description: 状況によっては、既存のソースアカウントの詳細の更新が必要になる場合があります。 ソースワークスペースでは、既存のバッチ接続またはストリーミング接続（名前、説明、秘密鍵証明書など）の詳細を追加、編集および削除できます。
solution: Experience Platform
title: UIでのソース接続アカウントの詳細の更新
topic: 概要
type: チュートリアル
translation-type: tm+mt
source-git-commit: 04cf2cc1f15d9a673a0753643fc6263bcaf41464
workflow-type: tm+mt
source-wordcount: '681'
ht-degree: 7%

---


# UIでのアカウントの詳細の更新

状況によっては、既存のソースアカウントの詳細の更新が必要になる場合があります。 [!UICONTROL ソース]ワークスペースでは、既存のバッチ接続またはストリーミング接続の詳細（名前、説明、秘密鍵証明書など）を追加、編集および削除できます。

[!UICONTROL ソース]ワークスペースでは、バッチデータフローのスケジュールを編集する機能も提供され、取り込み頻度と間隔を更新できます。

このチュートリアルでは、[!UICONTROL ソース]ワークスペースから既存のアカウントの詳細と秘密鍵証明書を更新し、データフローのインジェストスケジュールを更新する手順を説明します。

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

## スケジュールを編集

**[!UICONTROL アカウント]**&#x200B;ページから、データフローのインジェストスケジュールを編集できます。 アカウントのリストから、再スケジュールするデータフローを含むアカウントを選択します。

![select-account](../../images/tutorials/update/select-account.png)

データフローページが表示されます。 このページには、選択したアカウントに関連付けられた既存のデータフローのリストが含まれます。 再スケジュールするデータフローの横にある三点リーダー(`...`)を選択します。

![再計画](../../images/tutorials/update/reschedule.png)

ドロップダウンメニューが表示され、[**[!UICONTROL スケジュール]**&#x200B;の編集]、[**[!UICONTROL データフロー]**&#x200B;の有効化]、[**[!UICONTROL 表示（監視中）]**]、[**[!UICONTROL 削除]**]の各オプションが表示されます。 メニューから[**[!UICONTROL スケジュール]**&#x200B;の編集]を選択します。

![スケジュールの編集](../../images/tutorials/update/edit-schedule.png)

**[!UICONTROL スケジュールを編集]**&#x200B;ダイアログボックスには、データフローの取り込み頻度と間隔を更新するオプションが表示されます。 更新した頻度と間隔の値を設定したら、「**[!UICONTROL 保存]**」を選択します。

![schedule-dialog-box](../../images/tutorials/update/schedule-dialog-box.png)

| スケジュール設定 | 説明 |
| ---------- | ----------- |
| 頻度 | データフローがデータを収集する頻度。 既存のデータフローの頻度スケジュールを編集する場合に指定できる値は、次のとおりです。`minute`、`hour`、`day`、または`week`。 |
| 間隔 | この間隔は、連続する2つのフローの実行間隔を指定します。 間隔の値は、ゼロ以外の整数で、`15`以上である必要があります。 |

しばらくすると、画面下部に、更新が成功したことを確認する確認ボックスが表示されます。

![スケジュール確認](../../images/tutorials/update/schedule-confirm.png)

## 次の手順

このチュートリアルに従うと、[!UICONTROL ソース]ワークスペースを使用してアカウント情報を更新し、データフロースケジュールを編集することに成功します。

[!DNL Flow Service] APIを使用してプログラムでこれらの操作を実行する手順については、[Flow Service API](../../tutorials/api/update.md)を使用した接続情報の更新に関するチュートリアルを参照してください。