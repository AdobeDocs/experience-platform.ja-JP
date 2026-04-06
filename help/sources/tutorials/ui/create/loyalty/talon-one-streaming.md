---
title: UIを使用してTalon.OneからExperience Platformにデータをストリーミングする
description: UIを使用してTalon.OneからAdobe Experience Platformにデータをストリーミングする方法を説明します。 このガイドでは、設定、データ選択、データフロー設定について説明します。
badge: ベータ版
last-substantial-update: 2026-04-06T00:00:00Z
exl-id: a92e17dd-123c-4e83-a851-3cf2861751e5
source-git-commit: f3026e0a717c07d95f12e3aeaf380ddc1b87c712
workflow-type: tm+mt
source-wordcount: '961'
ht-degree: 14%

---

# UIを使用して[!DNL Talon.One] データをExperience Platformにストリーミング

>[!AVAILABILITY]
>
>[!DNL Talon.One] ソースはベータ版です。ベータ版のソースの使用について詳しくは、ソースの概要の[条件](../../../../home.md#terms-and-conditions)を参照してください。

このガイドでは、UIのソースワークスペースを使用して、[!DNL Talon.One]からAdobe Experience Platformにデータを接続してストリーミングする方法について説明します。

## はじめに

このチュートリアルは、 Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md)：Experience Platform が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

>[!IMPORTANT]
>
>アカウントをExperience Platformに接続する前に完了する必要がある前提条件の手順については、[[!DNL Talon.One] 概要](../../../../connectors/loyalty/talon-one.md)を参照してください。

## ソースカタログを移動する

Experience Platform UIで、左側のナビゲーションから「**[!UICONTROL Sources]**」を選択して、*[!UICONTROL Sources]* ワークスペースにアクセスします。 *[!UICONTROL Categories]* パネルで適切なカテゴリを選択します。 または、検索バーを使用して、使用する特定のソースに移動します。

[!DNL Talon.One]からデータをストリーミングするには、**[!UICONTROL Talon.One Streaming Events]**&#x200B;の下にある&#x200B;*[!UICONTROL Loyalty]* ソースカードを選択し、**[!UICONTROL Add data]**&#x200B;を選択します。

>[!TIP]
>
>ソースカタログのソースには、特定のソースがまだ認証済みアカウントを持っていない場合、**[!UICONTROL Set up]** オプションが表示されます。 認証済みアカウントが作成されると、このオプションは&#x200B;**[!UICONTROL Add data]**&#x200B;に変更されます。

![Talon.One ストリーミングイベントカードが選択されたUIのソースカタログ。](../../../../images/tutorials/create/talon-one-streaming/catalog.png)

## データの選択

次に、*[!UICONTROL Select data]* インターフェイスを使用して、サンプル JSON ファイルをアップロードし、ソーススキーマを定義します。 この手順では、プレビューインターフェイスを使用して、ペイロードのファイル構造を表示できます。 終了したら「**[!UICONTROL Next]**」を選択します。

![&#x200B; ソースワークフローのデータを選択ステップ &#x200B;](../../../../images/tutorials/create/talon-one-streaming/select-data.png)

## データフローの詳細

次に、データセットとデータフローに関する情報を提供する必要があります。

### データセットの詳細

データセットとは、スキーマ（列/フィールド）とレコード（行）を含むデータのコレクション（通常はテーブル）のストレージおよび管理構造です。 Experience Platformに正常に取り込まれたデータは、データセットとしてデータレイク内に保持されます。

この手順では、既存のデータセットを使用するか、新しいデータセットを作成できます。

>[!NOTE]
>
>既存のデータセットを使用するか、新しいデータセットを作成するかに関係なく、プロファイル **の取り込みに対してデータセットが**&#x200B;有効になっていることを確認する必要があります。

+++プロファイルの取り込み、エラー診断、および部分取り込みを有効にする手順を選択します。

データセットがリアルタイム顧客プロファイルに対して有効になっている場合、この手順では、**[!UICONTROL Profile dataset]**&#x200B;を切り替えて、プロファイル取り込み用のデータを有効にできます。 この手順を使用して、**[!UICONTROL Error diagnostics]**&#x200B;と&#x200B;**[!UICONTROL Partial ingestion]**&#x200B;を有効にすることもできます。

* **[!UICONTROL Error diagnostics]**: データセットのアクティビティとデータフローのステータスを監視する際に、後で参照できるエラー診断を生成するようにソースに指示するには、**[!UICONTROL Error diagnostics]**&#x200B;を選択します。
* **[!UICONTROL Partial ingestion]**：部分的なバッチ取り込みは、特定の設定可能なしきい値まで、エラーを含むデータを取り込む機能です。 この機能を使用すると、正確なデータをすべてExperience Platformに正常に取り込むことができますが、誤ったデータはすべて、無効な理由に関する情報とともに個別にバッチ化されます。

+++

### データフローの詳細

データセットを設定したら、名前、オプションの説明、アラート設定など、データフローの詳細を指定する必要があります。

![&#x200B; データフローの詳細インターフェイス &#x200B;](../../../../images/tutorials/create/talon-one-streaming/dataflow-details.png)

| データフロー設定 | 説明 |
| --- | --- |
| データフロー名 | データフローの名前。 デフォルトでは、読み込まれるファイルの名前が使用されます。 |
| 説明 | （オプション）データフローの簡単な説明。 |
| アラート | Experience Platformは、ユーザーが購読できるイベントベースのアラートを生成できます。これらのオプションを使用すると、実行中のデータフローがこれらのアラートをトリガーできます。  詳しくは、[&#x200B; アラートの概要](../../alerts.md)を参照してください <ul><li>**ソースデータフロー実行開始**：このアラートを選択すると、データフロー実行が開始されたときに通知を受け取ります。</li><li>**ソースデータフローの実行成功**：このアラートを選択すると、データフローがエラーなしで終了した場合に通知を受け取ります。</li><li>**ソースデータフロー実行エラー**: データフロー実行がエラーで終了した場合に通知を受け取るには、このアラートを選択します。</li></ul> |

{style="table-layout:auto"}

## マッピング

マッピングインターフェイスを使用して、Experience Platformにデータを取り込む前に、ソースデータを適切なスキーマフィールドにマッピングします。 詳しくは、UI[の](../../../../../data-prep/ui/mapping.md) マッピングガイドを参照してください。

<!--
>[!TIP]
>
>You can download the [Events and Profile mappings](../../../../images/tutorials/create/capillary/mappings.zip) for [!DNL Capillary] and [import the files to Data Prep](../../../../../data-prep/ui/mapping.md#import-mapping) when you are ready to map your data.
-->

![Talon.One ストリーミングのマッピングインターフェイス。](../../../../images/tutorials/create/talon-one-streaming/mapping.png)

## レビュー

*[!UICONTROL Review]* ステップが表示され、データフローを作成する前にデータフローの詳細を確認できます。 詳細は、次のカテゴリに分類されます。

* **[!UICONTROL Connection]**: アカウント名、ソースプラットフォーム、およびソース名を表示します。
* **[!UICONTROL Assign dataset and map fields]**: ターゲットデータセットと、データセットが準拠しているスキーマを表示します。

詳細が正しいことを確認したら、**[!UICONTROL Finish]**&#x200B;を選択します。

![&#x200B; ソースワークフローのレビューステップ。](../../../../images/tutorials/create/talon-one-streaming/review.png)

## ストリーミングエンドポイント URLの取得

接続を作成すると、ソースの詳細ページが表示されます。 このページには、以前に実行したデータフロー、ID、ストリーミングエンドポイント URLなど、新しく作成した接続の詳細が表示されます。

![&#x200B; ストリーミングエンドポイント URL。](../../../../images/tutorials/create/talon-one-streaming/streaming-endpoint.png)

## データフローの監視

データフローを作成したら、そのデータフローを通じて取り込まれるデータをモニターすると、取り込み速度、成功、エラーに関する情報を確認できます。データフローを監視する方法について詳しくは、[UIでのアカウントとデータフローの監視に関するチュートリアル &#x200B;](../../monitor-streaming.md)を参照してください。

## 既知の制限事項

正確なデータ取り込みを確実にするには、[!DNL Talon.One]のロイヤルティポイントの変更、階層アップグレード、階層ダウングレード通知からコネクタにデータを送信する必要があります。 ロイヤルティポイント変更通知には階層情報が含まれていないので、これらの通知を別のプロファイルデータセットに送信する必要があります。 同じデータセット内でポイント変更されたデータを階層アップグレードまたはダウングレード通知と組み合わせる場合、階層情報はnull値で失われるか上書きされます。 階層のアップグレードとダウングレードの通知は、両方とも階層の詳細を含むため、同じデータセットを使用できます。 取り込み後、プロファイル結合ルールは、最新のポイントと階層情報を反映するように、結合プロファイルを自動的に更新します。
