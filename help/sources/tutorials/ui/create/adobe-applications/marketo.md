---
keywords: Experience Platform；ホーム；人気の高いトピック；Marketoソースコネクタ；Marketoコネクタ；Marketoソース；Marketo
solution: Experience Platform
title: UI でのMarketo Engageソースコネクタの作成
topic-legacy: overview
type: Tutorial
description: このチュートリアルでは、UI でMarketo Engageソースコネクタを作成して B2B データをAdobe Experience Platformに取り込む手順を説明します。
exl-id: a6aa596b-9cfa-491e-86cb-bd948fb561a8
source-git-commit: 21617c6ec364fc05d7b8b6d00daa68608d1ed318
workflow-type: tm+mt
source-wordcount: '1336'
ht-degree: 6%

---

# の作成 [!DNL Marketo Engage] UI のソースコネクタ

このチュートリアルでは、 [!DNL Marketo Engage] （以下「」という。）[!DNL Marketo]&quot;)B2B データをAdobe Experience Platformに取り込むための UI 内のソースコネクタ

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ソース](../../../../home.md):Experience Platformを使用すると、様々なソースからデータを取り込みながら、Platform サービスを使用して、受信データの構造化、ラベル付け、拡張をおこなうことができます。
* [エクスペリエンスデータモデル (XDM)](../../../../../xdm/home.md):Experience Platformが顧客体験データを整理する際に使用する標準化されたフレームワーク。
   * [UI でのスキーマの作成と編集](../../../../../xdm/ui/resources/schemas.md):UI でスキーマを作成および編集する方法について説明します。
* [ID 名前空間](../../../../../identity-service/namespaces.md):ID 名前空間は、 [!DNL Identity Service] id が関連するコンテキストのインジケーターとして機能する 完全修飾 ID には、ID 値と名前空間が含まれます。
* [[!DNL Real-time Customer Profile]](/help/profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。
* [サンドボックス](../../../../../sandboxes/home.md)：Experience Platform は、単一の Platform インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

### 必要な資格情報の収集

次の項目にアクセスするには、 [!DNL Marketo] プラットフォームのアカウントでは、次の値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `munchkinId` | Munchkin ID は、特定の [!DNL Marketo] インスタンス。 |
| `clientId` | の一意のクライアント ID [!DNL Marketo] インスタンス。 |
| `clientSecret` | の一意のクライアント秘密鍵 [!DNL Marketo] インスタンス。 |

これらの値の取得について詳しくは、 [[!DNL Marketo] 認証ガイド](../../../../connectors/adobe-applications/marketo/marketo-auth.md).

必要な資格情報を収集したら、次の節の手順に従います。

## 接続 [!DNL Marketo] アカウント

Platform UI で、「 **[!UICONTROL ソース]** 左側のナビゲーションバーから [!UICONTROL ソース] ワークスペース。 この [!UICONTROL カタログ] 画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索バーを使用して、作業対象の特定のソースを見つけることもできます。

以下 [!UICONTROL Adobe] カテゴリ、選択 **[!UICONTROL Marketo Engage]**. 次に、 **[!UICONTROL データを追加]** 新しい [!DNL Marketo] データフロー。

![カタログ](../../../../images/tutorials/create/marketo/catalog.png)

この **[!UICONTROL Marketo Engageに接続]** ページが表示されます。 このページでは、新しいアカウントを使用するか、既存のアカウントにアクセスできます。

### 新しいアカウント

新しいアカウントを作成する場合は、「 **[!UICONTROL 新しいアカウント]**. 表示される入力フォームで、アカウント名、オプションの説明および [!DNL Marketo] 認証資格情報。 終了したら、「 」を選択します。 **[!UICONTROL ソースに接続]** その後、新しい接続が確立されるまでしばらく時間をかけます。

![新規アカウント](../../../../images/tutorials/create/marketo/new.png)

### 既存のアカウント

既存のアカウントでデータフローを作成するには、「 」を選択します。 **[!UICONTROL 既存のアカウント]** 次に、 [!DNL Marketo] アカウントを使用します。 選択 **[!UICONTROL 次へ]** をクリックして続行します。

![既存](../../../../images/tutorials/create/marketo/existing.png)

## データセットの選択

作成後、 [!DNL Marketo] アカウントを使用する場合、次の手順で確認できるインターフェイスが表示されます [!DNL Marketo] データセット。

インターフェイスの左半分はディレクトリブラウザで、10 が表示されています。 [!DNL Marketo] データセット。 完全に機能している [!DNL Marketo] ソース接続では、9 つの異なるデータセットを取り込む必要があります。 また、 [!DNL Marketo] アカウントベースマーケティング (ABM) 機能を使用する場合は、次に、10 番目のデータフローを作成して、 [!UICONTROL 特定顧客] データセット。

>[!NOTE]
>
>簡潔にするために、次のチュートリアルではを使用します。 [!UICONTROL 特定顧客] 例として、以下の手順は 10 のいずれかに適用されます。 [!DNL Marketo] データセット。

最初に取り込むデータセットを選択し、次に「 」を選択します。 **[!UICONTROL 次へ]**.

![select-data](../../../../images/tutorials/create/marketo/select-data.png)

## マップ [!DNL Marketo] スキーマから Platform へ

この [!UICONTROL マッピング] ステップが表示され、マッピングするインターフェイスが提供されます [!DNL Marketo] スキーマを Platform に追加します。

取り込むインバウンドデータのデータセットを選択します。 既存のデータセットを使用するか、新しいデータセットを作成できます。

### 既存のデータセットを使用する

既存のデータセットにデータを取り込むには、「 」を選択します。 **[!UICONTROL 既存のデータセット]**「 」、「 」の順に選択し、データセットアイコンを選択します。

![existing-dataset](../../../../images/tutorials/create/marketo/existing-dataset.png)

この **[!UICONTROL データセットを選択]** ダイアログが表示されます。 使用する適切なスキーマのデータセットを見つけ、選択して、「 」を選択します。 **[!UICONTROL 確認]**.

![select-existing-dataset](../../../../images/tutorials/create/marketo/select-dataset.png)

### 新しいデータセットを使用

データを新しいデータセットに取り込むには、「 **[!UICONTROL 新しいデータセット]** をクリックし、提供されたフィールドにデータセットの名前と説明を入力します。

スキーマの名前を **[!UICONTROL スキーマを選択]** 検索バー。 ドロップダウンアイコンを選択して、既存のスキーマのリストを表示することもできます。 または、 **[!UICONTROL 詳細検索]** 既存のスキーマの各ページ（それぞれの詳細を含む）にアクセスする。

切り替え **[!UICONTROL プロファイルデータセット]** ボタンを使用して [!DNL Profile]を使用して、エンティティの属性と動作の全体像を作成できます。 すべてのデータから [!DNL Profile]有効なデータセットは、 [!DNL Profile] および変更は、データフローを保存する際に適用されます。

![create-new-dataset](../../../../images/tutorials/create/marketo/new-dataset-schema.png)

スキーマを選択したら、下にスクロールしてマッピングダイアログを表示し、 [!DNL Marketo] データセットフィールドを適切なターゲット XDM フィールドに設定します。

### マッピング [!DNL Marketo] XDM フィールドをターゲットにするデータセットソースフィールド

各 [!DNL Marketo] データセットには、従うべき独自のマッピングルールがあります。 マッピング方法の詳細については、次を参照してください [!DNL Marketo] データセットから XDM:

* [アクティビティ](../../../../connectors/adobe-applications/mapping/marketo.md#activities)
* [プログラム](../../../../connectors/adobe-applications/mapping/marketo.md#programs)
* [プログラムのメンバーシップ](../../../../connectors/adobe-applications/mapping/marketo.md#program-memberships)
* [会社](../../../../connectors/adobe-applications/mapping/marketo.md#companies)
* [静的リスト](../../../../connectors/adobe-applications/mapping/marketo.md#static-lists)
* [静的リストのメンバーシップ](../../../../connectors/adobe-applications/mapping/marketo.md#static-list-memberships)
* [特定顧客](../../../../connectors/adobe-applications/mapping/marketo.md#named-accounts)
* [商談](../../../../connectors/adobe-applications/mapping/marketo.md#opportunities)
* [オポチュニティ連絡先の役割](../../../../connectors/adobe-applications/mapping/marketo.md#opportunity-contact-roles)
* [人物](../../../../connectors/adobe-applications/mapping/marketo.md#persons)

選択 **[!UICONTROL データをプレビュー]** 選択したデータセットに基づいてマッピング結果を確認するには、次の手順を実行します。

![マッピング](../../../../images/tutorials/create/marketo/mapping.png)

この [!UICONTROL プレビュー] ポップオーバーには、選択したデータセットから最大 100 行のサンプルデータのマッピング結果を調査するためのインターフェイスが用意されています。

![プレビュー](../../../../images/tutorials/create/marketo/mapping-preview.png)

ソースフィールドが適切なターゲットフィールドにマッピングされたら、 **[!UICONTROL 閉じる]**.

## データフローの詳細を入力

この [!UICONTROL データフローの詳細] 手順が表示され、新しいデータフローの名前と簡単な説明を指定できます。

![データフローの詳細](../../../../images/tutorials/create/marketo/dataflow-detail.png)

を有効にします。 **[!UICONTROL エラー診断]** を切り替えて、新しく取り込んだバッチに関する詳細なエラーメッセージを生成し、API を使用してダウンロードできます。 詳しくは、 [データ取得エラー診断の取得](../../../../../ingestion/quality/error-diagnostics.md).

![エラー](../../../../images/tutorials/create/marketo/errors.png)

この [!DNL Marketo] コネクタは、バッチ取り込みを使用してすべての履歴レコードを取り込み、リアルタイム更新にストリーミング取り込みを使用します。 これにより、誤ったレコードを取り込みながら、コネクタのストリーミングを続行できます。 を有効にします。 **[!UICONTROL 部分取り込み]** 切り替えて、 [!UICONTROL エラーしきい値%] を最大にして、データフローが失敗するのを防ぎます。

**[!UICONTROL 部分取り込み]** は、エラーを含むデータを特定のしきい値まで取り込む機能を提供します。 詳しくは、 [部分バッチ取得の概要](../../../../../ingestion/batch-ingestion/partial.md).

データフローの詳細を指定し、エラーしきい値を「最大」に設定したら、「 」を選択します。 **[!UICONTROL 次へ]**.

![部分取り込み](../../../../images/tutorials/create/marketo/partial-ingestion.png)

## データフローの確認

この **[!UICONTROL レビュー]** 手順が表示され、新しいデータフローを作成する前に確認できます。 詳細は、次のカテゴリに分類されます。

* **[!UICONTROL 接続]**:ソースのタイプ、選択したソースエンティティの関連パス、およびそのソースエンティティ内の列の数を表示します。
* **[!UICONTROL データセットの割り当てとフィールドのマッピング]**:データセットが準拠するスキーマを含め、ソースデータの取り込み先のデータセットを示します。

データフローをレビューしたら、「 」を選択します。 **[!UICONTROL 完了]** とは、データフローが作成されるまでしばらく時間をかけます。

![レビュー](../../../../images/tutorials/create/marketo/review.png)

## データフローの監視

データフローを作成したら、そのデータフローを通じて取り込まれるデータを監視して、取り込み率、成功、エラーに関する情報を確認できます。 データフローの監視方法の詳細については、 [UI でのデータフローの監視](../../../../../dataflows/ui/monitor-sources.md).

## 属性を削除

データセット内のカスタム属性を遡って非表示にしたり削除したりすることはできません。 既存のデータセットのカスタム属性を非表示または削除する場合は、このカスタム属性のない新しいデータセット（新しい XDM スキーマ）を作成し、作成する新しいデータセット用に新しいデータフローを設定する必要があります。 また、非表示または削除するカスタム属性を持つデータセットで構成される元のデータフローを無効または削除する必要があります。

## データフローを削除

不要になったデータフローや、 **[!UICONTROL 削除]** 関数 [!UICONTROL データフロー] ワークスペース。 データフローの削除方法の詳細については、 [UI でのデータフローの削除](../../delete.md).

## 次の手順

このチュートリアルに従うことで、を取り込むためのデータフローを正常に作成できました [!DNL Marketo] データ。 受信データは、次のようなダウンストリームの Platform サービスで使用できるようになりました。 [!DNL Real-time Customer Profile] および [!DNL Data Science Workspace]. 詳しくは、次のドキュメントを参照してください。

* [[!DNL Real-time Customer Profile] の概要](/help/profile/home.md)
* [[!DNL Data Science Workspace] の概要](/help/data-science-workspace/home.md)
