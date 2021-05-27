---
keywords: Experience Platform；ホーム；人気のあるトピック；名前空間；名前空間；名前空間；名前空間；ID名前空間；ID名前空間；ID;ID;IDサービス；IDサービス
solution: Experience Platform
title: ID名前空間の概要
topic-legacy: overview
description: ID 名前空間は、ID が関連するコンテキストのインジケーターとして機能する ID サービスのコンポーネントです。例えば、「name@email.com」の値を電子メールアドレスとして、または「443522」を数値CRM IDとして区別します。
exl-id: 86cfc7ae-943d-4474-90c8-e368afa48b7c
source-git-commit: 700012988fd46e835dcbc441c39f08e4c172ef0f
workflow-type: tm+mt
source-wordcount: '1638'
ht-degree: 18%

---

# ID 名前空間の概要

ID名前空間は、IDが関連するコンテキストのインジケーターとして機能する[[!DNL Identity Service]](./home.md)のコンポーネントです。 例えば、「name<span>@email.com」の値を電子メールアドレスとして、または「443522」を数値 CRM ID として区別します。

## はじめに

ID 名前空間を使用するには、関連する様々な Adobe Experience Platform サービスについて理解している必要があります。名前空間の使用を開始する前に、次のサービスのドキュメントを確認してください。

- [[!DNL Real-time Customer Profile]](../profile/home.md):複数のソースからの集計データに基づいて、統合された顧客プロファイルをリアルタイムで提供します。
- [[!DNL Identity Service]](./home.md):デバイスやシステム間でIDを結び付けることで、個々の顧客とその行動をより良く把握できます。
- [[!DNL Privacy Service]](../privacy-service/home.md)：ID 名前空間を使用して、EU 一般データ保護規則（GDPR）に準拠します。名前空間には、GDPR リクエストを実行できます。

## ID 名前空間について

完全修飾 ID には、ID 値と名前空間が含まれます。プロファイルフラグメント間でレコードデータを一致させる場合は、[!DNL Real-time Customer Profile]がプロファイルデータを結合する場合と同様に、ID値と名前空間の両方が一致する必要があります。

例えば、2つのプロファイルフラグメントに異なるプライマリIDが含まれていても、「Eメール」名前空間で同じ値が共有されるので、[!DNL Platform]は、これらのフラグメントが実際には同じ個人であることを確認し、個人のIDグラフにデータを統合できます。

![](images/identity-service-stitching.png)

### ID タイプ

データは、複数の異なる ID タイプで識別できます。ID タイプは、ID 名前空間の作成時に指定されます。この ID タイプによって、データを ID グラフに保持するかどうか、およびそのデータの処理方法の手順が決まります。**非ユーザー識別子**&#x200B;を除くすべてのIDタイプは、名前空間と対応するID値をIDグラフクラスターに結び付けるのと同じ動作に従います。 **人以外の識別子**&#x200B;を使用する場合、データは結合されません。

[!DNL Platform]内では、次のIDタイプを使用できます。

| ID タイプ | 説明 |
| --- | --- |
| cookie ID | cookie IDはWebブラウザーを識別します。 この ID は拡張に不可欠で、ID グラフの大部分を占めます。ただし、cookie は、その性質上急速に劣化し、時間の経過と共にその価値が失われます。 |
| クロスデバイスID | クロスデバイスIDは個人を識別し、通常は他のIDを結び付けます。 例としては、ログインID、CRM ID、ロイヤルティIDなどがあります。 これは、[!DNL Identity Service]に対して、値を慎重に処理することを示しています。 |
| デバイスID | デバイスIDは、IDFA（iPhoneおよびiPad）、GAID(Android)、RIDA(Roku)などのハードウェアデバイスを識別し、世帯の複数のユーザーが共有できます。 |
| 電子メールアドレス | Eメールアドレスは多くの場合、1人の人物に関連付けられているので、様々なチャネルをまたいでその人物を識別するために使用できます。 このタイプの ID には、個人を特定できる情報（PII）が含まれています。これは、[!DNL Identity Service]に対して、値を慎重に処理することを示しています。 |
| 人以外の識別子 | 非ユーザーIDは、名前空間を必要とするが、人物クラスターに接続されていないIDを保存するために使用されます。 例えば、製品SKU、製品、組織、店舗に関連するデータなどです。 |
| 電話番号 | 電話番号は多くの場合、1人の人物に関連付けられているので、様々なチャネルをまたいでその人物を識別するために使用できます。 このタイプの ID には PII が含まれます。これは、[!DNL Identity Service]に対して、値を慎重に処理することを示しています。 |

### 標準名前空間

 Experience Platform には、すべての組織で使用できる ID 名前空間が複数用意されています。これらは標準名前空間と呼ばれ、[!DNL Identity Service] APIを使用して、またはPlatform UIを通じて表示できます。

次の標準名前空間は、 Platform 内のすべての組織で使用できます。

| 表示名 | 説明 |
| ------------ | ----------- |
| AdCloud | AdCloudを表すAdobe。 |
| Adobe Analytics（レガシーID） | Adobe Analyticsを表す名前空間。 詳しくは、[Adobe Analytics名前空間](https://experienceleague.adobe.com/docs/analytics/admin/data-governance/gdpr-namespaces.html?lang=en#namespaces)に関する次のドキュメントを参照してください。 |
| Apple IDFA（広告主のID） | Apple IDを表す名前空間。 詳しくは、[関心に基づく広告](https://support.apple.com/ja-jp/HT202074)に関する以下のドキュメントを参照してください。 |
| Apple Push Notificationサービス | Apple Push Notificationサービスを使用して収集されたIDを表す名前空間。 詳しくは、[Apple Push Notification service](https://developer.apple.com/library/archive/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html#//apple_ref/doc/uid/TP40008194-CH8-SW1)の次のドキュメントを参照してください。 |
| CORE | Adobe Audience Managerを表す名前空間。 この名前空間は、従来の名前でも参照できます。「Adobe AudienceManager」 詳しくは、[Audience ManagerID](https://experienceleague.adobe.com/docs/audience-manager/user-guide/overview/data-privacy/data-privacy-reference/data-privacy-ids.html?lang=en#aam-ids)に関する以下のドキュメントを参照してください。 |
| ECID | ECIDを表す名前空間。 この名前空間は、次のエイリアスでも参照できます。&quot;Adobe Marketing Cloud ID&quot;、&quot;Adobe Experience Cloud ID&quot;、&quot;Adobe Experience Platform ID&quot;。 詳しくは、[ECID](./ecid.md)に関する次のドキュメントを参照してください。 |
| Email | 電子メールアドレスを表す名前空間。 このタイプの名前空間は、多くの場合、1人の人に関連付けられるので、異なるチャネルをまたいでその人を識別するために使用できます。 |
| 電子メール（SHA256、小文字） | 事前にハッシュ化された電子メールアドレス用の名前空間。 この名前空間で指定された値は、SHA256でハッシュする前に小文字に変換されます。 Eメールアドレスを正規化する前に、先頭と末尾の空白文字をトリミングする必要があります。 この設定を遡って変更することはできません。 詳しくは、[SHA256ハッシュサポート](https://experienceleague.adobe.com/docs/id-service/using/reference/hashing-support.html?lang=en#hashing-support)に関する次のドキュメントを参照してください。 |
| Firebase Cloud Messaging | プッシュ通知用にGoogle Firebase Cloud Messagingを使用して収集されたIDを表す名前空間です。 詳しくは、[Google Firebase Cloud Messaging](https://firebase.google.com/docs/cloud-messaging)の次のドキュメントを参照してください。 |
| Google広告ID(GAID) | Google広告IDを表す名前空間。 詳しくは、[Google広告ID](https://support.google.com/googleplay/android-developer/answer/6048248?hl=en)の次のドキュメントを参照してください。 |
| GoogleクリックID | GoogleクリックIDを表す名前空間。 詳しくは、[Google広告のクリック追跡](https://developers.google.com/adwords/api/docs/guides/click-tracking)に関する以下のドキュメントを参照してください。 |
| Phone | 電話番号を表す名前空間。 このタイプの名前空間は、多くの場合、1人の人に関連付けられるので、異なるチャネルをまたいでその人を識別するために使用できます。 |
| 電話(E.164) | E.164形式でハッシュ化する必要がある生の電話番号を表す名前空間。 E.164形式には、プラス記号(`+`)、国際通話コード、地域番号、電話番号が含まれます。 例：`(+)(country code)(area code)(phone number)`。 |
| 電話(SHA256) | SHA256を使用してハッシュ化する必要がある電話番号を表す名前空間。 記号、文字、および先頭のゼロを削除する必要があります。 また、呼び出し元の国をプレフィックスとして追加する必要があります。 |
| 電話(SHA256_E.164) | SHA256形式とE.164形式の両方を使用してハッシュ化する必要がある生の電話番号を表す名前空間。 |
| TNTID | Adobe Targetを表す名前空間。 詳しくは、[Target](https://experienceleague.adobe.com/docs/target/using/target-home.html?lang=en)に関する次のドキュメントを参照してください。 |
| Windows AID | Windows広告IDを表す名前空間。 詳しくは、[Windows Advertising ID](https://docs.microsoft.com/en-us/uwp/api/windows.system.userprofile.advertisingmanager.advertisingid?view=winrt-19041)の次のドキュメントを参照してください。 |

### ID名前空間の表示

UIでID名前空間を表示するには、左側のナビゲーションで「**[!UICONTROL ID]**」を選択し、「**[!UICONTROL 参照]**」を選択します。

![参照](./images/browse.png)

ID名前空間のリストがページのメインインターフェイスに表示され、名前、ID記号、最終更新日、および標準名前空間かカスタム名前空間かに関する情報が表示されます。 右側のレールには、[!UICONTROL 一意のID]と[!UICONTROL IDグラフの強さ]に関する情報が含まれます。 [!UICONTROL 一意の] IDは、使用する特定のサンドボックスに存在するIDの数を表しま [!UICONTROL す。IDグラフは強化されていま] す。サンドボックス内のCookieの数と非Cookie IDの数に関する情報を表示します。

![id](./images/identities.png)

Platformは、統合のための名前空間も提供します。 これらの名前空間は、他のシステムとの接続に使用され、IDのステッチには使用されないので、デフォルトでは非表示になっています。 統合名前空間を表示するには、「**[!UICONTROL 統合IDを表示]**」を選択します。

![view-integration-identities](./images/view-integration-identities.png)

リストからID名前空間を選択して、特定の名前空間の情報を表示します。 ID名前空間を選択すると、右側のパネルの表示が更新され、取得されたIDの数や、失敗してスキップされたレコード数など、選択したID名前空間に関するメタデータが表示されます。

![select-namespace](./images/select-namespace.png)

## カスタム名前空間{#manage-namespaces}を管理します

組織のデータや使用事例によっては、カスタム名前空間が必要な場合があります。カスタム名前空間は、[[!DNL Identity Service]](./api/create-custom-namespace.md) APIを使用して、またはUIを使用して作成できます。

UIを使用してカスタム名前空間を作成するには、**[!UICONTROL ID]**&#x200B;ワークスペースに移動し、**[!UICONTROL 参照]**&#x200B;を選択して、**[!UICONTROL ID名前空間を作成]**&#x200B;を選択します。

![select-create](./images/select-create.png)

**[!UICONTROL ID名前空間の作成]**&#x200B;ダイアログボックスが表示されます。 一意の&#x200B;**[!UICONTROL 表示名]**&#x200B;と&#x200B;**[!UICONTROL ID記号]**&#x200B;を指定し、作成するIDタイプを選択します。 オプションで説明を追加して、名前空間に関する詳細情報を追加することもできます。 **Non-people identifier**&#x200B;を除くすべてのIDタイプは、結び付けの同じ動作に従います。 名前空間の作成時にIDタイプとして&#x200B;**Non-people identifier**&#x200B;を選択した場合、ステッチはおこなわれません。 各IDタイプに関する詳細は、[IDタイプ](#identity-types)の表を参照してください。

完了したら、「**[!UICONTROL 作成]**」をクリックします。

>[!IMPORTANT]
>
>定義した名前空間は組織内で非公開で、正常に作成するには一意のID記号が必要です。

![create-identity-namespace](./images/create-identity-namespace.png)

標準の名前空間と同様に、「**[!UICONTROL 参照]**」タブからカスタム名前空間を選択して、詳細を表示できます。 ただし、カスタム名前空間では、詳細領域から表示名と説明を編集することもできます。

>[!NOTE]
>
>作成した名前空間は削除できず、ID記号とタイプは変更できません。

## ID データの名前空間

ID の名前空間をどのように指定するかは、ID データの提供方法によって異なります。ID データの提供方法について詳しくは、「 overview」の [ID データの提供](./home.md#supplying-identity-data-to-identity-service)に関する節を参照してください。[!DNL Identity Service]

## 次の手順

ID名前空間の主要概念を理解したので、[IDグラフビューア](./ui/identity-graph-viewer.md)を使用してIDグラフを操作する方法を学ぶことができます。
