---
title: Id グラフリンクルール
description: Id サービスでの ID グラフリンクルールについて説明します。
exl-id: 317df52a-d3ae-4c21-bcac-802dceed4e53
source-git-commit: 0aefcfbbbed675a08d9e3023b9f667ec59874e46
workflow-type: tm+mt
source-wordcount: '1547'
ht-degree: 5%

---

# [!DNL Identity Graph Linking Rules] の概要 {#identity-graph-linking-rules-overview}

>[!CONTEXTUALHELP]
>id="platform_identities_linkingrules_overview"
>title="ID グラフのリンクルール"
>abstract="このような不要な結合を防ぐために、ID グラフリンクルールで提供されている設定を使用して、ユーザーに対して正確なパーソナライゼーションを可能にします。"

>[!IMPORTANT]
>
>ID 設定を有効にした後、折りたたまれたグラフを折りたたみ解除（「固定」）する必要がある既存のサンドボックスがある場合は、Adobe アカウントチームにお問い合わせください。

Adobe Experience Platform ID サービスとリアルタイム顧客プロファイルを使用すると、データが完全に取り込まれ、すべての結合プロファイルが CRMID などの人物識別子を使用して 1 人の個人を表すと簡単に想定できます。 ただし、特定のデータが複数の異なるプロファイルを 1 つのプロファイルに結合しようとする可能性があるシナリオがあります（「グラフ折りたたみ」）。 これらの不要な結合を防ぐために、[!DNL Identity Graph Linking Rules] を通じて提供される設定を使用して、ユーザーに対して正確なパーソナライゼーションを可能にできます。

## 基本を学ぶ

[!DNL Identity Graph Linking Rules] の理解には、次のドキュメントが不可欠です。

* [ID 最適化アルゴリズム](./identity-optimization-algorithm.md)
* [実装ガイド](./implementation-guide.md)
* [グラフ設定の例](./example-configurations.md)
* [トラブルシューティングと FAQ](./troubleshooting.md)
* [名前空間の優先度](./namespace-priority.md)
* [グラフシミュレーション UI](./graph-simulation.md)
* [ID 設定 UI](./identity-settings-ui.md)

## ビデオライブラリ

次のビデオでは、ID グラフリンクルールの基本的な側面の一部について説明しています。

<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Identity Graph Linking Rules: Overview">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://video.tv.adobe.com/v/3448273/?learn=on&enablevpops&captions=jpn" title="Id グラフリンクルール：概要" target="_blank" rel="referrer">
                        <img class="is-bordered-r-small" src="https://video.tv.adobe.com/v/3429845/?format=jpeg&nocache=1732633205780" alt="Id グラフリンクルール：概要"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://video.tv.adobe.com/v/3448273/?learn=on&enablevpops&captions=jpn" target="_blank" rel="referrer" title="Id グラフリンクルール：概要">Id グラフリンクルール：概要 </a>
                    </p>
                    <p class="is-size-6">このビデオでは、ID グラフリンクルールの概要を説明し、この機能を使用してグラフの折りたたみを防ぐ方法を説明します。</p>
                </div>
                <div style="display: flex; flex-direction; row;">
                  <a href="https://video.tv.adobe.com/v/3448273/?learn=on&enablevpops&captions=jpn" target="_blank" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                      <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> ウォッチ </span>
                  </a>
                  <a href="./overview.md" target="_blank" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="margin-top: 1rem; margin-left: 0.5rem;">
                      <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> 読取り </span>
                  </a>
                </div>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Identity Graph Linking Rules: Identity Settings">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://video.tv.adobe.com/v/3458487/?learn=on&enablevpops" title="Id グラフリンクルール：Id 設定" target="_blank" rel="referrer">
                        <img class="is-bordered-r-small" src="https://video.tv.adobe.com/v/3441081/?format=jpeg&nocache=1732633205785&captions=jpn" alt="Id グラフリンクルール：Id 設定"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://video.tv.adobe.com/v/3458487/?learn=on&enablevpops" target="_blank" rel="referrer" title="Id グラフリンクルール：Id 設定">Id グラフリンクルール：Id 設定 </a>
                    </p>
                    <p class="is-size-6">このビデオでは、ID 設定を指定し、Real-Time CDP、Adobe Journey Optimizer、Customer Journey AnalyticsなどのAdobe Experience Platform アプリケーション用に高品質の ID グラフと顧客プロファイルを作成する方法を説明します。</p>
                </div>
                <div style="display: flex; flex-direction: row;">
                  <a href="https://video.tv.adobe.com/v/3458487/?learn=on&enablevpops" target="_blank" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                      <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> ウォッチ </span>
                  </a>
                  <a href="identity-settings-ui.md" target="_blank" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="margin-top: 1rem; margin-left: 0.5rem;">
                      <span class="spectrum-Button-label has-no-wrap has-text-weight-bold"> 読取り </span>
                  </a>
                </div>            
            </div>
        </div>
    </div>
</div>

## グラフ折りたたみシナリオ {#graph-collapse-scenarios}

>[!CONTEXTUALHELP]
>id="platform_identities_graphcollapsescenarios"
>title="グラフ折りたたみシナリオ"
>abstract="グラフが「折りたたむ」ことや、複数のユーザーエンティティを表すことがある理由は複数あります。"

この節では、[!DNL Identity Graph Linking Rules] を設定する際に検討する可能性のあるシナリオの例について概説します。

### 共有デバイス

1 台のデバイスで複数のログインが発生する場合があります。

| 共有デバイス | 説明 |
| --- | --- |
| 家族用コンピューターとタブレット | ご夫婦ともそれぞれの銀行口座にログインします。 |
| 公開キオスク | 空港で搭乗者がロイヤルティ ID を使用してログインし、手荷物をチェックインして搭乗券を印刷した。 |
| コールセンター | カスタマーサポートに連絡して問題を解決するお客様の代わりに、コールセンターの担当者が 1 台のデバイスでログインします。 |

![ 一般的な共有デバイスの図。](../images/identity-settings/shared-devices.png)

このような場合、グラフの観点から見ると、制限なしが有効になっている状態で、1 つの ECID が複数の CRMID にリンクされます。

[!DNL Identity Graph Linking Rules] では、以下のことが可能です。

* ログインに使用する ID を一意の ID として設定します。 例えば、CRMID 名前空間を使用して ID を 1 つだけ格納するようにグラフを制限し、その CRMID を共有デバイスの一意の識別子として定義できます。
   * これにより、CRMID が ECID によって結合されないようにすることができます。

### 無効なメール/電話のシナリオ

登録時に電話番号やメールアドレスに偽りの値を提供するケースもあります。 この場合、制限が有効になっていないと、電話やメールに関連する ID が複数の異なる CRMID にリンクされます。

![ 無効なメールまたは電話のシナリオを表す図。](../images/identity-settings/invalid-email-phone.png)

[!DNL Identity Graph Linking Rules] では、以下のことが可能です。

* CRMID、電話番号、またはメールアドレスを一意の識別子として設定し、1 人のユーザーを、自分のアカウントに関連付けられた 1 つの CRMID、電話番号、またはメールアドレスに制限します。

### ID 値がエラーまたは正しくない

名前空間に関係なく、一意でない誤った ID 値がシステムに取り込まれる場合があります。 以下に例を示します。

* ID 値「user_null」の IDFA 名前空間。
   * IDFA ID 値は 36 文字、英数字 32 文字、ハイフン 4 文字にする必要があります。
* ID 値が「指定なし」の電話番号名前空間。
   * 電話番号にはアルファベットを使用しないでください。

これらの ID により、次のグラフが表示される場合があります。このグラフでは、複数の CRMID が「無効」 ID と結合されます。

![ID 値が間違っている、または正しくない ID データのグラフ例。](../images/identity-settings/bad-data.png)

[!DNL Identity Graph Linking Rules] を使用すると、CRMID を一意の識別子として設定して、このタイプのデータに起因する不要なプロファイルの折りたたみを防ぐことができます。

## [!DNL Identity Graph Linking Rules] {#identity-graph-linking-rules}

[!DNL Identity Graph Linking Rules] を使用すると、次のことが可能です。

* 一意の名前空間を設定することで、各ユーザーに対して単一の ID グラフ/結合プロファイルを作成します。これにより、2 つの異なる人物 ID が 1 つの ID グラフに結合するのを防ぎます。
* 優先度を設定して、オンラインの認証済みイベントをその人物に関連付けます

### 用語 {#terminology}

| 用語 | 説明 |
| --- | --- |
| 一意の名前空間 | 一意の名前空間は、ID グラフのコンテキスト内で異なるように設定された ID 名前空間です。 UI を使用して、一意の名前空間を設定できます。 名前空間が一意と定義されると、グラフには、その名前空間を含む ID を 1 つだけ割り当てることができます。 |
| 名前空間の優先度 | 名前空間の優先度とは、名前空間の相対的な重要度を指します。 名前空間の優先度は、UI を通じて設定できます。 特定の ID グラフ内の名前空間をランク付けできます。 有効にすると、ID 最適化アルゴリズムの入力や、エクスペリエンスイベントフラグメントのプライマリ ID の決定など、様々なシナリオで名前の優先度が使用されます。 |
| ID 最適化アルゴリズム | ID 最適化アルゴリズムは、一意の名前空間と名前空間の優先順位を設定することで作成されたガイドラインが、特定の ID グラフで適用されるようにします。 |

### 一意の名前空間 {#unique-namespace}

ID 設定 UI ワークスペースを使用して、一意の名前空間を設定できます。 これにより、は、特定のグラフが、その一意の名前空間を含む ID を 1 つだけ持つ可能性があることを ID 最適化アルゴリズムに通知します。 これにより、同じグラフ内で 2 つの異なる人物識別子が結合されるのを防ぎます。

次のシナリオについて考えてみます。

* Scott はタブレットを使用し、GoogleのChromeブラウザーを開いて acme<span>.com にアクセスし、そこでサインインして新しいバスケットボールシューズを閲覧します。
   * このシナリオでは、バックグラウンドで次の ID がログに記録されます。
      * ブラウザーの使用を表す ECID 名前空間および値
      * 認証済みユーザー（Scott がユーザー名とパスワードの組み合わせでログインした）を表す CRMID 名前空間と値。
* その後、息子のピーターは同じタブレットを使用し、Google Chromeも使用して acme<span>.com にアクセスし、そこで自分のアカウントでサインインしてサッカー用品を参照します。
   * このシナリオでは、バックグラウンドで次の ID がログに記録されます。
      * ブラウザーを表すための同じ ECID 名前空間および値。
      * 認証済みユーザーを表す新しい CRMID 名前空間および値。

CRMID が一意の名前空間として設定された場合、ID 最適化アルゴリズムは、CRMID を結合する代わりに、2 つの異なる ID グラフに分割します。

一意の名前空間を設定しないと、同じ CRMID 名前空間を持つ 2 つの ID が異なる ID 値を持つなど、グラフの不要な結合が発生する場合があります（このようなシナリオは、多くの場合、同じグラフ内の 2 つの異なる人物エンティティを表します）。

ID 最適化アルゴリズムに通知する一意の名前空間を設定して、特定の ID グラフに取り込まれる ID データに対して制限を適用する必要があります。

### 名前空間の優先度 {#namespace-priority}

名前空間の優先度とは、名前空間の相対的な重要度を指します。 名前空間の優先順位は UI を通じて設定でき、特定の ID グラフで名前空間をランク付けできます。

名前空間の優先度を使用する 1 つの方法は、リアルタイム顧客プロファイルでエクスペリエンスイベントフラグメントのプライマリ ID （ユーザー行動）を決定することです。 優先度設定が設定されている場合、どのプロファイルフラグメントが保存されるかを決定する際に、web SDKのプライマリ ID 設定は使用されなくなります。

一意の名前空間と名前空間の優先度は、どちらも ID 設定 UI ワークスペースで設定できます。 ただし、設定の影響は次のように異なります。

| | ID サービス | リアルタイム顧客プロファイル |
| --- | --- | --- |
| 一意の名前空間 | ID サービスでは、ID 最適化アルゴリズムは一意の名前空間を参照し、特定の ID グラフに取り込まれる ID データを決定します。 | 一意の名前空間は、リアルタイム顧客プロファイルには影響しません。 |
| 名前空間の優先度 | ID サービスでは、複数のレイヤーを持つグラフの場合、名前空間の優先度によって適切なリンクが削除されたことが判断されます。 | エクスペリエンスイベントがプロファイルに取り込まれると、優先度が最も高い名前空間がプロファイルフラグメントのプライマリ ID になります。 |

* グラフあたり 50 個の ID の制限に達した場合、名前空間の優先度はグラフの動作に影響しません。
* **名前空間の優先度は数値です** 名前空間の相対的な重要度を示す名前空間に割り当てられます。 これは、名前空間のプロパティです。
* **プライマリ ID は、** に対してプロファイルフラグメントが保存される ID です。 プロファイルフラグメントは、特定のユーザーに関する情報を格納するデータのレコードです。属性（通常は CRM レコードを介して取り込まれる）またはイベント （通常はエクスペリエンスイベントまたはオンラインデータから取り込まれる）です。
* 名前空間の優先度は、エクスペリエンスイベントフラグメントのプライマリ ID を決定します。
   * プロファイルレコードの場合、Experience Platform UI のスキーマ ワークスペースを使用して、プライマリ ID などの ID フィールドを定義できます。 詳しくは、[UI での ID フィールドの定義 ](../../xdm/ui/fields/identity.md) に関するガイドを参照してください。
* エクスペリエンスイベントに、identityMap で最も名前空間の優先順位が高い 2 つ以上の ID がある場合、そのイベントは「無効なデータ」と見なされるので、取り込みから拒否されます。 例えば、identityMap に `{ECID: 111, CRMID: John, CRMID: Jane}` が含まれている場合、イベントが `CRMID: John` と `CRMID: Jane` の両方に同時に関連付けられていることを意味するので、イベント全体が不正なデータとして拒否されます。

詳しくは、[ 名前空間の優先度 ](./namespace-priority.md) に関するガイドを参照してください。

## 次の手順

[!DNL Identity Graph Linking Rules] について詳しくは、次のドキュメントを参照してください。

* [ID 最適化アルゴリズム](./identity-optimization-algorithm.md)
* [実装ガイド](./implementation-guide.md)
* [グラフ設定の例](./example-configurations.md)
* [トラブルシューティングと FAQ](./troubleshooting.md)
* [名前空間の優先度](./namespace-priority.md)
* [グラフシミュレーション UI](./graph-simulation.md)
* [ID 設定 UI](./identity-settings-ui.md)