---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Adobe Experience Platform ID サービス
topic: overview
translation-type: tm+mt
source-git-commit: 6ffdcc2143914e2ab41843a52dc92344ad51bcfb
workflow-type: tm+mt
source-wordcount: '721'
ht-degree: 1%

---


# ID名前空間の概要

ID名前空間は、IDが関連付けら [!DNL Identity Service](./home.md) れるコンテキストのインジケータとして機能するコンポーネントです。 例えば、「name<span>@email.com」の値は電子メールアドレスとして、または「443522」は数値のCRM IDとして区別されます。

## はじめに

ID名前空間を使用するには、関連する様々なAdobe Experience Platformサービスについて理解しておく必要があります。 名前空間の操作を開始する前に、次のサービスに関するドキュメントを確認してください。

- [!DNL Real-time Customer Profile](../profile/home.md): 複数のソースからの集計データに基づいて、リアルタイムで統合された顧客プロファイルを提供します。
- [!DNL Identity Service](./home.md): デバイスとシステム間でIDをブリッジ化することで、個々の顧客とその行動をより良く表示できます。
- [!DNL Privacy Service](../privacy-service/home.md): ID名前空間は、GDPR(General Data Protection Regulation)に準拠するために使用されます。GDPRは、名前空間に対してGDPR要求を行うことができます。

## ID名前空間について

完全修飾IDには、ID値と名前空間が含まれます。 プロファイルフラグメント間でレコードデータを一致させる場合は、プロファイルデータを [!DNL Real-time Customer Profile] 結合する場合と同様に、ID値と名前空間の両方を一致させる必要があります。

例えば、2つのプロファイルフラグメントに異なるプライマリIDが含まれていても、それらが「電子メール」名前空間と同じ値を共有している場合、Platformは、これらのフラグメントが実際には同じ個人であることを確認し、個々のIDグラフにデータをまとめることができます。

![](images/identity-service-stitching.png)

### IDタイプ

データは、複数の異なるIDタイプで識別できます。 IDタイプは、ID名前空間の作成時に指定され、データがIDグラフに保持されるかどうかと、そのデータの処理方法に関する特別な指示を制御します。

では、次のIDタイプを使用でき [!DNL Platform]ます。

| IDタイプ | 説明 |
| --- | --- |
| Cookie | これらのIDは拡張に不可欠で、IDグラフの大部分を占めます。 しかし、自然によって、彼らは急速に衰え、時間と共に価値を失います。 Cookieの削除は、IDグラフで特別に処理されます。 |
| デバイス間 | これは、これが強力な人々の識別子であると考え [!DNL Identity Service] るべきであり、その識別子を永遠に保持すべきであることを示しています。 ログインID、CRM ID、忠誠度IDなどがあります。 |
| デバイス | IDFA、GAID、その他のIOT IDが含まれます。 家庭の人が共有できます。 |
| 電子メール | このタイプのIDには、個人識別情報(PII)が含まれます。 これは、値を慎重に処理す [!DNL Identity Service] ることを示します。 |
| Mobile | このタイプのIDにはPIIが含まれます。 これは、値を慎重に処理す [!DNL Identity Service] ることを示します。 |
| 非ユーザー | 名前空間を必要とするが、個人クラスターには結び付けられていないIDを格納するために使用します。 次に、これらの識別子は、アイデンティティグラフからフィルタリングされる。 使用例としては、製品、組織、店舗などに関連するデータが挙げられます。 （製品のSKUなど）。 |
| 電話番号 | このタイプのIDにはPIIが含まれます。 これは、値を慎重に処理す [!DNL Identity Service] ることを示しています。 |

### 標準名前空間

Adobe Experience Platformには、すべての組織で使用できるID名前空間がいくつか用意されています。 これらは標準名前空間と呼ばれ、 [!DNL Identity Service] APIを使用して、または [!DNL Platform] UIを通して表示されます。

UIで標準名前空間を表示するには、左側のナビゲーションバーの「 **[!UICONTROL ID]** 」をクリックし、「 *[!UICONTROL 参照]* 」タブをクリックします。 組織がアクセスできるすべてのID名前空間が表示されますが、「[!UICONTROL Standard]」を「[!UICONTROL Owner]」として持つID名前空間は、アドビが提供するStandardです。

その後、表示の詳細に表示された名前空間の1つをクリックします。

![](./images/standard-namespace-detail.png)

## 組織の名前空間の管理

組織のデータや使用例に応じて、カスタム名前空間が必要になる場合があります。

これらは、「[!UICONTROL Custom]」を「[!UICONTROL Owner]」として持つ名前空間としてUIに表示されます。 カスタム名前空間は、 [!DNL Identity Service] APIを使用して、またはユーザーインターフェイスを通じて作成できます。

UIを使用してカスタム名前空間を作成するには、「ID名前空間を **[!UICONTROL 作成」をクリックし]**、ダイアログボックスに情報を入力して「 **[!UICONTROL 作成]**」をクリックします。

定義した名前空間は組織に対して非公開で、正常に作成するために一意の「[!UICONTROL IDシンボル]」（APIを使用している場合は「コード」）が必要です。

![](./images/create-identity-namespace.png)

標準名前空間と同様に、「 *[!UICONTROL 参照]* 」タブでカスタム名前空間をクリックして詳細を表示できますが、カスタム名前空間では、詳細領域から表示名と説明を編集することもできます。

>[!NOTE] 名前空間を作成すると、そのシンボルを削除できなくなり、「IDシンボル」（APIでは「コード」）と「タイプ」は変更できなくなります。

## IDデータの名前空間

IDの名前空間の入力は、IDデータの提供に使用する方法に応じて異なります。 データIDデータの提供について詳しくは、 [概要のIDデータの](./home.md#supplying-identity-data-to-identity-service)[!DNL Identity Service] 供給に関する節を参照してください。
