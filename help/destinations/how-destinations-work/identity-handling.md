---
title: 宛先アクティベーションワークフローでの ID 処理
description: 宛先のタイプに応じて、アクティベーションワークフローで ID の書き出しがどのように処理されるかを説明します
source-git-commit: 6ccf26cbdbbe675d0d731f59cee589d63ca6f8ad
workflow-type: tm+mt
source-wordcount: '1174'
ht-degree: 3%

---

# 宛先アクティベーションワークフローでの ID 処理

このページでは、ID が様々な宛先タイプに書き出される方法の詳細について説明し、宛先に応じて、書き出しで使用できる ID を見つける方法について説明します。

>[!TIP]
>
> ID、ID 名前空間、ID 関連用語の定義に関する詳細については、 [id サービスの概要](/help/identity-service/home.md).

各宛先 ( [カタログ](/help/destinations/catalog/overview.md) は少し異なるので、すべての宛先での 1 サイズ適合の設定はおこなわれません。 ただし、以下の節で説明するように、宛先とその ID 要件の設定をガイドするパターンがいくつかあります。

## ファイルベースの宛先 {#file-based}

の場合 [ファイルベースの宛先](/help/destinations/destination-types.md#file-based) 例： [!DNL Amazon S3]、SFTP、ほとんどの電子メールマーケティングの宛先 ( 例： [!DNL Adobe Campaign], [!DNL Oracle Eloqua], [!DNL Salesforce Marketing Cloud]) の場合、これらの宛先のほとんどで id 設定が開いているので、 [属性を選択](/help/destinations/ui/activate-batch-profile-destinations.md#select-attributes) バッチアクティベーションワークフローのステップ。

ファイルエクスポートに ID を追加する場合、 [id 名前空間](/help/identity-service/ui/identity-graph-viewer.md#access-identity-graph-viewer) は、エクスポートで選択できます。 エクスポートする ID を選択すると、自動的に「 [必須属性](/help/destinations/ui/activate-batch-profile-destinations.md#mandatory-attributes) および [重複排除キー](/help/destinations/ui/activate-batch-profile-destinations.md#deduplication-keys).

![必須の属性と重複排除キーとして選択された ID。](/help/destinations/assets/how-destinations-work/selected-identity.png)

回避策として、属性としてExperience Platformに取り込まれている場合は、書き出しに ID を追加できます。 ID 名前空間に加えて、書き出し用に XDM 属性電子メールアドレスが選択された例を以下に示します `Phone_E.164`.

![エクスポート用に選択された電子メールアドレス属性の例。](/help/destinations/assets/how-destinations-work/email-selected.png)

## ID マップからの ID の書き出しと XDM 属性としての ID の書き出し — 違い {#identity-map-or-attribute}

書き出されるレコードの数は、ID マップから ID を書き出し用に選択したか、属性としてExperience Platformに取り込まれた ID を書き出すかに応じて異なる場合があります。 [結合ポリシー](/help/profile/merge-policies/overview.md) また、id マップから id を選択する際に、書き出されるレコードの数にも重要な役割を果たします。

例えば、2 つの異なるデータセットから、1 つの顧客プロファイルに結合される次のプロファイルフラグメントがあるとします。

**プロファイルフラグメント 1**

| ID マップ | 名 | 姓 | メール属性 |
|---------|----------|---------|--------|
| email1, Loyalty ID1 | John | Doe | メール 1 |


**プロファイルフラグメント 2**

| ID マップ | 名 | 姓 | メール属性 |
|---------|----------|---------|--------|
| email2, Loyalty ID1 | John | Doe | 電子メール 2 |

結合されたプロファイルは次のようになります。

| ID マップ | 名 | 姓 | メール属性 |
|---------|----------|---------|--------|
| 電子メール 1，電子メール 2，ロイヤルティ ID1 | John | Doe | 電子メール 2 |

書き出しの動作は、 `IdentityMap: Email` または `xdm: personalEmail.address` エクスポート用。

顧客がアクティブ化した場合 `IdentityMap: Email`の場合、エクスポートされたファイルには 2 つのレコードが存在します。1 つは email1 用、もう 1 つは email2 用です。

ただし、顧客が `xdm: personalEmail.address`電子メール属性フィールドには email2 のみが含まれるので、電子メール 2 のみがレコードに存在します。 これらの状況では、顧客向けのファイルにあるすべての電子メールアドレス、または顧客向けのファイルにある最新の電子メールアドレスのみをアクティブ化する場合が考えられる、様々な使用例に対処できます。

書き出すレコードの数は、選択した結合ポリシーと、書き出しで ID と属性のどちらを選択したかによって異なります。

## API ベースのストリーミングの宛先 {#streaming-destinations}

[API ベースのストリーミングの宛先](/help/destinations/destination-types.md#streaming-destination) で作成 [Destination SDK](/help/destinations/destination-sdk/overview.md) 例： [!DNL Facebook], [!DNL Google Customer Match], [!DNL Pinterest], [!DNL Braze]など ) は、書き出し用に特定の ID のみをサポートします。 各宛先に書き出し可能な特定の ID について詳しくは、 *サポートされている id* 各宛先ドキュメントページの「 」セクション ( [サポートされる ID セクション](/help/destinations/catalog/advertising/pinterest.md) 内 [!DNL Pinterest] リンク先ページ )。

ただし、次のいずれかのデータを柔軟に使用できることに注意してください。 [プライベートグラフ](/help/profile/merge-policies/overview.md#id-stitching) または属性から ID として。 つまり、XDM 属性を、宛先で必要な ID フィールドにマッピングできます。 以下に [!DNL Pinterest] 宛先（XDM 属性） `personalEmail.address` が必須の [!DNL Pinterest] id `pinterest_audience`.

>[!TIP]
>
>ソースフィールドにハッシュ化されていない属性が含まれている場合は、 **[!UICONTROL 変換を適用]** オプションを使用して、Experience Platformがアクティベーション時にデータを自動的にハッシュ化するように設定する必要があります。 詳しくは、 **[!UICONTROL 変換を適用]** オプション [ストリーミング先のアクティベーションチュートリアル](/help/destinations/ui/activate-segment-streaming-destinations.md#apply-transformation).

![pinterest宛先の ID フィールドにマッピングされる電子メールアドレス属性の例。](/help/destinations/assets/how-destinations-work/email-mapped-to-identity.png)

### サードパーティ Cookie の統合に依存する広告の宛先 {#third-party-cookie-destinations}

サードパーティ Cookie に基づく広告の宛先 ( 例： [!DNL Google Ads], [!DNL Google Ad Manager], [!DNL Google DV360], [!DNL Bing], [!DNL The Trade Desk]) を使用する場合、アクティベーションワークフローで顧客が ID を選択する必要はありません。 これらの宛先について、アクティベーションワークフローを設定する際に、Experience Platformは、 [[!UICONTROL Experience CloudID サービス]](https://experienceleague.adobe.com/docs/id-service/using/intro/overview.html?lang=ja) およびは、プロファイルで使用可能で、宛先でサポートされているすべての id を書き出します。

これらの宛先では、 [!UICONTROL Experience CloudID サービス] または [!UICONTROL Experience PlatformWeb SDK].

次を使用する場合： [!UICONTROL Experience PlatformWeb SDK] およびレガシー [!UICONTROL Experience CloudID サービス] がページに実装されていない場合は、対象の Web サイトのデータストリームで、サードパーティ ID の同期が有効になっていることを確認する必要があります。詳しくは、 [datastream に関するドキュメントの設定](/help/edge/datastreams/configure.md#create).

上記にリンクされたドキュメントで説明したようにデータストリームを設定する場合、 **[!UICONTROL サードパーティ ID の同期]** スライダが有効になっている。 ほとんどのお客様は、 `container_id` フィールドが空白（デフォルトは 0）です。 従来のAudience Manager実装で特定のコンテナ ID を使用していた場合にのみ、この値を変更する必要があります（ただし、これは少数のお客様です）。

>[!NOTE]
>
>これらの広告の宛先のほとんどは、Audience Managerでサポートされています ( これらの宛先タイプは、Audience Managerではデバイスベースの宛先と呼ばれます )。 参照先 [Audience Managerでサポートされるすべてのデバイスベースの宛先のリスト](https://experienceleague.adobe.com/docs/audience-manager/user-guide/features/destinations/device-based/device-based-destinations-list.html?lang=en)) をクリックします。 Experience Platform: Experience PlatformとAudience Manager間でのデータの共有について詳しくは、 [Experience PlatformからAudience Managerへのデータ共有の有効化](https://experienceleague.adobe.com/docs/audience-manager/user-guide/implementation-integration-guides/integration-experience-platform/aam-aep-audience-sharing.html?lang=en#enable-aep-to-aam-data). 現在、より多くのサードパーティ Cookie の宛先をサポートする予定はありません。

## Enterprise の宛先 {#enterprise-destinations}

[Enterprise の宛先](/help/destinations/destination-types.md#streaming-profile-export) ([!DNL Amazon Kinesis], [!DNL Azure Event Hubs]、HTTP API) の場合は、データの書き出しに特定の ID は必要ありません。これらは、企業統合の使用例向けに設計されています。 ただし、必要に応じて、ID を XDM 属性として、または ID マップから書き出すことができます。 表示 [HTTP 宛先に書き出したデータの例](/help/destinations/catalog/streaming/http-destination.md#exported-data)（両方を含む） `personalEmail.address` XDM 属性と ID `ECID` および `email_lc_sha256` （ハッシュ化された電子メールアドレス）を ID マップから削除します。

## パーソナライゼーションの宛先 {#personalization-destinations}

[パーソナライズ機能（またはエッジ）の宛先](/help/destinations/destination-types.md#edge-personalization-destinations) ( 例：Adobe Target [!DNL Custom Personalization]) は、統合がプロファイル参照なので、アクティベーションワークフローで id を選択する必要はありません。 クライアント ([!DNL Target], [!DNL Web SDK]、またはその他 ) は、 [[!UICONTROL Edge]](/help/collection/home.md#edge) およびは、オンサイトのパーソナライゼーションに必要なプロファイル情報を取り込みます。

<!--
![Table with all supported identities](/help/destinations/assets/how-destinations-work/identities-table.png)

-->

## 次の手順 {#next-steps}

このドキュメントを読むと、個々の宛先でサポートされている ID や必要な ID を見つける方法がわかります。 また、各宛先タイプに対する ID 選択の仕組みも理解できるようになりました。

次に、どの [書き出し設定](/help/destinations/how-destinations-work/destinations-configurations.md) の宛先は、宛先のタイプ間で共通です。宛先のタイプは、開発者が個々の宛先レベルで設定でき、アクティベーションワークフローでユーザーが編集できる設定です。