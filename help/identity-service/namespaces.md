---
keywords: Experience Platform；ホーム；人気のあるトピック；名前空間;名前空間;名前空間;名前空間;ID名前空間;ID名前空間;ID;ID;IDサービス；IDサービス
solution: Experience Platform
title: ID名前空間の概要
topic-legacy: overview
description: ID 名前空間は、ID が関連するコンテキストのインジケーターとして機能する ID サービスのコンポーネントです。例えば、「name@email.com」という値は電子メールアドレスとして、または「443522」という数値のCRM IDとして区別されます。
exl-id: 86cfc7ae-943d-4474-90c8-e368afa48b7c
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '1474'
ht-degree: 20%

---

# ID 名前空間の概要

ID名前空間は、IDが関連付けられるコンテキストのインジケーターとして機能する[[!DNL Identity Service]](./home.md)のコンポーネントです。 例えば、「name<span>@email.com」の値を電子メールアドレスとして、または「443522」を数値 CRM ID として区別します。

## はじめに

ID 名前空間を使用するには、関連する様々な Adobe Experience Platform サービスについて理解している必要があります。名前空間の使用を開始する前に、次のサービスのドキュメントを確認してください。

- [[!DNL Real-time Customer Profile]](../profile/home.md):複数のソースからの集計データに基づいて、リアルタイムで統合された顧客プロファイルを提供します。
- [[!DNL Identity Service]](./home.md):デバイスとシステム間でIDをブリッジ化することで、個々の顧客とその行動をより良く表示できます。
- [[!DNL Privacy Service]](../privacy-service/home.md)：ID 名前空間を使用して、EU 一般データ保護規則（GDPR）に準拠します。名前空間には、GDPR リクエストを実行できます。

## ID 名前空間について

完全修飾 ID には、ID 値と名前空間が含まれます。プロファイルフラグメント間でレコードデータを照合する場合は、[!DNL Real-time Customer Profile]がプロファイルデータを結合する場合と同様に、ID値と名前空間の両方を一致させる必要があります。

例えば、2つのプロファイルフラグメントに異なるプライマリIDが含まれていても、それらが「電子メール」名前空間の同じ値を共有している場合、[!DNL Platform]は、これらのフラグメントが実際には同じ個人であることを確認し、個々のIDグラフにデータをまとめます。

![](images/identity-service-stitching.png)

### ID タイプ

データは、複数の異なる ID タイプで識別できます。ID タイプは、ID 名前空間の作成時に指定されます。この ID タイプによって、データを ID グラフに保持するかどうか、およびそのデータの処理方法の手順が決まります。

[!DNL Platform]内では次のIDタイプを使用できます。

| ID タイプ | 説明 |
| --- | --- |
| cookie ID | cookie IDはWebブラウザーを識別します。 この ID は拡張に不可欠で、ID グラフの大部分を占めます。ただし、cookie は、その性質上急速に劣化し、時間の経過と共にその価値が失われます。 |
| デバイス間ID | デバイス間IDは個人を識別し、通常は他のIDを相互に関連付けます。 ログインID、CRM ID、忠誠度IDなどがあります。 これは[!DNL Identity Service]に対して、値を慎重に処理することを示しています。 |
| デバイスID | デバイスIDは、IDFA（iPhoneおよびiPad）、GAID(Android)、RIDA(Roku)などのハードウェアデバイスを識別し、世帯の複数の人が共有できます。 |
| 電子メールアドレス | 電子メールアドレスは多くの場合、1人のユーザーに関連付けられているので、異なるチャネルでそのユーザーを識別するために使用できます。 このタイプの ID には、個人を特定できる情報（PII）が含まれています。これは[!DNL Identity Service]に対して、値を慎重に処理することを示しています。 |
| 非人ID | 人物以外のIDは、名前空間を必要とするが、人物クラスターに接続されていないIDを格納するために使用されます。 例えば、製品SKU、製品、組織、ストアに関連するデータなどです。 |
| 電話番号 | 電話番号は多くの場合、1人の人と関連付けられているので、異なるチャネルでその人を識別するのに使用できます。 このタイプの ID には PII が含まれます。これは[!DNL Identity Service]に対して、値を慎重に処理することを示しています。 |

### 標準名前空間

 Experience Platform には、すべての組織で使用できる ID 名前空間が複数用意されています。これらは標準名前空間と呼ばれ、[!DNL Identity Service] APIを使用して、またはプラットフォームUIから表示されます。

次の標準名前空間は、 Platform 内のすべての組織で使用できます。

| 表示名 | 説明 |
| ------------ | ----------- |
| AdCloud | AdobeAdCloudを表す名前空間。 |
| Adobe Analytics（レガシーID） | Adobe Analyticsを表す名前空間。 詳しくは、[Adobe Analytics名前空間](https://experienceleague.adobe.com/docs/analytics/admin/data-governance/gdpr-namespaces.html?lang=en#namespaces)の次のドキュメントを参照してください。 |
| Apple IDFA（広告主のID） | 広告主のApple IDを表す名前空間です。 詳しくは、[関心ベースの広告](https://support.apple.com/en-us/HT202074)に関する以下のドキュメントを参照してください。 |
| Apple Push Notificationサービス | Apple Push Notificationサービスを使用して収集されたIDを表す名前空間ーです。 詳しくは、[Apple Push Notification service](https://developer.apple.com/library/archive/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html#//apple_ref/doc/uid/TP40008194-CH8-SW1)の次のドキュメントを参照してください。 |
| CORE | Adobe Audience Managerを表す名前空間。 この名前空間は、従来の名前で参照することもできます。「Adobe AudienceManager」 詳しくは、[Audience ManagerID](https://experienceleague.adobe.com/docs/audience-manager/user-guide/overview/data-privacy/data-privacy-reference/data-privacy-ids.html?lang=en#aam-ids)の次のドキュメントを参照してください。 |
| ECID | ECIDを表す名前空間。 この名前空間は、次の別名でも参照できます。&quot;Adobe Marketing CloudID&quot;、&quot;Adobe Experience CloudID&quot;、&quot;Adobe Experience PlatformID&quot;の3種類のIDを使用できます。 詳しくは、[ECID](./ecid.md)の次のドキュメントを参照してください。 |
| Email | 電子メールアドレスを表す名前空間。 このタイプの名前空間は多くの場合、1人の人に関連付けられるので、異なるチャネル間でその人を識別するのに使用できます。 |
| 電子メール（SHA256、小文字） | 事前にハッシュされた電子メールアドレスの名前空間です。 この名前空間で指定する値は、SHA256でハッシュする前に小文字に変換されます。 電子メールアドレスを標準化する前に、先頭と末尾の空白文字をトリミングする必要があります。 この設定は、遡って変更することはできません。 詳しくは、[SHA256 hashing support](https://experienceleague.adobe.com/docs/id-service/using/reference/hashing-support.html?lang=en#hashing-support)の次のドキュメントを参照してください。 |
| Firebase Cloud Messaging | Google Firebase Cloud Messagingを使用してプッシュ通知用に収集されたIDを表す名前空間です。 詳しくは、[Google Firebase Cloud Messaging](https://firebase.google.com/docs/cloud-messaging)の次のドキュメントを参照してください。 |
| Google広告ID(GAID) | Google広告IDを表す名前空間。 詳しくは、[Google広告ID](https://support.google.com/googleplay/android-developer/answer/6048248?hl=en)の次のドキュメントを参照してください。 |
| Google Click ID | GoogleクリックIDを表す名前空間。 詳しくは、[Google広告でのクリック追跡](https://developers.google.com/adwords/api/docs/guides/click-tracking)の次のドキュメントを参照してください。 |
| Phone | 電話番号を表す名前空間。 このタイプの名前空間は多くの場合、1人の人に関連付けられるので、異なるチャネル間でその人を識別するのに使用できます。 |
| 電話(E.164) | E.164形式でハッシュする必要がある生の電話番号を表す名前空間です。 E.164形式には、プラス記号(`+`)、国際国の通話コード、地域番号、電話番号が含まれます。 例：`(+)(country code)(area code)(phone number)`。 |
| 電話(SHA256) | SHA256を使用してハッシュする必要がある電話番号を表す名前空間です。 記号、文字、および先頭のゼロを削除する必要があります。 また、コードを呼び出す国をプレフィックスとして追加する必要があります。 |
| 電話(SHA256_E.164) | SHA256とE.164の両方の形式を使用してハッシュする必要がある生の電話番号を表す名前空間です。 |
| TNTID | Adobe Targetを表す名前空間。 詳しくは、[ターゲット](https://experienceleague.adobe.com/docs/target/using/target-home.html?lang=en)の次のドキュメントを参照してください。 |
| Windows AID | Windows広告IDを表す名前空間です。 詳しくは、[Windows広告ID](https://docs.microsoft.com/en-us/uwp/api/windows.system.userprofile.advertisingmanager.advertisingid?view=winrt-19041)の次のドキュメントを参照してください。 |

UIの標準名前空間を表示するには、左側のナビゲーションで「**[!UICONTROL ID]**」を選択し、「**[!UICONTROL 参照]**」タブを選択して、組織がアクセスできる標準ID名前空間のリストを表示します。 名前空間は、**[!UICONTROL 表示名]**、**[!UICONTROL ID記号]**、または&#x200B;**[!UICONTROL 所有者]**&#x200B;でアルファベット順に並べ替えることができます。 または、名前空間を最新の更新日順に並べ替えることもできます。

名前空間を選択すると、右側のパネルに詳細情報が表示されます。

>[!NOTE]
>
>プラットフォームは、統合のための名前空間も提供します。 これらの名前空間は、他のシステムとの接続に使用されるので、デフォルトでは非表示になり、IDのステッチには使用されません。 表示統合名前空間を選択するには、**[!UICONTROL 表示統合ID]**&#x200B;を選択します。

![](./images/browse-namespaces.png)

## カスタム名前空間の管理{#manage-namespaces}

組織のデータや使用事例によっては、カスタム名前空間が必要な場合があります。カスタム名前空間は[[!DNL Identity Service]](./api/create-custom-namespace.md) APIを使用して、またはUIを通じて作成できます。

UIを使用してカスタム名前空間を作成するには、**[!UICONTROL ID]**&#x200B;ワークスペースに移動し、**[!UICONTROL 参照]**&#x200B;を選択してから、**[!UICONTROL ID名前空間を作成]**&#x200B;を選択します。

![](./images/create.png)

**[!UICONTROL ID名前空間の作成]**&#x200B;ダイアログボックスが表示されます。 一意の&#x200B;**[!UICONTROL 表示名]**&#x200B;と&#x200B;**[!UICONTROL 識別記号]**&#x200B;を指定し、作成するIDの種類を選択します。 オプションで、名前空間に関する詳細情報に説明を追加することもできます。 終了したら、「**[!UICONTROL 作成]**」を選択します。

>[!IMPORTANT]
>
>定義した名前空間は組織内で非公開で、正しく作成するには一意のID記号が必要です。

![](./images/create-namespace.png)

標準名前空間と同様に、「**[!UICONTROL 参照]**」タブからカスタム名前空間を選択して、詳細を表示できます。 ただし、カスタム名前空間では、詳細領域から表示名や説明を編集することもできます。

>[!NOTE]
>
>名前空間を作成すると、削除できなくなり、そのID記号とタイプを変更できなくなります。

## ID データの名前空間

ID の名前空間をどのように指定するかは、ID データの提供方法によって異なります。ID データの提供方法について詳しくは、「 overview」の [ID データの提供](./home.md#supplying-identity-data-to-identity-service)に関する節を参照してください。[!DNL Identity Service]

## 次の手順

ID名前空間の主要な概念を理解したら、[IDグラフビューア](./ui/identity-graph-viewer.md)を使用して、IDグラフの操作方法を学ぶことができます。
