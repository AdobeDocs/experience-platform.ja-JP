---
title: Oracle Eloqua （V2）Sourceの概要
description: Oracle Eloqua をAdobe Experience Platformに接続する方法を説明します。
source-git-commit: 180754969d4ae8dbd1308dfc85dae73baf64f759
workflow-type: tm+mt
source-wordcount: '1824'
ht-degree: 2%

---

# [!DNL Oracle Eloqua] （V2）ソースの概要

>[!IMPORTANT]
>
>元の [[!DNL Oracle Eloqua]  （V1） &#x200B;](oracle-eloqua.md) ソースは、2026 年 1 月をもって非推奨（廃止予定）となりました。 この非推奨ソースに利用可能な移行はありません。新しい [!DNL Oracle Eloqua] （V2）ソースを使用してデータを再実装する必要があります。

[!DNL Oracle Eloqua] は、主に B2B 分野の組織が、リードの管理やバイヤージャーニーの調整という複雑なプロセスを自動化およびパーソナライズするのに役立つように設計された、強力なエンタープライズグレードのマーケティング自動化プラットフォームです。 マーケティングチームが複数のデジタルチャネルをまたいで高度なキャンペーンを定義、デプロイ、測定できる中央ハブとして機能し、見込み客が最も関与している正確なタイミングで適切なコンテンツを受け取れるようにします。 [!DNL Eloqua] を介した取り込みでサポートされているオブジェクトは **連絡先**、**アカウント**、**キャンペーン** および **アクティビティ** です。 最初の取り込みが完了すると、スケジュールされた増分プロセスを使用して、変更されたデータがインポートされます。

[!DNL Eloqua] ソースを使用して、[!DNL Eloqua] アカウントをAdobe Experience Platformに接続できます。 開始する方法については、以下のドキュメントを参照してください。

## 使用例 {#use-case-examples}

次の表に、Adobe Experience Platformとの [!DNL Eloqua] （V2）統合でサポートされるマーケティングオブジェクトの概要を示します。 各オブジェクトについて、[!DNL Eloqua] データをReal-Time CDPと統合してマーケティング効果とキャンペーン成果を高める方法を示すサンプルのユースケースと説明を確認します。

| オブジェクト | 説明 | 使用例 |
| --- | --- | --- |
| 連絡先 | 連絡先データ（名前、メール、電話番号、役職など）をReal-Time CDPに取り込んで、詳細で統一された顧客プロファイルを作成し、個々の連絡先とのすべてのインタラクションやエンゲージメントを統合します。 | **キャンペーンの最適化：** [!DNL Eloqua] の連絡先データを統合することで、マーケティングチームは、メール開封数、フォーム送信、イベント登録などの最近のアクティビティに基づいて、優先度の高い見込み客を特定できます。 Real-Time CDPでは、メール、web サイト、その他のマーケティングタッチポイントをまたいだ各連絡先の行動を 360°表示できるので、マーケティングチームはキャンペーンをカスタマイズし、メッセージを最適化して、より良いエンゲージメントとコンバージョンを実現できます。 |
| アカウント | アカウントレベルのデータ（会社名、業界、会社の規模、売上高、場所など）を取り込んで、Real-Time CDPでアカウントベースのマーケティング（ABM）戦略を構築し、チームが適切な組織をターゲットにして、関連するメッセージをエンゲージできるようにします。 | **ABM キャンペーン：** ABM からのアカウントデータの統合は、ターゲット設定され [!DNL Eloqua]ABM キャンペーンの作成に役立ちます。 例えば、ソフトウェア会社は、アカウントデータを使用して、金融分野の企業の意思決定者にカスタマイズされたメールキャンペーンをセグメント化して送信し、業界に合わせた新しいソリューションを促進できます。 |
| キャンペーン | キャンペーンデータ（キャンペーン名、タイプ、目標、開封率や CTR などのパフォーマンス指標）をReal-Time CDPに取り込み、複数のチャネルでキャンペーンのパフォーマンスをトラッキングし最適化します。 このデータを使用して ROI を測定し、戦略を調整します。 | **クロスチャネルアトリビューション：**&#x200B;[!DNL Eloqua] がキャンペーンデータをReal-Time CDPに送信する場合、マーケティングチームは様々なチャネル（メール、ソーシャルメディア、広告など）にわたるキャンペーンのパフォーマンスを確認し、コンバージョンを適切なタッチポイントに関連付け、そのinsightに基づいて将来の戦略を調整できます。 |
| アクティビティ | アクティビティデータ（メールの開封数、クリック数、web サイトの訪問、フォーム送信、ウェビナーの出席など）を取り込んで、様々なチャネルをまたいでリアルタイムの行動と連絡先を追跡し、リアルタイムにパーソナライズされたエンゲージメントの機会を作成します。 | **リアルタイム育成：** Real-Time CDPでは、[!DNL Eloqua] からのアクティビティデータを統合することにより、コンタクト先がコンテンツに関与した場合（ホワイトペーパーのダウンロードやメールリンクのクリックなど）に、パーソナライズされたメールや通知を営業チームにトリガーすることができ、タイムリーなフォローアップとコンバージョンの向上が可能になります。 |

{style="table-layout:auto"}

## 前提条件 {#prerequisites}

ソースをExperience Platformに接続する前に完了する必要がある前提条件の設定については、以下の節を参照してください。

### 認証用のアプリケーションの設定

次の手順に従って、[!DNL Eloqua] アカウントを設定し、基本認証を使用してExperience Platformに接続する方法を学習します。

開始するには、管理者（またはユーザー、セキュリティグループ、アプリを作成するためのアクセス権を持つユーザー）として [!DNL Eloqua] インスタンスにログインします。

![My Eloqua ダッシュボード &#x200B;](../../images/tutorials/create/eloqua/admin.png)

**設定**/**Platform 拡張機能**/**アプリクラウド開発者**/**アプリを作成** に移動します。 名前、説明、アイコン、OAuth コールバック URL など、アプリの詳細を指定します。 完了したら、「**保存**」をクリックします。

![Eloqua ダッシュボードのアプリ開発者パネルと「アプリを作成」ボタン &#x200B;](../../images/tutorials/create/eloqua/create-app.png)

| プロパティ | 説明 |
| --- | --- |
| 名前 | アプリの名前。 |
| 説明 | アプリの簡単な説明。 |
| アイコン | アイコンの URL。 |
| OAuth コールバック URL | アプリをインストールして [!DNL Eloqua] で認証した後にユーザーがリダイレクトする必要のある URL。 |

![Eloqua のアプリ作成ウィンドウ &#x200B;](../../images/tutorials/create/eloqua/new-app.png)

アプリが作成されたら、[!DNL Authentication to Eloqua] に移動し、新しく作成したアプリから **クライアント ID** および **クライアントシークレット** を取得します。 これらの値は、後でExperience Platformに接続するときに使用されます。

![Eloqua のクライアント ID とクライアントシークレット &#x200B;](../../images/tutorials/create/eloqua/credentials.png)

セキュリティグループを使用すると、管理者は、アセット、機能、インターフェイスなどに対するユーザーのアクセスレベルを制御できます。 セキュリティグループを作成するには、**設定**/**ユーザー** に移動します。 次に、左側のパネルの **グループ** タブを選択し、**新しいセキュリティグループの作成** を選択します。

![Eloqua のユーザー管理ダッシュボード &#x200B;](../../images/tutorials/create/eloqua/user-management.png)

**[!DNL Security Group Overview]** ウィンドウを使用して、セキュリティ・グループの名前と頭字語を指定します。 作成したら、[!DNL Action Permissions] に移動してリストから [!DNL Consume] API 権限を追加し、「**保存**」を選択します。

![Eloqua のセキュリティグループの概要ウィンドウ。](../../images/tutorials/create/eloqua/security-group-overview.png)

![&#x200B; 使用 API の選択ウィンドウ &#x200B;](../../images/tutorials/create/eloqua/consume-api.png)

>[!NOTE]
>
>API[!DNL Consume] 必要な権限ですが、アプリの使用状況に応じてさらに権限を追加できます。

キャンペーンデータを取り込むには、**ユーザーを編集** インターフェイスに移動し、選択したセキュリティグループに [!DNL Guided Campaigns] を追加します。

![&#x200B; ガイド付きキャンペーンが追加されたセキュリティグループ。](../../images/tutorials/create/eloqua/add-guided-campaigns.png)

オプションで、追加のユーザーを作成し、そのユーザーをセキュリティグループに追加できます。 手順について詳しくは、[!DNL Eloqua] のドキュメント [&#x200B; ユーザーの作成 &#x200B;](https://docs.oracle.com/en/cloud/saas/marketing/eloqua-user/Help/UserManagement/Tasks/CreatingIndividualUsers.htm) および [&#x200B; セキュリティグループへのユーザーの割り当て &#x200B;](https://docs.oracle.com/en/cloud/saas/marketing/eloqua-user/Help/SecurityGroups/Tasks/AddingUsersToSecurityGroups.htm) を参照してください。

### 必要な資格情報の収集

Experience Platformに接続するには、次の資格情報の値を指定する必要 [!DNL Eloqua] あります。

| 資格情報 | 説明 |
| --- | --- |
| クライアント ID | [!DNL Eloqua] がExperience Platformの認証を行う際に、アカウントを識別するために使用する、公開されている識別子。 |
| クライアントシークレット | クライアントアプリケーションおよび認証サーバーにのみ認識される秘密鍵。 このキーは、アカウントを認証するためにクライアント ID と共に必要です。 |
| ユーザー名 | [!DNL Eloqua] アカウントに関連付けられたユーザー名。 これは、アクセスを検証および認証するために使用されます。 ユーザー名は、`CompanyName\Username` の形式に従います。 |
| パスワード | [!DNL Eloqua] アカウントに関連付けられたパスワード。 ユーザー名と共に、Eloqua 環境へのアクセスを許可します。 |
| 基本エンドポイント | [!DNL Eloqua] の認証ベース URI のプレフィックス。 認証時には、ベースエンドポイントに `http://` または `https://` を含めないでください。 |

## [!DNL Eloqua] マッピングガイド

>[!NOTE]
>
>増分データの読み込みに内部的に使用される差分フィールドを次に示します。
>
>- **連絡先：** `C_DateModified`
>- **アカウント：** `M_DateModified`
>- **アクティビティ：** `CreatedAt`
>- **カスタム オブジェクト：** `UpdatedAt`
>- **Campaign:** `updatedAt`

次の表は、Experience Platformの [!DNL Eloqua] ソースフィールドと、対応するエクスペリエンスデータモデル（XDM）宛先フィールドとの詳細なマッピングを示しています。 各行では、フィールドが不変かどうかについての変換ロジックの概要を示し、Experience Platformでの [!DNL Eloqua] データの取り込み方法や構造を理解するのに役立つ追加の注意事項を示します。

### アカウント

| Eloqua Source列 | XDM 宛先パス | メモ |
| --- | --- | --- |
| `"Eloqua"` | accountKey.sourceType | このフィールドは常に固定値「Eloqua」に設定されます。 |
| `"${SOURCE_INSTANCE_ID}"` | accountKey.sourceInstanceID | `SOURCE_INSTANCE_ID` は自動的にコネクタに置き換えられます。 |
| `Id` | accountKey.sourceID | |
| `concat(Id, "\\@${SOURCE_INSTANCE_ID}.Eloqua")` | accountKey.sourceKey | `SOURCE_INSTANCE_ID` は自動的にコネクタに置き換えられます。 |
| `M_CompanyName` | accountName | |
| `M_Country` | accountPhysicalAddress.country | |
| `M_Address1` | accountPhysicalAddress.street1 | |
| `M_City` | accountPhysicalAddress.city | |
| `M_State_Prov` | accountPhysicalAddress.stateProvince | |
| `M_Zip_Postal` | accountPhysicalAddress.postalCode | |
| `M_BusPhone` | accountPhone.number | |
| `M_Fax1` | accountFax.number | |
| `M_Account_Engagement_Score` | accountScore | |
| `M_Account_Type1` | accountType | |
| `M_Wesbsite1` | accountOrganization.website | |
| `M_Employees1` | accountOrganization.numberOfEmployees | |
| `to_decimal(M_Annual_Revenue1)` | accountOrganization.annualRevenue.amount | |
| `M_DateModified` | extSourceSystemAudit.lastUpdatedDate | |
| `M_DateCreated` | extSourceSystemAudit.createdDate | |
| `M_Industry1` | accountOrganization.industry | |
| `iif(M_SFDCAccountID != null && M_SFDCAccountID != "", to_object("sourceType", "Salesforce", "sourceInstanceID", "${CRM_INSTANCE_ID}", "sourceID", M_SFDCAccountID, "sourceKey", concat(M_SFDCAccountID, "\\@${CRM_INSTANCE_ID}.Salesforce")), iif(M_MSCRMAccountID != null && M_MSCRMAccountID != "", to_object("sourceType", "Dynamics", "sourceInstanceID", "${CRM_INSTANCE_ID}", "sourceID", M_MSCRMAccountID, "sourceKey", concat(M_MSCRMAccountID, "\\@${CRM_INSTANCE_ID}.Dynamics")), null))` | extSourceSystemAudit.externalKey | コネクタは、CRM インスタンス ID を自動的に検出できません。 `${CRM_INSTANCE_ID}` を実際の CRM インスタンス ID （Salesforceまたは Dynamics インスタンス ID のいずれか）に手動で置き換える必要があります。 取り込み中に、`M_SFDCAccountID` が存在する場合、コネクタは、その値を使用して外部キーを生成し、`\@CRM_INSTANCE_ID.Salesforce` を追加します。 そのフィールドが空の場合、コネクタは `M_MSCRMAccountID` を使用して、代わりに `\@CRM_INSTANCE_ID.Dynamics` を追加します。 両方のフィールドが空の場合、このフィールドは null に設定されます。 |

{style="table-layout:auto"}

### アクティビティ

| Eloqua Source列 | XDM 宛先パス | メモ |
| --- | --- | --- |
| `"Eloqua"` | personKey.sourceType | このフィールドは常に固定値「Eloqua」に設定されます。 |
| `"${SOURCE_INSTANCE_ID}"` | personKey.sourceInstanceID | `SOURCE_INSTANCE_ID` は自動的にコネクタに置き換えられます。 |
| `ContactId` | personKey.sourceID |  |
| `concat(ContactId, "\\@${SOURCE_INSTANCE_ID}.Eloqua")` | personKey.sourceKey | `SOURCE_INSTANCE_ID` は自動的にコネクタに置き換えられます。 |
| `ExternalId` | _id |  |
| `iif(ActivityType!=null && ActivityType!="", iif(ActivityType=="EmailSend", "directMarketing.emailSent", iif(ActivityType=="EmailOpen", "directMarketing.emailOpened", iif(ActivityType=="EmailClickthrough", "directMarketing.emailClicked", iif(ActivityType=="Unsubscribe", "directMarketing.emailUnsubscribed", iif(ActivityType=="Bounceback", "directMarketing.emailBounced", iif(ActivityType=="FormSubmit", "web.formFilledOut", iif(ActivityType=="PageView", "web.webpagedetails.pageViews", ActivityType))))))), null)` | eventType | ActivityType に基づいて、対応するExperience Platform eventType の値が設定されます。 ExternalActivities の場合、Experience Platformには eventType はありません。 このマッピングを変更すると、より多くのタイプを処理できます。 |
| `ActivityDate` | タイムスタンプ | |
| `iif(AssetType == "Email", AssetName, null)` | directMarketing.mailingName | |
| `iif(AssetType == "Email", to_object("sourceType", "Eloqua", "sourceInstanceID", "${SOURCE_INSTANCE_ID}","sourceID",${AssetId}, "sourceKey", concat(${AssetId},"\\@${SOURCE_INSTANCE_ID}.Eloqua")), null)` | directMarketing.mailingKey | `SOURCE_INSTANCE_ID` は自動的にコネクタに置き換えられます。 |
| `iif(AssetType == "Email", EmailAddress, null)` | directMarketing.email | |
| `iif(ActivityType == "Bounceback", SmtpStatusCode, null)` | directMarketing.emailBouncedCode | |
| `iif(AssetType == "Email", SmtpMessage, null)` | directMarketing.emailBouncedDetails | |
| `iif(AssetType == "Email", EmailWebLink, null)` | directMarketing.linkURL | |
| `iif(ActivityType == "FormSubmit", AssetName, null)` | web.fillOutForm.webFormName | |
| `iif(ActivityType == "FormSubmit", to_object("sourceType", "Eloqua", "sourceInstanceID", "${SOURCE_INSTANCE_ID}","sourceID",${AssetId}, "sourceKey", concat(${AssetId},"\\@${SOURCE_INSTANCE_ID}.Eloqua")), null)` | web.fillOutForm.webFormKey | `SOURCE_INSTANCE_ID` は自動的にコネクタに置き換えられます。 |
| `iif(ActivityType == "PageView", AssetName, null)` | web.webPageDetails.name | |
| `iif(ActivityType == "PageView", to_object("sourceType", "Eloqua", "sourceInstanceID", "${SOURCE_INSTANCE_ID}","sourceID",${AssetId}, "sourceKey", concat(${AssetId},"\\@${SOURCE_INSTANCE_ID}.Eloqua")), null)` | web.webPageDetails.webPageKey | `SOURCE_INSTANCE_ID` は自動的にコネクタに置き換えられます。 |
| `iif(ActivityType == "PageView", Url, null)` | web.webPageDetails.URL | |

{style="table-layout:auto"}

### キャンペーン

| Eloqua Source列 | XDM 宛先パス | メモ |
| --- | --- | --- |
| `"Eloqua"` | campaignKey.sourceType | このフィールドは常に固定値「Eloqua」に設定されます。 |
| `"${SOURCE_INSTANCE_ID}"` | campaignKey.sourceInstanceID | `SOURCE_INSTANCE_ID` は自動的にコネクタに置き換えられます。 |
| `id` | campaignKey.sourceID | |
| `concat(id, "\\@${SOURCE_INSTANCE_ID}.Eloqua")` | campaignKey.sourceKey | `SOURCE_INSTANCE_ID` は自動的にコネクタに置き換えられます。 |
| `name` | campaignName | |
| `endAt` | campaignEndDate | |
| `startAt` | campaignStartDate | |
| `actualCost` | actualCost.amount | |
| `budgetedCost` | budgetedCost.amount | |
| `description` | campaignDescription | |
| `currentStatus` | campaignStatus | |
| `campaignType` | campaignType | |
| `createdAt` | extSourceSystemAudit.createdDate | |
| `updatedAt` | extSourceSystemAudit.lastUpdatedDate | |

{style="table-layout:auto"}

### 連絡先

| Eloqua Source列 | XDM 宛先パス | メモ |
| --- | --- | --- |
| `"Eloqua"` | b2b.personKey.sourceType | このフィールドは常に固定値「Eloqua」に設定されます。 |
| `"${SOURCE_INSTANCE_ID}"` | b2b.personKey.sourceInstanceID | `SOURCE_INSTANCE_ID` は自動的にコネクタに置き換えられます。 |
| `Id` | b2b.personKey.sourceID |  |
| `concat(Id, "\\@${SOURCE_INSTANCE_ID}.Eloqua"` | b2b.personKey.sourceKey | `SOURCE_INSTANCE_ID` は自動的にコネクタに置き換えられます。 |
| `C_Company` | b2b.companyName |  |
| `C_Website1` | b2b.companyWebsite |  |
| `C_Job_Title1` | extendedWorkDetails.jobTitle |  |
| `C_Fax` | faxPhone.number |  |
| `C_MobilePhone` | mobilePhone.number |  |
| `iif(C_SFDCLeadID != null && C_SFDCLeadID != "\\", to_object("sourceType", "Salesforce", "sourceInstanceID", "${CRM_INSTANCE_ID}", "sourceID", C_SFDCLeadID, "sourceKey", concat(C_SFDCLeadID, "\\@${CRM_INSTANCE_ID}.Salesforce")), iif(C_SFDCContactID != null && C_SFDCContactID != "\\", to_object("sourceType", "Salesforce", "sourceInstanceID", "${CRM_INSTANCE_ID}", "sourceID", C_SFDCContactID, "sourceKey", concat(C_SFDCContactID, "\\@${CRM_INSTANCE_ID}.Salesforce")), null))` | personComponents.sourceExternalKey | [!DNL Eloqua] インスタンスがSalesforceと同期されている場合、このマッピングを維持します。 それ以外の場合は、削除します。 コネクタには CRM_INSTANCE_ID を判別する手段がないので、${CRM_INSTANCE_ID} を同期したSalesforce インスタンス ID に置き換える必要があります。 この同じマッピングは personComponents と extSourceSystemAudit に適用されるので、両方を維持します。 |
| `iif(C_MSCRMLeadID != null && C_MSCRMLeadID != "\\", to_object("sourceType", "Dynamics", "sourceInstanceID", "${CRM_INSTANCE_ID}", "sourceID", C_MSCRMLeadID, "sourceKey", concat(C_MSCRMLeadID, "\\@${CRM_INSTANCE_ID}.Dynamics")), iif(C_MSCRMContactID != null && C_MSCRMContactID != "\\", to_object("sourceType", "Dynamics", "sourceInstanceID", "${CRM_INSTANCE_ID}", "sourceID", C_MSCRMContactID, "sourceKey", concat(C_MSCRMContactID, "\\@${CRM_INSTANCE_ID}.Dynamics")), null))"` | personComponents.sourceExternalKey | [!DNL Eloqua] インスタンスが Dynamics と同期されている場合は、このマッピングを維持します。 それ以外の場合は、削除します。 コネクタには CRM_INSTANCE_ID を決定する方法がないので、${CRM_INSTANCE_ID} を同期した Dynamics インスタンス ID に置き換える必要があります。 この同じマッピングは personComponents と extSourceSystemAudit に適用されるので、両方を維持します。 |
| `iif(C_SFDCLeadID != null && C_SFDCLeadID != "\\", to_object("sourceType", "Salesforce", "sourceInstanceID", "${CRM_INSTANCE_ID}", "sourceID", C_SFDCLeadID, "sourceKey", concat(C_SFDCLeadID, "\\@${CRM_INSTANCE_ID}.Salesforce")), iif(C_SFDCContactID != null && C_SFDCContactID != "\\", to_object("sourceType", "Salesforce", "sourceInstanceID", "${CRM_INSTANCE_ID}", "sourceID", C_SFDCContactID, "sourceKey", concat(C_SFDCContactID, "\\@${CRM_INSTANCE_ID}.Salesforce")), null))"` | extSourceSystemAudit.externalKey | [!DNL Eloqua] インスタンスがSalesforceと同期されている場合、このマッピングを維持します。 それ以外の場合は、削除します。 コネクタには CRM_INSTANCE_ID を判別する手段がないので、${CRM_INSTANCE_ID} を同期したSalesforce インスタンス ID に置き換える必要があります。 この同じマッピングは personComponents と extSourceSystemAudit に適用されるので、両方を維持します。 |
| `iif(C_MSCRMLeadID != null && C_MSCRMLeadID != "\\", to_object("sourceType", "Dynamics", "sourceInstanceID", "${CRM_INSTANCE_ID}", "sourceID", C_MSCRMLeadID, "sourceKey", concat(C_MSCRMLeadID, "\\@${CRM_INSTANCE_ID}.Dynamics")), iif(C_MSCRMContactID != null && C_MSCRMContactID != "\\", to_object("sourceType", "Dynamics", "sourceInstanceID", "${CRM_INSTANCE_ID}", "sourceID", C_MSCRMContactID, "sourceKey", concat(C_MSCRMContactID, "\\@${CRM_INSTANCE_ID}.Dynamics")), null))` | extSourceSystemAudit.externalKey | [!DNL Eloqua] インスタンスが Dynamics と同期されている場合は、このマッピングを維持します。 それ以外の場合は、削除します。 コネクタには CRM_INSTANCE_ID を決定する方法がないので、${CRM_INSTANCE_ID} を同期した Dynamics インスタンス ID に置き換える必要があります。 この同じマッピングは personComponents と extSourceSystemAudit に適用されるので、両方を維持します。 |
| `C_DateCreated` | extSourceSystemAudit.createdDate |  |
| `C_DateModified` | extSourceSystemAudit.lastUpdatedDate |  |
| `iif(C_SFDCAccountID != null && C_SFDCAccountID != "\\", to_object("sourceType", "Salesforce", "sourceInstanceID", "${CRM_INSTANCE_ID}", "sourceID", C_SFDCAccountID, "sourceKey", concat(C_SFDCAccountID, "\\@${CRM_INSTANCE_ID}.Salesforce")), iif(C_MSCRMAccountID != null && C_MSCRMAccountID != "\\", to_object("sourceType", "Dynamics", "sourceInstanceID", "${CRM_INSTANCE_ID}", "sourceID", C_MSCRMAccountID, "sourceKey", concat(C_MSCRMAccountID, "\\@${CRM_INSTANCE_ID}.Dynamics")), null))` | b2b.accountKey | コネクタには CRM_INSTANCE_ID を特定する手段がないので、${CRM_INSTANCE_ID} を同期された CRM インスタンス ID （Salesforceのインスタンス ID または Dynamics のインスタンス ID）に置き換える必要があります。 この同じマッピングは、b2b.accountKey と personComponents.sourceAccountKey の両方に適用されるので、両方を維持します。 |
| `iif(C_SFDCAccountID != null && C_SFDCAccountID != "\\", to_object("sourceType", "Salesforce", "sourceInstanceID", "${CRM_INSTANCE_ID}", "sourceID", C_SFDCAccountID, "sourceKey", concat(C_SFDCAccountID, "\\@${CRM_INSTANCE_ID}.Salesforce")), iif(C_MSCRMAccountID != null && C_MSCRMAccountID != "\\", to_object("sourceType", "Dynamics", "sourceInstanceID", "${CRM_INSTANCE_ID}", "sourceID", C_MSCRMAccountID, "sourceKey", concat(C_MSCRMAccountID, "\\@${CRM_INSTANCE_ID}.Dynamics")), null))` | personComponents.sourceAccountKey | コネクタには CRM_INSTANCE_ID を特定する手段がないので、${CRM_INSTANCE_ID} を同期された CRM インスタンス ID （Salesforceのインスタンス ID または Dynamics のインスタンス ID）に置き換える必要があります。 この同じマッピングは、b2b.accountKey と personComponents.sourceAccountKey の両方に適用されるので、両方を維持します。 |
| `C_Lead_Source___Original1` | b2b.personSource | |
| `C_Lead_Source___Original1` | personComponents.personSource | |
| `C_Lead_Status1` | b2b.personStatus | |
| `C_Lead_Status1` | personComponents.personStatus | |
| `C_FirstName` | person.name.firstName | |
| `C_LastName` | person.name.lastName | |
| `C_Middle_Name1` | person.name.middleName | |
| `C_Salutation` | person.name.courtesyTitle | |
| `C_City` | workAddress.city | |
| `C_Country` | workAddress.country | |
| `C_Zip_Postal` | workAddress.postalCode | |
| `C_State_Prov` | workAddress.state | |

{style="table-layout:auto"}

### アクティビティタイプマッピングの参照

| Eloqua ActivityType | XDM eventType |
| -------------------- | --------------- |
| `EmailSend` | directMarketing.emailSent |
| `EmailOpen` | directMarketing.emailOpened |
| `EmailClickthrough` | directMarketing.emailClicked |
| `Unsubscribe` | directMarketing.emailUnsubscribed |
| `Bounceback` | directMarketing.emailBounced |
| `FormSubmit` | web.formFilledOut |
| `PageView` | web.webpagedetails.pageViews |
| `Other` | そのまま通過する |

{style="table-layout:auto"}

### 変数プレースホルダー

マッピングテンプレートでは、データフローの実行後に置き換えられる次の変数プレースホルダーを使用します。

| プレースホルダー | 説明 | 用途 |
| ----------- | ----------- | ----- |
| `${SOURCE_INSTANCE_ID}` | Eloqua ソースインスタンスの一意の ID | ソースキーで使用 |
| `${CRM_INSTANCE_ID}` | CRM システムの一意の ID （Salesforce/Dynamics） | 外部キーで使用 |

## [!DNL Eloqua] をExperience Platformに接続

次に、Experience Platform内で [!DNL Eloqua] ソース接続を設定します。 UI を使用した接続の設定手順については、[&#x200B; こちらのチュートリアル &#x200B;](../../tutorials/ui/create/marketing-automation/eloqua.md) を参照してください。 このチュートリアルでは、[!DNL Eloqua] アカウントの接続、データの選択、フィールドのマッピング、取り込みのスケジュール設定およびデータフローの監視について説明します。

