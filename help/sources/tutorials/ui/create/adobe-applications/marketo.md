---
keywords: Experience Platform；ホーム；人気のあるトピック；Marketoソースコネクタ；Marketoコネクタ；Marketoソース；Marketo
solution: Experience Platform
title: UIでMarketo Engageソースコネクタを作成する
topic-legacy: overview
type: Tutorial
description: このチュートリアルでは、B2BデータをAdobe Experience Platformに取り込むためのMarketo EngageソースコネクタをUIで作成する手順を説明します。
translation-type: tm+mt
source-git-commit: f12baaa9d4b37f1101792a4ae479b5a62893eb68
workflow-type: tm+mt
source-wordcount: '1383'
ht-degree: 4%

---


# （ベータ版）UIに[!DNL Marketo Engage]ソースコネクタを作成する

>[!IMPORTANT]
>
>[!DNL Marketo Engage]ソースは現在ベータ版です。 この機能とドキュメントは、変更される場合があります。 さらに、ベータプログラム中にコネクタを使用する場合は、実稼動用以外のサンドボックスを使用していることを確認する必要があります。 サンドボックスの詳細については、[サンドボックスのドキュメント](https://experienceleague.adobe.com/docs/experience-platform/sandbox/home.html?lang=en#understanding-sandboxes)を参照してください。

このチュートリアルでは、UIに[!DNL Marketo Engage] （以下「[!DNL Marketo]」と呼ばれる）ソースコネクタを作成して、コンシューマデータをAdobe Experience Platformに取り込む手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ソース](../../../../home.md):Experience Platformを使用すると、様々なソースからデータを取り込むことができ、Platform Servicesを使用して、データの構造化、ラベル付け、および入力データの拡張を行うことができます。
* [Experience Data Model(XDM)](../../../../../xdm/home.md):Experience Platformが顧客体験データを編成する際に使用する標準化されたフレームワーク。
   * [UIでのスキーマの作成と編集](../../../../../xdm/ui/resources/schemas.md):UIでスキーマを作成および編集する方法を説明します。
* [ID名前空間](../../../../../identity-service/namespaces.md):ID名前空間は、IDが関連付けら [!DNL Identity Service] れるコンテキストのインジケータとして機能するコンポーネントです。完全修飾 ID には、ID 値と名前空間が含まれます。
* [[!DNL Real-time Customer Profile]](../../../../../profile/home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。
* [サンドボックス](../../../../../sandboxes/home.md)：Experience Platform は、単一の Platform インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

### 必要な資格情報の収集

プラットフォームの[!DNL Marketo]アカウントにアクセスするには、次の値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `munchkinId` | Munchkin IDは、特定の[!DNL Marketo]インスタンスの一意の識別子です。 |
| `clientId` | [!DNL Marketo]インスタンスの一意のクライアントID。 |
| `clientSecret` | [!DNL Marketo]インスタンスの一意のクライアントシークレット。 |

これらの値の取得について詳しくは、[[!DNL Marketo] 認証ガイド](../../../../connectors/adobe-applications/marketo/marketo-auth.md)を参照してください。

必要な資格情報を収集したら、次のセクションの手順に従います。

## [!DNL Marketo]アカウントに接続

プラットフォームUIで、左のナビゲーションバーから「**[!UICONTROL ソース]**」を選択し、[!UICONTROL ソース]ワークスペースにアクセスします。 [!UICONTROL カタログ]画面には様々なソースが表示され、このソースを使用してアカウントを作成できます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索バーを使用して、使用する特定のソースを見つけることもできます。

[!UICONTROL Adobeアプリケーション]カテゴリの下で、**[!UICONTROL Marketo Engage]**&#x200B;を選択します。 次に、**[!UICONTROL 追加data]**&#x200B;を選択して、新しい[!DNL Marketo]データフローを作成します。

![カタログ](../../../../images/tutorials/create/marketo/catalog.png)

「**[!UICONTROL Marketo Engageに接続]**」ページが表示されます。 このページでは、新しいアカウントを使用するか、既存のアカウントにアクセスできます。

### 新しいアカウント

新しいアカウントを作成する場合は、「**[!UICONTROL 新しいアカウント]**」を選択します。 表示される入力フォームで、アカウント名、オプションの説明、[!DNL Marketo]認証資格情報を入力します。 終了したら、[**[!UICONTROL ソースに接続]**]を選択し、新しい接続が確立されるまでの時間を許可します。

![新規勘定](../../../../images/tutorials/create/marketo/new.png)

### 既存のアカウント

既存のアカウントを使用してデータフローを作成するには、「**[!UICONTROL 既存のアカウント]**」を選択し、使用する[!DNL Marketo]アカウントを選択します。 「**[!UICONTROL 次へ]**」を選択して次に進みます。

![既存の](../../../../images/tutorials/create/marketo/existing.png)

## データセットの選択

[!DNL Marketo]アカウントを作成した後、次の手順で[!DNL Marketo]データセットを調査するためのインターフェイスを提供します。

インターフェイスの左半分はディレクトリブラウザで、10 [!DNL Marketo]データセットが表示されています。 完全に機能する[!DNL Marketo]ソース接続には、9種類のデータセットを取り込む必要があります。 [!DNL Marketo's]アカウントベースマーケティング(ABM)機能も使用する場合は、[!UICONTROL 名前付きアカウント]データセットを取り込むための10番目のデータフローも作成する必要があります。

>[!NOTE]
>
>簡潔にするため、次のチュートリアルでは、[!UICONTROL 名前付きアカウント]を例として使用しますが、以下の手順は10個の[!DNL Marketo]データセットのいずれにも適用されます。

取り込むデータセットを最初に選択し、**[!UICONTROL 次へ]**&#x200B;を選択します。

![select-data](../../../../images/tutorials/create/marketo/select-data.png)

## データフィールドのXDMスキーマへのマッピング

[!UICONTROL マッピング]の手順が表示され、[!DNL Marketo]データセットをプラットフォームデータセットにマッピングするためのインターフェイスが提供されます。

取り込む受信データのデータセットを選択します。 既存のデータセットを使用することも、新しいデータセットを作成することもできます。

### 既存のデータセットを使用する

既存のデータセットにデータを取り込むには、**[!UICONTROL 既存のデータセット]**&#x200B;を使用を選択し、データセットアイコンを選択します。

![既存のデータセット](../../../../images/tutorials/create/marketo/existing-dataset.png)

**[!UICONTROL データセットを選択]**&#x200B;ダイアログが表示されます。 使用する適切なスキーマのデータセットを探し、選択して、**[!UICONTROL 確認]**&#x200B;を選択します。

![select-existing-dataset](../../../../images/tutorials/create/marketo/select-dataset.png)

### 新しいデータセットの使用

データを新しいデータセットに取り込むには、**[!UICONTROL 新しいデータセットを作成]**&#x200B;を選択し、提供されたフィールドにデータセットの名前と説明を入力します。

**[!UICONTROL スキーマを選択]**&#x200B;検索バーに名前を入力して、スキーマを検索できます。 ドロップダウンアイコンを選択して、既存のスキーマのリストを表示することもできます。 または、「**[!UICONTROL アドバンス検索]**」を選択して、既存のスキーマのページにそれぞれの詳細を含めてアクセスすることもできます。

**[!UICONTROL プロファイルデータセット]**&#x200B;ボタンを切り替えて[!DNL Profile]のターゲットデータセットを有効にし、エンティティの属性と動作の全体的な表示を作成できます。 すべての[!DNL Profile]有効なデータセットのデータは[!DNL Profile]に含まれ、データフローの保存時に変更が適用されます。

![create-new-dataset](../../../../images/tutorials/create/marketo/new-dataset-schema.png)

スキーマを選択したら、マッピングダイアログが表示します。[!DNL Marketo]データセットフィールドを該当するターゲットXDMフィールドにマッピングする開始にマッピングします。

### [!DNL Marketo]データセットソースフィールドをターゲットのXDMフィールドにマップする

各[!DNL Marketo]データセットには、従うべき固有のマッピングルールがあります。 [!DNL Marketo]データセットをXDMにマップする方法について詳しくは、次を参照してください。

* [アクティビティ](../../../../connectors/adobe-applications/mapping/marketo.md#activities)
* [プログラム](../../../../connectors/adobe-applications/mapping/marketo.md#programs)
* [プログラムメンバーシップ](../../../../connectors/adobe-applications/mapping/marketo.md#program-memberships)
* [会社](../../../../connectors/adobe-applications/mapping/marketo.md#companies)
* [静的リスト](../../../../connectors/adobe-applications/mapping/marketo.md#static-lists)
* [静的リストメンバーシップ](../../../../connectors/adobe-applications/mapping/marketo.md#static-list-memberships)
* [名前付きアカウント](../../../../connectors/adobe-applications/mapping/marketo.md#named-accounts)
* [オポチュニティ](../../../../connectors/adobe-applications/mapping/marketo.md#opportunities)
* [営業案件連絡先ロール](../../../../connectors/adobe-applications/mapping/marketo.md#opportunity-contact-roles)
* [人](../../../../connectors/adobe-applications/mapping/marketo.md#persons)

**[!UICONTROL プレビューデータ]**&#x200B;を選択すると、選択したデータセットに基づくマッピング結果が表示されます。

![マッピング](../../../../images/tutorials/create/marketo/mapping.png)

[!UICONTROL プレビュー]プロバーは、選択したデータセットから最大100行のサンプルデータのマッピング結果を調べるためのインターフェイスを提供します。

![プレビュー](../../../../images/tutorials/create/marketo/mapping-preview.png)

ソースフィールドが適切なターゲットフィールドにマップされたら、「**[!UICONTROL 閉じる]**」を選択します。

## データフローの詳細の指定

[!UICONTROL Dataflow detail]ステップが表示され、新しいデータフローに関する名前と簡単な説明を入力できます。

![データフロー詳細](../../../../images/tutorials/create/marketo/dataflow-detail.png)

「**[!UICONTROL エラー診断]**」トグルを有効にして、新しく取り込んだバッチに関する詳細なエラーメッセージの生成を許可します。このメッセージはAPIを使用してダウンロードできます。

![エラー](../../../../images/tutorials/create/marketo/errors.png)

[!DNL Marketo]コネクタは、バッチ取り込みを使用してすべての履歴レコードを取り込み、リアルタイム更新のためにストリーミング取り込みを使用します。 これにより、エラーのあるレコードを取り込みながら、コネクタのストリーミングを続行できます。 「**[!UICONTROL 部分的なインジェスト]**」切り替えを有効にし、[!UICONTROL エラーしきい値%]を最大値に設定して、データフローの失敗を防ぎます。

**[!UICONTROL 部分]** 的な取り込みでは、エラーを含むデータを特定のしきい値まで取り込むことができます。詳しくは、[部分的なバッチインジェストの概要](../../../../../ingestion/batch-ingestion/partial.md)を参照してください。

データフローの詳細を入力し、エラーしきい値を最大に設定したら、「**[!UICONTROL 次へ]**」を選択します。

![部分摂取](../../../../images/tutorials/create/marketo/partial-ingestion.png)

## データフローの確認

「**[!UICONTROL レビュー]**」ステップが表示され、新しいデータフローを作成前に確認できます。 詳細は次のカテゴリに分類されます。

* **[!UICONTROL 接続]**:ソースの種類、選択したソースファイルの関連パス、およびそのソースファイル内の列数が表示されます。
* **[!UICONTROL データセットとマップのフィールドの割り当て]**:ソースデータが取り込まれるデータセット(データセットに従うスキーマなど)を示します。

データフローをレビューしたら、**[!UICONTROL 「完了」]**&#x200B;を選択し、データフローの作成に時間をかけます。

![レビュー](../../../../images/tutorials/create/marketo/review.png)

## データフローの監視

データフローを作成したら、データフローを介して取り込まれるデータを監視し、取り込み率、成功、エラーに関する情報を確認できます。 データフローの監視方法の詳細については、UI](../../../../../dataflows/ui/monitor-sources.md)の[監視データフローのチュートリアルを参照してください。

## 属性を削除します。

データセット内のカスタム属性を遡って非表示にしたり削除したりすることはできません。 既存のデータセットからカスタム属性を非表示にしたり削除したりする場合は、このカスタム属性と新しいXDMスキーマを持たない新しいデータセットを作成し、作成する新しいデータセット用の新しいデータフローを設定する必要があります。 また、非表示または削除するカスタム属性を持つデータセットから構成される元のデータフローを無効または削除する必要もあります。

## データフローの削除

不要になった、または不適切に作成されたデータフローは、[!UICONTROL Dataflows]ワークスペースにある&#x200B;**[!UICONTROL Delete]**&#x200B;関数を使用して削除できます。 データフローの削除方法の詳細については、UI](../../delete.md)での[データフローの削除に関するチュートリアルを参照してください。

## 次の手順

このチュートリアルに従うと、[!DNL Marketo]データを取り込むデータフローが正常に作成されます。 受信データは、[!DNL Real-time Customer Profile]や[!DNL Data Science Workspace]などのダウンストリームプラットフォームサービスで使用できるようになりました。 詳しくは、次のドキュメントを参照してください。

* [[!DNL Real-time Customer Profile] 概要](../../../../profile/home.md)
* [[!DNL Data Science Workspace] 概要](../../../../data-science-workspace/home.md)
