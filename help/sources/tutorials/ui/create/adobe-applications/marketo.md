---
keywords: Experience Platform；ホーム；人気の高いトピック；Marketoソースコネクタ；Marketoコネクタ；Marketoソース；Marketo
solution: Experience Platform
title: UI でのMarketo Engageソースコネクタの作成
topic-legacy: overview
type: Tutorial
description: このチュートリアルでは、UI でMarketo Engageソースコネクタを作成して B2B データをAdobe Experience Platformに取り込む手順を説明します。
exl-id: a6aa596b-9cfa-491e-86cb-bd948fb561a8
source-git-commit: cffa2edf5746f0412bf8366c32ea777ca1974334
workflow-type: tm+mt
source-wordcount: '1369'
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

この **[!UICONTROL 接続Marketo Engageアカウント]** ページが表示されます。 このページでは、新しいアカウントを使用するか、既存のアカウントにアクセスできます。

### 既存のアカウント

既存のアカウントでデータフローを作成するには、「 」を選択します。 **[!UICONTROL 既存のアカウント]** 次に、 [!DNL Marketo] アカウントを使用します。 選択 **[!UICONTROL 次へ]** をクリックして続行します。

![既存](../../../../images/tutorials/create/marketo/existing.png)

### 新しいアカウント

新しいアカウントを作成する場合は、「 **[!UICONTROL 新しいアカウント]**. 表示される入力フォームで、アカウント名、オプションの説明および [!DNL Marketo] 認証資格情報。 終了したら、「 」を選択します。 **[!UICONTROL ソースに接続]** その後、新しい接続が確立されるまでしばらく時間をかけます。

![新規](../../../../images/tutorials/create/marketo/new.png)

## データセットの選択

作成後、 [!DNL Marketo] アカウントを使用する場合、次の手順で確認できるインターフェイスが表示されます [!DNL Marketo] データセット。

インターフェイスの左半分はディレクトリブラウザで、10 が表示されています。 [!DNL Marketo] データセット。 完全に機能している [!DNL Marketo] ソース接続では、9 つの異なるデータセットを取り込む必要があります。 また、 [!DNL Marketo] アカウントベースマーケティング (ABM) 機能を使用する場合は、次に、10 番目のデータフローを作成して、 [!UICONTROL 特定顧客] データセット。

>[!NOTE]
>
>簡潔にするために、次のチュートリアルではを使用します。 [!UICONTROL 商談] 例として、以下の手順は 10 のいずれかに適用されます。 [!DNL Marketo] データセット。

最初に取り込むデータセットを選択し、次に「 」を選択します。 **[!UICONTROL 次へ]**.

![select-data](../../../../images/tutorials/create/marketo/select-data.png)

## データフローの詳細を入力

この [!UICONTROL データフローの詳細] ページでは、既存のデータセットと新しいデータセットのどちらを使用するかを選択できます。 この処理の間に、 [!UICONTROL プロファイルデータセット], [!UICONTROL エラー診断], [!UICONTROL 部分取り込み]、および [!UICONTROL アラート].

![データフローの詳細](../../../../images/tutorials/create/marketo/dataflow-details.png)

### 既存のデータセットを使用する

既存のデータセットにデータを取り込むには、「 」を選択します。 **[!UICONTROL 既存のデータセット]**. 既存のデータセットは、 [!UICONTROL 詳細検索] 」オプションを使用するか、ドロップダウンメニュー内の既存のデータセットのリストをスクロールします。 データセットを選択したら、データフローの名前と説明を入力します。

![existing-dataset](../../../../images/tutorials/create/marketo/existing-dataset.png)

### 新しいデータセットを使用

新しいデータセットに取り込むには、「 **[!UICONTROL 新しいデータセット]** 次に、出力データセット名とオプションの説明を入力します。 次に、 [!UICONTROL 詳細検索] 」オプションを使用するか、ドロップダウンメニュー内の既存のスキーマのリストをスクロールします。 スキーマを選択したら、データフローの名前と説明を指定します。

![new-dataset](../../../../images/tutorials/create/marketo/new-dataset.png)

### 有効にする [!DNL Profile] およびエラー診断

次に、 **[!UICONTROL プロファイルデータセット]** をに切り替えて、データセットを有効にします。 [!DNL Profile]. これにより、エンティティの属性と動作の全体像を作成できます。 すべてのデータから [!DNL Profile]有効なデータセットは、 [!DNL Profile] および変更は、データフローを保存する際に適用されます。

[!UICONTROL エラー診断] は、データフローで発生するエラーレコードに対して、詳細なエラーメッセージ生成を有効にします。 [!UICONTROL 部分取り込み] では、エラーを含むデータを、手動で定義した特定のしきい値まで取り込むことができます。 詳しくは、 [部分バッチ取得の概要](../../../../../ingestion/batch-ingestion/partial.md) を参照してください。

>[!IMPORTANT]
>
>この [!DNL Marketo] コネクタは、バッチ取り込みを使用してすべての履歴レコードを取り込み、リアルタイム更新にストリーミング取り込みを使用します。 これにより、誤ったレコードを取り込みながら、コネクタのストリーミングを続行できます。 を有効にします。 **[!UICONTROL 部分取り込み]** 切り替えて、 [!UICONTROL エラーしきい値%] を最大にして、データフローが失敗するのを防ぎます。

![profile-and-errors](../../../../images/tutorials/create/marketo/profile-and-errors.png)

### アラートを有効にする

アラートを有効にして、データフローのステータスに関する通知を受け取ることができます。 リストからアラートを選択して、データフローのステータスに関する通知を受け取るようサブスクライブします。 アラートの詳細については、 [UI を使用したソースアラートの購読](../../alerts.md).

データフローへの詳細の指定が完了したら、「 」を選択します。 **[!UICONTROL 次へ]**.

![アラート](../../../../images/tutorials/create/marketo/alerts.png)

## マッピング [!DNL Marketo] XDM フィールドをターゲットにするデータセットソースフィールド

この [!UICONTROL マッピング] 手順が表示され、ソーススキーマのソースフィールドをターゲットスキーマ内の適切なターゲット XDM フィールドにマッピングするためのインターフェイスが提供されます。

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

必要に応じて、フィールドを直接マッピングするか、データ準備関数を使用してソースデータを変換し、計算値または計算値を導出できます。 マッピングインターフェイスの使用に関する包括的な手順については、 [データ準備 UI ガイド](../../../../../data-prep/ui/mapping.md).

![マッピング](../../../../images/tutorials/create/marketo/mapping.png)

マッピングセットの準備が整ったら、「 」を選択します。 **[!UICONTROL 次へ]** 新しいデータフローを作成するまでしばらく待ちます。

## データフローの確認

この **[!UICONTROL レビュー]** 手順が表示され、新しいデータフローを作成する前に確認できます。 詳細は、次のカテゴリに分類されます。

* **[!UICONTROL 接続]**:ソースのタイプ、選択したソースエンティティの関連パス、およびそのソースエンティティ内の列の数を表示します。
* **[!UICONTROL データセットの割り当てとフィールドのマッピング]**:データセットが準拠するスキーマを含め、ソースデータの取り込み先のデータセットを示します。

データフローをレビューしたら、「 」を選択します。 **[!UICONTROL 保存して取り込み]** とは、データフローが作成されるまでしばらく時間をかけます。

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
