---
keywords: Experience Platform；ホーム；人気のトピック；ローカルシステム；ファイルアップロード；CSV のマッピング；CSV ファイルのマッピング；xdm への CSV ファイルのマッピング；xdm への CSV のマッピング；ui ガイド；
solution: Experience Platform
title: UI でのローカルファイルの作成アップロード Source コネクタ
type: Tutorial
description: ローカルシステムのソース接続を作成して、ローカルファイルをExperience Platformに取り込む方法を説明します
exl-id: 9ce15362-c30d-40cc-9d9c-caa650579390
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '777'
ht-degree: 68%

---

# UI でのローカルファイルアップロードソースコネクタの作成

このチュートリアルでは、ユーザーインターフェイスを使用してローカルファイルをExperience Platformに取り込むためのローカルファイルアップロードソースコネクタを作成する手順について説明します。

## はじめに

このチュートリアルは、 Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md)：Experience Platform が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

## Experience Platformへのローカルファイルのアップロード

Experience Platform UI の左側のナビゲーションバーで「**[!UICONTROL ソース]**」を選択し、「[!UICONTROL &#x200B; ソース &#x200B;] ワークスペースにアクセスします。 [!UICONTROL &#x200B; カタログ &#x200B;] 画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

[!UICONTROL &#x200B; ローカルシステム &#x200B;] カテゴリで、「**[!UICONTROL ローカルファイルのアップロード]**」を選択し、次に「**[!UICONTROL データを追加]**」を選択します。

![カタログ](../../../../images/tutorials/create/local/catalog.png)

### 既存のデータセットを使用する

この [!UICONTROL &#x200B; データフローの詳細 &#x200B;] ページでは、CSV データを既存のデータセットに取り込むか、新しいデータセットに取り込むかを選択できます。

CSV データを既存のデータセットに取り込むには、「**[!UICONTROL 既存のデータセット]**」を選択します。既存のデータセットは、「[!UICONTROL 詳細検索]」オプションを使用するか、ドロップダウンメニュー内の既存のデータセットのリストをスクロールして取得することができます。

データセットを選択し、データフローの名前と説明（オプション）を入力します。

このプロセスの間に、[!UICONTROL エラー診断]および[!UICONTROL 部分取り込み]を有効にすることもできます。[!UICONTROL エラー診断]は、データフローで発生するエラーレコードに対して、詳細なエラーメッセージ生成を有効にします。[!UICONTROL 部分取り込み]では、手動で定義した特定のしきい値に到達するまで、エラーを含むデータを取り込むことができます。詳しくは、[バッチ取り込みの概要](../../../../../ingestion/batch-ingestion/partial.md)を参照してください。

![existing-dataset](../../../../images/tutorials/create/local/existing-dataset.png)

### 新しいデータセットの使用

CSV データを新しいデータセットに取り込むには、「**[!UICONTROL 新しいデータセット]**」を選択します。次に、出力データセット名とオプションの説明を入力します。次に、「[!UICONTROL 詳細検索]」オプションを使用するか、ドロップダウンメニュー内の既存のスキーマのリストをスクロールすることで、マッピングするスキーマを選択します。

スキーマを選択し、データフローの名前とオプションの説明を入力して、データフローの[!UICONTROL エラー診断]および[!UICONTROL 部分取り込み]の設定を適用します。終了したら、「**[!UICONTROL 次へ]**」を選択します。

![new-dataset](../../../../images/tutorials/create/local/new-dataset.png)

### データの選択

[!UICONTROL データの選択]手順が表示され、ローカルファイルをアップロードおよび構造と内容をプレビューするためのインターフェイスが表示されます。「**[!UICONTROL ファイルを選択]**」をクリックして、ローカルシステムから CSV ファイルをアップロードします。または、アップロードする CSV ファイルを[!UICONTROL ファイルをドラッグ＆ドロップ]パネルにドラッグ＆ドロップすることもできます。

>[!TIP]
>
>現在、ローカルファイルのアップロードでは CSV ファイルのみがサポートされています。 各ファイルの最大ファイルサイズは 1 GB です。

![choose-files](../../../../images/tutorials/create/local/choose-files.png)

ファイルがアップロードされると、プレビューインターフェイスが更新され、ファイルの内容と構造が表示されます。

![preview-sample-data](../../../../images/tutorials/create/local/preview-sample-data.png)

ファイルに応じて、ソースデータの列区切り文字（タブ、コンマ、パイプ、カスタム列区切り文字など）を選択できます。**[!UICONTROL 区切り文字]**&#x200B;ドロップダウン矢印をクリックし、メニューから適切な区切り文字を選択します。

終了したら、「**[!UICONTROL 次へ]**」を選択します。

![区切り文字](../../../../images/tutorials/create/local/delimiter.png)

## マッピング

[!UICONTROL マッピング]手順が表示され、ソーススキーマのソースフィールドを、ターゲットスキーマの適切なターゲット XDM フィールドにマッピングするためのインターフェイスが提供されます。

必要に応じて、フィールドを直接マッピングするか、データ準備機能を使用してソースデータを変換して計算値を導き出すかを選択できます。マッピングインターフェイスの使用に関する包括的な手順については、[データ準備 UI ガイド](../../../../../data-prep/ui/mapping.md)を参照してください。

マッピングセットの準備が整ったら「**[!UICONTROL 終了]**」を選択し、新しいデータフローが作成されるまでしばらく待ちます。

![マッピング](../../../../images/tutorials/create/local/mapping.png)

## データ取得の監視

CSV ファイルがマッピングされ、作成されたら、監視ダッシュボードを使用して、CSV ファイルを通じて取り込まれるデータを監視できます。 詳しくは、[UI でのソースデータフローの監視 ](../../../../../dataflows/ui/monitor-sources.md) のチュートリアルを参照してください。

## 次の手順

このチュートリアルに従うと、フラットな CSV ファイルを XDM スキーマにマッピングし、Experience Platformに取り込むことができます。 [!DNL Real-Time Customer Profile] などのダウンストリームの [!DNL Experience Platform] サービスで、このデータを使用できるようになりました。詳しくは、[[!DNL Real-Time Customer Profile]](../../../../../profile/home.md) の概要を参照してください。
