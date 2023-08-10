---
keywords: Experience Platform;ホーム;人気のトピック;セグメント化;Segment Match;segment match
solution: Experience Platform
title: Segment Match の概要
description: Segment Match は、Adobe Experience Platform のセグメント共有サービスであり、安全で管理された、プライバシーに配慮した方法で 2 人以上の Platform ユーザーがセグメントデータを交換できるようにします。
exl-id: 4e6ec2e0-035a-46f4-b171-afb777c14850
source-git-commit: b66a50e40aaac8df312a2c9a977fb8d4f1fb0c80
workflow-type: tm+mt
source-wordcount: '1996'
ht-degree: 98%

---

# [!DNL Segment Match] の概要

Adobe Experience Platform Segment Match は、安全で管理された、プライバシーに配慮した方法で 2 人以上の Platform ユーザーがセグメントデータを交換できるようにするセグメント共有サービスです。[!DNL Segment Match] では、Platform のプライバシー基準と個人識別子（ハッシュ化されたメールアドレス、ハッシュ化された電話番号、IDFA や GAID などのデバイス識別子など）を使用します。

[!DNL Segment Match] を使用すると、次のことが可能です。

* ID 重複プロセスを管理します。
* 共有前の推定を表示します。
* データ使用ラベルを適用して、データをパートナーと共有できるかどうかを制御します。
* フィードを公開した後も共有オーディエンスのライフサイクル管理を維持し、追加、削除および共有解除の機能を通じてデータの動的交換を継続します。

[!DNL Segment Match] では、ID 重複プロセスを使用して、安全でプライバシーを重視した方法でセグメント共有が確実に行われるようにします。**重複 ID** は、自分のセグメントと選択したパートナーのセグメントの両方で一致する ID です。送信者と受信者の間でセグメントを共有する前に、ID 重複プロセスによって、名前空間の重複チェックと送信者と受信者の間の同意チェックが行われます。セグメントを共有するには、両方の重複チェックに合格する必要があります。

以下の節では、セットアップとそのエンドツーエンドワークフローの詳細など、[!DNL Segment Match] について詳しく説明します。

## セットアップ

以下の節では、[!DNL Segment Match] のセットアップおよび設定方法の概要を説明します。

### ID データと名前空間のセットアップ {#namespaces}

[!DNL Segment Match] の使用を開始するための最初の手順として、サポートされている ID 名前空間に対してデータを確実に取り込みます。

ID 名前空間は、[Adobe Experience Platform ID サービス](../../../identity-service/home.md)のコンポーネントです。各顧客 ID には、ID のコンテキストを示す関連する名前空間が含まれています。例えば、名前空間では、「name<span>@email.com」の値をメールアドレスとして識別でき、「443522」を数値 CRM ID として識別できます。

完全修飾 ID には、ID 値と名前空間が含まれています。プロファイルフラグメント間でレコードデータを照合する場合（[!DNL Real-Time Customer Profile] でプロファイルデータを結合する場合など）は、ID 値と名前空間の両方が一致する必要があります。

[!DNL Segment Match] のコンテキストでは、名前空間は、データを共有する際に重複プロセスで使用されます。

サポートされている名前空間のリストを以下に示します。

| 名前空間 | 説明 |
| --------- | ----------- |
| メール（SHA256、小文字） | 事前にハッシュされたメールアドレスの名前空間。この名前空間で指定された値は、小文字に変換されてから SHA256 でハッシュ化されます。メールアドレスを正規化する前に、先頭と末尾のスペースを削除する必要があります。 この設定を過去にさかのぼって変更することはできません。Platform には、データ収集時のハッシュ化をサポートする方法として、[`setCustomerIDs`](https://experienceleague.adobe.com/docs/id-service/using/reference/hashing-support.html?lang=ja#hashing-support) と[データ準備](../../../data-prep/functions.md#hashing)の 2 つが用意されています。 |
| 電話（SHA256_E.164） | SHA256 形式と E.164 形式の両方を使用してハッシュする必要がある生の電話番号を表す名前空間。 |
| ECID | Experience Cloud ID（ECID）値を表す名前空間。この名前空間は、「Adobe Marketing Cloud ID」、「Adobe Experience Cloud ID」、「Adobe Experience Platform ID」という別名で呼ばれることもあります。詳しくは、[ECID の概要](../../../identity-service/ecid.md)を参照してください。 |
| Apple IDFA（広告主の ID） | 広告主の Apple ID を表す名前空間。詳しくは、[興味／関心に基づく広告](https://support.apple.com/ja-jp/HT202074)に関するドキュメントを参照してください。 |
| Google 広告 ID | Google 広告 ID を表す名前空間。詳しくは、[Google 広告 ID](https://support.google.com/googleplay/android-developer/answer/6048248?hl=ja) に関する次のドキュメントを参照してください。 |

### 同意設定の指定

同意設定を指定し、同意チェックの目的で、デフォルト値を `opt-in` または `opt-out` に設定する必要があります。

オプトインおよびオプトアウトの同意チェックにより、デフォルトでユーザーデータを共有することに同意して操作できるかどうかが決まります。同意設定のデフォルト値が `opt-out` に設定されている場合、ユーザーが明示的にオプトアウトしない限り、ユーザーデータを共有できます。デフォルト値が `opt-in` に設定されている場合、ユーザーが明示的にオプトインしない限り、ユーザーデータは共有できません。

[!DNL Segment Match] のデフォルトの同意設定は `opt-out` に指定されています。データのオプトインモデルを適用するには、アドビアカウントチームにメールでリクエストを送信してください。

詳しくは、 `share` 属性を使用して、データ共有の同意の値を設定します。 [プライバシーと同意フィールドグループ](../../../xdm/field-groups/profile/consents.md). プライバシー、パーソナライゼーション、マーケティングの環境設定に関連するデータの収集と使用に対する消費者の同意をキャプチャするために使用される特定のフィールドグループについては、次の[プライバシー、パーソナライゼーション、マーケティングの環境設定に関する同意の GitHub の例](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/consent/consent-preferences.schema.md)を参照してください。

### データ使用ラベルの設定

確立する必要がある最後の前提条件は、データの共有を防ぐための新しいデータ使用ラベルを設定することです。 データ使用ラベルを使用すると、どのようなデータを [!DNL Segment Match] を通じて共有できるかを管理できます。

データ使用状況ラベルを使用すると、データに適用される使用ポリシーに従ってデータセットとフィールドを分類できます。ラベルはいつでも付けることができ、それによってデータの管理方法を柔軟に選択できます。ベストプラクティスとしては、データが Experience Platform に取り込まれたときや Experience Platform で使用可能になったときに、すぐにデータにラベルを付けることをお勧めします。

[!DNL Segment Match] では C11 ラベル（[!DNL Segment Match] に固有の契約ラベルで、任意のデータセットや属性に手動で追加して、それらが [!DNL Segment Match] パートナー共有プロセスから確実に除外されるようにできるラベル）です。 C11 ラベルは、[!DNL Segment Match] プロセスで使用すべきでないデータを示します。[!DNL Segment Match] から除外するデータセットやフィールドを決定し、それに応じて C11 ラベルを追加したら、そのラベルが [!DNL Segment Match] ワークフローによって自動的に適用されます。[!DNL Segment Match] では、[!UICONTROL データ共有を制限]コアポリシーを自動的に有効にします。 データ使用ラベルをデータセットに適用する方法について詳しくは、[UI でのデータ使用ラベルの管理](../../../data-governance/labels/user-guide.md)に関するチュートリアルを参照してください。

データ使用ラベルとその定義の一覧については、[データ使用ラベルの用語集](../../../data-governance/labels/reference.md)を参照してください。データ使用ポリシーについて詳しくは、 [データ使用ポリシーの概要](../../../data-governance/policies/overview.md)を参照してください。

### [!DNL Segment Match] の権限について

[!DNL Segment Match] には、次の 2 つの権限が関連付けられています。

| 権限 | 説明 |
| --- | --- |
| オーディエンス共有接続の管理 | この権限を使用すると、2 つの組織を接続して [!DNL Segment Match] フローを有効にするパートナーハンドシェイクプロセスを完了できます。 |
| オーディエンス共有の管理 | この権限を使用すると、アクティブなパートナー（**[!UICONTROL オーディエンス共有接続]**&#x200B;にアクセスできる管理者ユーザーによって接続されたパートナー）と協力してフィード（[!DNL Segment Match] に使用されるデータのパッケージ）を作成、編集および公開できます。 |

アクセス制御と権限について詳しくは、[アクセス制御の概要](../../../access-control/home.md)を参照してください。

## [!DNL Segment Match] のエンドツーエンドワークフロー

ID データと名前空間、同意設定およびデータ使用ラベルを設定したら、[!DNL Segment Match] とその機能を使用した作業を開始できます。

### パートナーの管理

Platform UI で、左側のナビゲーションから「**[!UICONTROL セグメント]**」を選択したあと、上部のヘッダーから「**[!UICONTROL フィード]**」を選択します。

![segments-feed.png](./images/segments-feed.png)

この[!UICONTROL フィード]ページには、パートナーから受け取ったフィードのリストのほか、共有したフィードが含まれています。 既存のパートナーの一覧を表示するか、新しいパートナーとの接続を確立するには、「**[!UICONTROL パートナーを管理]**」を選択します。

![manage-partners.png](./images/manage-partners.png)

2 つのパートナー間の接続は、ユーザーがサンドボックスレベルで Platform 組織を接続するためのセルフサービス手法として機能する「双方向ハンドシェイク」です。 この接続は、契約が成立したことと、Platform がユーザーとユーザーのパートナーとのサービス共有を促進できることを Platform に通知するために必要です。

>[!NOTE]
>
>ユーザーとユーザーのパートナーとの「双方向ハンドシェイク」は、厳密には接続です。 このプロセス中にデータが交換されることはありません。

既存のパートナーとの接続のリストは、[!UICONTROL パートナーを管理]画面のメインインターフェイスに表示されます。右側にある[!UICONTROL 共有設定]パネルには、新しい[!UICONTROL 接続 ID] を生成するためのオプションと、パートナーの[!UICONTROL 接続 ID]を入力できる入力ボックスが用意されています。

![establish-connection.png](./images/establish-connection.png)

新しい[!UICONTROL 接続 ID] を作成するには、「[!UICONTROL 共有設定]」の「**[!UICONTROL 再生成]**」を選択したあと、新しく生成された ID の横にあるコピーアイコンを選択します。

![share-setting.png](./images/share-setting.png)

パートナーの[!UICONTROL 接続 ID] を使用してパートナーに接続するには、「[!UICONTROL パートナーを接続]」の入力ボックスに一意の ID 値を入力してから、「**[!UICONTROL リクエスト]**」を選択します。

![connect-partner.png](./images/connect-partner.png)

### フィードの作成 {#create-feed}

>[!CONTEXTUALHELP]
>id="platform_segment_match_marketing"
>title="制限付きマーケティングユースケース"
>abstract="制制限付きマーケティングユースケースは、データガバナンス制約に従って共有セグメントが適切に使用されるように、パートナーにガイダンスを提供するうえで役に立ちます。"
>text="Learn more in documentation"

**フィード**&#x200B;は、データ（セグメント）、そのデータの公開方法や使用方法についてのルール、およびユーザーのデータとパートナーのデータの照合方法を決定する設定をグループ化したものです。フィードは、個別に管理でき、[!DNL Segment Match] を通じて他の Platform ユーザーと交換できます。

新しいフィードを作成するには、[!UICONTROL フィード]ダッシュボードから「**[!UICONTROL フィードを作成]**」を選択します。

![create-feed.png](./images/create-feed.png)

フィードの基本設定には、名前、説明、マーケティングユースケースや ID 設定に関する設定が含まれます。 フィードの名前と説明を入力したあと、データを除外するマーケティングユースケースを適用します。 以下を含むリストから複数のユースケースを選択できます。

* [!UICONTROL Analytics]
* [!UICONTROL PII と組み合わせる]
* [!UICONTROL クロスサイトターゲティング]
* [!UICONTROL データサイエンス]
* [!UICONTROL 電子メールのターゲティング]
* [!UICONTROL サードパーティに書き出し]
* [!UICONTROL オンサイト広告]
* [!UICONTROL オンサイトのパーソナライズ機能]
* [!UICONTROL Segment Match]
* [!UICONTROL 単一 ID のパーソナライゼーション]

最後に、フィードに適した ID 名前空間を選択します。[!DNL Segment Match] でサポートされている具体的な名前空間について詳しくは、[ID データと名前空間の表](#namespaces)を参照してください。完了したら、「**[!UICONTROL 次へ]**」を選択します。

![audience-sharing.png](./images/audience-sharing.png)

フィードの設定を確立したら、共有するセグメントをファーストパーティセグメントのリストから選択します。リストから複数のセグメントを選択できます。また、右側のパネルを使用して、選択したセグメントのリストを管理できます。完了したら、「**[!UICONTROL 次へ]**」を選択します。

![select-segments.png](./images/select-segments.png)

[!UICONTROL 共有]ページが表示され、フィードを共有するパートナーを選択するためのインターフェイスが表示されます。この手順の間に、共有前の重複予測レポートを確認したり、自社とパートナーの間で重複している ID の数を名前空間ごとに確認したり、データの共有に同意していて重複している ID の数を確認したりできます。

**[!UICONTROL セグメント別に分析]**&#x200B;を選択して、予測レポートを表示します。

![analyze.png](./images/analyze.png)

重複予測レポートでは、フィードを共有する前に、パートナーごとおよびセグメントごとに重複および同意チェックを管理できます。

| 指標 | 説明 |
| ------- | ----------- |
| 同意を得た ID の予測 | 組織に設定された同意要件を満たす重複した ID の合計数です。 |
| 重複 ID の予測 | 選択したセグメントに適合し、さらに選択したパートナーとの一致が見つかる ID の数です。これらの ID は名前空間別に表示され、個々のプロファイル ID を表すものではありません。重複の予測は、プロファイルのスケッチに基づいています。 |

完了したら、「**[!UICONTROL 閉じる]**」を選択します。

![overlap-report.png](./images/overlap-report.png)

パートナーを選択し、重複予測レポートを確認したら、「**[!UICONTROL 次へ]**」を選択して続行します。

![share.png](./images/share.png)

[!UICONTROL レビュー]手順が表示され、新しいフィードを共有および公開する前に確認できます。この手順には、適用した ID 設定の詳細や、選択したマーケティングのユースケース、セグメント、パートナーに関する情報が含まれます。

「**[!UICONTROL 終了]**」を選択して次に進みます。

![review.png](./images/review.png)

### フィードを更新

セグメントを追加または削除するには、[!UICONTROL フィード]ページで「**[!UICONTROL フィードを作成]**」を選択し、次に「**[!UICONTROL 既存のフィード]**」を選択します。表示される既存のフィードのリストで、更新するフィードを選択したら、「**[!UICONTROL 次へ]**」を選択します。

![feed-list](./images/feed-list.png)

セグメントのリストが表示されます。 ここから、新しいセグメントをフィードに追加できます。また、右側のパネルを使用して、不要になったセグメントを削除できます。フィードのセグメントの管理が完了したら、「**[!UICONTROL 次へ]**」を選択し、それから上記の手順に従って、更新されたフィードを完成させます。

![更新](./images/update.png)

>[!NOTE]
>
>共有フィードにセグメントを追加または削除する場合、受信側のパートナーは、受信したフィードのリストの [!DNL Profile] 切替スイッチを再び有効にして変更を確定する必要があります。

### 受信フィードを受け入れる

受信フィードを表示するには、[!UICONTROL フィード]ページで「**[!UICONTROL 受信済み]**」を選択し、リストから表示するフィードを選択します。 フィードを受け入れるには、「**[!UICONTROL プロファイルに対して有効にする]**」を選択し、ステータスが「[!UICONTROL 保留中]」から「[!UICONTROL 有効]」に更新されるまで少し待ちます。

![received.png](./images/received.png)

共有フィードを承認したら、共有データを使用して新しいセグメントを作成できます。

## 次の手順

このドキュメントを読むことで、[!DNL Segment Match] やその機能、およびエンドツーエンドのワークフローについて理解しました。他の Platform サービスの詳細については、次のドキュメントを参照してください。

* [[!DNL Segmentation Service]](../../home.md)
* [[!DNL Identity Service]](../../../identity-service/home.md)
* [[!DNL Real-Time Customer Profile] の概要](../../../profile/home.md)
