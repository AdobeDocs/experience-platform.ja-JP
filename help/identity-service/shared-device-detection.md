---
keywords: Experience Platform；ホーム；人気のトピック；ID サービス；ID サービス；共有デバイス；共有デバイス
title: 共有デバイスの概要（Beta）
description: 共有デバイス検出は、同じデバイスの異なる認証済みユーザーを識別し、ID グラフで顧客データをより正確に表示できるようにします
hide: true
hidefromtoc: true
exl-id: 36318163-ba07-4209-b1be-dc193ab7ba41
source-git-commit: 2a2e3fcc4c118925795951a459a2ed93dfd7f7d7
workflow-type: tm+mt
source-wordcount: '1353'
ht-degree: 9%

---

# 共有デバイス検出の概要（Beta）

>[!IMPORTANT]
>
>[!DNL Shared Device Detection] の機能はベータ版です。 その機能とドキュメントは変更される可能性があります。

Adobe Experience Platform [!DNL Identity Service]を使用すると、デバイスやシステム間で ID を橋渡しすることで、顧客とその行動をわかりやすく表示できます。これによって、インパクトのある個人的なデジタル体験をリアルタイムで提供できます。

[!DNL Shared Device] は、複数の個人が使用するデバイスを指します。 共有デバイスの例としては、タブレット、図書館のコンピューター、キオスクなどがあります。 [!DNL Shared Device Detection] の機能により、同じデバイスの異なるユーザーが単一の ID に結合されるのを防ぎ、個人をより正確に表現できます。

[!DNL Shared Device Detection] を使用すると、次のことが可能です。

* 同じデバイスの異なるユーザーに対して個別の ID グラフを作成する。
* 同じデバイスを使用して、異なる個人のデータが混在するのを防ぐ。
* 顧客をより明確かつ正確に把握できるようにします。

>[!TIP]
>
>データセットのプロファイルを有効にする前に、[!DNL Shared Device Detection] の設定を完了する必要があります。これは、[!DNL Identity Service] でグラフを生成すると、設定を変更できなくなるからです。

## [!DNL Shared Device Detection] 入門

[!DNL Shared Device Detection] に取り組むには、関連する様々な Platform サービスを理解している必要があります。 [!DNL Shared Device Detection] の使用を開始する前に、次のサービスのドキュメントを確認してください。

* [[!DNL Identity Service]](./home.md)：デバイスやシステム間で ID を橋渡しすることで、個々の顧客とその行動をより確実に把握することができます。
   * [ID グラフビューア ](./features/identity-graph-viewer.md):ID グラフビューアを視覚化して操作し、顧客 ID の結び付きや方法をより深く理解します。
   * [ID 名前空間 ](./features/namespaces.md)：完全修飾 ID のコンポーネントと、ID 名前空間を使用して ID のコンテキストとタイプを区別する方法について説明します。

## [!DNL Shared Device Detection]について 

を使用する際は、以下の用語を理解することが重要です
[!DNL Shared Device Detection]。 [!DNL Shared Device Detection] を理解するために不可欠な用語のリストについては、以下の表を参照してください。

### 用語

| 用語 | 定義 |
| --- | --- |
| 共有デバイス | 共有デバイスとは、複数の個人が使用する任意のデバイスです。 共有デバイスの例としては、タブレット、図書館のコンピューター、キオスクなどがあります。 |
| [!DNL Shared Device Detection] | [!DNL Shared Device Detection] は、同じデバイスの異なるユーザーのデータを互いに分離できる設定を指します。 |
| 共有 ID 名前空間 | 共有 ID 名前空間は、複数のユーザーが使用できるデバイスを表します。 共有 ID 名前空間は通常 ECID ですが、他のデバイス ID に設定することもできます。 |
| ユーザー ID 名前空間 | ユーザー ID 名前空間は、共有デバイスの認証済み（ログイン済み）ユーザーを表します。 |
| 最後に認証されたユーザー | デバイスが複数のアカウントでログオンしている場合、最後に認証されたユーザーは、デバイスに最後にログインしたユーザーを表します。 |

{style="table-layout:auto"}

[!DNL Shared Device Detection] は、**共有 ID 名前空間** と **ユーザー ID 名前空間** の 2 つの名前空間を確立することで機能します。

* 共有 ID 名前空間は、複数のユーザーが使用できるデバイスを表します。 Adobeでは、共有デバイスの ID として ECID を使用することをお勧めします。
* ユーザー ID 名前空間は、ユーザーのログイン ID に対応する ID 名前空間にマッピングされます。これは、ユーザーの CRMID、メールアドレス、ハッシュ化されたメールまたは電話番号にすることができます。

共有デバイス（タブレットなど）には、単一の **共有 ID 名前空間** があります。 一方、共有デバイスの各ユーザーは、それぞれのログイン ID に対応する独自に指定された **ユーザー ID 名前空間** を持ちます。 例えば、Kevin と Nora が e コマースで共有するタブレットには、`1234` という独自の ECID があります。一方、Kevin は、`kevin@email.com` アカウントにマッピングされた独自の User Id Namespace を持ち、Nora は `nora@email.com` アカウントにマッピングされた独自の User Id Namespace を持っています。

共有 [!DNL Shared Device Detection]ID 名前空間をリンクすることで、同じデバイスの複数のユーザーを区別することができます（例： ECID）を最後の認証ユーザーのユーザー ID 名前空間（ログイン ID）に設定します。

### ID データを ID グラフに送信する方法

[!DNL Shared Device Detection] の仕組みを理解できるように、次の例を考えてみましょう。

>[!NOTE]
>
>この図では、共有 ID 名前空間は ECID に設定され、ユーザー ID 名前空間は CRMID に設定されています。

![図](./images/shared-device/diagram.png)

* Kevin と Nora はタブレットを共有して e コマースの web サイトにアクセスします。 ただし、どちらも独自の独立したアカウントを持っており、それぞれがオンラインでの閲覧や購入に使用します。
   * 共有デバイスとして、タブレットには対応する ECID があります。これは、タブレットの web ブラウザーの cookie ID を表します。
* Kevin がタブレットを使用し、e コマースアカウントに **ログイン** してヘッドフォンを参照すると、Kevin の CRMID （**ユーザー ID 名前空間**）がタブレットの ECID （**共有 ID 名前空間**）にリンクされるようになりました。 タブレットのブラウジングデータが、Kevin の ID グラフに組み込まれるようになりました。
   * Kevin が **ログアウト** し、Nora がタブレットを使用して自分のアカウントに **ログイン** してカメラを購入した場合、彼女の CRMID はタブレットの ECID にリンクされるようになりました。 そのため、Nora の ID グラフにタブレットの閲覧データが組み込まれるようになりました。
   * Nora が **ログアウトせず** Kevin がタブレットを使用し、**ログインしない** 場合、Nora は認証済みユーザーのままであり、CRMID がまだタブレットの ECID にリンクされているので、タブレットの閲覧データは Nora に組み込まれます。
   * Nora が **ログアウトする** と、Kevin がタブレットを使用しているが **ログインしない** 場合、タブレットの閲覧データは Nora の ID グラフに組み込まれます。これは、**最後に認証されたユーザー** として、彼女の CRMID がタブレットの ECID にリンクされたままになるからです。
   * Kevin が再度ログイン **すると、彼の CRMID がタブレットの ECID にリンクされるようになりました。これは、彼が最後に認証されたユーザーになり、タブレットの閲覧データが ID グラフに組み込まれるからです**

### [!DNL Shared Device Detection] が有効 [!DNL Profile Service] プロファイルフラグメントを結合する方法

[!DNL Profile Service] は、プロファイルフラグメントと結合プロファイルをメモします。 個々の顧客プロファイルは、その顧客に対する単一のビューを形成する複数のプロファイルフラグメントで構成されます。例えば、顧客が複数のチャネルをまたがって自社のブランドとやり取りを行う場合、1 人の顧客に関連する複数のプロファイルフラグメントが複数のデータセットに表示されます。これらのフラグメントが Platform に取り込まれると、それらのフラグメントが結合され、その顧客用に単一のプロファイルが作成されます。

[!DNL Shared Device Detection] が有効な場合、[!DNL Profile] は、エクスペリエンスイベントが認証済みか未認証かに基づいて、プロファイルフラグメントのプライマリ ID を定義します

**認証済みエクスペリエンスイベント** は、デバイスにログインしたユーザーが完了したアクションです。 認証済みのエクスペリエンスイベントの場合、プライマリ ID は **ユーザー ID 名前空間** （ログイン ID）です。 **未認証のエクスペリエンスイベント** は、デバイスにログインしていないユーザーが完了したアクションです。 認証されていないエクスペリエンスイベントの場合、プライマリ ID は **共有 ID 名前空間** （ECID）です。

詳しくは、[[!DNL Real-Time Customer Profile]  概要 ](../profile/home.md) を参照してください。

## 共有デバイス UI

Platform UI で、左側のナビゲーションから **[!UICONTROL ID]** を選択したあと、**[!UICONTROL ID 設定]** を選択します。

![identity-dashboard](./images/shared-device/identity-dashboard.png)

[!UICONTROL  共有デバイス設定 ] ページが表示され、データの共有デバイス設定を指定するためのインターフェイスが表示されます。 共有デバイス設定は、デフォルトでは無効になっています。

有効にすると、共有デバイス設定により、同じデバイスの異なるユーザーからのデータを互いに分離できます。 この設定を使用すると、同じデバイスのユーザー ID が組み合わされない ID グラフをより明確かつ正確に表示できます。

「**[!UICONTROL 有効]**」を選択して、共有デバイス設定の変更を開始します。

![enable-shared-device](./images/shared-device/enable-shared-device.png)

[!UICONTROL  共有 ID 名前空間 ] および [!UICONTROL  ユーザー ID 名前空間 ] 設定オプションが表示され、使用する ID 名前空間を変更できます。

![set-namespaces](./images/shared-device/set-namespaces.png)

[!UICONTROL  共有 ID 名前空間 ] は、複数の異なるユーザーが使用する単一のデバイスを表します。 すべての Platform ユーザーが Web ブラウザーの識別子として **[!UICONTROL ECID]** を使用するので、この名前空間は常に **[!UICONTROL ECID]** に設定されます。

![shared-identity-namespace](./images/shared-device/shared-identity-namespace.png)

[!UICONTROL  ユーザー ID 名前空間 ] を使用すると、同じデバイスの異なるユーザーを識別でき、データが同じ ID グラフに組み合わされるのを防ぐことができます。

![user-identity-namespace](./images/shared-device/user-identity-namespace.png)

**[!UICONTROL ユーザー ID 名前空間]** 検索バーを選択し、ID 名前空間を入力するか、ドロップダウンメニューから ID 名前空間を選択します。

>[!TIP]
>
>[!UICONTROL  ユーザー ID 名前空間 ] は、エンドユーザーのログイン ID に対応する ID 名前空間にマッピングする必要があります。 オプションには、顧客 ID、メール、ハッシュ化されたメールがあります。

![ メール ](./images/shared-device/emails.png)

[!UICONTROL  共有デバイス設定 ] を設定したら、「**[!UICONTROL 保存]**」を選択します。

![ 保存 ](./images/shared-device/save.png)

選択を確認するように求めるポップアップウィンドウが表示されます。 **[!UICONTROL はい]** を選択して、設定を完了します。

![ 確認 ](./images/shared-device/confirm.png)
