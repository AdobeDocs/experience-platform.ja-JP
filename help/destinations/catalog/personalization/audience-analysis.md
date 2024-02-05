---
title: Audience Analysis の宛先
description: 顧客がCustomer Journey Analyticsで認定したオーディエンスを表示する。
badgeLimitedAvailability: label="限定提供（LA）" type="Informative"
source-git-commit: 83b3d40e17f444555769020526bb723265a09eb9
workflow-type: tm+mt
source-wordcount: '801'
ht-degree: 37%

---

# Audience Analysis の宛先

The [!UICONTROL Audience Analysis] の宛先を使用すると、Adobe Experience Platformオーディエンスデータを [Customer Journey Analytics](https://experienceleague.adobe.com/docs/analytics-platform/using/cja-overview/cja-overview.html?lang=ja). エンリッチメントされた結果のデータに含めるオーディエンスを選択できます。 オーディエンス資格は、 [Analysis Workspace](https://experienceleague.adobe.com/docs/analytics-platform/using/cja-workspace/home.html) レポート。

>[!AVAILABILITY]
>
>この宛先は、限られたテスト段階にあります。 この宛先の使用に関心がある場合は、Adobeアカウントチームにお問い合わせください。

## 前提条件

この宛先を使用する前に、次の操作が必要です。

* Audience Analysis の宛先を使用するには、プロビジョニングがおこなわれている必要があります。 この宛先を使用するようにまだプロビジョニングされていない場合は、Adobeアカウントチームにお問い合わせください。
* Customer Journey Analyticsを使用するには、プロビジョニングが必要です。
* Adobe Experience Platformで 1 つ以上のオーディエンスを作成する必要があります。

## サポートされている ID

Audience Analysis では、以下の表で説明する ID のアクティブ化がサポートされています。 ID の詳細は[こちら](/help/identity-service/features/namespaces.md)から。通常、Experience CloudID(ECID) が使用されます。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| GAID | Google Advertising ID | ソース ID が GAID 名前空間の場合は、GAID ターゲット ID を選択します。 |
| IDFA | Apple の広告主 ID | ソース ID が IDFA 名前空間の場合は、IDFA ターゲット ID を選択します。 |
| ECID | Experience Cloud ID | ECID を表す名前空間。 この名前空間は、「Adobe Marketing Cloud ID」、「Adobe Experience Cloud ID」、「Adobe Experience Platform ID」という別名で呼ばれることもあります。次のドキュメントを参照してください： [ECID](/help/identity-service/features/ecid.md) を参照してください。 |
| phone_sha256 | SHA256 アルゴリズムでハッシュ化された電話番号 | Adobe Experience Platform では、プレーンテキストと SHA256 でハッシュ化された電話番号の両方がサポートされています。ハッシュ化されていない属性がソースフィールドに含まれている場合は、「**[!UICONTROL 変換を適用]**」オプションをオンにして、アクティブ化時に [!DNL Platform] がデータを自動的にハッシュ化するように設定します。 |
| email_lc_sha256 | SHA256 アルゴリズムでハッシュ化されたメールアドレス | Adobe Experience Platform では、プレーンテキストと SHA256 でハッシュ化されたメールアドレスの両方がサポートされています。ハッシュ化されていない属性がソースフィールドに含まれている場合は、「**[!UICONTROL 変換を適用]**」オプションをオンにして、アクティブ化時に [!DNL Platform] がデータを自動的にハッシュ化するように設定します。 |
| extern_id | カスタムユーザー ID | ソース ID がカスタム名前空間の場合は、このターゲット ID を選択します。 |

{style="table-layout:auto"}

## サポートされるオーディエンス

この宛先を使用する場合、次のタイプのオーディエンスがサポートされます。

| オーディエンスの起源 | サポートあり | 説明 |
---------|----------|----------|
| [!DNL Segmentation Service] | ✓ | Experience Platform [セグメント化サービス](../../../segmentation/home.md). |
| カスタムアップロード | ✓ | CSV ファイルから Experience Platform に[読み込まれた](../../../segmentation/ui/overview.md#import-audience)オーディエンス。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL オーディエンスの書き出し]** | Audience Analysis の宛先で使用されている識別子（名前、電話番号など）を使用して、オーディエンスのすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンスの評価に基づいてExperience Platformでプロファイルが更新されると、コネクタは更新を宛先プラットフォームに送信します。 [ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)の詳細についてはこちらを参照してください。 |

{style="table-layout:auto"}

## 新しい宛先の設定

>[!IMPORTANT]
> 
>宛先を作成するには、 **[!UICONTROL 宛先の表示]** および **[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions). [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

この宛先を作成するには、 [宛先設定のチュートリアル](../../ui/connect-destination.md).

### 宛先の詳細

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

* **[!UICONTROL 名前]**：宛先名。
* **[!UICONTROL 説明]**：宛先の説明。
* **[!UICONTROL Datastream ID]**：資格を満たすオーディエンスでエンリッチメントするデータストリーム ID。 この ID は、 [データストリームマネージャー](/help/datastreams/overview.md).
* **[!UICONTROL 統合エイリアス]**：統合のエイリアス。

### アラート

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

* **[!UICONTROL アクティベーションスキップ率超過]**：アクティベーションのスキップ率がしきい値を超えた場合に通知を受け取ります。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

### ガバナンスポリシーと実施アクション

この節では、データガバナンスポリシーを定義し、オーディエンスが送信されアクティブな場合に、使用するデータが準拠していることを確認できます。

宛先の目的のマーケティングアクションの選択が完了したら、「 」を選択します。 **[!UICONTROL 作成]**.

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>データをアクティブ化するには、 **[!UICONTROL 宛先の表示]**, **[!UICONTROL 宛先のアクティブ化]**, **[!UICONTROL プロファイルの表示]**、および **[!UICONTROL セグメントを表示]** [アクセス制御権限](/help/access-control/home.md#permissions). [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

宛先を作成したら、その宛先の目的のオーディエンスをアクティブ化できます。

1. 作成した宛先にまだ存在しない場合は、に移動して、再度見つけることができます。 **[!UICONTROL 宛先]** > **[!UICONTROL 参照]**.
1. 選択 **[!UICONTROL オーディエンスをアクティブ化]**.
1. 資格を分析するオーディエンスを選択します。 終了したら、「**[!UICONTROL 次へ]**」を選択します。
1. 宛先の設定とオーディエンスの設定を確認し、 **[!UICONTROL 完了]**.

次のページに戻ることで、今後分析するオーディエンスをさらに追加できます **[!UICONTROL オーディエンスをアクティブ化]** ページに貼り付けます。 オーディエンスがアクティブ化された後は削除できません。
