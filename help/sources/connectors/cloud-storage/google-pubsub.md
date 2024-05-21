---
title: Google PubSub ソースの概要
description: API またはユーザーインターフェイスを使用して、Google PubSub と Adobe Experience Platform を接続する方法について説明します。
badgeUltimate: label="Ultimate" type="Positive"
exl-id: 7c78173d-2639-47cb-8935-77fb7841a121
source-git-commit: c8fc447631f5382d49022b525a10edeadbd5ab46
workflow-type: tm+mt
source-wordcount: '827'
ht-degree: 19%

---

# [!DNL Google PubSub] ソース

>[!IMPORTANT]
>
>この [!DNL Google PubSub] ソースは、Real-time Customer Data Platform Ultimate を購入したユーザーがソースカタログから利用できます。

Adobe Experience Platform は、[!DNL AWS]、[!DNL Google Cloud Platform]、[!DNL Azure] などのクラウドプロバイダーとのネイティブ接続を提供し、これらのシステムからデータを Platform に取り込み、下流のサービスや配信先で使用できるようにします。

クラウドストレージのソースは、ダウンロード、フォーマット、アップロードの必要なく、データを Platform に取り込むことができます。取り込んだデータは、XDM JSON、XDM Parquet 形式または区切り形式で書式設定できます。 プロセスのすべての手順がソースワークフローに統合されます。 Platform では、[!DNL Google PubSub] からリアルタイムにデータを取り込むことができます。

## 前提条件 {#prerequisites}

この節では、を接続する前に完了する必要がある前提条件の設定について説明します [!DNL Google PubSub] Experience Platformするアカウント。

### サービスアカウントを作成 {#create-service-account}

A **サービスアカウント** は、ユーザーではなく、アプリケーションまたはコンピューティング ワークロードでよく使用されるアカウントのタイプです。 サービスアカウントは、そのアカウントに固有のメールアドレスによって識別されます。

* 一方で、サービス・アカウントは次のとおりです **プリンシパル** - サービスアカウントに次へのアクセスを付与できます。 [!DNL Google Cloud] リソース。 例えば、サービスアカウントに Compute 管理者ロールを付与できます `(roles/compute.admin)` 任意のプロジェクトで。 これにより、サービスアカウントがその特定のプロジェクトの Compute Engine リソースを管理できるようになります。
* 一方、サービスアカウントもリソースです。他のプリンシパルにサービスアカウントへのアクセス権限を付与することができます。 たとえば、ユーザーにサービス アカウント ユーザーの役割を付与できます `(roles/iam.serviceAccountUser)` サービスアカウント上で、ユーザーがそのサービスアカウントをリソースに添付できるようにします。 または、ユーザーにサービスアカウント管理者の役割を付与できます `(roles/iam.serviceAccountAdmin)` これにより、ユーザーはサービス アカウントの表示、編集、無効化、削除などのタスクを完了することができます。

ユースケースに適した認証タイプの決定について詳しくは、こちらを参照してください [[!DNL Google] 認証方法に関するガイド](https://cloud.google.com/docs/authentication).

サービスアカウントを作成するには、以下に説明する手順に従います。

まず、に移動します。 [!DNL IAM] のページ [!DNL Google Developer Console] を選択してから、 **[!DNL Create Service Account]**.

![Google開発者コンソールのサービスアカウントを作成ウィンドウ](../../images/tutorials/create/google-pubsub/create-service-account.png)

次に、サービスアカウントの表示名と ID を入力し、を選択します **[!DNL Create and Continue]**.

![Google Developer Console のサービスアカウントの詳細](../../images/tutorials/create/google-pubsub/service-account-details.png)

### サービスアカウントキーの生成 {#generate-service-account-keys}

サービスアカウントのキーを生成するには、サービスアカウントページでキーヘッダーを選択します。 そこから次を選択します **[!DNL Add key]** を選択してから、 **[!DNL Create new key]** ドロップダウンメニューから。 また、このパネルを使用して、既存のキーをアップロードすることもできます。

![Google Developer Console のキーを追加ウィンドウ](../../images/tutorials/create/google-pubsub/add-key.png)

成功すると、秘密鍵がコンピューターに保存され、ファイルがダウンロードされることを示すメッセージが表示されます。 その後、の作成時に、このファイルの内容を資格情報として使用できます。 [!DNL Google PubSub] Experience Platformのアカウント。

### トピックおよび購読レベルで権限を付与 {#grant-permissions}

トピックおよび購読レベルで権限を付与するには、トピックのコンソールページに移動してから以下を選択します。 **[!DNL Show info panel]**. 次に、の下 [!DNL Permissions] タブ、選択 [!DNL Add Principal] 次に、サービスアカウントプリンシパルを権限と共に追加します。

![トピックおよび購読レベルで権限を付与できる、Google Developer Console のポップアップウィンドウ](../../images/tutorials/create/google-pubsub/add-principal.png)

## 最適な構成 [!DNL Google PubSub usage] {#optimal-configurations}

この節では、の使用を最適化するために推奨される設定について説明します [!DNL Google PubSub] Experience Platformのソース。

### 購読プロパティ {#subscription-properties}

の使用 [!DNL Google Developer Console] 対象： **受信確認の期限を延長する**. これにより、 [!DNL Google Publisher] を使用して、設定した時間に従って待機してからメッセージを再送信します。 この遅延は、加入者レベルでの不要な負荷の低減に役立ちます。

![Google Developer Console の受信確認期限インターフェイス。](../../images/tutorials/create/google-pubsub/acknowledgement-deadline.png)

Enable （有効） **[!DNL exactly one delivery]**. この設定は、 [!DNL Google Publisher] 受信確認の期限が切れる前にサブスクリプションに送信されたメッセージが再送信されないようにするには、を指定します。 この設定を使用すると、受信確認メッセージがサブスクリプションに再送信されなくなります。

![Google Developer Console の 1 つの配信設定ページのみ。](../../images/tutorials/create/google-pubsub/exactly-one-delivery.png)

を有効にできます **[!DNL Retry after exponential backoff delay]** サーバに対してさらに圧倒されるリスクを軽減する。 この設定は、 [!DNL Google Developer Console] 一時的なエラー（通常は自分自身で解決される一時的なエラー）を軽減するには、別の接続を試す前に、システムの回復により多くの時間を費やします。

![Google Developer Console の「ポリシーを再試行」ウィンドウ。](../../images/tutorials/create/google-pubsub/retry-policy.png)

あなたは必要があります **サブスクリプションメッセージの保持期間を 24 時間以上に設定します** 確認されていないデータがピーク時のロード中に失われないようにする。 また、 **配信不能トピックを有効にする** これにより、まれなエッジケースでもデータ損失が発生しないようにします。

>[!IMPORTANT]
>
>作成できるソースデータフローは、1 つあたり 1 つだけです [!DNL Google PubSub] 登録。 複数のサンドボックスにまたがる場合でも、サブスクリプションを再利用すると、データが失われます。

## 接続 [!DNL Google PubSub] Experience Platformへ

以下のドキュメントでは、API やユーザーインターフェイスを使用して [!DNL Google PubSub] と Platform を接続する方法について説明します。

### API の使用

* [Flow Service API を使用した Google PubSub ソース接続の作成](../../tutorials/api/create/cloud-storage/google-pubsub.md)
* [Flow Service API を用いたストリーミングデータの収集](../../tutorials/api/collect/streaming.md)

### UI の使用

* [UI で Google PubSub のソース接続を作成](../../tutorials/ui/create/cloud-storage/google-pubsub.md)
* [UI でのクラウドストレージ接続のデータフローの設定](../../tutorials/ui/dataflow/streaming/cloud-storage-streaming.md)
