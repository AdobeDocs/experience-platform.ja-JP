---
title: Demandbase Intent
description: Experience PlatformのDemandbase Intent ソースについて説明します。
last-substantial-update: 2025-03-26T00:00:00Z
exl-id: 62dd27e0-b846-4c04-977f-8a3ab99bc464
source-git-commit: 6d86b6cfe966b210d105c9561428c001908007af
workflow-type: tm+mt
source-wordcount: '1675'
ht-degree: 12%

---

# [!DNL Demandbase Intent]

[!DNL Demandbase]は、B2B セールスおよびマーケティングの成功に使用できるアカウント ベースのマーケティング プラットフォームです。 [!DNL Demandbase Intent]はAdobe Experience Platform ソースであり、[!DNL Demandbase] アカウントをExperience Platformに接続し、アカウント インテント データを統合するために使用できます。

[!DNL Demandbase] ソースを使用すると、リアルタイムのエンゲージメントに基づいて関心の高いアカウントを特定できます。 最も強力なインテントシグナルを優先することで、正確なセグメントを作成し、高度にターゲティングした施策を実施することで、コンバージョンする可能性が最も高いアカウントにマーケティング活動を集中させることができます。 インテント主導の戦略を活用することで、広告費の最適化、エンゲージメントの向上、ROIの向上を実現できます。

[!DNL Demandbase] ソースに関する前提条件の情報については、このドキュメントを参照してください。

## 前提条件 {#prerequisites}

[!DNL Demandbase]をExperience Platformに接続する前の前提条件の手順については、次の節を参照してください。

### IP アドレスの許可リスト

ソースコネクタを使用する前に、IP アドレスのリストを許可リストに追加する必要があります。 地域に固有のIP アドレスをYoutube 許可リストに追加しないと、ソースを使用する際にエラーまたはパフォーマンスが低下する可能性があります。 詳しくは、[IP アドレスの許可リスト](../../ip-address-allow-list.md) ページを参照してください。

### Experience Platformの権限の設定

[!DNL Demandbase] アカウントをExperience Platformに接続するには、アカウントに対して&#x200B;**[!UICONTROL View Sources]**&#x200B;と&#x200B;**[!UICONTROL Manage Sources]**&#x200B;の両方の権限を有効にする必要があります。 必要な権限を取得するには、製品の管理者にお問い合わせください。 詳しくは、[ アクセス制御UI ガイド ](../../../access-control/abac/ui/permissions.md)を参照してください。

### ファイルとディレクトリの命名制約

クラウドストレージファイルまたはディレクトリに名前を付ける際には、以下に示す制限を考慮する必要があります。

* ディレクトリ名とファイルコンポーネント名は 255 文字を超えてはなりません。
* ディレクトリ名とファイル名の末尾にスラッシュ（`/`）は使用できません。 使用した場合、自動的に削除されます。
* 次の予約 URL 文字は、適切にエスケープする必要があります。`! ' ( ) ; @ & = + $ , % # [ ]`
* 次の文字は使用できません。`" \ / : | < > * ?`
* 不正なURL パス文字は使用できません。 `\uE000` のようなコードポイントは、NTFS ファイル名では有効ですが、有効な Unicode 文字ではありません。 また、一部の ASCII 文字や Unicode 文字、例えば制御文字（0x00 ～ 0x1F、\u0081 など）も使用できません。 HTTP/1.1 で Unicode 文字列を規定するルールについては、[RFC 2616、セクション 2.2：基本ルール](https://www.ietf.org/rfc/rfc2616.txt)および [RFC 3987](https://www.ietf.org/rfc/rfc3987.txt) を参照してください。
* 次のファイル名は使用できません：LPT1、LPT2、LPT3、LPT4、LPT5、LPT6、LPT7、LPT8、LPT9、COM1、COM2、COM3、COM4、COM5、COM6、COM7、COM8、COM9、PRN、AUX、NUL、CON、CLOCK$、ドット文字（.）、2 つのドット文字（..）。

### 必要な資格情報の収集

Experience Platformの[!DNL Demandbase]は[!DNL Google Cloud Storage]によってホストされています。 [!DNL Demandbase] アカウントを正常に認証するには、次の資格情報に適切な値を指定する必要があります。

| 資格情報 | 説明 |
| --- | --- |
| アクセスキー ID | [!DNL Demandbase] アクセス キーID。 これは、Experience Platformにアカウントを認証するために必要な61文字の英数字の文字列です。 |
| シークレットアクセスキー | [!DNL Demandbase] シークレット アクセス キー。 これは、Experience Platformにアカウントを認証するために必要な40文字の64でエンコードされた文字列です。 |
| バケット名 | データの取得元となる[!DNL Demandbase] バケット。 |
| フォルダーパス | アクセス権を付与するフォルダーへのパス。 |

これらの資格情報について詳しくは、[[!DNL Google Cloud Storage] HMAC キーガイド ](https://cloud.google.com/storage/docs/authentication/hmackeys#overview)を参照してください。 独自のアクセスキーを生成する手順については、 [!DNL Google Cloud Storage]  ソースの概要](../cloud-storage/google-cloud-storage.md#prerequisite-setup-for-connecting-your-google-cloud-storage-account)の[前提条件ガイドを参照してください。

## [!DNL Demandbase] スキーマ

>[!IMPORTANT]
>
>Experience Platform UIでB2B デマンドベースアカウントインテントスキーマを作成する場合は、スキーマのプロファイル取り込みを必ず有効にします。 詳しくは、[UIでのスキーマの作成と編集](../../../xdm/ui/resources/schemas.md)に関するガイドを参照してください。

[!DNL Demandbase] スキーマとデータ構造について詳しくは、この節を参照してください。

[!DNL Demandbase] スキーマは&#x200B;**B2B Demandbase Account Intent**&#x200B;と呼ばれます。 これは、指定されたアカウントとキーワードに関する週次のインテント情報（匿名のB2B バイヤー調査およびコンテンツ消費）です。 データは寄木細工の形式です。

* クラス - XDM [!DNL Demandbase Account Intent]
* 名前空間 – B2B [!DNL Demandbase Account Intent]
* プライマリ ID - `intentID`
* 関係 – B2B アカウント

| フィールド名 | データタイプ | 説明 |
|--------------------------|-----------|-------------------------------------------------------------------------------------------------------------|
| `extSourceSystemAudit` | オブジェクト | このフィールドには、外部ソースからのシステム監査情報が含まれます。 |
| `_id` | 文字列 | これは、レコードの一意のシステム識別子です。 |
| `accountDomain` | 文字列 | このフィールドには、アカウントドメインが含まれています。 |
| `accountID` | 文字列 | これは、このインテントレコードが関連付けられているB2B アカウント IDです。 |
| `demandbaseAccountID` | 文字列 | これは[!DNL Demandbase]の会社のIDです。 |
| `durationType` | 文字列 | このフィールドは、意図の有効期間タイプ（例：「週」）を指定します。 |
| `endDate` | 日付 | これは、意図の有効期間の終了日です。 |
| `intentID` | 文字列 | これは、インテントレコードのシステム生成の一意の値です。 |
| `intentStrength` | 文字列 | このフィールドは、「DAY」、「WEEK」、「MONTH」など、意図の有効期間タイプを指定します。 |
| `isTrending` | BOOLEAN | このフィールドは、キーワードがトレンドかどうか（可能な値はLow、Medium、High）を示します。 |
| `keyword` | 文字列 | このフィールドには、[!DNL Demandbase]からの意図を示すキーワードまたはフレーズが含まれています。 |
| `keywordSetID` | 文字列 | これはキーワードセットの識別子です。 |
| `keywordSetName` | 文字列 | これはキーワードセットの名前です。 |
| `numTrendingDays` | 整数 | このフィールドは、キーワードがトレンドになっている日数を示します。 |
| `partitionDate` | 日付 | これは、レコードのパーティション日です。 |
| `peopleResearchingCount` | 整数 | このフィールドは、キーワードを検索するユーザーの数を示します。 |
| `startDate` | 日付 | これは、意図の有効期間の開始日です。 |
| `trendingScore` | 整数 | このフィールドには、キーワードのトレンドスコアが含まれます。 |

{style="table-layout:auto"}

>[!TIP]
>
>スキーマの変更は、事前にAdobeに通知されます。 シームレスなスキーマ進化をサポートするには、後方互換性を維持することが不可欠です。 Experience Platformでは、追加のみのバージョン管理アプローチが適用され、スキーマの更新が非破壊的であることを保証します。 つまり、変更を解除することは固く禁止されており、既存のスキーマを強化または拡張する変更のみが許可されます。

## UIで[!DNL Demandbase] アカウントをExperience Platformに接続します

前提条件の設定が完了したら、 [!DNL Demandbase]  アカウントをExperience Platform](../../tutorials/ui/create/data-partners/demandbase.md)に接続する[に関するチュートリアルを参照して、統合を開始してください。

## よくある質問 {#faq}

[!DNL Demandbase] ソースに関するよくある質問に対する回答については、この節を参照してください。

### Real-Time CDP B2B editionでアカウントインテントデータを使用するには、[!DNL Demandbase]との既存の契約が必要ですか？

+++回答

はい。Experience PlatformおよびReal-Time CDP B2B edition内でインテントデータにアクセスして利用するには、[!DNL Demandbase]とのアクティブな契約が必要です。 この統合では、既存の[!DNL Demandbase]との契約書を活用して、Experience PlatformとReal-Time CDPでアカウントのインテントシグナルを取り込み、アクティブ化します。

+++

### [!DNL Demandbase]のカスタムフィールドは、この統合でサポートされていますか？

+++回答

現在、取り込みとアクティベーションには標準の[!DNL Demandbase] フィールドのみを使用できます。 サポートされているフィールドのリストを表示するには、[[!DNL Demandbase]  スキーマガイド ](#schema)を参照して、フィールドの可用性の詳細を確認してください。

+++

### アドホックベースで[!DNL Demandbase]からExperience Platformにデータを取り込むことはできますか？

+++回答

はい、[!DNL Demandbase]からデータをアドホックベースで取り込むことができます。 [!DNL Demandbase]から新しいデータがある限り、新しいデータフローを作成して、最新のインテントデータを取り込むことができます。 ただし、一度に1つのアクティブなデータフローのみを持つことができます。 したがって、新しいデータフローを作成する前に、既存のデータフローを削除してください。

+++

### インテントデータの検証プロセスと、特定のアカウントにリンクされているインテントデータを確認するにはどうすればよいですか？

+++回答

インテントデータを検証し、特定のアカウントにリンクされているインテントシグナルを判断するには、[AccountIDでAdobe Experience Platform Query Service](../../../query-service/home.md)を使用します。

+++

### 特定の企業のインテントを検索するにはどうすればよいですか？

+++回答

[ クエリサービス ](../../../query-service/home.md)でSQL クエリを実行し、会社名またはAccountIDを使用してインテントデータを検索します。 特定の企業のすべてのインテントデータを表示するには、企業名またはAccountIDを使用してクエリサービスでSQL クエリを実行し、関連するすべてのインテントシグナルを取得します。

+++


### Experience Platformでアカウント照合プロセスに問題が発生しました。どうすればよいですか？

+++回答

解決策は、特定の問題によって異なります。

* **Experience Platformの会社ドメインが正しくないか見つかりません**：問題がアカウントデータ内の会社ドメインの値が正しくない場合は、Experience Platformの会社ドメインフィールドを更新して、正確に一致するようにします。
* **データフロー**&#x200B;のフィールドマッピングが正しくありません。問題の原因がデータフロー内の会社ドメインフィールドパスが正しくない場合は、データフロー設定を更新して正しいフィールドパスを参照してください。

+++

### Experience Platformでインテントデータを削除するにはどうすればよいですか？

+++回答

Experience Platformでインテントデータを削除するには、[ データセット ](../../../catalog/datasets/user-guide.md#delete-a-dataset)を削除する必要があります。

+++

### [!DNL Demandbase]からExperience Platformへのアカウントの照合に使用されるフィールドは何ですか？

+++回答

`accountOrganization.domain` フィールドは、一致するアカウントに使用されます。 組織で別のカスタムフィールドを使用してweb サイト名を保存する場合は、正確なマッピングのために正しいフィールドパスを指定してください。

+++

### Experience Platformで会社ドメインが更新されるとどうなりますか？

+++回答

会社ドメインが更新されると、次のデータフロー実行で新しいドメイン値が適用されます。 これにより、次のことが可能になります。

* 今後のインテントデータの取り込みでは、更新されたドメインを使用してアカウントの照合を行います。
* 以前に一致しなかった意図シグナルが、意図したアカウントと正しく一致するようになりました。
* 過去に取り込まれたデータのみに対して遡及的な変更は行われず、新しいデータのみが取り込まれ、更新が反映されます。

+++

### ドメインマッチングプロセスとは何ですか？

+++回答

Experience Platformのドメインマッチングは、スクラブドメインフィールドの値と完全に一致することに基づいています。 Experience Platformは接頭辞（例：https:/<span>/www）を自動的に削除します。 トップレベルドメインを保持します（例：adobe.com）。 一致させるには、ファジィ一致やサブドメインをサポートしない正確なドメイン値が必要です。

+++

### インテントデータの活用場所？

+++回答

インテントデータは、[ アカウントオーディエンス ](../../../segmentation/types/account-audiences.md)で利用して、ターゲティング、セグメンテーション、パーソナライゼーションを強化できます。 インテントシグナルを活用することで、特定のトピックに高い関心を示しているアカウントを特定してエンゲージし、マーケティングとセールスのアウトリーチを最適化することができます

+++

### 標準の[!DNL Account Key] フィールドグループは[!DNL Demandbase Account Intent] スキーマと互換性がありますか？

+++回答

いいえ。 B2B アカウントスキーマとの関係を確立するには、`accountID` フィールドを使用します。 これにより、参照またはソーススキーマでフィールドグループ全体を導入する必要がなくなります。
+++

### [!DNL Demandbase Account Intent] スキーマは、B2B アカウントスキーマとの関係をどのように確立しますか？

+++回答

[!DNL Demandbase Account Intent] スキーマは、`accountID` フィールドを使用して、対応するB2B アカウントレコードにリンクします。 このフィールドは、両方のデータセットで一致するドメインが見つかった場合、取り込み中に自動的に入力されます。 具体的には、[!DNL Demandbase] スキーマの`accountID`は、標準B2B アカウントスキーマの`accountKey.sourceKey`を参照しています。

+++

### [!DNL Demandbase Account Intent] スキーマで、一般的な[!DNL Account Key] フィールドグループ構造ではなく`accountID`を使用するのはなぜですか？

+++回答

[!DNL Demandbase Intent]個のスキーマは、ストレージと処理効率に重点を置いています。 スキーマは、フィールドグループ全体を使用するのではなく、関係を確立するために合理化された単一フィールド （`accountID`）を使用します。 これにより、複雑さを軽減し、インテントデータの最適な処理パターンと連携させることができます。

+++
