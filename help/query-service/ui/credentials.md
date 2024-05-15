---
keywords: Experience Platform;ホーム;人気の高いトピック;クエリサービス;クエリサービス;クエリ;クエリエディター;クエリエディター;クエリエディター;
solution: Experience Platform
title: クエリサービス資格情報ガイド
description: Adobe Experience Platform クエリサービスは、クエリの書き込みと実行、以前に実行したクエリの表示、組織内のユーザーが保存したクエリへのアクセスに使用できるユーザーインターフェイスを提供します。
exl-id: ea25fa32-809c-429c-b855-fcee5ee31b3e
source-git-commit: ba4ff2715d4e3eb71377542ab2361b967cd3ac11
workflow-type: tm+mt
source-wordcount: '1807'
ht-degree: 3%

---

# 資格情報ガイド

Adobe Experience Platform クエリサービスを使用すると、外部クライアントと接続できます。 これらの外部クライアントに接続するには、有効期限のある資格情報または有効期限のない資格情報を使用します。

>[!NOTE]
>
>資格情報パネルは、すべてのユーザーが自動的に使用できるわけではありません。 Adobeアカウントチームに連絡して、 [!UICONTROL 資格情報] 必要に応じて、クエリサービス ワークスペースに含めるタブです。 リクエストされた場合、この変更は組織全体にわたり、Adobeのエンジニアリングチームが実施します。 ユーザーが制御する設定ではありません。

## 資格情報の期限切れ {#expiring-credentials}

>[!CONTEXTUALHELP]
>id="platform_queryservice_credentials_expiringcredentials"
>title="クライアントの SSL モード"
>abstract="クエリサービスに接続されたクライアントで SSL を有効にする必要があります。SSL モードが「必須」に設定されていることを確認してください。"

有効期限が切れる資格情報を使用して、外部クライアントへの接続をすばやく設定できます。

![「有効期限」の資格情報セクションがハイライト表示されたクエリダッシュボードの「資格情報」タブ](../images/ui/credentials/expiring-credentials.png)

この **[!UICONTROL 資格情報の有効期限が切れます]** セクションには次の情報が表示されます。

- **[!UICONTROL ホスト]**：クライアントの接続先ホストの名前。 これには、Platform UI の上部リボンに表示される組織の名前が含まれています。
- **[!UICONTROL ポート]**：接続先のホストのポート番号。
- **[!UICONTROL データベース]**：クライアントの接続先のデータベースの名前。
- **[!UICONTROL ユーザー名]**：クエリサービスへの接続に使用するユーザー名。
- **[!UICONTROL パスワード]**：クエリサービスへの接続に使用するパスワード。 UI のパスワードは、セキュリティのためにハッシュ化されています。 コピーアイコン（![「コピー」アイコン](../images/ui/credentials/copy-icon.png)）を選択して、ハッシュ化されていない完全な資格情報をクリップボードにコピーします。
- **[!UICONTROL PSQL コマンド]**：コマンドラインで PSQL を使用してクエリサービスに接続するためのすべての関連情報を自動的に挿入したコマンド。
- **[!UICONTROL Expires]**：有効期限が切れる資格情報の有効期限の日時。 トークンのデフォルトの有効期間は 24 時間ですが、Admin Consoleの詳細設定で変更できます。

>[!TIP]
>
>クエリサービスへの資格情報の接続が期限切れになるまでのセッションの有効期間を変更するには、に移動してください [Admin Console](https://adminconsole.adobe.com/) 次の画面上のオプションを選択します。 **設定** > **プライバシーとセキュリティ** > **認証設定** > **詳細設定** > **最大セッション時間**.
>
>![「Admin Consoleとセキュリティ」、「認証設定」、「最大セッション時間」がハイライト表示された「セッション設定」タブ。](../images/ui/credentials/max-session-life.png)
>
>について詳しくは、Adobeヘルプドキュメントを参照してください [詳細設定](https://helpx.adobe.com/enterprise/using/authentication-settings.html#advanced-settings) admin console によって提供されます。

### クエリセッション内のCustomer Journey Analyticsデータへの接続 {#connect-to-customer-journey-analytics}

Customer Journey AnalyticsBI 拡張機能をPower BIまたは Tableau と共に使用して、Customer Journey Analyticsにアクセスします [データビュー](https://experienceleague.adobe.com/en/docs/analytics-platform/using/cja-dataviews/data-views) （SQL を使用）。 クエリサービスを BI 拡張機能と統合すると、クエリサービスのセッション内でデータビューに直接アクセスできます。 この統合により、クエリサービスを PostgreSQL インターフェイスとして使用する BI ツールの機能が合理化されます。 この機能により、BI ツールでデータ・ビューを複製する必要がなくなり、プラットフォーム間で一貫性のあるレポートが保証され、BI プラットフォームでのCustomer Journey Analytics・データと他のソースとの統合が簡略化されます。

方法については、ドキュメントを参照してください。 [クエリサービスを様々なデスクトップクライアントアプリケーションに接続する](../clients/overview.md) 例： [Power BI](../clients/power-bi.md) または [Tableau](../clients/tableau.md)

>[!IMPORTANT]
>
>この機能を使用するには、Customer Journey Analyticsワークスペースプロジェクトとデータビューが必要です。

Power BIまたは Tableau でCustomer Journey Analyticsデータにアクセスするには、 [!UICONTROL データベース] ドロップダウンメニューを選択し、を選択します `prod:cja` 使用可能なオプションから。 次に、をコピーします [!DNL Postgres] Power BIまたは Tableau の構成で使用する資格情報パラメーター（Host、Port、Database、Username など）。

![データベースドロップダウンがハイライト表示された「クエリサービスの資格情報」タブ。](../images/ui/credentials/database-dropdown.png)

>[!NOTE]
>
>Power BIまたは Tableau をCustomer Journey Analyticsに接続すると、クエリサービスの「同時セッション」使用権限が消費されます。 追加のセッションとクエリが必要な場合は、追加のアドホッククエリユーザーパック アドオンを購入して、5 つの追加の同時セッションと 1 つの追加の同時クエリを取得できます。

また、クエリエディターまたは Postgres CLI から直接Customer Journey Analyticsデータにアクセスすることもできます。 これを行うには、 `cja` クエリを書き込む際のデータベース。 クエリエディターを参照 [クエリオーサリングガイド](./user-guide.md#query-authoring) クエリの記述、実行、保存の方法について詳しくは、こちらを参照してください。

を参照してください。 [BI 拡張機能ガイド](https://experienceleague.adobe.com/en/docs/analytics-platform/using/cja-dataviews/bi-extension) sql を使用してCustomer Journey Analyticsデータビューにアクセスするための詳細な手順を説明します。

## 有効期限のない資格情報 {#non-expiring-credentials}

有効期限のない資格情報を使用して、外部クライアントへのより永続的な接続を設定できます。

>[!NOTE]
>
>有効期限のない認証情報には、次の制限があります。<br><ul><li>ユーザーは、次のユーザー名とパスワードでログインする必要があります `{technicalAccountId}:{credential}`. 詳しくは、 [資格情報を生成](#generate-credentials) セクション。</li><li>有効期限が切れる認証情報が作成されると、一連の基本権限を持つ新しい役割が作成され、ユーザーがスキーマとデータセットを表示できるようになります。 また、「クエリの管理」権限も、クエリサービスで使用するためにこの役割に割り当てられています。</li><li>サードパーティクライアントは、クエリオブジェクトをリストアウトする際に、期待とは異なるパフォーマンスを発揮する可能性があります。 例えば、次のような一部のサードパーティクライアントです [!DNL DB Visualizer] は、左側のパネルにビュー名を表示しません。 ただし、SELECT クエリ内で呼び出した場合は、ビュー名にアクセスできます。 同様に、 [!DNL PowerUI] SQL を使用して作成された一時ビューをダッシュボードの作成用に選択するようにリストできない場合があります。</li></ul>

### 前提条件

有効期限のない認証情報を生成する前に、Adobe Admin Consoleで次の手順を実行する必要があります。

1. にログインします [Adobe Admin Console](https://adminconsole.adobe.com/) 上部のナビゲーションバーで、関連する組織を選択します。
2. [製品プロファイルを選択します。](../../access-control/ui/browse.md)
3. [両方を設定 **サンドボックス** および **クエリサービス統合の管理** 権限](../../access-control/ui/permissions.md) 製品プロファイル用。
4. [製品プロファイルへの新しいユーザーの追加](../../access-control/ui/users.md) そのため、設定済みの権限が付与されます。
5. [製品プロファイル管理者としてのユーザーの追加](https://helpx.adobe.com/jp/enterprise/using/manage-product-profiles.html) 任意のアクティブな製品プロファイル用のアカウントの作成を許可します。
6. [製品プロファイル開発者としてのユーザーの追加](https://helpx.adobe.com/jp/enterprise/using/manage-developers.html) 統合を作成するために使用します。

権限の割り当て方法について詳しくは、のドキュメントを参照してください。 [アクセス制御](../../access-control/home.md).

必要なすべての権限がAdobe Developer コンソールで設定され、ユーザーが期限切れの資格情報機能を使用できるようになりました。

### 資格情報を生成 {#generate-credentials}

有効期限のない資格情報のセットを作成するには、Platform UI に戻って次を選択します **[!UICONTROL クエリ]** 左側のナビゲーションから「」にアクセスする [!UICONTROL クエリ] ワークスペース。 次に、 **[!UICONTROL 資格情報]** タブの後に **[!UICONTROL 資格情報を生成]**.

![「資格情報」タブと「資格情報を生成」が強調表示されたクエリダッシュボード](../images/ui/credentials/generate-credentials.png)

資格情報を生成するためのダイアログが表示されます。 有効期限のない認証情報を作成するには、次の詳細を指定する必要があります。

- **[!UICONTROL 名前]**：生成する資格情報の名前。
- **[!UICONTROL 説明]**:（任意）生成する資格情報の説明。
- **[!UICONTROL 割り当て先]**：資格情報が割り当てられるユーザー。 この値は、資格情報を作成するユーザーのメールアドレスである必要があります。
- **[!UICONTROL パスワード]** （オプション）資格情報のオプションのパスワード。 パスワードが設定されていない場合、Adobeにより自動的にパスワードが生成されます。

必要な詳細をすべて入力したら、次のオプションを選択します **[!UICONTROL 資格情報を生成]** を入力して資格情報を生成します。

![資格情報を生成ダイアログがハイライト表示されます。](../images/ui/credentials/create-account.png)

>[!IMPORTANT]
>
>条件 **[!UICONTROL 資格情報を生成]** を選択すると、設定の JSON ファイルがローカルマシンにダウンロードされます。 Adobeではそうするため **ではない** 生成された資格情報を記録します。ダウンロードしたファイルを安全に保存し、資格情報の記録を保持する必要があります。
>
>さらに、資格情報が 90 日間使用されない場合、資格情報は削除されます。

設定の JSON ファイルには、テクニカルアカウント名、テクニカルアカウント ID、資格情報などの情報が含まれています。 これは次の形式で提供されます。

```json
{"technicalAccountName":"9F0A21EE-B8F3-4165-9871-846D3C8BC49E@TECHACCT.ADOBE.COM","credential":"3d184fa9e0b94f33a7781905c05203ee","technicalAccountId":"4F2611B8613AA3670A495E55"}
```

生成した認証情報を保存したら、「」を選択します。 **[!UICONTROL 閉じる]**. 有効期限のない資格情報のリストがすべて表示されます。

![有効期限のない資格情報セクションがハイライト表示されたクエリダッシュボードの「資格情報」タブ](../images/ui/credentials/list-credentials.png)

有効期限のない認証情報は、編集または削除できます。 有効期限のない秘密鍵証明書を編集するには、鉛筆アイコン（![鉛筆アイコン。](../images/ui/credentials/edit-icon.png) ）サードパーティリクエストを待機する、特別なコア Adobe JavaScript モジュールです。有効期限のない秘密鍵証明書を削除するには、削除アイコン（![ごみ箱アイコン。](../images/ui/credentials/delete-icon.png)）に設定します。

有効期限のない秘密鍵証明書を編集すると、モーダルが表示されます。 更新する次の詳細を指定できます。

- **[!UICONTROL 名前]**：生成する資格情報の名前。
- **[!UICONTROL 説明]**:（任意）生成する資格情報の説明。
- **[!UICONTROL 割り当て先]**：資格情報が割り当てられるユーザー。 この値は、資格情報を作成するユーザーのメールアドレスである必要があります。

![アカウントを更新ダイアログ。](../images/ui/credentials/update-credentials.png)

必要な詳細をすべて入力したら、次のオプションを選択します **[!UICONTROL アカウントを更新]** をクリックして、資格情報の更新を完了します。

## 資格情報を使用して外部クライアントに接続 {#use-credential-to-connect}

有効期限のある資格情報または有効期限のない資格情報を使用して、Aqua Data Studio、Looker、Power BIなどの外部クライアントと接続できます。 これらの資格情報の入力方法は、外部クライアントによって異なります。 これらの資格情報の使用手順については、外部クライアントのドキュメントを参照してください。

この画像は、有効期限のない資格情報のパスワードを除く、UI で見つかった各パラメーターの場所を示しています。 有効期限のない資格情報は JSON 設定ファイルによって提供されますが、有効期限のある資格情報は下で表示できます。 **資格情報** タブが表示されます。

![「有効期限のある資格情報」セクションがハイライト表示されたクエリワークスペースの「資格情報」タブ](../images/ui/credentials/expiring-credentials.png)

次の表に、外部クライアントに接続するために通常必要なパラメーターの概要を示します。

>[!NOTE]
>
>有効期限のない認証情報を使用してホストに接続する場合、にリストされているすべてのパラメーターを使用する必要があります [!UICONTROL 資格情報の有効期限が切れます] セクション（パスワードとユーザー名を除く）。
>ユーザー名とパスワードを入力する形式では、この例のようにコロンで区切られた値を使用します `username:{your_username}` および `password:{password_string}`.

| パラメーター | 説明 | 例 |
|---|---|---|
| **サーバ/ホスト** | 接続先のサーバー/ホストの名前。 <ul><li>この値は、有効期限のある資格情報と有効期限のない資格情報の両方に使用され、の形式を取ります。 `server.adobe.io`. 値はの下にあります。 **[!UICONTROL ホスト]** が含まれる [!UICONTROL 資格情報の有効期限が切れます] セクション。</ul></li> | `acme.platform.adobe.io` |
| **ポート** | 接続先のサーバー/ホストのポート。 <ul><li>この値は、有効期限のある資格情報と有効期限のない資格情報の両方に使用され、の下にあります。 **[!UICONTROL ポート]** が含まれる [!UICONTROL 資格情報の有効期限が切れます] セクション。</ul></li> | `80` |
| **データベース** | 接続先のデータベース。 <ul><li>この値は、有効期限のある資格情報と有効期限のない資格情報の両方に使用され、次の場所にあります **[!UICONTROL データベース]** が含まれる [!UICONTROL 資格情報の有効期限が切れます] セクション。 </ul></li> | `prod:all` |
| **ユーザー名** | 外部クライアントに接続するユーザーのユーザー名。 <ul><li>この値は、有効期限のある資格情報と有効期限のない資格情報の両方に使用されます。 前は英数字の形式です `@AdobeOrg`. この値はの下にあります。 **[!UICONTROL ユーザー名]**.</li></ul> | `ECBB80245ECFC73E8A095EC9@AdobeOrg` |
| **パスワード** | 外部クライアントに接続するユーザーのパスワード。 <ul><li>有効期限が切れる資格情報を使用している場合は、次の場所にあります。 **[!UICONTROL パスワード]** 内 [!UICONTROL 資格情報の有効期限が切れます] セクション。</li><li>有効期限のない資格情報を使用している場合、この値は、technicalAccountID からの連結引数と、設定 JSON ファイルから取得された資格情報です。 パスワードの値は次の形式で指定します。 `{technicalAccountId}:{credential}`.</li></ul> | <ul><li>有効期限が切れる資格情報のパスワードは、1,000 文字を超える英数字の文字列です。 例は示されません。</li><li>有効期限のない資格情報パスワードは次のとおりです。<br>`4F2611B8613DK3670V495N55:3d182fa9e0b54f33a7881305c06203ee`</li></ul> |

{style="table-layout:auto"}

## 次の手順

これで、有効期限のある資格情報と有効期限のない資格情報の両方の仕組みがわかったので、これらの資格情報を使用して外部クライアントに接続できます。 外部クライアントの詳細については、を参照してください。 [クエリサービスへのクライアントの接続ガイド](../clients/overview.md).
