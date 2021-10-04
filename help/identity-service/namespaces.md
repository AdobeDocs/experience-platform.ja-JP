---
keywords: Experience Platform；ホーム；人気のあるトピック；名前空間；名前空間；名前空間；名前空間；名前空間；ID 名前空間；ID 名前空間；ID;ID;ID サービス；ID サービス
solution: Experience Platform
title: ID 名前空間の概要
topic-legacy: overview
description: ID 名前空間 は、ID の関連先コンテキストのインジケーターとして機能する ID サービスのコンポーネントです。例えば、「name@email.com」の値を電子メールアドレスとして、または「443522」を数値 CRM ID として区別します。
exl-id: 86cfc7ae-943d-4474-90c8-e368afa48b7c
source-git-commit: 5373b8fcd84cee749a85bdb755a23eb7292cf352
workflow-type: tm+mt
source-wordcount: '1598'
ht-degree: 19%

---

# ID 名前空間の概要

ID 名前空間は [[!DNL Identity Service]](./home.md) のコンポーネントで、ID が関連付けられているコンテキストのインジケーターとして機能します。 例えば、「name<span>@email.com」の値を電子メールアドレスとして、または「443522」を数値 CRM ID として区別します。

## はじめに

ID 名前空間を使用するには、関連する様々な Adobe Experience Platform サービスについて理解している必要があります。名前空間の使用を開始する前に、次のサービスのドキュメントを確認してください。

- [[!DNL Real-time Customer Profile]](../profile/home.md):複数のソースからの集計データに基づいて、統合された顧客プロファイルをリアルタイムで提供します。
- [[!DNL Identity Service]](./home.md):デバイスやシステム間で ID を結び付けることで、個々の顧客とその行動をより良く把握できます。
- [[!DNL Privacy Service]](../privacy-service/home.md)：ID 名前空間を使用して、EU 一般データ保護規則（GDPR）に準拠します。名前空間には、GDPR リクエストを実行できます。

## ID 名前空間について

完全修飾 ID には、ID 値と名前空間が含まれます。プロファイルフラグメント間でレコードデータを一致させる場合は、[!DNL Real-time Customer Profile] がプロファイルデータを結合する場合と同様に、ID 値と名前空間の両方が一致する必要があります。

例えば、2 つのプロファイルフラグメントに異なるプライマリ ID が含まれていても、「Email」名前空間で同じ値が共有されている場合、[!DNL Platform] は、これらのフラグメントが実際には同じ個人であり、個人の ID グラフでデータを統合できます。

![](images/identity-service-stitching.png)

### ID タイプ

データは、複数の異なる ID タイプで識別できます。ID タイプは、ID 名前空間の作成時に指定されます。この ID タイプによって、データを ID グラフに保持するかどうか、およびそのデータの処理方法の手順が決まります。**非ユーザー識別子** を除くすべての ID タイプは、同じ動作に従って、名前空間と対応する ID 値を ID グラフクラスターに結び付けます。 **人以外の識別子** を使用する場合、データは結び付けられません。

[!DNL Platform] 内では、次の ID タイプを使用できます。

| ID タイプ | 説明 |
| --- | --- |
| cookie ID | cookie ID は Web ブラウザーを識別します。 この ID は拡張に不可欠で、ID グラフの大部分を占めます。ただし、cookie は、その性質上急速に劣化し、時間の経過と共にその価値が失われます。 |
| クロスデバイス ID | クロスデバイス ID は個人を識別し、通常は他の ID を結び付けます。 例としては、ログイン ID、CRM ID、ロイヤリティ ID などがあります。 これは、[!DNL Identity Service] に対して、値を慎重に処理することを示しています。 |
| デバイス ID | デバイス ID は、IDFA（iPhone および iPad）、GAID(Android)、RIDA(Roku) などのハードウェアデバイスを識別し、世帯の複数のユーザーが共有できます。 |
| 電子メールアドレス | 多くの場合、E メールアドレスは 1 人のユーザーに関連付けられているので、異なるチャネルをまたいでそのユーザーを識別するために使用できます。 このタイプの ID には、個人を特定できる情報（PII）が含まれています。これは、[!DNL Identity Service] に対して、値を慎重に処理することを示しています。 |
| 非人物識別子 | 非人物 ID は、名前空間を必要とするが、人物クラスターに接続されていない ID の保存に使用されます。 例えば、製品 SKU、製品、組織、店舗に関連するデータなどです。 |
| 電話番号 | 電話番号は多くの場合、1 人の人物に関連付けられているので、様々なチャネルをまたいでその人物を識別するために使用できます。 このタイプの ID には PII が含まれます。これは、[!DNL Identity Service] に対して、値を慎重に処理することを示しています。 |

### 標準名前空間

 Experience Platform には、すべての組織で使用できる ID 名前空間が複数用意されています。これらは標準名前空間と呼ばれ、 [!DNL Identity Service] API または Platform UI を使用して表示できます。

次の標準名前空間は、 Platform 内のすべての組織で使用できます。

| 表示名 | 説明 |
| ------------ | ----------- |
| AdCloud | AdCloud を表すAdobe。 |
| Adobe Analytics（レガシー ID） | Adobe Analyticsを表す名前空間。 詳しくは、[Adobe Analytics名前空間 ](https://experienceleague.adobe.com/docs/analytics/admin/data-governance/gdpr-namespaces.html?lang=en#namespaces) に関する次のドキュメントを参照してください。 |
| Apple IDFA（広告主の ID） | Apple ID for Advertisers を表す名前空間。 詳しくは、[ 興味ベースの広告 ](https://support.apple.com/ja-jp/HT202074) に関する次のドキュメントを参照してください。 |
| Apple Push Notification サービス | Apple Push Notification サービスを使用して収集された ID を表す名前空間。 詳しくは、[Apple Push Notification service](https://developer.apple.com/library/archive/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html#//apple_ref/doc/uid/TP40008194-CH8-SW1) の次のドキュメントを参照してください。 |
| CORE | Adobe Audience Managerを表す名前空間。 この名前空間は、従来の名前でも参照できます。「Adobe AudienceManager」 詳しくは、[Audience ManagerID](https://experienceleague.adobe.com/docs/audience-manager/user-guide/overview/data-privacy/data-privacy-reference/data-privacy-ids.html?lang=en#aam-ids) に関する次のドキュメントを参照してください。 |
| ECID | ECID を表す名前空間。 この名前空間は、次のエイリアスでも参照できます。&quot;Adobe Marketing Cloud ID&quot;、&quot;Adobe Experience Cloud ID&quot;、&quot;Adobe Experience Platform ID&quot;。 詳しくは、[ECID](./ecid.md) の次のドキュメントを参照してください。 |
| メール | 電子メールアドレスを表す名前空間。 このタイプの名前空間は、多くの場合、1 人のユーザーに関連付けられているので、異なるチャネルをまたいでそのユーザーを識別するために使用できます。 |
| 電子メール（SHA256、小文字） | 事前にハッシュ化された電子メールアドレス用の名前空間。 この名前空間で指定された値は、SHA256 でハッシュする前に小文字に変換されます。 E メールアドレスが正規化される前に、先頭と末尾の空白文字をトリミングする必要があります。 この設定を遡って変更することはできません。 詳しくは、[SHA256 ハッシュサポート ](https://experienceleague.adobe.com/docs/id-service/using/reference/hashing-support.html?lang=en#hashing-support) に関する次のドキュメントを参照してください。 |
| Firebase Cloud Messaging | プッシュ通知用に Google Firebase Cloud Messaging を使用して収集された ID を表す名前空間です。 詳しくは、[Google Firebase Cloud Messaging](https://firebase.google.com/docs/cloud-messaging) の次のドキュメントを参照してください。 |
| Google 広告 ID(GAID) | Google 広告 ID を表す名前空間。 詳しくは、[Google 広告 ID](https://support.google.com/googleplay/android-developer/answer/6048248?hl=en) の次のドキュメントを参照してください。 |
| Google クリック ID | Google クリック ID を表す名前空間。 詳しくは、[Google 広告でのクリック追跡 ](https://developers.google.com/adwords/api/docs/guides/click-tracking) に関する次のドキュメントを参照してください。 |
| Phone | 電話番号を表す名前空間。 このタイプの名前空間は、多くの場合、1 人のユーザーに関連付けられているので、異なるチャネルをまたいでそのユーザーを識別するために使用できます。 |
| 電話 (E.164) | E.164 形式でハッシュ化する必要がある生の電話番号を表す名前空間。 E.164 形式には、プラス記号 (`+`)、国際通話コード、地域コード、電話番号が含まれます。 例：`(+)(country code)(area code)(phone number)`。 |
| 電話 (SHA256) | SHA256 を使用してハッシュ化する必要がある電話番号を表す名前空間。 記号、文字、および先頭のゼロを削除する必要があります。 また、呼び出し元の国をプレフィックスとして追加する必要があります。 |
| 電話 (SHA256_E.164) | SHA256 形式と E.164 形式の両方を使用してハッシュ化する必要がある生の電話番号を表す名前空間。 |
| TNTID | Adobe Targetを表す名前空間。 詳しくは、[Target](https://experienceleague.adobe.com/docs/target/using/target-home.html?lang=en) の次のドキュメントを参照してください。 |
| Windows AID | Windows 広告 ID を表す名前空間。 詳しくは、[Windows 広告 ID](https://docs.microsoft.com/en-us/uwp/api/windows.system.userprofile.advertisingmanager.advertisingid?view=winrt-19041) の次のドキュメントを参照してください。 |

### ID 名前空間の表示

ID 名前空間を UI に表示するには、左のナビゲーションで「**[!UICONTROL ID]**」を選択し、「**[!UICONTROL 参照]**」を選択します。

![参照](./images/browse.png)

ID 名前空間のリストがページのメインインターフェイスに表示され、名前、ID 記号、最終更新日、および標準名前空間かカスタム名前空間かに関する情報が表示されます。 右側のレールには、[!UICONTROL ID グラフの強さ ] に関する情報が含まれています。

![ID](./images/identities.png)

Platform は、統合のための名前空間も提供します。 これらの名前空間は、他のシステムとの接続に使用され、ID のステッチには使用されないので、デフォルトでは非表示になっています。 統合名前空間を表示するには、「**[!UICONTROL 統合 ID を表示]**」を選択します。

![view-integration-identities](./images/view-integration-identities.png)

リストから ID 名前空間を選択して、特定の名前空間の情報を表示します。 ID 名前空間を選択すると、右側のパネルの表示が更新され、選択した ID 名前空間に関するメタデータ（取得された ID の数や、失敗およびスキップされたレコードの数など）が表示されます。

![select-namespace](./images/select-namespace.png)

## カスタム名前空間の管理 {#manage-namespaces}

組織のデータや使用事例によっては、カスタム名前空間が必要な場合があります。カスタム名前空間は、[[!DNL Identity Service]](./api/create-custom-namespace.md) API を使用して、または UI を通じて作成できます。

UI を使用してカスタム名前空間を作成するには、**[!UICONTROL ID]** ワークスペースに移動し、**[!UICONTROL 参照]** を選択して、**[!UICONTROL ID 名前空間を作成]** を選択します。

![select-create](./images/select-create.png)

**[!UICONTROL ID 名前空間の作成]** ダイアログボックスが表示されます。 一意の **[!UICONTROL 表示名]** と **[!UICONTROL ID 記号]** を指定し、作成する ID タイプを選択します。 オプションで説明を追加して、名前空間に関する詳細情報を追加することもできます。 **Non-people identifier** を除くすべての ID タイプは、結び付けと同じ動作に従います。 名前空間の作成時に ID タイプとして **Non-people identifier** を選択した場合、結び付けはおこなわれません。 各 ID タイプに関する詳細は、[ID タイプ ](#identity-types) の表を参照してください。

完了したら、「**[!UICONTROL 作成]**」をクリックします。

>[!IMPORTANT]
>
>定義した名前空間は組織内で非公開で、正常に作成するには一意の ID 記号が必要です。

![create-identity-namespace](./images/create-identity-namespace.png)

標準の名前空間と同様に、「**[!UICONTROL 参照]**」タブからカスタム名前空間を選択して、その詳細を表示できます。 ただし、カスタム名前空間では、詳細領域から表示名や説明を編集することもできます。

>[!NOTE]
>
>作成した名前空間は削除できず、ID 記号とタイプは変更できません。

## ID データの名前空間

ID の名前空間をどのように指定するかは、ID データの提供方法によって異なります。ID データの提供方法について詳しくは、「 overview」の [ID データの提供](./home.md#supplying-identity-data-to-identity-service)に関する節を参照してください。[!DNL Identity Service]

## 次の手順

ID 名前空間の主要概念を理解したら、[ID グラフビューア ](./ui/identity-graph-viewer.md) を使用して ID グラフを操作する方法を学び始めます。
