---
keywords: Experience Platform；ホーム；人気のあるトピック；セグメント化；セグメント化；セグメントの一致；セグメントの一致
solution: Experience Platform
title: セグメントマッチの概要
topic: 概要
description: セグメントマッチは、2人以上のPlatformユーザーが、安全で管理され、プライバシーに優れた方法でセグメントデータを交換できる、Adobe Experience Platformのセグメント共有サービスです。
source-git-commit: 481160f83e82c80ea5b73e9b56003dc625a34b5e
workflow-type: tm+mt
source-wordcount: '1901'
ht-degree: 5%

---


# （アルファ） [!DNL Segment Match]の概要

>[!IMPORTANT]
>
>[!DNL Segment Match] は現在アルファです。ドキュメントと機能は変更される場合があります。

Adobe Experience Platform Segment Matchは、2人以上のPlatformユーザーが、安全で管理されたプライバシーに優しい方法でセグメントデータを交換できるセグメント共有サービスです。 [!DNL Segment Match] は、ハッシュ化された電子メール、ハッシュ化された電話番号、IDFAやGAIDなどのデバイス識別子など、Platformのプライバシー標準や個人識別子を使用します。

[!DNL Segment Match] では、以下のことが可能です。

* ID重複プロセスを管理します。
* 共有前の見積もりを表示します。
* データ使用ラベルを適用して、データをパートナーと共有できるかどうかを制御します。
* フィードを公開した後も共有オーディエンスのライフサイクル管理を維持し、追加、削除、共有解除の機能を通じて、データの動的な交換を続行します。

[!DNL Segment Match] では、idの重複プロセスを使用して、セグメントの共有がプライバシーに焦点を当てた安全な方法でおこなわれるようにします。**重複するID**&#x200B;は、セグメントと選択したパートナーのセグメントの両方で一致するIDです。 送信者と受信者の間でセグメントを共有する前に、ID重複プロセスは名前空間の重複を確認し、送信者と受信者の間で同意を確認します。 セグメントを共有するには、両方の重複チェックが合格する必要があります。

以下の節では、セットアップとエンドツーエンドのワークフローの詳細など、[!DNL Segment Match]に関する詳細を説明します。

## セットアップ

次の節では、[!DNL Segment Match]の設定方法と設定方法について説明します。

### IDデータと名前空間の設定 {#namespaces}

[!DNL Segment Match]を使い始める最初の手順は、サポートされているID名前空間に対してデータを取り込むことです。

ID名前空間は、[Adobe Experience Platform IDサービス](../../identity-service/home.md)のコンポーネントです。 各顧客IDには、IDのコンテキストを示す関連名前空間が含まれます。 例えば、名前空間では、「name<span>@email.com」の値を電子メールアドレスとして、または「443522」を数値CRM IDとして区別できます。

完全修飾 ID には、ID 値と名前空間が含まれます。プロファイルフラグメント間でレコードデータを一致させる場合（[!DNL Real-time Customer Profile]がプロファイルデータを結合する場合など）は、ID値と名前空間の両方が一致する必要があります。

[!DNL Segment Match]のコンテキストでは、名前空間はデータを共有する際の重複プロセスで使用されます。

サポートされる名前空間のリストを次に示します。

| 名前空間 | 説明 |
| --------- | ----------- |
| 電子メール（SHA256、小文字） | 事前にハッシュ化された電子メールアドレス用の名前空間。 この名前空間で指定された値は、SHA256でハッシュする前に小文字に変換されます。 Eメールアドレスを正規化する前に、先頭と末尾の空白文字をトリミングする必要があります。 この設定を遡って変更することはできません。 詳しくは、[SHA256ハッシュサポート](https://experienceleague.adobe.com/docs/id-service/using/reference/hashing-support.html?lang=en#hashing-support)に関する次のドキュメントを参照してください。 |
| 電話(SHA256_E.164) | SHA256形式とE.164形式の両方を使用してハッシュ化する必要がある生の電話番号を表す名前空間。 |
| ECID | Experience CloudID(ECID)値を表す名前空間。 この名前空間は、次のエイリアスでも参照できます。&quot;Adobe Marketing Cloud ID&quot;、&quot;Adobe Experience Cloud ID&quot;、&quot;Adobe Experience Platform ID&quot;。 詳しくは、[ECIDの概要](../../identity-service/ecid.md)を参照してください。 |
| Apple IDFA（広告主のID） | Apple IDを表す名前空間。 詳しくは、[関心に基づく広告](https://support.apple.com/ja-jp/HT202074)に関する以下のドキュメントを参照してください。 |
| Google 広告 ID | Google広告IDを表す名前空間。 詳しくは、[Google広告ID](https://support.google.com/googleplay/android-developer/answer/6048248?hl=en)の次のドキュメントを参照してください。 |

### 同意設定の設定

同意の設定を指定し、同意の確認のデフォルト値を`opt-in`または`opt-out`に設定する必要があります。

オプトインおよびオプトアウトの同意チェックは、デフォルトでユーザーデータを共有することに同意して操作できるかどうかを決定します。 同意設定のデフォルトが`opt-in`に設定されている場合は、ユーザーが明示的にオプトアウトしない限り、ユーザーデータを共有できます。 デフォルトが`opt-out`に設定されている場合、ユーザーが明示的にオプトインしない限り、ユーザーデータを共有できません。

[!DNL Segment Match]のデフォルトの同意設定は`opt-out`に設定されます。 データに対してオプトインモデルを強制するには、担当のAdobeアカウントマネージャーに電子メールでリクエストを送信してください。

データ共有の同意値の設定に使用される`share`属性について詳しくは、[privacy and consents field group](../../xdm/field-groups/profile/consents.md)に関する次のドキュメントを参照してください。 プライバシー、パーソナライゼーション、マーケティングの環境設定に関するデータの収集と使用に関する消費者の同意を取り込むために使用する特定のフィールドグループについて詳しくは、次の[GitHubの例](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/consent-preferences.schema.md)を参照してください。

### データ使用ラベルの設定

最後に確立する必要がある前提条件は、データの共有を防ぐために新しいデータ使用ラベルを設定することです。 データ使用ラベルを使用して、[!DNL Segment Match]を通じて共有できるデータを管理できます。

データ使用状況ラベルを使用すると、データに適用される使用ポリシーに従ってデータセットとフィールドを分類できます。ラベルはいつでも付けることができ、それによってデータの管理方法を柔軟に選択できます。ベストプラクティスとしては、データが Experience Platform に取り込まれたときや Experience Platform で使用可能になったときに、すぐにデータにラベルを付けることをお勧めします。

[!DNL Segment Match] はC11ラベルを使用します。C11ラベルはに固有の契約ラベルで [!DNL Segment Match] す。データセットや属性を手動で追加すると、パートナー共有プロセスから除外さ [!DNL Segment Match] れます。C11ラベルは、[!DNL Segment Match]プロセスで使用すべきでないデータを示します。 [!DNL Segment Match]から除外するデータセットやフィールドを決定し、それに応じてC11ラベルを追加したら、[!DNL Segment Match]ワークフローによってラベルが自動的に適用されます。 [!DNL Segment Match] は、「データの共有を制 [!UICONTROL 限」コアポリ] シーを自動的に有効にします。データ使用ラベルをデータセットに適用する方法について詳しくは、UIでのデータ使用ラベルの管理に関するチュートリアル([)を参照してください。](../../data-governance/labels/user-guide.md)

データ使用ラベルとその定義のリストについては、[データ使用ラベルの用語集](../../data-governance/labels/reference.md)を参照してください。 データ使用ポリシーについて詳しくは、「[データ使用ポリシーの概要](../../data-governance/policies/overview.md)」を参照してください。

## [!DNL Segment Match] エンドツーエンドのワークフロー

IDデータと名前空間、同意設定、データ使用ラベルを設定したら、[!DNL Segment Match]とその機能の使用を開始できます。

### パートナーの管理

Platform UIで、左側のナビゲーションから「**[!UICONTROL セグメント]**」を選択し、上部のヘッダーから「**[!UICONTROL フィード]**」を選択します。

![segments-feed.png](../images/ui/segment-match/segments-feed.png)

[!UICONTROL フィード]ページには、パートナーから受信したフィードのリストと、共有しているフィードが含まれます。 既存のパートナーのリストを表示したり、新しいパートナーとの接続を確立したりするには、**[!UICONTROL 「パートナーを管理]**」を選択します。

![manage-partners.png](../images/ui/segment-match/manage-partners.png)

2つのパートナー間の接続は、ユーザーがサンドボックスレベルでPlatform組織を接続するためのセルフサービスメソッドとして機能する「双方向ハンドシェイク」です。 接続は、契約が確立され、Platformがお客様とパートナー間でサービスを共有しやすくすることをPlatformに通知するために必要です。

>[!NOTE]
>
>あなたとパートナーの間の「双方向ハンドシェイク」は、厳密に結びつきです。 このプロセス中はデータは交換されません。

[!UICONTROL パートナーの管理]画面のメインインターフェイスで、既存のパートナーとの接続のリストを表示できます。 右側のレールには[!UICONTROL 共有設定]パネルがあり、新しい[!UICONTROL 接続ID]を生成するオプションと、パートナーの[!UICONTROL 接続ID]を入力できる入力ボックスを提供します。

![establish-connection.png](../images/ui/segment-match/establish-connection.png)

新しい[!UICONTROL 接続ID]を作成するには、[!UICONTROL 共有設定]の下の「**[!UICONTROL 再生成]**」を選択し、新しく生成されたIDの横にあるコピーアイコンを選択します。

![share-setting.png](../images/ui/segment-match/share-setting.png)

[!UICONTROL 接続ID]を使用してパートナーに接続するには、「[!UICONTROL 接続パートナー]」の下にある入力ボックスに一意のID値を入力し、「**[!UICONTROL リクエスト]**」を選択します。

![connect-partner.png](../images/ui/segment-match/connect-partner.png)

### フィードの作成

**フィード**&#x200B;は、データ（セグメント）のグループ、そのデータの公開方法や使用方法に関するルール、およびデータとパートナーのデータとの照合方法を決定する設定です。 フィードは、[!DNL Segment Match]を介して、別々に管理し、他のPlatformユーザーと交換できます。

新しいフィードを作成するには、[!UICONTROL フィード]ダッシュボードから「**[!UICONTROL フィード]**&#x200B;を作成」を選択します。

![create-feed.png](../images/ui/segment-match/create-feed.png)

フィードの基本的なセットアップには、名前、説明、マーケティングの使用例やID設定に関する設定が含まれます。 フィードの名前と説明を入力し、データの除外元となるマーケティングの使用例を適用します。 以下のようなリストから複数のユースケースを選択できます。

* [!UICONTROL Analytics]
* [!UICONTROL PII と組み合わせる]
* [!UICONTROL クロスサイトターゲティング]
* [!UICONTROL データサイエンス]
* [!UICONTROL Eメールのターゲティング]
* [!UICONTROL サードパーティへの書き出し]
* [!UICONTROL オンサイト広告]
* [!UICONTROL オンサイトのパーソナライゼーション]
* [!UICONTROL セグメントの一致]
* [!UICONTROL 単一のIDパーソナライゼーション]

最後に、フィードに適したID名前空間を選択します。 [!DNL Segment Match]でサポートされている特定の名前空間について詳しくは、[IDデータと名前空間の表](#namespaces)を参照してください。 終了したら、「**[!UICONTROL 次へ]**」を選択します。

![audience-sharing.png](../images/ui/segment-match/audience-sharing.png)

フィードの設定を確立したら、共有するセグメントをファーストパーティセグメントのリストから選択します。 リストから1つ以上のセグメントを選択でき、右側のパネルを使用して、選択したセグメントのリストを管理できます。 完了したら、「**[!UICONTROL 次へ]**」を選択します。

![select-segments.png](../images/ui/segment-match/select-segments.png)

[!UICONTROL 共有]ページが表示され、フィードを共有するパートナーを選択するためのインターフェイスが表示されます。 この手順では、共有前の重複の予測レポートを表示し、パートナーとの間の名前空間別に、重複するIDの数、データの共有に同意する重複するIDの数を確認することもできます。

**[!UICONTROL セグメント別に分析]**&#x200B;を選択して、予測レポートを表示します。

![analyze.png](../images/ui/segment-match/analyze.png)

重複の予測レポートを使用すると、フィードを共有する前に、パートナーごとおよびセグメントごとに重複および同意のチェックを管理できます。

| 指標 | 説明 |
| ------- | ----------- |
| 同意を伴う推定ID | 組織に設定された同意要件を満たす重複するIDの合計数です。 |
| 重複する推定ID | 選択したセグメントに適合し、選択したパートナーと一致するIDの数。 これらのIDは名前空間別に表示され、個々のプロファイルIDとは異なります。 重複の予測は、プロファイルのスケッチに基づいています。 |

終了したら、「**[!UICONTROL 閉じる]**」を選択します。

![overlap-report.png](../images/ui/segment-match/overlap-report.png)

パートナーを選択し、重複予測レポートを表示したら、「**[!UICONTROL 次へ]**」を選択して次に進みます。

![share.png](../images/ui/segment-match/share.png)

[!UICONTROL 確認]手順が表示され、新しいフィードを共有して公開する前に確認できます。 この手順には、適用したID設定の詳細に加えて、選択したマーケティングの使用例、セグメント、パートナーに関する情報が含まれます。

「**[!UICONTROL 完了]**」を選択して次に進みます。

![review.png](../images/ui/segment-match/review.png)

### フィードの更新

セグメントを追加または削除するには、**[!UICONTROL フィード]ページから「[!UICONTROL フィード]**&#x200B;を作成&#x200B;**[!UICONTROL 既存のフィード]**」を選択します。 表示される既存のフィードのリストで、更新するフィードを選択し、「**[!UICONTROL 次へ]**」を選択します。

![feed-list](../images/ui/segment-match/feed-list.png)

セグメントのリストが表示されます。 ここから、新しいセグメントをフィードに追加し、右側のパネルを使用して不要になったセグメントを削除できます。 フィードのセグメントの管理が完了したら、「**[!UICONTROL 次へ]**」を選択し、上記の手順に従って更新されたフィードを完了します。

![更新](../images/ui/segment-match/update.png)

>[!NOTE]
>
>共有フィードにセグメントを追加または削除する場合、受信パートナーは、受信したフィードのリストの[!DNL Profile]切り替えを再度有効にして、変更を確認する必要があります。

### 受信フィードの受け入れ

受信フィードを表示するには、[!UICONTROL フィード]ページのヘッダーから「**[!UICONTROL 受信済み]**」を選択し、リストから表示するフィードを選択します。 フィードを受け入れるには、「**[!UICONTROL プロファイルで有効]**」を選択し、ステータスが[!UICONTROL 保留中]から[!UICONTROL 有効]に更新されるまでしばらく待ちます。

![received.png](../images/ui/segment-match/received.png)

共有フィードを承認したら、共有データを使用して新しいセグメントを作成できます。

## 次の手順

このドキュメントでは、[!DNL Segment Match]とその機能、エンドツーエンドのワークフローについて理解しています。 その他のPlatformサービスについて詳しくは、次のドキュメントを参照してください。

* [[!DNL Segmentation Service]](../home.md)
* [[!DNL Identity Service]](../../identity-service/home.md)
* [[!DNL Real-time Customer Profile] の概要](../../profile/home.md)
