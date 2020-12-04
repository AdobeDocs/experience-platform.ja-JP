---
keywords: Experience Platform;home;popular topics;namespace;Namespace;Namespaces;namespaces;identity namespace;Identity namespace;identity;Identity;Identity service;identity service
solution: Experience Platform
title: Adobe Experience Platform ID サービス
topic: overview
description: 'ID 名前空間は、ID が関連するコンテキストのインジケーターとして機能する ID サービスのコンポーネントです。例えば、「name@email.com」という値は電子メールアドレスとして、または「443522」という数値のCRM IDとして区別されます。 '
translation-type: tm+mt
source-git-commit: dfb16c1808ac61e6c4f4d97c08ac3d1dcc8499a8
workflow-type: tm+mt
source-wordcount: '759'
ht-degree: 68%

---


# ID 名前空間の概要

Identity namespaces are a component of [[!DNL Identity Service]](./home.md) that serve as indicators of the context to which an identity relates. 例えば、「name<span>@email.com」の値を電子メールアドレスとして、または「443522」を数値 CRM ID として区別します。

## はじめに

ID 名前空間を使用するには、関連する様々な Adobe Experience Platform サービスについて理解している必要があります。名前空間の使用を開始する前に、次のサービスのドキュメントを確認してください。

- [[!DNL Real-time Customer Profile]](../profile/home.md):複数のソースからの集計データに基づいて、リアルタイムで統合された顧客プロファイルを提供します。
- [[!DNL Identity Service]](./home.md):デバイスとシステム間でIDをブリッジ化することで、個々の顧客とその行動をより良く表示できます。
- [[!DNL Privacy Service]](../privacy-service/home.md)：ID 名前空間を使用して、EU 一般データ保護規則（GDPR）に準拠します。名前空間には、GDPR リクエストを実行できます。

## ID 名前空間について

完全修飾 ID には、ID 値と名前空間が含まれます。When matching record data across profile fragments, as when [!DNL Real-time Customer Profile] merges profile data, both the identity value and the namespace must match.

例えば、2 つのプロファイルフラグメントには別のプライマリ ID を含めることができますが、それらは「電子メール」名前空間の同じ値を共有します。このため、Platform は、これらのフラグメントが実際には同じ個人であることを確認し、個人の ID グラフにデータをまとめることができます。

![](images/identity-service-stitching.png)

### ID タイプ

データは、複数の異なる ID タイプで識別できます。ID タイプは、ID 名前空間の作成時に指定されます。この ID タイプによって、データを ID グラフに保持するかどうか、およびそのデータの処理方法の手順が決まります。

The following identity types are available within [!DNL Platform]:

| ID タイプ | 説明 |
| --- | --- |
| cookie | この ID は拡張に不可欠で、ID グラフの大部分を占めます。ただし、cookie は、その性質上急速に劣化し、時間の経過と共にその価値が失われます。cookie の削除は、特に ID グラフでおこなわれます。 |
| クロスデバイス | This indicates that [!DNL Identity Service] should consider this to be a strong people identifier and hence preserve it forever. 例えば、ログイン ID、CRM ID、ロイヤルティ ID などがあります。 |
| デバイス | IDFA、GAID、その他の IOT の IDが含まれます。これは、家庭内の人々が共有できます。 |
| 電子メール | このタイプの ID には、個人を特定できる情報（PII）が含まれています。This is an indication to [!DNL Identity Service] to handle the value sensitively. |
| モバイル | このタイプの ID には PII が含まれます。This is an indication to [!DNL Identity Service] to handle the value sensitively. |
| 人以外 | 名前空間を必要とする一方で、個人のクラスターに関連付けられていない識別子を格納するために使用されます。その後、これらの識別子は、ID グラフからフィルタリングされます。使用事例としては、製品、組織、店舗などに関連するデータが考えられます（製品 SKUなど）。 |
| 電話番号 | このタイプの ID には PII が含まれます。This is indication to [!DNL Identity Service] to handle the value sensitively. |

### 標準名前空間 {#standard}

Adobe Experience Platform には、すべての組織で使用できる ID 名前空間が複数用意されています。These are known as Standard namespaces and are visible using the [!DNL Identity Service] API or through the [!DNL Platform] UI.

UI で標準名前空間を表示するには、左側のパネルの「**[!UICONTROL ID]**」をクリックして、「**[!UICONTROL 参照]**」タブをクリックします。All identity namespaces accessible to your organization will be shown, however those with &quot;[!UICONTROL Standard]&quot; as the &quot;[!UICONTROL Owner]&quot; are the Standard namespaces provided by Adobe.

次に、表示された名前空間の 1 つをクリックすると詳細が表示されます。

![](./images/standard-namespace-detail.png)

## 組織の名前空間の管理

組織のデータや使用事例によっては、カスタム名前空間が必要な場合があります。

These are visible in the UI as those namespaces with &quot;[!UICONTROL Custom]&quot; as the &quot;[!UICONTROL Owner]&quot;. Custom namespaces can be created using the [!DNL Identity Service] API or through the user interface.

UI を使用してカスタム名前空間を作成するには、「**[!UICONTROL ID 名前空間を作成]**」をクリックし、ダイアログに情報を入力してから「**[!UICONTROL 作成]**」をクリックします。

Namespaces that you define are private to your organization and require a unique &quot;[!UICONTROL Identity Symbol]&quot; (or &quot;code&quot; if you are using the API) in order to be created successfully.

![](./images/create-identity-namespace.png)

標準名前空間と同様に、「**[!UICONTROL 参照]**」タブのカスタム名前空間をクリックすると、その詳細が表示されます。ただし、カスタム名前空間では、詳細領域で表示名と説明を編集することができます。

>[!NOTE]
>
>作成した名前空間は削除できません。また、その「ID 記号（API の場合は「コード」）」と「タイプ」を変更することはできません。

## ID データの名前空間

ID の名前空間をどのように指定するかは、ID データの提供方法によって異なります。ID データの提供方法について詳しくは、「 overview」の [ID データの提供](./home.md#supplying-identity-data-to-identity-service)に関する節を参照してください。[!DNL Identity Service]
