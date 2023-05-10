---
keywords: Experience Platform；ホーム；人気の高いトピック；ID サービス；ID サービス；共有デバイス；共有デバイス
title: 共有デバイスの概要（ベータ版）
description: 共有デバイス検出は、同じデバイスの異なる認証済みユーザーを識別し、顧客データを ID グラフでより正確に表示できるようにします
hide: true
hidefromtoc: true
exl-id: 36318163-ba07-4209-b1be-dc193ab7ba41
source-git-commit: 34e0381d40f884cd92157d08385d889b1739845f
workflow-type: tm+mt
source-wordcount: '1360'
ht-degree: 9%

---

# 共有デバイス検出の概要（ベータ版）

>[!IMPORTANT]
>
>この [!DNL Shared Device Detection] 機能はベータ版です。 その機能とドキュメントは変更される可能性があります。

Adobe Experience Platform [!DNL Identity Service]を使用すると、デバイスやシステム間で ID を橋渡しすることで、顧客とその行動をわかりやすく表示できます。これによって、インパクトのある個人的なデジタル体験をリアルタイムで提供できます。

[!DNL Shared Device] は、複数の個人が使用するデバイスを指します。 共有デバイスの例としては、タブレット、ライブラリコンピューター、キオスクなどがあります。 を通じて [!DNL Shared Device Detection] 機能を使用すると、同じデバイスの異なるユーザーが単一の ID に結合されるのを防ぐことができ、個人をより正確に表現できます。

[!DNL Shared Device Detection] を使用すると、次のことが可能です。

* 同じデバイスの異なるユーザーに対して別々の ID グラフを作成する
* 同じデバイスを使用して異なる個人からのデータが混在しないようにする。
* より明確で正確な顧客のビューを生成します。

>[!TIP]
>
>の設定 [!DNL Shared Device Detection] は、データセットのプロファイルを有効にする前に設定を完了する必要があります。これは、でグラフを生成すると、設定を変更できなくなるからです。 [!DNL Identity Service].

## [!DNL Shared Device Detection] 入門

の操作 [!DNL Shared Device Detection] には、関連する様々な Platform サービスに関する理解が必要です。 操作を開始する前に [!DNL Shared Device Detection]次のサービスのドキュメントを確認してください。

* [[!DNL Identity Service]](../home.md):デバイスやシステム間で ID を結び付けることで、個々の顧客とその行動をより良く把握できます。
   * [ID グラフビューア](./identity-graph-viewer.md):ID グラフビューアを視覚化および操作して、顧客 ID がどのように結び付けられているか、およびどのような方法で結び付けられているかをより深く理解します。
   * [ID 名前空間](../namespaces.md):完全修飾 ID のコンポーネント、および ID 名前空間で ID のコンテキストとタイプを区別する方法を確認します。

## [!DNL Shared Device Detection]について 

を使用する際は、次の用語を理解することが重要です。
[!DNL Shared Device Detection]. 次の表に、理解に不可欠な用語のリストを示します [!DNL Shared Device Detection].

### 用語

| キーワード | 定義 |
| --- | --- |
| 共有デバイス | 共有デバイスは、複数の個人が使用する任意のデバイスです。 共有デバイスの例としては、タブレット、ライブラリコンピューター、キオスクなどがあります。 |
| [!DNL Shared Device Detection] | [!DNL Shared Device Detection] は、同じデバイスの異なるユーザーのデータを相互に分離できる設定を指します。 |
| 共有 ID 名前空間 | 共有 ID 名前空間は、複数のユーザーが使用できるデバイスを表します。 共有 ID 名前空間は通常 ECID ですが、他のデバイス ID に設定することもできます。 |
| ユーザー ID 名前空間 | ユーザー ID 名前空間は、共有デバイスの認証済み（ログイン済み）ユーザーを表します。 |
| 最後に認証されたユーザー | 最後に認証されたユーザーは、デバイスが複数のアカウントでログオンしている場合に、そのデバイスに最後にログインしたユーザーを表します。 |

{style="table-layout:auto"}

[!DNL Shared Device Detection] は、2 つの名前空間を設定することで機能します。の **共有 ID 名前空間** そして **ユーザー ID 名前空間**.

* 共有 ID 名前空間は、複数のユーザーが使用できるデバイスを表します。 Adobeでは、共有デバイス識別子として ECID を使用することをお勧めします。
* ユーザー ID 名前空間は、ユーザーのログイン ID に対応する ID 名前空間にマッピングされます。これは、ユーザーの CRM ID、電子メールアドレス、ハッシュ化された電子メール、電話番号です。

タブレットのような共有デバイスには、単一の **共有 ID 名前空間**. 一方、共有デバイスの各ユーザーは、それぞれ独自に指定された **ユーザー ID 名前空間** はそれぞれのログイン ID に対応しています。 例えば、Kevin と Nora が e コマース用に共有するタブレットには、次の独自の ECID があります。 `1234`Kevin は独自のユーザー ID 名前空間を持ち、 `kevin@email.com` アカウントと Nora は、自分のユーザー ID 名前空間を自分にマッピングしています `nora@email.com` アカウント

[!DNL Shared Device Detection] は、共有 id 名前空間 ( 例： ECID) を最後に認証されたユーザーのユーザー ID 名前空間（ログイン ID）に置き換えます。

### ID データが ID グラフに送信される方法

次の例を考えて、 [!DNL Shared Device Detection] 動作：

>[!NOTE]
>
>この図では、共有 ID 名前空間が ECID に設定され、ユーザー ID 名前空間が CRM ID に設定されています。

![図](../images/shared-device/diagram.png)

* Kevin と Nora は、e コマースの Web サイトにアクセスするためのタブレットを共有しています。 しかし、彼らはそれぞれ独自のアカウントを持ち、それぞれがオンラインで閲覧し、買い物をするのに使用します。
   * 共有デバイスの場合、タブレットには対応する ECID（タブレットの Web ブラウザー Cookie ID を表す）があります。
* Kevin がタブレットと **ログイン** を e コマースアカウントに追加してヘッドフォンを参照すると、Kevin の CRM ID(**ユーザー ID 名前空間**) がタブレットの ECID(**共有 ID 名前空間**) をクリックします。 タブレットの閲覧データが Kevin の ID グラフに組み込まれました。
   * Kevin の場合 **ログアウト** Nora はタブレットと **ログイン** を自分のアカウントに追加してカメラの購入をおこなうと、CRM ID がタブレットの ECID にリンクされます。 従って、タブレットの閲覧データは Nora の ID グラフに組み込まれるようになりました。
   * Nora の場合 **ログアウトしない** ケビンはタブレットを使っていますが、 **ログインしない**&#x200B;をクリックした場合、Nora は認証済みユーザーのままとなり、CRM ID は引き続きタブレットの ECID にリンクされるので、タブレットの閲覧データは Nora に組み込まれます。
   * Nora の場合 **ログアウトする** ケビンはタブレットを使っていますが、 **ログインしない**&#x200B;の場合、タブレットの閲覧データは、引き続き Nora の ID グラフに組み込まれます。これは、 **最後に認証されたユーザー**&#x200B;の場合、CRM ID はタブレットの ECID にリンクされたままになります。
   * Kevin の場合 **ログイン** この場合も、CRM ID がタブレットの ECID にリンクされます。これは、この ID が最後に認証されたユーザーになり、タブレットの閲覧データが自分の ID グラフに組み込まれるからです。

### 方法 [!DNL Profile Service] プロファイルフラグメントを [!DNL Shared Device Detection] 有効

[!DNL Profile Service] は、プロファイルフラグメントと結合されたプロファイルを書き留めます。 個々の顧客プロファイルは、その顧客に対する単一のビューを形成する複数のプロファイルフラグメントで構成されます。例えば、顧客が複数のチャネルをまたがって自社のブランドとやり取りを行う場合、1 人の顧客に関連する複数のプロファイルフラグメントが複数のデータセットに表示されます。これらのフラグメントが Platform に取り込まれると、それらのフラグメントが結合され、その顧客用に単一のプロファイルが作成されます。

条件 [!DNL Shared Device Detection] が有効になっている場合は、 [!DNL Profile] エクスペリエンスイベントが認証済みか未認証かに基づいて、プロファイルフラグメントのプライマリ ID を定義します

An **認証済みエクスペリエンスイベント** は、デバイスにログインした際にユーザーが完了したアクションです。 認証済みのエクスペリエンスイベントの場合、プライマリ ID は **ユーザー ID 名前空間** （ログイン ID）。 An **未認証エクスペリエンスイベント** は、デバイスにログインしていないユーザーが完了したアクションです。 未認証のエクスペリエンスイベントの場合、プライマリ ID は **共有 ID 名前空間** (ECID) を使用します。

詳しくは、[[!DNL Real-Time Customer Profile] 概要](../../profile/home.md)を参照してください。

## 共有デバイス UI

Platform UI で、「 **[!UICONTROL ID]** 左側のナビゲーションから「 」を選択し、 **[!UICONTROL ID 設定]**.

![identity-dashboard](../images/shared-device/identity-dashboard.png)

この [!UICONTROL 共有デバイスの設定] ページが表示され、データの共有デバイス設定を指定するインターフェイスが提供されます。 共有デバイスの設定は、デフォルトで無効になっています。

有効にすると、共有デバイスの設定により、同じデバイスの異なるユーザーのデータを相互に分離できます。 この設定により、同じデバイスのユーザー ID を組み合わせることなく、ID グラフをより簡潔かつ正確に表現できます。

選択 **[!UICONTROL 有効にする]** 共有デバイスの設定を変更し始めます。

![enable-shared-device](../images/shared-device/enable-shared-device.png)

この [!UICONTROL 共有 ID 名前空間] および [!UICONTROL ユーザー ID 名前空間] 設定オプションが表示され、使用する id 名前空間を変更できます。

![set-namespaces](../images/shared-device/set-namespaces.png)

[!UICONTROL 共有 ID 名前空間] は、複数の異なるユーザーが使用する単一のデバイスを表します。 この名前空間は常にに設定されます **[!UICONTROL ECID]** ( すべての Platform ユーザーが **[!UICONTROL ECID]** を web ブラウザー識別子として使用します。

![shared-identity-namespace](../images/shared-device/shared-identity-namespace.png)

この [!UICONTROL ユーザー ID 名前空間] では、同じデバイスの異なるユーザーを識別し、データが同じ id グラフに組み合わされるのを防ぐことができます。

![user-identity-namespace](../images/shared-device/user-identity-namespace.png)

を選択します。 **[!UICONTROL ユーザー ID 名前空間]** 検索バーを開き、id 名前空間を入力するか、ドロップダウンメニューから id 名前空間を選択します。

>[!TIP]
>
>この [!UICONTROL ユーザー ID 名前空間] は、エンドユーザーのログイン ID に対応する id 名前空間にマッピングされる必要があります。 オプションには、顧客 ID、電子メール、ハッシュ化された電子メールが含まれます。

![電子メール](../images/shared-device/emails.png)

設定が完了したら、 [!UICONTROL 共有デバイス設定]を選択します。 **[!UICONTROL 保存]**.

![保存](../images/shared-device/save.png)

選択を確認するメッセージが表示されるポップアップウィンドウが表示されます。 選択 **[!UICONTROL はい]** をクリックして設定を完了します。

![confirm](../images/shared-device/confirm.png)
