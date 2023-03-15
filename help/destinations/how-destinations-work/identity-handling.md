---
title: 宛先アクティブ化ワークフローでの ID の処理
description: 宛先のタイプに応じた、アクティベーションワークフローにおける ID の書き出しの処理方法を学ぶ
source-git-commit: 372231ab4fc1148c1c2c0c5fdbfd3cd5328b17cc
workflow-type: tm+mt
source-wordcount: '1186'
ht-degree: 100%

---

# 宛先アクティブ化ワークフローでの ID の処理

このページでは、ID を様々な宛先タイプに書き出す方法の詳細について説明し、宛先に応じて書き出すことができる ID を見つける方法についても説明します。

>[!TIP]
>
> ID、ID 名前空間および ID 関連用語の定義に関する情報について詳しくは、[ID サービスの概要](/help/identity-service/home.md)を参照してください。

[カタログ](/help/destinations/catalog/overview.md)内の宛先はそれぞれ微妙に異なり、すべての宛先に万能の設定はありません。ただし、以下の節で説明するように、宛先とその ID 要件の設定をガイドするパターンがいくつかあります。

## ファイルベースの宛先 {#file-based}

[ファイルベースの宛先](/help/destinations/destination-types.md#file-based)（例えば、[!DNL Amazon S3]、SFTP、[!DNL Adobe Campaign]、[!DNL Oracle Eloqua]、[!DNL Salesforce Marketing Cloud] などのほとんどのメールマーケティングの宛先）の場合、これらの宛先のほとんどでの ID 設定はオープンです。つまり、バッチアクティベーションワークフローの[属性を選択](/help/destinations/ui/activate-batch-profile-destinations.md#select-attributes)手順で ID を選択する必要はありません。

ファイルの書き出しに ID を追加することを選択した場合、書き出しでは [ID 名前空間](/help/identity-service/ui/identity-graph-viewer.md#access-identity-graph-viewer)から 1 つの ID しか選択できません。書き出す ID を選択すると、[必須属性](/help/destinations/ui/activate-batch-profile-destinations.md#mandatory-attributes)および[重複排除キー](/help/destinations/ui/activate-batch-profile-destinations.md#deduplication-keys)として自動的に選択されます。

![必須属性および重複排除キーとして選択された ID。](/help/destinations/assets/how-destinations-work/selected-identity.png)

回避策として、ID が属性として Experience Platform に取り込まれている場合は、さらに ID を書き出しに追加できます。ID 名前空間 `Phone_E.164` に加えて、XDM 属性メールアドレスが書き出し用に選択された例を以下に示します。

![書き出し用に選択されたメールアドレス属性の例。](/help/destinations/assets/how-destinations-work/email-selected.png)

## ID マップからの ID の書き出しと XDM 属性としての ID の書き出し - 相違点 {#identity-map-or-attribute}

書き出されるレコードの数は、ID マップから書き出す ID を選択するか、Experience Platform に属性として取り込まれた ID を選択するかによって異なる場合があります。[結合ポリシー](/help/profile/merge-policies/overview.md)は、ID マップから ID を選択する際に書き出されるレコードの数にも重要な役割を果たします。

例えば、2 つの異なるデータセットから、1 つの顧客プロファイルに結合される次のプロファイルフラグメントがあるとします。

**プロファイルフラグメント 1**

| ID マップ | 名 | 姓 | メール属性 |
|---------|----------|---------|--------|
| email1、Loyalty ID1 | John | Doe | email 1 |


**プロファイルフラグメント 2**

| ID マップ | 名 | 姓 | メール属性 |
|---------|----------|---------|--------|
| email2、Loyalty ID1 | John | Doe | email 2 |

結合されたプロファイルは次のようになります。

| ID マップ | 名 | 姓 | メール属性 |
|---------|----------|---------|--------|
| email 1、email2、Loyalty ID1 | John | Doe | email 2 |

書き出しの動作は、書き出しに `IdentityMap: Email` または `xdm: personalEmail.address` のどちらを選択するかによって異なります。

顧客が `IdentityMap: Email` をアクティブ化すると、書き出されたファイルには、email1 用と email2 用の 2 つのレコードが含まれます。

ただし、顧客が `xdm: personalEmail.address` をアクティブ化すると、email 属性フィールドには email2 のみが含まれるので、email2 のみがレコードに表示されます。これらの状況は、顧客のファイルにあるすべてのメールアドレスや、顧客のファイルにある最新のメールアドレスに対してアクティブ化する必要がある様々なユースケースに対応できます。

書き出すレコードの数は、選択した結合ポリシーと、書き出しで ID または属性を選択したかどうかによって決まります。

## API ベースのストリーミング宛先 {#streaming-destinations}

[Destination SDK](/help/destinations/destination-sdk/overview.md) で作成した [API ベースのストリーミングの宛先](/help/destinations/destination-types.md#streaming-destination)（[!DNL Facebook]、[!DNL Google Customer Match]、[!DNL Pinterest]、[!DNL Braze] など）は、書き出し用に特定の ID のみをサポートします。各宛先に書き出し可能な特定の ID について詳しくは、各宛先のドキュメントページの&#x200B;*サポートされている ID* 節（例えば、[!DNL Pinterest] の宛先のページの[サポートされている ID](/help/destinations/catalog/advertising/pinterest.md) に関する節）を参照してください。

ただし、[プライベートグラフ](/help/profile/merge-policies/overview.md#id-stitching)または属性のどちらのデータも ID として柔軟に使用できることに注意してください。つまり、XDM 属性を、宛先で必要な ID フィールドにマッピングできます。 [!DNL Pinterest] の宛先の例を以下に示します。この例では、XDM 属性 `personalEmail.address` が必須の [!DNL Pinterest] ID `pinterest_audience` にマッピングされています。

>[!TIP]
>
>ハッシュ化されていない属性がソースフィールドに含まれている場合は、「**[!UICONTROL 変換を適用]**」オプションをオンにして、アクティブ化時に Experience Platform がデータを自動的にハッシュ化するように設定します。 詳しくは、[ストリーミング宛先のアクティブ化に関するチュートリアル](/help/destinations/ui/activate-segment-streaming-destinations.md#apply-transformation)で「**[!UICONTROL 変換を適用]**」オプションの説明を参照してください。

![Pinterest の宛先で ID フィールドにマッピングされるメールアドレス属性の例](/help/destinations/assets/how-destinations-work/email-mapped-to-identity.png)

### サードパーティ Cookie の統合に基づく広告宛先 {#third-party-cookie-destinations}

サードパーティ Cookie に基づく広告の宛先（例えば [!DNL Google Ads]、[!DNL Google Ad Manager]、[!DNL Google DV360]、[!DNL Bing]、[!DNL The Trade Desk] など）を使用する場合は、アクティブ化ワークフローで顧客が ID を選択する必要はありません。 これらの宛先については、アクティブ化ワークフローを設定する際に、Experience Platform が、[[!UICONTROL Experience Cloud ID サービス]](https://experienceleague.adobe.com/docs/id-service/using/intro/overview.html?lang=ja)で作成された ID 照合テーブルを自動的に検索し、プロファイルに使用可能で宛先でサポートされているすべての ID を書き出します。

これらの宛先では、[!UICONTROL Experience CloudI D サービス] または [!UICONTROL Experience Platform Web SDK] のどちらかで ID 同期が行われる必要があります。

[!UICONTROL Experience Platform Web SDK] を使用していて、従来の [!UICONTROL Experience Cloud ID サービス]がページに実装されていない場合は、問題の web サイトのデータストリームでサードパーティ ID の同期が有効になっていることを確認する必要があります（概要については[データストリームの設定に関するドキュメント](/help/edge/datastreams/configure.md#create)を参照）。

上記でリンクを示したドキュメントの説明に従ってデータストリームを設定する際は、**[!UICONTROL サードパーティ ID の同期]**&#x200B;スライダーが有効になっていることを確認する必要があります。ほとんどのお客様は、`container_id` フィールドを空白のまま残すでしょう（デフォルトは 0）。 従来の Audience Manager 実装で特定のコンテナ ID を使用していた場合にのみ、この値を変更する必要があります（ただし、それはごく少数のお客様の場合です）。

>[!NOTE]
>
>これらの広告宛先のほとんどは、Audience Manager でサポートされています（これらの宛先タイプは、Audience Manager ではデバイスベースの宛先と呼ばれます）。 [Audience Manager でサポートされているデバイスベースの宛先の全リスト](https://experienceleague.adobe.com/docs/audience-manager/user-guide/features/destinations/device-based/device-based-destinations-list.html?lang=ja)を参照してください。Experience Platform には、ほんの一部のみリストされています。Experience Platform と Audience Manager とのデータの共有について詳しくは、[Experience Platform から Audience Manager へのデータ共有の有効化](https://experienceleague.adobe.com/docs/audience-manager/user-guide/implementation-integration-guides/integration-experience-platform/aam-aep-audience-sharing.html?lang=ja#enable-aep-to-aam-data)に関する節を参照してください。現在、サポートしているサードパーティ Cookie の宛先を増やす予定はありません。

## エンタープライズの宛先 {#enterprise-destinations}

[エンタープライズの宛先](/help/destinations/destination-types.md#streaming-profile-export)（[!DNL Amazon Kinesis]、[!DNL Azure Event Hubs]、HTTP API）の場合は、データの書き出しに特定の ID は必要ありません。これらは、エンタープライズ統合のユースケース向けに設計されているからです。 ただし、必要に応じて、ID を XDM 属性として、または ID マップから書き出すことができます。 [HTTP の宛先に書き出されたデータの例](/help/destinations/catalog/streaming/http-destination.md#exported-data)を示します。この例では、`personalEmail.address` XDM 属性と ID `ECID` および `email_lc_sha256`（ハッシュ化されたメールアドレス）をどちらも含んでいます。

## パーソナライゼーションの宛先 {#personalization-destinations}

[パーソナライゼーション（またはエッジ）の宛先](/help/destinations/destination-types.md#edge-personalization-destinations)（例えば Adobe Target や [!DNL Custom Personalization] ）では、統合がプロファイルルックアップなので、アクティブ化ワークフローでの ID 選択は必要ありません。 クライアント（[!DNL Target]、[!DNL Web SDK] など）は、[[!UICONTROL Edge]](/help/collection/home.md#edge) にクエリを発行し、オンサイトのパーソナライゼーションに必要なプロファイル情報を取り込みます。

<!--
![Table with all supported identities](/help/destinations/assets/how-destinations-work/identities-table.png)

-->

## 次の手順 {#next-steps}

このドキュメントを通して、個々の宛先でサポートされている ID や必要な ID を見つける方法がわかりました。 また、宛先タイプごとの ID 選択の仕組みも理解できました。

次は、宛先の[書き出し設定](/help/destinations/how-destinations-work/destinations-configurations.md)について、様々な宛先タイプ間で共通なものはどれか、開発者が個々の宛先レベルで設定できるものはどれか、アクティブ化ワークフローでユーザーが編集できるものはどれかを学びます。

また、[カタログ](/help/destinations/catalog/overview.md)に登録されているすべての宛先も確認できます。