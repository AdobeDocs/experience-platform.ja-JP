---
keywords: Experience Platform;ホーム;人気の高いトピック;クエリサービス;クエリサービス;クエリ;クエリエディター;クエリエディター;クエリエディター;
solution: Experience Platform
title: クエリサービス資格情報ガイド
description: Adobe Experience Platform クエリサービスは、クエリの書き込みと実行、以前に実行したクエリの表示、組織内のユーザーが保存したクエリへのアクセスに使用できるユーザーインターフェイスを提供します。
exl-id: ea25fa32-809c-429c-b855-fcee5ee31b3e
source-git-commit: 1b507e9846a74b7ac2d046c89fd7c27a818035ba
workflow-type: tm+mt
source-wordcount: '1975'
ht-degree: 6%

---

# 資格情報ガイド

Adobe Experience Platform クエリサービスを使用すると、外部クライアントと接続できます。 これらの外部クライアントに接続するには、有効期限のある資格情報または有効期限のない資格情報を使用します。

>[!NOTE]
>
>すべてのユーザーに対して、資格情報パネルは、自動的には使用できません。必要に応じて、Adobe アカウントチームに連絡して、「[!UICONTROL Credentials]」タブをクエリサービス ワークスペースに含めてください。 ご要望があれば、この変更は組織全体にわたり、Adobeのエンジニアリングチームが実施します。 ユーザーが制御する設定ではありません。

## 資格情報の期限切れ {#expiring-credentials}

>[!CONTEXTUALHELP]
>id="platform_queryservice_credentials_expiringcredentials"
>title="クライアントの SSL モード"
>abstract="クエリサービスに接続されたクライアントで SSL を有効にする必要があります。SSL モードが「必須」に設定されていることを確認してください。"

有効期限が切れる資格情報を使用して、外部クライアントへの接続をすばやく設定できます。

![クエリ ダッシュボードの [資格情報] タブ、[資格情報の有効期限] セクションが強調表示されます。](../images/ui/credentials/expiring-credentials.png)

**[!UICONTROL Expiring credentials]**&#x200B;セクションには、次の情報が表示されます。

- **[!UICONTROL Host]**:クライアントを接続するホストの名前。 これには、Experience Platform UIの上部のリボンに表示される組織の名前が組み込まれます。
- **[!UICONTROL Port]**:接続先のホストのポート番号。
- **[!UICONTROL Database]**: クライアントを接続するデータベースの名前。
- **[!UICONTROL Username]**: クエリ サービス への接続に使用するユーザー名。
- **[!UICONTROL Password]**: クエリ サービス への接続に使用されるパスワード。 UI内のパスワードは、セキュリティのためにハッシュ化されています。 コピー アイコン (![コピー アイコン) を選択します。](/help/images/icons/copy.png)) をクリックして、ハッシュ化されていない完全な資格情報を クリップボード にコピーします。
- **[!UICONTROL PSQL command]**: コマンド ラインで PSQL を使用してクエリ サービス に接続するためのすべての関連情報を自動的に挿入するコマンド。
- **[!UICONTROL Expires]**:有効期限が近づいている資格情報の有効期限の日時。 トークンのデフォルトの有効期間は24時間ですが、Admin Consoleの詳細設定で変更できます。

>[!TIP]
>
>期限切れの資格情報接続の Query サービス へのセッション有効期間を変更するには、[ [Admin Console](https://adminconsole.adobe.com/) ] に移動し、画面上のオプション **設定** > **プライバシー と Security** > **Authentication設定** > **詳細 設定** > **最大セッション有効期間**&#x200B;を選択します。
>
>![Admin Console設定タブ、プライバシーとセキュリティ、Authentication設定、および最大セッション寿命が強調表示されています。](../images/ui/credentials/max-session-life.png)
>
>管理コンソールが提供する [詳細設定の詳細については](https://helpx.adobe.com/enterprise/using/authentication-settings.html#advanced-settings) Adobe Systemsヘルプのドキュメントをご覧ください。

### クエリセッション内でCustomer Journey Analyticsデータに接続する {#connect-to-customer-journey-analytics}

Power BI または Tableau で Customer Journey Analytics BI 拡張機能を使用して、SQL で Customer Journey Analytics [データビュー](https://experienceleague.adobe.com/ja/docs/analytics-platform/using/cja-dataviews/data-views) にアクセスします。 クエリサービスを BI 拡張機能と統合することにより、クエリサービスセッション内でデータビューに直接アクセスすることができます。 この統合により、PostgreSQL インターフェイスとしてクエリ サービスを使用する BI ツールの機能が合理化されます。 この機能により、BI ツールでデータビュー重複する必要がなくなり、プラットフォーム間で一貫したレポートが確保され、BI プラットフォーム内の他のソースとのCustomer Journey Analyticsデータの統合が簡素化されます。

Query サービス を [Power BI](../clients/overview.md) や [Tableau などのさまざまな デスクトップ クライアント アプリケーションに](../clients/power-bi.md)接続する方法については[ドキュメントを参照してください](../clients/tableau.md)

>[!IMPORTANT]
>
>この機能を使用するには、Customer Journey Analytics ワークスペース プロジェクトとデータ 表示が必要です。

Power BI または Tableau でCustomer Journey Analyticsデータにアクセスするには、[ [!UICONTROL Database] ] ドロップダウン メニューを選択し、使用可能なオプションから [ `prod:cja` ] を選択します。 次へ、Power BI または Tableau 構成で使用するために、 [!DNL Postgres] 資格情報パラメーター (ホスト、ポート、データベース、ユーザー名など) をコピーします。

![[資格情報サービスクエリ] タブ、データベース ドロップダウンが強調表示されます。](../images/ui/credentials/database-dropdown.png)

>[!NOTE]
>
>Power BI または Tableau を Customer Journey Analytics に接続すると、「同時セッション」エンタイトルメントサービスクエリが使用されます。 追加のセッションとクエリが必要な場合は、追加の広告ホッククエリユーザーパックアドオンを購入して、5つの追加の同時セッションと1つの追加の同時クエリを取得できます。

また、クエリエディターまたは Postgres CLI からCustomer Journey Analytics データに直接アクセスすることもできます。 これを行うには、クエリを記述するときに `cja` データベースを参照します。 クエリの記述、実行、保存の方法について詳しくは、クエリエディター [ クエリオーサリングガイド ](./user-guide.md#query-authoring) を参照してください。

SQL を使用してCustomer Journey Analyticsのデータビューにアクセスする手順について詳しくは、[BI 拡張機能ガイド ](https://experienceleague.adobe.com/en/docs/analytics-platform/using/cja-dataviews/bi-extension) を参照してください。

## 資格情報の期限切れなし {#non-expiring-credentials}

>[!CONTEXTUALHELP]
>id="platform_queryservice_credentials_migratenonexpiringcredentials"
>title="OAuth サーバー間の資格情報への移行"
>abstract="JWT 資格情報は 2025年6月30日（PT）以降に機能しなくなるので、この移行は必須です。約 30～40 秒かかり、開始するとキャンセルできません。すべての既存のジョブと統合は、移行後も引き続き OAuth で機能します。この画面を離れて、いつでも戻ってステータスを確認できます。"

有効期限のない資格情報を使用して、外部クライアントへのより永続的な接続を設定できます。

>[!IMPORTANT]
>
>有効期限のない資格情報を OAuth サーバー to サーバー に初めて作成または移行する場合は、システム管理者アカウントを使用する必要があります。 組織でこのアクションを実行できるのはシステム管理者のみです。 システム管理者以外のユーザーがこの手順を試行すると、認証エラーが発生してプロセスが失敗します。 初期セットアップ後、後続の有効期限のない資格情報は、必要なアクセス許可を持つユーザーが作成または移行できます。

>[!NOTE]
>
>有効期限のない資格情報には、次の制限があります。
>
>- ユーザーは、 `{technicalAccountId}:{credential}` の形式にユーザー名とパスワードパスワードを使用してログインする必要があります。 詳細については、「 [資格情報の生成](#generate-credentials) セクションを参照してください。
>- 既定では、有効期限のない資格情報には、 `SELECT` クエリのみを実行するアクセス許可が付与されます。 `CTAS` クエリまたは `ITAS` クエリを実行するには、有効期限のない資格情報に関連付けられている役割に &quot;データセットの管理&quot; アクセス許可と &quot;スキーマの管理&quot; アクセス許可を手動で追加します。「スキーマの管理」権限は「データモデリング」セクションにあり、「データセットの管理」権限は [Adobe Systems開発者コンソールの「データ管理」セクションにあります](https://developer.adobe.com/console/)。
>- サードパーティのクライアントは、クエリオブジェクトをリストアウトするときに予想とは異なる動作をする可能性があります。 たとえば、 [!DNL DB Visualizer] などの一部の サードパーティ クライアントでは、左側のパネルに表示名が表示されません。 ただし、 `SELECT` クエリ内で呼び出した場合は、表示名にアクセスできます。 同様に、 [!DNL PowerUI] は、ダッシュボード作成で選択するために SQL によって作成された一時ビューをリストしない場合があります。

### 前提条件

有効期限のない認証情報を生成する前に、Adobe Admin Consoleで次の手順を実行する必要があります。

1. [Adobe Admin Console](https://adminconsole.adobe.com/) にログインし、上部のナビゲーションバーから関連する組織を選択します。
2. [製品プロファイルを選択します。](../../access-control/ui/browse.md)
3. [製品プロファイルに対して&#x200B;**サンドボックス**&#x200B;権限と&#x200B;**クエリサービス統合の管理**](../../access-control/ui/permissions.md)権限の両方を設定します。
4. [製品プロファイルに新しいユーザー追加](../../access-control/ui/users.md) 設定済みの権限が付与されます。
5. [ユーザーを製品プロファイル管理者として追加](https://helpx.adobe.com/jp/enterprise/using/manage-product-profiles.html) アクティブな製品プロファイルのアカウントの作成を許可します。
6. 統合を作成するには、[ ユーザーを製品プロファイル開発者として追加 ](https://helpx.adobe.com/jp/enterprise/using/manage-developers.html) します。

これらの手順を実行すると、OAuth サーバー間資格情報を生成し、有効期限のある資格情報機能または有効期限のない資格情報機能を使用するために必要な権限が [0}Adobe Developer Console} で設定されます。](https://developer.adobe.com/console/)

権限の割り当てについて詳しくは、[ アクセス制御に関するドキュメント ](../../access-control/home.md) を参照してください。

### 資格情報を生成 {#generate-credentials}

有効期限のない一連の資格情報を作成するには、Experience Platform UI に戻り、左側のナビゲーションから「**[!UICONTROL Queries]**」を選択して、[!UICONTROL Queries] Workspace にアクセスします。 次に、「**[!UICONTROL Credentials]**」タブを選択し、続いて「**[!UICONTROL Generate credentials]**」を選択します。

![ 「資格情報」タブと「資格情報を生成」がハイライト表示されたクエリダッシュボード ](../images/ui/credentials/generate-credentials.png)

資格情報を生成するためのダイアログが表示されます。 有効期限のない認証情報を作成するには、次の詳細を指定する必要があります。

- **[!UICONTROL Name]**：生成する資格情報の名前。
- **[!UICONTROL Description]**:（任意）生成する資格情報の説明。
- **[!UICONTROL Assigned to]**：資格情報の割り当て先となるユーザー。 この値は、資格情報を作成するユーザーのメールアドレスである必要があります。
- **[!UICONTROL Password]** (オプション)資格情報のオプションパスワード。 パスワードが設定されていない場合は、Adobe Systems が自動的にパスワードを生成します。

必要な詳細をすべて指定したら、[ **[!UICONTROL Generate credentials]** ] を選択して資格情報を生成します。

![[資格情報の生成] ダイアログが強調表示されます。](../images/ui/credentials/create-account.png)

>[!IMPORTANT]
>
>**[!UICONTROL Generate credentials]**&#x200B;を選択すると、設定 JSON ファイルがローカルマシンにダウンロードされます。Adobe Systems は生成された資格情報を記録しない&#x200B;****&#x200B;ため、ダウンロードしたファイルを安全にストアし、資格情報の記録を保持する必要があります。
>
>さらに、資格情報が 90 日間使用されない場合、資格情報は消去されます。

構成 JSON ファイルには、技術アカウント名、技術アカウント ID、資格情報などの情報が含まれています。 これは、次の形式で提供されます。

```json
{"technicalAccountName":"9F0A21EE-B8F3-4165-9871-846D3C8BC49E@TECHACCT.ADOBE.COM","credential":"3d184fa9e0b94f33a7781905c05203ee","technicalAccountId":"4F2611B8613AA3670A495E55"}
```

生成された資格情報を保存したら、 [ **[!UICONTROL Close]**] を選択します。 これで、無期限のすべての資格情報のリストを確認できます。

![クエリダッシュボードの [資格情報] タブ、[有効期限のない資格情報] セクションが強調表示されます。](../images/ui/credentials/list-credentials.png)

有効期限のない資格情報は、編集または削除できます。 有効期限のない資格情報を編集するには、鉛筆アイコン (![鉛筆アイコン) を選択します。](/help/images/icons/edit.png)）に設定します。 有効期限のない資格情報を削除するには、削除アイコン (![ごみ箱アイコン](/help/images/icons/delete.png)) を選択します。

有効期限のない資格情報を編集すると、モーダルが表示されます。 更新する次の詳細を指定できます。

- **[!UICONTROL Name]**:生成する資格情報の名前。
- **[!UICONTROL Description]**:(オプション)生成する秘密鍵証明書の説明。
- **[!UICONTROL Assigned to]**:資格情報を割り当てるユーザー。 この値は、資格情報を作成するユーザーの電子メールアドレスである必要があります。

![アカウントの更新ダイアログ。](../images/ui/credentials/update-credentials.png)

必要な詳細をすべて指定したら、[ **[!UICONTROL Update account]** ] を選択して資格情報の更新を完了します。

### 資格情報を OAuth に移行する {#migrate-credentials}

有効期限のない JWT 資格情報を使用している場合は、サービスの中断を回避するために、2025 年 6 月 30 日までにそれぞれを OAuth サーバー-to-サーバーに移行する必要があります。

>[!IMPORTANT]
>
>JWT 資格情報は、2025 年 6 月 30 日以降に機能しなくなります。 認証を維持するには、この移行を手動で完了する必要があります。

影響を受ける資格情報を特定して移行を完了する方法については、「 [JWT から OAuth へのサーバーからサーバーへの資格情報の移行ガイド](./migrate-jwt-to-oauth.md)」を参照してください。

一般的な質問については、 [移行に関する FAQ](./migrate-jwt-to-oauth.md#faq) を参照してください。

## 資格情報を使用して外部クライアントに接続する {#use-credential-to-connect}

有効期限が切れる認証情報または有効期限が近づいていない認証情報を使用して、Aqua データ Studio、Looker、Power BI などの外部クライアントに接続できます。 これらの資格情報の入力方法は、外部クライアントによって異なります。 これらの資格情報の使用に関する具体的な手順については、外部クライアントのドキュメントを参照してください。

この図は、有効期限のない資格情報のパスワードを除き、UIで見つかった各パラメーターの場所を示しています。 有効期限のない資格情報は JSON 構成ファイルによって提供されますが、UIの **資格情報** タブで期限切れの資格情報表示できます。

![[資格情報ワークスペースクエリ] タブ、[期限切れの資格情報] セクションが強調表示されます。](../images/ui/credentials/expiring-credentials.png)

次の表は、外部クライアントへの接続に通常必要なパラメーターの概要を示しています。

>[!NOTE]
>
>有効期限のない資格情報を使用してホストに接続する場合でも、パスワードとユーザー名を除く [!UICONTROL EXPIRING CREDENTIALS] セクションにリストされているすべてのパラメーターを使用する必要があります。
>ユーザー名とパスワードを入力する形式では、この例の `username:{your_username}` と `password:{password_string}` のようにコロン区切りの値を使用します。

| パラメーター | 説明 | 例 |
|---|---|---|
| **サーバー/ホスト** | 接続しているサーバーまたはホストの名前。 <ul><li>この値は、期限切れの資格情報と有効期限のない資格情報の両方に使用され、 `server.adobe.io` の形式を取ります。 値は、**[!UICONTROL Host]**&#x200B;セクションの[!UICONTROL EXPIRING CREDENTIALS]の下にあります。</ul></li> | `acme.platform.adobe.io` |
| **ポート** | 接続しているサーバーまたはホストのポート。 <ul><li>この値は、有効期限のある資格情報と有効期限のない資格情報の両方に使用され、**[!UICONTROL Port]** のセクションの [!UICONTROL EXPIRING CREDENTIALS] にあります。</ul></li> | `80` |
| **データベース** | 接続先のデータベース。 <ul><li>この値は、期限切れの資格情報と有効期限のない資格情報の両方に使用され、[**[!UICONTROL Database]**] セクションの [[!UICONTROL EXPIRING CREDENTIALS]] の下にあります。 </ul></li> | `prod:all` |
| **ユーザー名** | 外部クライアントに接続するユーザーのユーザー名。 <ul><li>この値は、期限切れの資格情報と有効期限のない資格情報の両方に使用されます。 `@AdobeOrg`前に英数字の文字列の形式を取ります。この値は **[!UICONTROL Username]** の下にあります。</li></ul> | `ECBB80245ECFC73E8A095EC9@AdobeOrg` |
| **パスワード** | 外部クライアントに接続するユーザーのパスワード。 <ul><li>有効期限が切れる認証情報を使用している場合は、**[!UICONTROL Password]** のセクション内の [!UICONTROL EXPIRING CREDENTIALS] で確認できます。</li><li>有効期限のない資格情報を使用している場合、この値は、technicalAccountID からの連結引数と、設定 JSON ファイルから取得された資格情報です。 パスワードの値は `{technicalAccountId}:{credential}` 形式で指定します。</li></ul> | <ul><li>有効期限が切れる資格情報のパスワードは、1,000 文字を超える英数字の文字列です。 例は示されません。</li><li>有効期限のない資格情報のパスワードは次のとおりです。<br>`4F2611B8613DK3670V495N55:3d182fa9e0b54f33a7881305c06203ee`</li></ul> |

{style="table-layout:auto"}

## 次の手順

これで、有効期限のある資格情報と有効期限のない資格情報の両方の仕組みがわかったので、これらの資格情報を使用して外部クライアントに接続できます。 外部クライアントの詳細については、[ クエリサービスへのクライアント接続ガイド ](../clients/overview.md) を参照してください。
