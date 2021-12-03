---
keywords: Experience Platform;ホーム;人気のトピック;アクセス制御;adobe admin console
solution: Experience Platform
topic-legacy: overview
title: アクセス制御の概要
description: Adobe Experience Platform のアクセス制御は、Adobe Admin Console を通じて提供されます。この機能は、Admin Console の製品プロファイルを利用して、ユーザーを権限およびサンドボックスにリンクします。
exl-id: 591d59ad-2784-4ae4-a509-23649ce712c9
source-git-commit: 2effccfa9b1975292f350369201269099dc1b2a1
workflow-type: ht
source-wordcount: '1383'
ht-degree: 100%

---

# アクセス制御の概要

[!DNL Experience Platform] のアクセス制御は、[Adobe Admin Console](https://adminconsole.adobe.com) を通じて提供されます。この機能は、[!DNL Admin Console] の製品プロファイルを利用して、ユーザーを権限およびサンドボックスにリンクします。

## アクセス制御階層とワークフロー

[!DNL Experience Platform] 用にアクセス制御を設定するには、[!DNL Experience Platform] 製品統合を持つ組織の管理者権限が必要です。権限を付与または取り消す最小の役割は、製品プロファイル管理者です。権限を管理できる他の管理者の役割は、 製品管理者（製品内のすべてのプロファイルを管理）とシステム管理者（制限なし）です。詳しくは、 [管理者の役割](https://helpx.adobe.com/jp/enterprise/using/admin-roles.html) に関する Adobe Help Center の記事を参照してください。

>[!NOTE]
>
> これ以降、このドキュメントでの「管理者」は、（前述の説明に従って）製品プロファイル管理者以上を指します。

アクセス権限を取得し、割り当てるための高度なワークフローを、次のように要約できます。

- Adobe Experience Platform のライセンス認証、または Experience Platform を使用するアプリケーション／アプリケーションサービスのライセンス認証が完了すると、ライセンス認証時に指定した管理者に電子メールが送信されます。
- 管理者は [Adobe Admin Console](#adobe-admin-console) にログインし、概要ページの製品のリストから **Adobe Experience Platform** を選択します。
- 管理者は、必要に応じて、デフォルトの[製品プロファイル](#product-profiles)を表示、または新しい顧客製品プロファイルを作成できます。
- 管理者は、既存の製品プロファイルの権限とユーザーを編集できます。
- 製品プロファイルを作成または編集する際、管理者は「**[!UICONTROL ユーザー]**」タブを使用してプロファイルにユーザーを追加し、「**[!UICONTROL 権限]**」タブにアクセスしてこれらのユーザーに権限（「[!UICONTROL データセットの読み取り]」や「[!UICONTROL スキーマの管理]」など）を付与します。同様に、管理者は同じ「権限」タブを使用してサンドボックスにアクセス権を割り当てることができます。
- ユーザーが [!DNL Experience Platform] ユーザーインターフェイスにログインすると、[!DNL Platform] 機能へのアクセスは、手順 2 でユーザーに付与された権限によって決まります。例えば、ユーザーが「[!UICONTROL データセットの表示]」権限を持っていない場合、サイドメニューの「**[!UICONTROL データセット]**」タブはそのユーザーには表示されません。

[!DNL Experience Platform] でアクセス制御を管理する方法について詳しくは、『[アクセス制御ユーザーガイド](./ui/overview.md)』を参照してください。

[!DNL Experience Platform] API へのすべての呼び出しでは権限が検証され、現在のユーザーコンテキストで適切な権限が見つからない場合はエラーを返します。UI 内では、現在のユーザーに付与されている権限に応じて、要素が非表示になるか変更されます。

## Adobe Admin Console

Adobe Admin Console は、アドビ製品の権限を管理し、組織のアクセス権を一元的に管理するためのツールです。コンソールを使用して、「[!UICONTROL データセットの管理]」、「[!UICONTROL データセットの表示]」、「[!UICONTROL プロファイルの管理]」など、様々な [!DNL Platform] 機能に対するアクセス権限をユーザーのグループに付与できます。

### 製品プロファイル

[!DNL Admin Console] では、製品プロファイルを使用して権限がユーザーに割り当てられます。製品プロファイルを使用すると、1 人または複数のユーザーに権限を付与でき、また、製品プロファイルを通じて割り当てられるサンドボックスの範囲に対するアクセス権を含めることができます。ユーザーは、組織に属する 1 つ以上の製品プロファイルに割り当てることができます。

### デフォルトの製品プロファイル

[!DNL Experience Platform] には、事前設定済みのデフォルト製品プロファイルが 2 つあります。次の表に、各デフォルトプロファイルで提供される内容を示します。これには、ユーザーがアクセスを許可するサンドボックスと、そのサンドボックスの範囲内で許可する権限が含まれます。

| 製品プロファイル | サンドボックスアクセス | 権限 |
| --- | --- | --- |
| デフォルトの実稼働 - すべてのアクセス | 実稼動 | サンドボックス管理権限を除く、[!DNL Experience Platform] に適用されるすべての権限。 |
| サンドボックス管理者 | なし | サンドボックス管理権限へのアクセスのみを提供します。 |

## サンドボックスと権限

非実稼働サンドボックスは、他のサンドボックスからデータを分離できるデータ仮想化の一種で、通常は開発実験、テストまたは試用に使用されます。製品プロファイルの権限は、プロファイルがアクセス権を付与されたサンドボックス環境内の [!DNL Platform] 機能に対するユーザーのアクセスを提供します。デフォルトの Experience Platform ライセンスでは、5 つのサンドボックス（1 つの実稼動環境と 4 つの非実稼動環境）が許可されます。10 個の非実稼動用サンドボックス（合計 75 個まで）のパックを追加できます。詳しくは、IMS 組織管理者またはアドビのセールス担当者にお問い合わせください。

[!DNL Experience Platform] のサンドボックスについて詳しくは、「[サンドボックスの概要](../sandboxes/home.md)」を参照してください。

### サンドボックスへのアクセス

サンドボックスへのアクセスは、製品のプロファイルを通じて管理されます。製品プロファイルのサンドボックスへのアクセスを有効にする手順について詳しくは、『[アクセス制御ユーザーガイド](./ui/overview.md)』を参照してください。

ユーザーには、製品プロファイル内の 1 つ以上のサンドボックスへのアクセス権を付与できます。1 人のユーザーが複数の製品プロファイルに含まれている場合、そのユーザーはそれらのプロファイルに含まれるすべてのサンドボックスにアクセスできます。

「サンドボックスの管理」権限を使用すると、ユーザーはサンドボックスの管理、表示またはリセットをおこなえます。

### 権限 {#permissions}

製品プロファイル内の「権限」タブには、そのプロファイル：に対してアクティブなサンドボックスと権限が表示されます。

![権限の概要](./images/permissions.png)

[!DNL Admin Console] から付与される権限は、カテゴリ別に分類され、一部の権限では複数の低レベル機能へのアクセスが許可されます。

次の表に、[!DNL Admin Console] で使用できる [!DNL Experience Platform] の権限と、権限がアクセス権を付与する特定の [!DNL Platform] 機能の説明を示します。製品プロファイルに権限を追加する手順について詳しくは、『[アクセス制御ユーザーガイド](./ui/overview.md)』を参照してください。

| カテゴリ | 権限 | 説明 |
| --- | --- | --- |
| [!DNL Data Modeling] | [!UICONTROL スキーマの管理] | 各スキーマと関連リソースへの読み取り、作成、編集および削除アクセス |
| [!DNL Data Modeling] | [!UICONTROL スキーマの表示] | スキーマおよび関連リソースへの読み取り専用アクセス |
| [!DNL Data Modeling] | [!UICONTROL 関係の管理] | スキーマ関係の読み取り、作成、編集、および削除へのアクセス。 |
| [!DNL Data Modeling] | [!UICONTROL ID メタデータの管理] | スキーマ の ID メタデータの読み取り、作成、編集、および削除へのアクセス。 |
| [!DNL Data Management] | [!UICONTROL データセットの管理] | データセットへの読み取り、作成、編集、削除アクセススキーマへの読み取り専用アクセス |
| [!DNL Data Management] | [!UICONTROL データセットの表示] | データセットおよびスキーマへの読み取り専用アクセス |
| [!DNL Data Management] | [!UICONTROL データ監視] | 監視データセットおよびストリームへの読み取り専用アクセス |
| [!DNL Profile Management] | [!UICONTROL プロファイルの管理] | 顧客プロファイルに使用するデータセットへの読み取り、作成、編集、削除アクセス使用可能なプロファイルへの読み取り専用アクセス |
| [!DNL Profile Management] | [!UICONTROL プロファイルの表示] | 使用可能なプロファイルへの読み取り専用アクセス |
| [!DNL Profile Management] | [!UICONTROL セグメントの管理] | セグメントの読み取り、作成、編集、および削除へのアクセス。 |
| [!DNL Profile Management] | [!UICONTROL セグメントの表示] | 使用可能なセグメントへの読み取り専用アクセス。 |
| [!DNL Profile Management] | [!UICONTROL 結合ポリシーの管理] | 結合ポリシーの読み取り、作成、編集、および削除へのアクセス。 |
| [!DNL Profile Management] | [!UICONTROL 結合ポリシーの表示] | 使用可能な結合ポリシーへの読み取り専用アクセス。 |
| [!DNL Profile Management] | [!UICONTROL セグメントのオーディエンスの書き出し] | 評価済みのデータセットセグメントをオーディエンスセットに書き出す機能 |
| [!DNL Profile Management] | [!UICONTROL オーディエンスに対するセグメントの評価] | セグメント定義を評価して、オーディエンスのプロファイルを生成する機能。 |
| [!DNL Identities] | [!UICONTROL ID 名前空間の管理] | ID 名前空間への読み取り、作成、編集および削除アクセス |
| [!DNL Identities] | [!UICONTROL ID 名前空間の表示] | ID 名前空間への読み取り専用アクセス |
| [!DNL Sandbox Administration] | [!UICONTROL サンドボックスの管理] | サンドボックスへの読み取り、作成、編集、削除アクセス |
| [!DNL Sandbox Administration] | [!UICONTROL サンドボックスの表示] | 組織に属するサンドボックスへの読み取り専用アクセス |
| [!DNL Sandbox Administration] | [!UICONTROL サンドボックスのリセット] | サンドボックスをリセットする機能 |
| [!DNL Destinations] | [!UICONTROL 宛先の管理] | 宛先への読み取り、作成、編集、無効化アクセス |
| [!DNL Destinations] | [!UICONTROL 宛先の表示] | 「**[!UICONTROL カタログ]**」タブの使用可能な宛先と「**[!UICONTROL 参照]**」タブの認証済みの宛先への読み取り専用アクセス。 |
| [!DNL Destinations] | [!UICONTROL 宛先のアクティブ化] | 作成済みのアクティブな宛先に対するデータのアクティブ化機能この権限を持つユーザーは、宛先をアクティブ化するユーザーに、「宛先の表示」または「[!UICONTROL 宛先]の管理」のいずれかを付与する必要があります。 |
| [!DNL Destinations] | [!UICONTROL 宛先のオーサリング] | [Adobe Experience Platform Destination SDK](../destinations/destination-sdk/overview.md) を使用して宛先を作成する機能。 |
| [!DNL Data Ingestion] | [!UICONTROL ソースの管理] | ソースへの読み取り、作成、編集、無効化アクセス |
| [!DNL Data Ingestion] | [!UICONTROL ソースの表示] | 「**[!UICONTROL カタログ]**」タブでの使用可能なソースおよび「**[!UICONTROL 参照]**」タブでの認証済みのソースへの読み取り専用アクセス |
| [!DNL Data Ingestion] | [!DNL Manage Audience Share Connections] | 2 つの IMS 組織を接続し、[!DNL Segment Match] フローを有効にするパートナーハンドシェイクを作成、承認、拒否するためのアクセス。 |
| [!DNL Data Ingestion] | [!DNL Manage Audience Share] | アクティブなパートナーで [!DNL Segment Match] フィードを読み取り、作成、編集、公開するためのアクセス権。 |
| [!DNL Data Science Workspace] | [!UICONTROL Data Science Workspace の管理] | [!DNL Data Science Workspace] での読み取り、作成、編集、および削除へのアクセス。 |
| データガバナンス | [!UICONTROL データ使用ラベルの適用] | 使用ラベルを読み取り、作成および削除するアクセス権。 |
| データガバナンス | [!UICONTROL データ使用ポリシーの管理] | データ使用ポリシーの読み取り、作成、編集、および削除へのアクセス。 |
| データガバナンス | [!UICONTROL データ使用ポリシーの表示] | 組織に属するデータ使用ポリシーに対する読み取り専用アクセス。 |
| データガバナンス | [!UICONTROL 監査ログの表示] | Platform のアクティビティを記録した [監査ログ](../landing/governance-privacy-security/audit-logs/overview.md) を表示する読み取り専用アクセス。 |
| [!DNL Dashboards] | [!UICONTROL ライセンス使用状況ダッシュボードの表示] | ライセンス使用状況ダッシュボードを表示する読み取り専用アクセス。 |
| [!DNL Dashboards] | [!UICONTROL 標準ダッシュボードの管理] | Data Warehouse にないカスタム属性の追加。 |
| [!DNL Query Service] | [!UICONTROL クエリの管理] | Platform データの構造化 SQL クエリの読み取り、作成、編集、および削除へのアクセス。 |
| [!DNL Query Service] | [!UICONTROL クエリサービス統合の管理] | クエリサービスアクセスの有効期限が切れていない認証情報を作成、更新、削除するためのアクセス。 |

## 次の手順

このガイドを読むことで、[!DNL Experience Platform] のアクセス制御の主な原則を学びました。[!DNL Admin Console] を使用して製品プロファイルを作成して [!DNL Platform] の権限を割り当てる方法に関する詳細な手順について、引き続き『[アクセス制御ユーザーガイド](./ui/overview.md)』をご覧ください。
