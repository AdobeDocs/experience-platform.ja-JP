---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: アクセス制御の概要
topic: overview
translation-type: tm+mt
source-git-commit: 73a492ba887ddfe651e0a29aac376d82a7a1dcc4
workflow-type: tm+mt
source-wordcount: '1097'
ht-degree: 3%

---


# アクセス制御の概要

Access control for [!DNL Experience Platform] is provided through the [Adobe Admin Console](https://adminconsole.adobe.com). この機能は、の製品プロファイルを活用し [!DNL Admin Console]ます。この製品ユーザーは、権限とサンドボックスをリンクします。

## アクセス制御階層とワークフロー

のアクセス制御を設定するに [!DNL Experience Platform]は、製品統合を持つ組織の管理者権限が必要で [!DNL Experience Platform] す。 権限を付与または取り消す最小の役割は、 **[!UICONTROL 製品プロファイル管理者]**&#x200B;です。 権限を管理できる他の管理者の役割は、 **[!UICONTROL 製品管理者]** (製品内のすべてのプロファイルを管理できます)と **[!UICONTROL システム管理者]** （制限なし）です。 詳しくは、 [管理者の役割に関するAdobe Help Centerの記事](https://helpx.adobe.com/jp/enterprise/using/admin-roles.html) を参照してください。

>[!NOTE]
>
>これ以降、このドキュメントで「administrator」というメンションが記述される場合は、（前述の説明を参照して）製品プロファイル以降の管理者を指します。

アクセス権限を取得および割り当てるための高度なワークフローを、次にまとめます。

- Adobe Experience Platformを購読すると、登録フォームで指定された管理者に電子メールが送信されます。
- 管理者は [AdobeAdmin Consoleにログインし](#adobe-admin-console) 、概要ページの製品のリストから **Adobe Experience Platform** を選択します。
- 管理者は、必要に応じて、デフォルトの [製品プロファイルを表示したり](#product-profiles) 、新しい顧客製品プロファイルを作成したりできます。
- 管理者は、既存の製品プロファイルの権限とユーザーを編集できます。
- 製品プロファイルを作成または編集する際に、管理者は **[!UICONTROL users]** タブを使用してプロファイルにユーザーを追加し、権限タブにアクセスする権限(「Read Datasets[!UICONTROL 」、「]Manage Provisations」など)を **** スキーマに付与します。 同様に、管理者は同じ権限タブを使用してサンドボックスにアクセス権を割り当てることができます。
- ユーザーがユーザーインターフェイスにログインすると、その [!DNL Experience Platform][!DNL Platform] 機能へのアクセスは、手順2で許可された権限によって決まります。 例えば、ユーザーが「[!UICONTROL 表示データセット]」権限を持っていない場合、サイドメニューの「 *[!UICONTROL データセット]* 」タブは表示されません。

でアクセス制御を管理する方法について詳しくは、『 [!DNL Experience Platform]アクセス制御ユーザーガイド [](./ui/overview.md)』を参照してください。

APIへのすべての呼び出しは権限に対して検証され、適切な権限が現在のユーザーコンテキストで見つからない場合は、エラーを返します。 [!DNL Experience Platform] UI内では、現在のユーザーに付与されている権限に応じて、要素が非表示または変更されます。

## Adobe Admin Console

アドビのAdmin Consoleは、アドビ製品の登録と組織のアクセスを一元的に管理します。 コンソールを使用して、「データセットの [!DNL Platform] 管理[!UICONTROL 」、「]表示データセット[!UICONTROL 」、「プロファイルの]管理」など、様々な機能に対するユーザーのアクセス権限を付与できます。

### 製品プロファイル

では、 [!DNL Admin Console]製品プロファイルを使用して権限がユーザーに割り当てられ **[!UICONTROL ます]**。 製品プロファイルを使用すると、1人または複数のユーザーに権限を付与でき、また、製品プロファイルを介して割り当てられるサンドボックスの範囲へのアクセス権も含めることができます。 ユーザーは、組織に属する1人または複数の製品プロファイルに割り当てることができます。

### デフォルトの製品プロファイル

[!DNL Experience Platform] には、事前設定済みのデフォルト製品プロファイルが2つ用意されています。 次の表に、各デフォルトプロファイルで提供される内容を示します。これには、ユーザがアクセスを許可するサンドボックスと、そのサンドボックスの範囲内で許可する権限が含まれます。

| 製品プロファイル | Sandboxへのアクセス | 権限 |
| --- | --- | --- |
| デフォルトの実稼動 — すべてのアクセス | 実稼動 | Sandboxの管理権限を除く、に適用でき [!DNL Experience Platform]るすべての権限です。 |
| 既定のサンドボックス管理 | 該当なし | Sandbox管理権限へのアクセスのみを提供します。 |

## サンドボックスと権限

[!DNL Experience Platform] 1つの実稼働用サンドボックスへのアクセスを提供し、非実稼働用サンドボックスの作成を許可 **します**。 非実稼動サンドボックスは、他のサンドボックスからデータを分離できるデータ仮想化の一種で、通常は開発実験、テストまたは試用に使用されます。 製品プロファイルの **[!UICONTROL 権限は]** 、プロファイルがアクセス権を付与されたSandbox環境内の [!DNL Platform] 機能に対するアクセスをユーザに与えます。

のサンドボックスについて詳し [!DNL Experience Platform]くは、 [サンドボックスの概要を参照してください](../sandboxes/home.md)。

### サンドボックスへのアクセス

サンドボックスへのアクセスは、製品プロファイルを通じて管理されます。 製品プロファイル用のSandboxへのアクセスを有効にする方法について詳しくは、 [アクセス制御ユーザーガイドを参照してください](./ui/overview.md)。

ユーザーには、製品プロファイル内の1つ以上のサンドボックスへのアクセスを許可できます。 1人のユーザーが複数の製品プロファイルに含まれている場合、そのプロファイルーに含まれているすべてのサンドボックスにそのユーザーがアクセスできます。

「Sandboxの管理」権限を使用すると、サンドボックスの管理、表示またはリセットが可能になります。

### 権限

製品プロファイル内の **権限** タブには、そのプロファイルに対してアクティブなサンドボックスと権限が表示されます。

![](./images/permissions-overview.png)

を通じて付与される権限は、カテゴリ [!DNL Admin Console] 別に並べ替えられます。一部の権限では、低レベルの機能に対するアクセス権を付与します。

次の表に、の使用可能な権限と、そ [!DNL Experience Platform] の権限がアクセス権を付与した個々の [!DNL Admin Console][!DNL Platform] 機能の説明を示します。 製品プロファイルに権限を追加する方法について詳しくは、 [アクセス制御ユーザーガイドを参照してください](./ui/overview.md)。

| カテゴリ | 権限 | 説明 |
| --- | --- | --- |
| [!DNL Data Modeling] | [!UICONTROL スキーマの管理] | スキーマおよび関連リソースの読み取り、作成、編集および削除を行う。 |
| [!DNL Data Modeling] | [!UICONTROL スキーマの表示] | スキーマおよび関連リソースへの読み取り専用アクセス。 |
| [!DNL Data Management] | [!UICONTROL データセットの管理] | データセットの読み取り、作成、編集、削除を行う。 スキーマの読み取り専用アクセス。 |
| [!DNL Data Management] | [!UICONTROL データセットの表示] | データセットとスキーマに対する読み取り専用アクセス。 |
| [!DNL Data Management] | [!UICONTROL データの監視] | 監視データセットおよびストリームへの読み取り専用アクセス。 |
| [!DNL Profile Management] | [!UICONTROL プロファイルの管理] | お客様のプロファイルに使用されるデータセットの読み取り、作成、編集、削除を行う。 使用可能なプロファイルへの読み取り専用アクセス権。 |
| [!DNL Profile Management] | [!UICONTROL 表示プロファイル] | 使用可能なプロファイルへの読み取り専用アクセス権。 |
| [!DNL Profile Management] | [!UICONTROL セグメントのオーディエンスをエクスポート] | 評価されたオーディエンスセグメントをデータセットにエクスポートする機能。 |
| [!DNL Identities] | [!UICONTROL ID 名前空間の管理] | ID名前空間の読み取り、作成、編集および削除を行う。 |
| [!DNL Identities] | [!UICONTROL 表示ID名前空間] | ID名前空間の読み取り専用アクセス。 |
| [!DNL Sandbox Administration] | [!UICONTROL サンドボックスの管理] | 読み取り、作成、編集および削除のサンドボックスにアクセスできます。 |
| [!DNL Sandbox Administration] | [!UICONTROL サンドボックスの表示] | 組織に属するサンドボックスに対する読み取り専用アクセス権。 |
| [!DNL Sandbox Administration] | [!UICONTROL サンドボックスのリセット] | サンドボックスをリセットする機能。 |
| [!DNL Destinations] | [!UICONTROL 宛先の管理] | 読み取り先、作成先、編集先および無効化先へのアクセス権。* |
| [!DNL Destinations] | [!UICONTROL 表示先] | 「 *[!UICONTROL カタログ]* 」タブの使用可能な宛先および「 *[!UICONTROL 参照]* 」タブの認証済み宛先への読み取り専用アクセス権。* |
| [!DNL Destinations] | [!UICONTROL 宛先のアクティブ化] | 作成済みのアクティブな宛先に対するデータをアクティブ化する機能。 この権限を持つには、宛先をアクティブ化する表示に対して、「 [!UICONTROL 宛先を管理」] 「宛先を管理」を付与する必要があります。* |
| [!DNL Data Ingestion] | [!UICONTROL ソースの管理] | ソースの読み取り、作成、編集および無効化を行うことができます。 |
| [!DNL Data Ingestion] | [!UICONTROL 表示ソース] | 「 *[!UICONTROL カタログ]* 」タブの使用可能なソースおよび「 *[!UICONTROL 参照]* 」タブの認証されたソースへの読み取り専用アクセス権。 |
| [!DNL Data Science Workspace] | [!UICONTROL Data Science Workspaceの管理] | での読み取り、作成、編集および削除へのアクセス権 [!DNL Data Science Workspace]。 |

_(*)この許可には，に対する規定が必要[!DNL Real-time Customer Data Platform]である。 Real-time CDPの詳細は、まず[Real-time CDPの概要を読んでください](https://docs.adobe.com/content/help/ja-JP/experience-platform/rtcdp/overview.html)。_

## 次の手順

本書を読むことで、アクセス制御の主な原則についての説明が終わり [!DNL Experience Platform]ました。 これで、を使用して製品プロファイルを作成し、権限を [割り当てる方法の詳細手順については、](./ui/overview.md) アクセス制御ユーザーガイド [!DNL Admin Console] （英語） [!DNL Platform]に進むことができます。