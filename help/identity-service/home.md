---
keywords: Experience Platform;ホーム;人気のトピック;ID;ID;XDM グラフ;ID サービス;ID サービス
solution: Experience Platform
title: ID サービスの概要
topic-legacy: overview
description: Adobe Experience Platform ID サービスを利用すると、デバイスやシステム間で ID を橋渡しすることで、顧客とその行動をよりよく把握できます。これによって、インパクトのある個人的なデジタルエクスペリエンスをリアルタイムで提供できます。
exl-id: a22dc3f0-3b7d-4060-af3f-fe4963b45f18
source-git-commit: 3e073d2c45f88c56473ccc2e3d18a2bbedd4f254
workflow-type: tm+mt
source-wordcount: '1839'
ht-degree: 96%

---

# [!DNL Identity Service] の概要

関連性のあるデジタルエクスペリエンスを提供するには、顧客を完全に理解しておく必要があります。これは、顧客データが異なる複数のシステムに断片化され、個々の顧客が複数の「ID」を持っているように見える場合に、より難しくなります。

Adobe Experience Platform ID サービスを利用すると、デバイスやシステム間で ID を橋渡しすることで、顧客とその行動を包括的に把握し、インパクトのある個人的なデジタルエクスペリエンスをリアルタイムで提供できます。

[!DNL Identity Service] では、以下のことが可能です。

- 各インタラクションを通じて一貫性のある、パーソナライズされた、関連性の高いエクスペリエンスを顧客が確実に受け取れるようにします。
- 異なるソースの複数の異なる ID をつなぎ合わせ、顧客の包括的なビューを作成します。
- ID グラフを利用して様々な ID 名前空間をマッピングすることで、様々なチャネルをまたいで、顧客がブランドとどのようにやり取りしているかを視覚的に示します。

## はじめに

[!DNL Identity Service] の詳細を説明する前に、重要な用語の概要を以下に示します。

| 用語 | 定義 |
| --- | --- |
| ID | ID とは、エンティティ（通常は個人）に固有のデータです。ログイン ID、ECID、ロイヤリティ ID などの ID は、「既知の ID」とも呼ばれます。 |
| ECID | Experience Cloud ID（ECID）は、Experience Platform および Adobe Experience Cloud アプリケーション全体で使用される共有 ID 名前空間です。ECID は、顧客 ID の基盤を提供し、デバイスのプライマリ ID および ID グラフのベースノードとして使用されます。詳しくは、[ECID の概要](./ecid.md)を参照してください。 |
| ID 名前空間 | ID 名前空間は、ID のコンテキストまたはタイプを区別するのに役立ちます。例えば、ある ID は、「name<span>@email.com」をメールアドレスとして、「443522」を数値 CRM ID として区別します。ID 名前空間は、個々の ID を検索し、ID 値のコンテキストを提供するために使用されます。これにより、異なるプライマリ ID を持ち、`email` ID 名前空間の値が同じ 2 つの [!DNL Profile] フラグメントが、実際には同じ人物であると判断できます。詳しくは、「[ID 名前空間の概要](./namespaces.md)」を参照してください。 |
| ID グラフ | ID グラフは、異なる ID 間の関係のマップで、どの顧客 ID がどのように結び付けられているかを視覚化し、よりよく理解できます。詳しくは、[ID グラフビューアの使用](./ui/identity-graph-viewer.md)に関するチュートリアルを参照してください。 |
| 個人を識別できる情報（PII） | PII は、電子メールアドレスや電話番号など、顧客を直接識別できる情報です。PII 値は、照合に使用されることがよくあります。異なるシステムをまたぐ、顧客の複数の ID。 |
| 不明または匿名の ID | 不明または匿名の ID は、デバイスを使用している実際の人物を識別せずにデバイスを分離する指標です。不明 ID と匿名 ID には、訪問者の IP アドレスや cookie ID などの情報が含まれます。不明 ID と匿名 ID は行動データを提供できますが、顧客が PII を提供するまでは制限されます。 |

## [!DNL Identity Service] とは？

毎日、顧客はビジネスとやり取りし、ブランドとの関係を高め続けています。典型的な顧客は、組織のデータインフラストラクチャ内の任意の数のシステム（eコマース、ロイヤルティ、ヘルプデスクシステムなど）でアクティブになっている可能性があります。同じ顧客が任意の数のデバイスに匿名で関与する場合もあります。[!DNL Identity Service]を使用すると、顧客の全体像をまとめ、異なる複数のシステムに分散される可能性があった関連データを集計できます。

消費者とブランドとの関係の日常的な例を考えてみましょう。

- Mary は、eコマースサイトにアカウントを持っており、過去に何度か注文をおこなっています。彼女は普段、買い物にパソコンを使い、毎回ログインします。ある日、訪問中にタブレットを使用してサンダルを買い物しますが、注文はせず、ログインもしません。
- この時点で、Mary のアクティビティは、次の 2 つの異なるプロファイルとして表示されます。
   - eコマースログイン
   - タブレットデバイス（おそらくデバイス ID で識別される）
- Mary は後でタブレットセッションを再開し、ニュースレターを購読するためにメールアドレスを提供します。この場合、ストリーミング取得によって、新しい ID がレコードデータとして Mary のプロファイルに追加されます。その結果、[!DNL Identity Service] は、Mary のタブレットデバイスのアクティビティと eコマースのアカウント履歴を関連付けました。
- Mary がタブレットを次にクリックするまでには、単なる不明な買い物客が使用するタブレットではなく、Mary の完全なプロファイルと履歴を反映したターゲットコンテンツを表示させることができます。

![Platform での ID の組み合わせ](./images/identity-service-stitching.png)

基本的に、[!DNL Identity Service] を使用すれば、顧客の全体像をまとめ、異なる複数のシステムにまたがって分散している可能性のある関連データを集計できます。リアルタイム顧客プロファイルは、[!DNL Identity Service] が定義および維持する ID 関係を、顧客の全体像とブランドとのインタラクションを構築するために活用します。詳しくは、「[リアルタイム顧客プロファイルの概要](../profile/home.md)」を参照してください。

### ユースケース

[!DNL Identity Service]実装の例を次に示します。

- 通信会社は「電話番号」の値に依存する場合があります。この場合、電話番号は、オフラインとオンラインの両方のデータセットに関心を持つ同じ個人を指します。
- 小売会社は、匿名訪問者の高い割合が原因で、オフラインデータセットで「メールアドレス」を使用し、オンラインデータセットで ECID を使用できます。
- 銀行では、支店トランザクションなど、オフラインのデータセットで「口座番号」を使用する場合があります。ほとんどの訪問者は訪問中に認証されるので、オンラインデータセットの「ログイン ID」に依存する場合があります。
- また、GUID やその他のユニバーサル固有識別子（UUID）など、顧客固有の専用 ID を持つ場合もあります。

## ID 名前空間 {#identity-namespace}

>[!CONTEXTUALHELP]
>id="platform_identity_namespace"
>title="ID 名前空間"
>abstract="ID 名前空間は、ID のコンテキストまたはタイプを区別するのに役立ちます。例えば、ある ID は、「name<span>@email.com」をメールアドレスとして、「443522」を数値 CRM ID として区別します。"
>text="Learn more in documentation"

>[!CONTEXTUALHELP]
>id="platform_identity_value"
>title="ID 値"
>abstract="ID 値は、一意の個人、組織またはアセットを表す識別子です。 値が表す ID のコンテキストまたはタイプは、対応する ID 名前空間によって定義されます。 プロファイルフラグメント間でレコードデータを一致させる場合、名前空間と ID 値は一致する必要があります。プロファイルフラグメント間でレコードデータを一致させる場合、名前空間と ID 値が一致する必要があります。"
>text="Learn more in documentation"

「ID は何ですか」と聞いた場合、それ以上の文脈がなければ、役に立つ答えを出すのは難しいものです。同じ論理で、ID 値を表す文字列値は、システム生成の ID であるかメールアドレスであるかにかかわらず、文字列値のコンテキストを与える修飾子（ID 名前空間）が提供されたときにのみ完成します

顧客は、オンラインチャネルとオフラインチャネルを組み合わせてブランドとやり取りを行う場合があります。その結果、それらの断片化されたインタラクションをどのようにして単一の顧客 ID にまとめるかという課題が生じます。

複数のデバイスとチャネルにわたって顧客を理解するには、まず各チャネルでそれらを認識することから始めます。Platform では、ID 名前空間を使用することでこれを実現します。ID 名前空間はメールや電話などの識別子であり、顧客 ID に追加のコンテキストを提供するために使用されます。 ID 名前空間は個々の ID を検索またはリンクし、ID 値のコンテキストを提供するために使用されます。詳しくは、「[ID 名前空間の概要](./namespaces.md)」を参照してください。

## ID グラフ

ID グラフは異なる ID 名前空間の関係を表すマップで、顧客 ID 同士の結び付きを視覚化し、わかりやすくします。詳しくは、[ID グラフビューア の使用](./ui/identity-graph-viewer.md)に関するチュートリアルを参照してください。

次のビデオは、ID と ID のグラフについての理解をサポートすることを目的としています。

>[!VIDEO](https://video.tv.adobe.com/v/27841?quality=12&learn=on)

## [!DNL Identity Service]に ID データを提供する

この節では、Adobe Experience Platform に提供されるデータが、[!DNL Identity Service]で使用される前に処理されて各顧客の ID グラフが作成される仕組みについて説明します。

### ID フィールドの決定

エンタープライズデータ収集戦略に応じて、ID としてラベルを付けるデータフィールドによって、ID マップに含めるデータが決まります。Adobe Experience Platform の最大限のメリットと、可能な限り包括的な顧客 ID を得るには、オンラインとオフラインの両方のデータをアップロードする必要があります。

- オンラインデータは、ユーザー名やメールアドレスなど、オンラインでの存在と動作を説明するデータです。

- オフラインデータは、CRM システムの ID など、オンラインプレゼンスに直接関連しないデータを指します。このタイプのデータは、ID をより堅牢にし、異なるシステム間でのデータの統合をサポートします。

### 追加の ID 名前空間

Experience Platform オファーには様々な標準名前空間がありますが、ID を適切に分類するために、追加の名前空間を作成する必要がある場合があります。詳しくは、「ID 名前空間の概要」の、[組織の名前空間の表示と作成](./namespaces.md)に関する節を参照してください。

>[!NOTE]
>
> ID 名前空間は ID の修飾子です。その結果、一度作成した名前空間は削除できません。

### [!DNL Experience Data Model]（XDM）に ID データを含める

[!DNL Platform] が顧客データを整理するための標準化されたフレームワークとして、[!DNL Experience Data Model] （XDM）を使用すると、Experience Platform や [!DNL Platform] とやり取りする他のサービス間でデータを共有および理解できます。詳しくは、「[XDM システムの概要](../xdm/home.md)を参照してください」。

レコードスキーマと時系列スキーマは両方、ID データを含める手段を備えています。データが取得されると、異なる名前空間のデータフラグメントが共通の ID データを共有していることが判明した場合、ID グラフはデータフラグメント間に新しい関係を作成します。

### ID としての XDM フィールドの指定

レコードまたは時系列の XDM クラスを実装するスキーマ内の `string` タイプのフィールドは、ID フィールドとしてラベル付けできます。その結果、そのフィールドに取得されたすべてのデータは、ID データと見なされます。

>[!NOTE]
>
>配列およびマップタイプのフィールドはサポートされておらず、ID フィールドとしてマークおよびラベル付けできません。

ID フィールドでは、共通の PII データを共有している ID をリンクすることもできます。
例えば、電話番号フィールドを ID フィールドとしてラベル付けすると、[!DNL Identity Service] は同じ電話番号を使用している他の個人との関係を自動的にグラフ化します。

>[!NOTE]
>
> 結果の ID の名前空間は、フィールドにラベルが付けられた時点で提供されます。

### [!DNL Identity Service] のデータセットの設定

ストリーミング取得プロセス中、[!DNL Identity Service ]はレコードおよび時系列データから ID データを自動的に抽出します。ただし、データを取得する前に、[!DNL Identity Service]を有効にする必要があります。詳しくは、[API を使用したリアルタイム顧客プロファイルおよび ID サービスのデータセットの設定](../profile/tutorials/dataset-configuration.md)に関するチュートリアルを参照してください。

### データを [!DNL Identity Service] に取り込む

[!DNL Identity Service] は、[バッチ取得](../ingestion/batch-ingestion/overview.md)または[ストリーミング取得](../ingestion/streaming-ingestion/overview.md)のいずれかによって Experience Platform に送信される XDM 準拠のデータを使用します。

次のビデオは、 ID サービスを理解できるようサポートすることを目的としています。このビデオでは、データフィールドに ID としてラベルを付け、ID データを取り込んで、データが Adobe Experience Platform ID サービスのプライベートグラフに到達したことを確認する方法を説明します。

>[!WARNING]
>
>次のビデオに示す [!DNL Platform] UI は古くなっています。最新の UI のスクリーンショットと機能については、ドキュメントを参照してください。

>[!VIDEO](https://video.tv.adobe.com/v/28167?quality=12&learn=on)

## データガバナンス

Adobe Experience Platform はプライバシーを考慮して構築され、顧客 PII データを保護するデータガバナンスフレームワークを含んでいます。「email」または「phone」名前空間の ID データはデフォルトで暗号化されますが、機密データを永続化する前に確実に暗号化するために、データ取得時または [!DNL Platform] への到着時に、データにデータ使用ラベルを適用できます。詳しくは、「[データガバナンスの概要](../data-governance/home.md)」を参照してください。

## 次の手順

[!DNL Identity Service] の主要な概念と Experience Platform 内でのその役割を理解しましたので、次に、[[!DNL Identity Service API]](./api/getting-started.md) を使用して ID グラフを操作する方法を学習します。
