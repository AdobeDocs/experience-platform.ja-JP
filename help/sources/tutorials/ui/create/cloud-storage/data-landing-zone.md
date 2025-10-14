---
title: UI を使用したExperience Platformへのデータランディングゾーンの接続
description: Experience Platform ユーザーインターフェイスを使用して、データランディングゾーンソースコネクタを作成する方法を説明します。
exl-id: 653c9958-5d89-4b0c-af3d-a3e74aa47a08
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '780'
ht-degree: 10%

---

# UI を使用した [!DNL Data Landing Zone] のExperience Platformへの接続

>[!IMPORTANT]
>
>このページは、Experience Platformの [!DNL Data Landing Zone] *ソース* コネクタに固有のページです。 [!DNL Data Landing Zone] *宛先* コネクタへの接続について詳しくは、[[!DNL Data Landing Zone]  宛先ドキュメントページ &#x200B;](/help/destinations/catalog/cloud-storage/data-landing-zone.md) を参照してください。

[!DNL Data Landing Zone] は、ファイルをAdobe Experience Platformに取り込むための、セキュリティで保護されたクラウドベースのファイルストレージ機能です。 データは、7 日後に [!DNL Data Landing Zone] から自動的に削除されます。

このチュートリアルでは、Experience Platform ユーザーインターフェイスを使用して [!DNL Data Landing Zone] ソース接続を作成する手順について説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [&#x200B; ソース &#x200B;](../../../../home.md):Experience Platformを使用すると、データを様々なソースから取得しながら、Experience Platform サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [&#x200B; サンドボックス &#x200B;](../../../../../sandboxes/home.md): Experience Platformには、1 つのExperience Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

## [!DNL Data Landing Zone] からExperience Platformへのファイルの取り込み

>[!IMPORTANT]
>
> ソースに接続するには、**[!UICONTROL ソースの表示]** および **[!UICONTROL ソースの管理]** アクセス制御権限が必要です。 [アクセス制御の概要](../../../../../access-control/home.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

Experience Platformの UI で、左側のナビゲーションから **[!UICONTROL Sources]** を選択し、[!UICONTROL Sources] ワークスペースにアクセスします。 [!UICONTROL カタログ]画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、検索バーを使用して、利用したい特定のソースを見つけることもできます。

[!UICONTROL &#x200B; クラウドストレージ &#x200B;] カテゴリで、「[!DNL Data Landing Zone]」を選択し、次に「**[!UICONTROL データを追加]**」を選択します。

![&#x200B; データランディングゾーンが選択されたソースカタログ &#x200B;](../../../../images/tutorials/create/dlz/catalog.png)

[!UICONTROL &#x200B; データを追加 &#x200B;] 手順が表示され、Experience Platformに取り込むデータを選択およびプレビューするためのインターフェイスが利用できます。

* インターフェイスの左側はフォルダーブラウザーで、コンテナにあるファイルのリストが表示されます。このファイルはExperience Platformに取り込むことができます。
* インターフェイスの右側の部分では、互換性のあるファイルから最大 100 行のデータをプレビューできます。

Experience Platformに取り込むファイルを選択し、適切なインターフェイスでプレビュー画面が更新されるまでしばらく待ちます。

![&#x200B; ソースワークスペースのデータインターフェイスの追加。](../../../../images/tutorials/create/dlz/add-data.png)

>[!TIP]
>
>Experience Platformは、ファイルのデータフォーマット、指定された列区切り文字、圧縮タイプなど、選択したファイルのプロパティ情報を自動検出します。

プレビューインターフェイスを使用すると、ファイルの内容と構造を検査できます。 デフォルトでは、プレビューインターフェイスには、選択したフォルダー内の最初のファイルが表示されます。

別のファイルをプレビューするには、検査するファイル名の横にあるプレビューアイコンを選択します。

終了したら、「**[!UICONTROL 次へ]**」を選択します。

![&#x200B; ソースワークスペースのデータプレビューページ。](../../../../images/tutorials/create/dlz/file-detection.png)

クラウドストレージソースのデータフローを作成する方法に関する詳細な手順のガイドについては、[Experience Platformにデータを取り込むためのクラウドストレージデータフローの作成 &#x200B;](../../dataflow/batch/cloud-storage.md) のチュートリアルを参照してください。

## [!DNL Data Landing Zone] 資格情報の取得

[!DNL Data Landing Zone] は、Adobe Experience Platform ソースライセンスに付属するソースです。 [!DNL Data Landing Zone] では、SAS URI および SAS トークンベースの認証を使用します。 認証資格情報は [!UICONTROL &#x200B; ソースカタログ &#x200B;] ページから取得できます。

資格情報を取得するには、**[!UICONTROL データランディングゾーン]** カードを選択し、表示される右側のパネルから資格情報をコピーします。

![&#x200B; データランディングゾーンの表示オプションのリスト。](../../../../images/tutorials/create/dlz/view-credentials.png)

ポップオーバーが表示され、コンテナ名、SAS トークン、ストレージアカウント名、SAS URI および有効期限が表示されます。

## [!DNL Data Landing Zone] 資格情報を更新します

[!DNL Data Landing Zone] 資格情報は 90 日後に自動的に期限切れになるように設定されています。有効期限が切れたら、新しい資格情報を使用して [!DNL Data Landing Zone] に再接続する必要があります。 Experience Platformのデータフローは、有効期限が切れる資格情報の影響を受けず、新しい資格情報を使用して新しいデータフローおよび既存のデータフローを引き続き使用できます。

[!DNL Data Landing Zone] 資格情報を更新するには、次の 2 つの方法があります。

>[!BEGINTABS]

>[!TAB  ソースカードを使用 ]

ソースカタログページから資格情報を更新するには、[!DNL Data Landing Zone] ールカードで省略記号（**`...`**）を選択してから、「**[!UICONTROL 資格情報を更新]**」を選択します。

![&#x200B; ソースカードを使用して資格情報を更新します。](../../../../images/tutorials/create/dlz/refresh-with-card.png)

ポップアップウィンドウが表示され、続行する前に確認を求められます。 準備が整ったら、「**[!UICONTROL 認証情報を更新]**」を選択します。

![&#x200B; 認証情報を更新の確認ウィンドウ &#x200B;](../../../../images/tutorials/create/dlz/confirm.png)

>[!TAB  右側のパネルを使用 ]

右側のパネルを使用して資格情報を更新するには、**[!UICONTROL データランディングゾーン]** ソースカードを選択してから **[!UICONTROL その他のアクション]** を選択します。 次に、「**[!UICONTROL 認証情報を更新]**」を選択し、表示されるポップアップウィンドウを使用して確認します。

![&#x200B; 右側のパネルを使用して資格情報を更新します &#x200B;](../../../../images/tutorials/create/dlz/refresh-with-right-rail.png)。

>[!ENDTABS]

## 次の手順

このチュートリアルでは、[!DNL Data Landing Zone] コンテナにアクセスし、資格情報を取得して更新する方法を学びました。 次は、[&#x200B; クラウドストレージからExperience Platformにデータを取り込むためのデータフローの作成 &#x200B;](../../dataflow/batch/cloud-storage.md) に関するチュートリアルに進むことができます。
