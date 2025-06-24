---
title: Demandbase の目的
description: Experience Platformの Demandbase Intent ソースについて説明します。
last-substantial-update: 2025-03-26T00:00:00Z
badgeB2B: label="B2B edition" type="Informative" url=" https://experienceleague.adobe.com/docs/experience-platform/rtcdp/intro/rtcdp-intro/overview.html?lang=ja#rtcdp-editions newtab=true"
badgeB2P: label="B2P エディション" type="Positive" url=" https://experienceleague.adobe.com/docs/experience-platform/rtcdp/intro/rtcdp-intro/overview.html?lang=ja#rtcdp-editions newtab=true"
exl-id: 62dd27e0-b846-4c04-977f-8a3ab99bc464
source-git-commit: 5757bc84a9aeec18eb5fe21d6f02160b2ba55166
workflow-type: tm+mt
source-wordcount: '1480'
ht-degree: 11%

---

# [!DNL Demandbase Intent]

[!DNL Demandbase] は、B2B のセールスとマーケティングの成功に使用できる、アカウントベースのマーケティングプラットフォームです。 [!DNL Demandbase Intent] は、[!DNL Demandbase] アカウントをExperience Platformに接続し、アカウントインテントデータを統合するために使用できるAdobe Experience Platform ソースです。

[!DNL Demandbase] ソースを使用すると、リアルタイムエンゲージメントに基づいて関心の高いアカウントを特定できます。 最も強力なインテント信号を優先順位付けすることで、正確なセグメントを作成し、ターゲットを絞ったキャンペーンを配信でき、マーケティング活動がコンバージョンする可能性が最も高いアカウントに集中できるようになります。 インテントドリブン戦略をアクティブ化すると、広告費用の最適化、エンゲージメントの向上、ROI の向上を実現できます。

[!DNL Demandbase] ソースに関する前提条件については、このドキュメントを参照してください。

## 前提条件 {#prerequisites}

[!DNL Demandbase] をExperience Platformに接続する前に、次の前提条件の手順をお読みください。

### IP アドレスの許可リスト

ソースコネクタを操作する前に、IP アドレスのリストを許可リストに追加する必要があります。 地域固有の IP アドレスを許可リストに追加しないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下する場合があります。 許可リストに加える詳しくは、[IP アドレス ](../../ip-address-allow-list.md) ページを参照してください。

### Experience Platformに対する権限の設定

[!DNL Demandbase] アカウントをExperience Platformに接続するには、アカウントで **[!UICONTROL ソースの表示]** および **[!UICONTROL ソースの管理]** 権限の両方が有効になっている必要があります。 必要な権限を取得するには、製品管理者にお問い合わせください。 詳しくは、[ アクセス制御 UI ガイド ](../../../access-control/abac/ui/permissions.md) を参照してください。

### ファイルとディレクトリの命名制約

クラウドストレージファイルまたはディレクトリに名前を付ける場合は、次の制限事項を考慮する必要があります。

* ディレクトリ名とファイルコンポーネント名は 255 文字を超えてはなりません。
* ディレクトリ名とファイル名の末尾にスラッシュ（`/`）は使用できません。使用した場合、自動的に削除されます。
* 次の予約 URL 文字は、適切にエスケープする必要があります。`! ' ( ) ; @ & = + $ , % # [ ]`
* 次の文字は使用できません。`" \ / : | < > * ?`
* 無効な URL パス文字は使用できません。 `\uE000` のようなコードポイントは、NTFS ファイル名では有効ですが、有効な Unicode 文字ではありません。また、一部の ASCII 文字や Unicode 文字、例えば制御文字（0x00 ～ 0x1F、\u0081 など）も使用できません。HTTP/1.1 で Unicode 文字列を規定するルールについては、[RFC 2616、セクション 2.2：基本ルール](https://www.ietf.org/rfc/rfc2616.txt)および [RFC 3987](https://www.ietf.org/rfc/rfc3987.txt) を参照してください。
* 次のファイル名は使用できません：LPT1、LPT2、LPT3、LPT4、LPT5、LPT6、LPT7、LPT8、LPT9、COM1、COM2、COM3、COM4、COM5、COM6、COM7、COM8、COM9、PRN、AUX、NUL、CON、CLOCK$、ドット文字（.）、2 つのドット文字（..）。

### 必要な資格情報の収集

Experience Platform上の [!DNL Demandbase] は [!DNL Google Cloud Storage] によってホストされています。 [!DNL Demandbase] アカウントを正常に認証するには、次の資格情報に対する適切な値を指定する必要があります。

| 資格情報 | 説明 |
| --- | --- |
| アクセスキー ID | [!DNL Demandbase] アクセスキー ID。 これは、Experience Platformに対してアカウントを認証するために必要な 61 文字の英数字の文字列です。 |
| シークレットアクセスキー | [!DNL Demandbase] 秘密アクセスキー。 これは、Base-64 でエンコードされた 40 文字の文字列で、Experience Platformに対してアカウントを認証するために必要です。 |
| バケット名 | データの取り出し元の [!DNL Demandbase] バケット。 |
| フォルダーパス | アクセス権を付与するフォルダーへのパス。 |

これらの資格情報について詳しくは、[[!DNL Google Cloud Storage] HMAC キーガイド ](https://cloud.google.com/storage/docs/authentication/hmackeys#overview) を参照してください。 独自のアクセスキーを生成する手順については、[ ソースの概要の前提条件ガイド  [!DNL Google Cloud Storage]  を参照してください ](../cloud-storage/google-cloud-storage.md#prerequisite-setup-for-connecting-your-google-cloud-storage-account)。

## [!DNL Demandbase] スキーマ

[!DNL Demandbase] スキーマとデータ構造については、この節を参照してください。

この [!DNL Demandbase] スキーマは、**Company Intent Weekly** と呼ばれます。 指定されたアカウントとキーワードに関する週別の意図情報（匿名の B2B 購入者の調査とコンテンツ消費）です。 データは parquet 形式です。

| フィールド名 | データタイプ | 必須 | ビジネスキー | メモ |
| --- | --- | --- | --- | --- |
| `company_id` | STRING | TRUE | はい | 正規の会社 ID。 |
| `domain` | STRING | TRUE | はい | インテントを示すアカウントの識別されたドメイン。 |
| `start_date` | 日付 | TRUE | はい | 期間にインテント アクティビティが発生した開始日。 |
| `end_date` | 日付 | TRUE | はい | 期間にインテント アクティビティが発生した終了日。 |
| `duration_type` | STRING | TRUE | はい | 期間のタイプ。 通常、この値は、選択したロールアップ期間に応じて、毎日、毎週、毎月のいずれかに設定されます。 このデータサンプルの場合、この値は `week` です。 |
| `keyword_set_id` | STRING | TRUE | はい | キーワード セット ID。 これは、特定の顧客ごとに一意です。 |
| `keyword_set` | STRING | TRUE | はい | キーワード セット名。 |
| `keyword` | STRING | TRUE | | intent キーワード。 |
| `is_trending` | STRING | TRUE | | 特定のトレンドの現在の状態。 トレンド状態は、過去 7 週間の平均に対する先週の意図的なアクティビティのバーストとして測定されます。 |
| `intent_strength` | ENUM[STRING] | TRUE | | インテントの強さの定量化された指標。 使用可能な値は、`HIGH`、`MED`、`LOW` です。 |
| `num_people_researching` | 整数 | TRUE | | 過去 7 日間に `company_id` ーザーに属し、キーワードを検索したユーザーの数。 |
| `num_trending_days` | 整数 | TRUE | | 特定の期間でキーワードがトレンドにあった日数。 |
| `trending_score` | 整数 | TRUE | | トレンドスコア。 |
| `record_id` | STRING | TRUE | | 一意のプライマリレコード ID。 |
| `partition_date` | 日付 | TRUE | | スナップショットのカレンダー日付。 これは、毎週、週末に行われます。 |

{style="table-layout:auto"}

>[!TIP]
>
>スキーマに対する変更は、事前にAdobeに通知されます。 シームレスなスキーマ進化をサポートするには、後方互換性の維持が不可欠です。 Experience Platformでは、追加専用バージョン管理アプローチを採用し、スキーマの更新が非破壊的されるようにします。 つまり、重大な変更は厳しく禁止され、既存のスキーマを拡張または拡張する変更のみが許可されます。

## UI で [!DNL Demandbase] アカウントをExperience Platformに接続する

前提条件の設定が完了したら、[ アカウントのExperience Platformへの接続 ](../../tutorials/ui/create/data-partners/demandbase.md) に関するチュートリアルを読み  [!DNL Demandbase]  統合を開始します。

## よくある質問 {#faq}

[!DNL Demandbase] ソースに関するよくある質問への回答については、この節を参照してください。

### Real-Time CDP B2B editionでアカウントインテントデータを使用するには、[!DNL Demandbase] との既存の契約が必要ですか？

+++回答

はい。Experience PlatformおよびReal-Time CDP B2B edition内のインテント データにアクセスして利用するには、[!DNL Demandbase] とのアクティブな契約が必要です。 この統合では、[!DNL Demandbase] との既存の契約を活用して、Experience PlatformおよびReal-Time CDPでアカウントインテント信号を取り込み、アクティブ化します。

+++

### [!DNL Demandbase] のカスタムフィールドはこの統合でサポートされていますか？

+++回答

現在、取り込みとアクティベーションに使用できるのは、標準の [!DNL Demandbase] フィールドのみです。 サポートされるフィールドのリストを表示するには、[[!DNL Demandbase]  スキーマガイド ](#schema) で使用可能なフィールドの詳細を参照してください。

+++

### [!DNL Demandbase] からExperience Platformにデータをアドホックベースで取り込むことができますか？

+++回答

はい、アドホックベースで [!DNL Demandbase] からデータを取り込むことができます。 新しいデータフローを作成して、最新のインテントデータを取り込むことができま [!DNL Demandbase] （アドビからの新しいデータがある場合）。 ただし、一度にアクティブにできるデータフローは 1 つだけです。 したがって、新しいデータフローを作成する前に、既存のデータフローを削除するようにしてください。

+++

### インテントデータの検証プロセスとは何ですか？また、どのインテントデータが特定のアカウントにリンクされているかを確認するにはどうすればよいですか？

+++回答

インテントデータを検証し、どのインテントシグナルが特定のアカウントにリンクされているかを判断するには、アカウント ID で [Adobe Experience Platform クエリサービス ](../../../query-service/home.md) を使用します。

+++

### 特定の会社のインテントを検索するにはどうすればよいですか？

+++回答

[ クエリサービス ](../../../query-service/home.md) で SQL クエリを実行し、会社名またはアカウント ID を使用してインテントデータを検索します。 特定の会社のすべてのインテント データを表示するには、会社名またはアカウント ID を使用してクエリサービスで SQL クエリを実行し、関連するすべてのインテント シグナルを取得します。

+++


### Experience Platformのアカウント照合プロセスで問題が見つかりました。どうすればよいですか？

+++回答

解決策は、具体的な問題に応じて異なります。

* **Experience Platformの会社ドメインが正しくないか、見つかりません**：問題がアカウントデータの会社ドメイン値が正しくない場合に発生した場合は、Experience Platformの会社ドメインフィールドを更新して、正確に一致するようにします。
* **データフローのフィールドマッピングが正しくありません**：データフローの会社ドメインフィールドパスが正しくないことが原因で問題が発生した場合は、正しいフィールドパスを参照するようにデータフロー設定を更新します。

+++

### Experience Platformでインテントデータを削除するにはどうすればよいですか？

+++回答

Experience Platformでインテントデータを削除するには、データセットを [ 削除 ](../../../catalog/datasets/user-guide.md#delete-a-dataset) する必要があります。

+++

### [!DNL Demandbase] からExperience Platformへのアカウントの照合に使用するフィールドは何ですか？

+++回答

`accountOrganization.domain` フィールドは、一致するアカウントに使用されます。 組織で別のカスタムフィールドを使用して web サイト名を保存する場合は、正確なマッピングを行うために、正しいフィールドパスを指定してください。

+++

### Experience Platformで会社ドメインが更新されるとどうなりますか。

+++回答

会社ドメインが更新されると、新しいドメイン値は次回のデータフロー実行で適用されます。 これにより、次のことが保証されます。

* 今後のインテントのデータ取り込みでは、アカウントのマッチングに更新されたドメインを使用します。
* 以前に一致しなかったインテント信号は、意図したアカウントに正しく揃うようになりました。
* 過去に取り込んだデータに対して遡及的な変更は行われず、新しいデータのみが取り込まれ、取り込まれたデータが更新を反映します。

+++

### ドメインのマッチングプロセスはどのようなものですか？

+++回答

Experience Platformでのドメインの一致は、スクラブされたドメインフィールド値の完全一致に基づいています。 Experience Platformはプレフィックス（https:/<span>/www など）を自動的に削除し、最上位ドメイン（adobe.comなど）を保持します。 一致には、完全なドメイン値が必要で、ファジーマッチングやサブドメインはサポートされません。

+++

### インテントデータはどこで使用できますか？

+++回答

インテントデータを [ アカウントオーディエンス ](../../../segmentation/types/account-audiences.md) で利用して、ターゲティング、セグメント化およびパーソナライゼーションを強化できます。 インテントシグナルを活用することで、企業は特定のトピックに高い関心を示すアカウントを特定して関与し、マーケティングとセールスアウトリーチを最適化できます

+++
