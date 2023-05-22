---
keywords: Experience Platform;ホーム;人気のトピック;アクセス制御;adobe admin console
solution: Experience Platform
title: アクセス制御の概要
description: Adobe Experience Platform のアクセス制御は、Adobe Admin Console を通じて提供されます。この機能は、Admin Console の製品プロファイルを利用して、ユーザーを権限およびサンドボックスにリンクします。
exl-id: 591d59ad-2784-4ae4-a509-23649ce712c9
source-git-commit: 88bfcdef65b4a938d573b1beb1952c7e030ebc13
workflow-type: tm+mt
source-wordcount: '1567'
ht-degree: 65%

---

# アクセス制御の概要

Adobe Experience Platformのアクセス制御は、 **[!UICONTROL 権限]** in [Adobe Experience Cloud](https://experience.adobe.com/). この機能は、役割とポリシーを活用し、権限とサンドボックスを持つユーザーをリンクします。

## アクセス制御階層とワークフロー

Experience Platformのアクセス制御を設定するには、Experience Platform製品を持つ組織のシステム管理者権限または製品管理者権限が必要です。 権限を付与または取り消すことができる最小の役割は、製品管理者です。 権限を管理できる他の管理者の役割は、システム管理者です（制限なし）。 詳しくは、 [管理者の役割](https://helpx.adobe.com/jp/enterprise/using/admin-roles.html) に関する Adobe Help Center の記事を参照してください。

>[!NOTE]
>
>これ以降、このドキュメントでの「管理者」は、（前述の説明に従って）製品管理者以上を指します。

アクセス権限を取得し、割り当てるための高度なワークフローを、次のように要約できます。

- Adobe Experience Platform のライセンス認証、または Experience Platform を使用するアプリケーション／アプリケーションサービスのライセンス認証が完了すると、ライセンス認証時に指定した管理者に電子メールが送信されます。
- 管理者は [Adobe Admin Console](#adobe-admin-console) にログインし、概要ページの製品のリストから **Adobe Experience Platform** を選択します。
- 管理者がExperience Platformに対するアクセス権を付与するには、デフォルトの製品プロファイルにユーザーを追加する必要があります。 `AEP-Default-All-Users`.
- Experience Platform権限では、管理者は、新しいロールを作成したり、既存のロールの権限とユーザーを編集したりできます。
- ロールを作成または編集する際、管理者は、 **[!UICONTROL ユーザー]** タブに追加し、これらのユーザーに権限を付与します (「[!UICONTROL データセットの読み取り]&quot;または&quot;[!UICONTROL スキーマを管理]」) をクリックします。 同様に、管理者は同じ編集オプションを使用してサンドボックスにアクセスを割り当てることができます。
- ユーザーがExperience Platformユーザーインターフェイスにログインすると、Experience Platform機能へのアクセスは、前の手順でユーザーに付与された権限によって決まります。 例えば、 [!UICONTROL データセットの表示] 権限 **[!UICONTROL データセット]** 」タブが表示されなくなります。

Experience Platform でアクセス制御を管理する方法について詳しくは、『[アクセス制御ユーザーガイド](./ui/overview.md)』を参照してください。

Experience Platform API へのすべての呼び出しは、権限が検証され、適切な権限が現在のユーザーコンテキストで見つからない場合はエラーを返します。UI 内では、現在のユーザーに付与されている権限に応じて、要素が非表示になるか変更されます。

## 権限 {#platform-permissions}

[!UICONTROL 権限] は、組織のExperience Platformアクセスを一元的に管理します。 ～ [!UICONTROL 権限]を使用すると、ユーザーのグループに、 [!UICONTROL データセットの管理], [!UICONTROL データセットの表示]または [!UICONTROL プロファイルの管理].

### ロール

内 [!UICONTROL 役割] 「 」セクションでは、役割を使用して権限がユーザーに割り当てられます。 役割を使用すると、1 人または複数のユーザーに権限を付与でき、役割を通じて割り当てられるサンドボックスの範囲に対するアクセス権を含めることができます。 ユーザーは、組織に属する 1 つ以上の役割に割り当てることができます。

### デフォルトの役割

Experience Platformには、事前設定済みのデフォルトの役割が 2 つ用意されています。 次の表に、各デフォルトプロファイルで提供される内容を示します。これには、ユーザーがアクセスを許可するサンドボックスと、そのサンドボックスの範囲内で許可する権限が含まれます。

| 役割 | サンドボックスアクセス | 権限 |
| --- | --- | --- |
| デフォルトの実稼働 - すべてのアクセス | 実稼動 | サンドボックス管理権限を除く、Experience Platform に適用されるすべての権限。 |
| サンドボックス管理者 | なし | サンドボックス管理権限へのアクセスのみを提供します。 |

## サンドボックスと権限

非実稼働サンドボックスは、他のサンドボックスからデータを分離できるデータ仮想化の一種で、通常は開発実験、テストまたは試用に使用されます。ロールの権限は、ロールがアクセス権を付与されたサンドボックス環境内のExperience Platform機能に対するロールのユーザーのアクセスを提供します。 デフォルトの Experience Platform ライセンスでは、5 つのサンドボックス（1 つの実稼動環境と 4 つの非実稼動環境）が許可されます。10 個の非実稼動用サンドボックス（合計 75 個まで）のパックを追加できます。詳しくは、組織の管理者またはアドビのセールス担当者にお問い合わせください。

Experience Platform のサンドボックスについて詳しくは、「[サンドボックスの概要](../sandboxes/home.md)」を参照してください。

### サンドボックスへのアクセス

サンドボックスへのアクセスは、役割を通じて管理されます。 ロールのサンドボックスへのアクセスを有効にする手順について詳しくは、 [属性ベースのアクセス制御ロールガイド](./abac/ui/roles.md).

ユーザーには、ロール内の 1 つ以上のサンドボックスへのアクセス権を付与できます。 1 人のユーザーが 2 つ以上の役割に含まれている場合、そのユーザーはそれらの役割に含まれるすべてのサンドボックスにアクセスできます。

「サンドボックスの管理」権限を使用すると、ユーザーはサンドボックスの管理、表示またはリセットをおこなえます。

### リソース権限 {#permissions}

リソース [!UICONTROL 権限] ロール内のタブには、そのロールに対してアクティブなサンドボックスと権限が表示されます。

![権限の概要](./images/permissions.png)

リソース権限を通じて付与される権限はカテゴリ別に並べ替えられます。一部の権限では、複数の低レベル機能へのアクセスが許可されます。

次の表に、ロールでExperience Platformに使用できる権限と、アクセス権を付与する特定のExperience Platform機能の説明を示します。 ロールに権限を追加する手順について詳しくは、 [属性ベースのアクセス制御ロールガイド](./abac/ui/roles.md).

| カテゴリ | 権限 | 説明 |
| --- | --- | --- |
| [!DNL Alerts] | [!UICONTROL アラート履歴の表示] | アラート履歴への読み取り専用アクセス。 |
| [!DNL Alerts] | [!UICONTROL アラートの解決] | アラートの読み取り、編集、削除へのアクセス。 |
| [!DNL Alerts] | [!UICONTROL アラートの表示] | アラートへの読み取り専用アクセス。 |
| [!DNL Alerts] | [!UICONTROL アラートの管理] | アラート履歴の読み取り、作成、編集、削除へのアクセス。 |
| [!DNL Data Hygiene] | [!UICONTROL データハイジーンの表示] | データハイジーンへの読み取り専用アクセス。 |
| [!DNL Data Hygiene] | [!UICONTROL データハイジーンの管理] | データハイジーンの読み取り、作成、編集、削除へのアクセス。 |
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
| [!DNL Identity Management] | [!UICONTROL ID 名前空間の管理] | ID 名前空間への読み取り、作成、編集および削除アクセス |
| [!DNL Identity Management] | [!UICONTROL ID 名前空間の表示] | ID 名前空間への読み取り専用アクセス |
| [!DNL Identity Management] | [!UICONTROL ID グラフの表示] | ID グラフへの読み取り専用アクセス |
| [!DNL Sandbox Administration] | [!UICONTROL サンドボックスの管理] | サンドボックスへの読み取り、作成、編集、削除アクセス |
| [!DNL Sandbox Administration] | [!UICONTROL サンドボックスの表示] | 組織に属するサンドボックスへの読み取り専用アクセス |
| [!DNL Sandbox Administration] | [!UICONTROL サンドボックスのリセット] | サンドボックスをリセットする機能 |
| [!DNL Destinations] | [!UICONTROL 宛先の管理] | 宛先アクティブ化フローと宛先アカウントの読み取り、作成および削除へのアクセス。 |
| [!DNL Destinations] | [!UICONTROL 宛先の表示] | 「**[!UICONTROL カタログ]**」タブの使用可能な宛先と「**[!UICONTROL 参照]**」タブの認証済みの宛先への読み取り専用アクセス。 |
| [!DNL Destinations] | [!UICONTROL 宛先のアクティブ化] | ユーザーが既存の宛先に対してセグメントをアクティブ化できるようにします。アクティブ化ワークフローのマッピングステップを有効にします。この権限の場合は、宛先に対してデータをアクティブ化するユーザーに「[!UICONTROL 宛先の表示]」または「[!UICONTROL 宛先の管理]」のいずれかを付与する必要があります。 |
| [!DNL Destinations] | [!UICONTROL マッピングを使用しないセグメントアクティブ化] | [マッピングステップ](../destinations/ui/activate-batch-profile-destinations.md#mapping)が表示されない状態でユーザーがセグメントを既存の宛先に対してアクティブ化できるようにします。ユーザーは、アクティブ化ワークフローでセグメントを追加および削除できますが、マッピングされた属性や ID を追加または削除することはできません。この権限の場合は、データを宛先に対してアクティブ化するユーザーに「[!UICONTROL 宛先のアクティブ化]」権限を付与する必要があります。 |
| [!DNL Destinations] | [!UICONTROL データセット宛先の管理とアクティブ化] | データセット書き出しフローを読み取り、作成、編集および無効化する機能。作成済みのアクティブなデータセットに対してデータもアクティブ化する機能。 |
| [!DNL Destinations] | [!UICONTROL 宛先のオーサリング] | [Adobe Experience Platform Destination SDK](../destinations/destination-sdk/overview.md) を使用して宛先を作成する機能。 |
| [!DNL Data Ingestion] | [!UICONTROL ソースの管理] | ソースへの読み取り、作成、編集、無効化アクセス |
| [!DNL Data Ingestion] | [!UICONTROL ソースの表示] | 「**[!UICONTROL カタログ]**」タブでの使用可能なソースおよび「**[!UICONTROL 参照]**」タブでの認証済みのソースへの読み取り専用アクセス |
| [!DNL Data Ingestion] | [!DNL Manage Audience Share Connections] | 2 つの 組織を接続し [!DNL Segment Match] フローを有効にするパートナーハンドシェイクを作成、承認または拒否するためのアクセス権。 |
| [!DNL Data Ingestion] | [!DNL Manage Audience Share] | アクティブなパートナーで [!DNL Segment Match] フィードを読み取り、作成、編集、公開するためのアクセス権。 |
| [!DNL Data Science Workspace] | [!UICONTROL Data Science Workspace の管理] | [!DNL Data Science Workspace] での読み取り、作成、編集、および削除へのアクセス。 |
| データガバナンス | [!UICONTROL データ使用ラベルの適用] | 使用ラベルを読み取り、作成および削除するアクセス権。 |
| データガバナンス | [!UICONTROL データ使用ポリシーの管理] | データ使用ポリシーの読み取り、作成、編集、および削除へのアクセス。 |
| データガバナンス | [!UICONTROL データ使用ポリシーの表示] | 組織に属するデータ使用ポリシーに対する読み取り専用アクセス。 |
| データガバナンス | [!UICONTROL ユーザーアクティビティログを表示] | Platform のアクティビティを記録した [監査ログ](../landing/governance-privacy-security/audit-logs/overview.md) を表示する読み取り専用アクセス。 |
| [!DNL Dashboards] | [!UICONTROL ライセンス使用状況ダッシュボードの表示] | ライセンス使用状況ダッシュボードを表示する読み取り専用アクセス。 |
| [!DNL Dashboards] | [!UICONTROL 標準ダッシュボードの管理] | Data Warehouse にないカスタム属性の追加。 |
| [!DNL Query Service] | [!UICONTROL クエリの管理] | Platform データの構造化 SQL クエリの読み取り、作成、編集、および削除へのアクセス。 |
| [!DNL Query Service] | [!UICONTROL クエリサービス統合の管理] | クエリサービスアクセスの有効期限が切れていない認証情報を作成、更新、削除するためのアクセス。 |

## 次の手順

このガイドを読むことで、Experience Platform. のアクセス制御の主な原則を学びました。これで、 [属性ベースのアクセス制御ユーザーガイド](./abac/overview.md) Experience Cloudを使用してロールを作成し、Experience Platformの権限を割り当てる方法に関する詳細な手順。
