---
title: ボンボラの意図
description: Experience Platformの Bombora Intent ソースについて説明します。
last-substantial-update: 2025-03-26T00:00:00Z
badgeB2B: label="B2B edition" type="Informative" url=" https://experienceleague.adobe.com/docs/experience-platform/rtcdp/intro/rtcdp-intro/overview.html?lang=ja#rtcdp-editions newtab=true"
badgeB2P: label="B2P エディション" type="Positive" url=" https://experienceleague.adobe.com/docs/experience-platform/rtcdp/intro/rtcdp-intro/overview.html?lang=ja#rtcdp-editions newtab=true"
exl-id: d2e81207-8ef5-4e52-bbac-a2fa262d8d08
source-git-commit: 8a5fdcfcf503df1b9d5aa338ff530181a2d03b5d
workflow-type: tm+mt
source-wordcount: '1607'
ht-degree: 10%

---

# [!DNL Bombora Intent]

[!DNL Bombora] は、B2B インテントデータを専門とする包括的なオーディエンスソリューションです。 [!DNL Bombora Intent] は、[!DNL Bombora] アカウントをExperience Platformに接続し、アカウントインテントデータを統合するために使用できるAdobe Experience Platform ソースです。

[!DNL Bombora Intent] ソースを使用すると、会社のサージインテント データ [!DNL Bombora's] 統合して、製品やサービスを積極的に調査しているアカウントを特定できます。 [!DNL Bombora] を使用して市場内アカウントに優先順位を付け、正確なセグメントを作成し、ターゲットを絞った ABM キャンペーンを実行し、マーケティング活動がコンバージョンする可能性が最も高いアカウントに集中できるようにします。 さらに、インテント主導の戦略を活用して、広告費用を最適化し、エンゲージメントを強化し、ROI を最大化できます。

[!DNL Bombora] ソースに関する前提条件については、このドキュメントを参照してください。

## ユースケース {#use-cases}

[!DNL Bombora] ソースに適用できるユースケースの例については、次をお読みください。

### Demand-Side Platform（DSP）の統合

B2B マーケターは、Real-Time CDPでアカウントリストを作成して、商品に対して高い意図を示す会社を特定し、[!DNL Bombora] でこのリストをアクティブ化できます。このリストは DSP と直接統合されているので、[!DNL Bombora's] データを使用してターゲットを設定したプログラム広告キャンペーンを実行できます。 これにより、コンバージョンが発生する可能性が最も高い企業に広告費用が集中するようになります。

### Account-Based Marketing（ABM）の機能

B2B マーケターは、ファーストパーティおよびサードパーティのインテント信号に基づいてアカウントリストを作成できます。 次に、[!DNL Bombora] でこのリストをアクティブ化できます。このリストでは、ABM 機能を使用して、これらのアカウントで従業員をターゲットに設定できるので、幅広いオーディエンスではなく、意思決定者に広告が届くようになります。

### マルチチャネル ABM アクティベーション

B2B マーケターは、Real-Time CDPでアカウントリストを作成して、目的の高い企業を特定し、アクティブ化することで、複数のチャネルでターゲットキャンペーンを実行で [!DNL Bombora] ます。 有料ソーシャルメディアでは、[!DNL Linkedin] や [!DNL Facebook] などのプラットフォーム上のターゲットアカウントで、パーソナライズされた広告を専門家に提供できます。

ネイティブの広告プラットフォームを使用すると、コンテンツが関連するコンテキストの意思決定者に確実に到達することができます。 また、キャンペーンを高度なテレビに拡張して、主要アカウントに広告を配信することもできます。 このマルチチャネルアプローチにより、プラットフォーム間で一貫したメッセージングが保証され、エンゲージメントとコンバージョン率が最大化されます。

## 前提条件 {#prerequisites}

[!DNL Bombora] をExperience Platformに接続する前に、次の前提条件の手順をお読みください。

### IP アドレスの許可リスト

ソースコネクタを操作する前に、IP アドレスのリストを許可リストに追加する必要があります。 地域固有の IP アドレスを許可リストに追加しないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下する場合があります。 許可リストに加える詳しくは、[IP アドレス ](../../ip-address-allow-list.md) ページを参照してください。

### Experience Platformに対する権限の設定

**[!UICONTROL アカウントをExperience Platformに接続するには、アカウントで]** ソースの表示 **[!UICONTROL および]** ソースの管理 [!DNL Bombora] 権限の両方が有効になっている必要があります。 必要な権限を取得するには、製品管理者にお問い合わせください。 詳しくは、[ アクセス制御 UI ガイド ](../../../access-control/abac/ui/permissions.md) を参照してください。

### ファイルとディレクトリの命名制約

クラウドストレージファイルまたはディレクトリに名前を付ける場合は、次の制限事項を考慮する必要があります。

* ディレクトリ名とファイルコンポーネント名は 255 文字を超えてはなりません。
* ディレクトリ名とファイル名の末尾にスラッシュ（`/`）は使用できません。使用した場合、自動的に削除されます。
* 次の予約 URL 文字は、適切にエスケープする必要があります。`! ' ( ) ; @ & = + $ , % # [ ]`
* 次の文字は使用できません。`" \ / : | < > * ?`
* 無効な URL パス文字は使用できません。 `\uE000` のようなコードポイントは、NTFS ファイル名では有効ですが、有効な Unicode 文字ではありません。また、一部の ASCII 文字や Unicode 文字、例えば制御文字（0x00 ～ 0x1F、\u0081 など）も使用できません。HTTP/1.1 で Unicode 文字列を規定するルールについては、[RFC 2616、セクション 2.2：基本ルール](https://www.ietf.org/rfc/rfc2616.txt)および [RFC 3987](https://www.ietf.org/rfc/rfc3987.txt) を参照してください。
* 次のファイル名は使用できません：LPT1、LPT2、LPT3、LPT4、LPT5、LPT6、LPT7、LPT8、LPT9、COM1、COM2、COM3、COM4、COM5、COM6、COM7、COM8、COM9、PRN、AUX、NUL、CON、CLOCK$、ドット文字（.）、2 つのドット文字（..）。

### 必要な資格情報の収集

Experience Platform上の [!DNL Bombora] は [!DNL Google Cloud Storage] によってホストされています。 [!DNL Bombora] アカウントを正常に認証するには、次の資格情報に対する適切な値を指定する必要があります。

| 資格情報 | 説明 |
| --- | --- |
| アクセスキー ID | [!DNL Bombora] アクセスキー ID。 これは、Experience Platformに対してアカウントを認証するために必要な 61 文字の英数字の文字列です。 |
| シークレットアクセスキー | [!DNL Bombora] 秘密アクセスキー。 これは、Base-64 でエンコードされた 40 文字の文字列で、Experience Platformに対してアカウントを認証するために必要です。 |
| バケット名 | データの取り出し元の [!DNL Bombora] バケット。 |

これらの資格情報について詳しくは、[[!DNL Google Cloud Storage] HMAC キーガイド ](https://cloud.google.com/storage/docs/authentication/hmackeys#overview) を参照してください。 独自のアクセスキーを生成する手順については、[ ソースの概要の前提条件ガイド  [!DNL Google Cloud Storage]  を参照してください ](../cloud-storage/google-cloud-storage.md#prerequisite-setup-for-connecting-your-google-cloud-storage-account)。

## [!DNL Bombora] スキーマ {#schema}

[!DNL Bombora] スキーマとデータ構造については、この節を参照してください。

この [!DNL Bombora] スキーマは、**B2B Bombora アカウントインテント** と呼ばれます。 特定のアカウントとトピックに関する週別目的情報（匿名の B2B バイヤー調査およびコンテンツ消費）です。 データは parquet 形式です。

* クラス - XDM [!DNL Bombora Account Intent]
* 名前空間 – B2B [!DNL Bombora Account Intent]
* プライマリ ID - `intentID`
* 関係 – B2B アカウント

| フィールド名 | データタイプ | 説明 |
|------------------------|-----------|----------------------------------------------------------------------------------------|
| `extSourceSystemAudit` | オブジェクト | このフィールドは、ソースシステムの監査用にシステムによって使用されます。 |
| `_id` | 文字列 | このフィールドは、一意の ID としてシステムで使用されます。 |
| `accountDomain` | 文字列 | このフィールドには、アカウントドメインが格納されます。 |
| `accountID` | 文字列 | このフィールドには、このインテントレコードが関連付けられている B2B アカウント ID が含まれています。 |
| `bomboraAccountName` | 文字列 | このフィールドには、Bombora での会社の ID が含まれます。 |
| `clusterID` | 文字列 | このフィールドにはクラスター ID が格納されます。 |
| `clusterName` | 文字列 | このフィールドにはクラスター名を入力します。 |
| `compositeScore` | 整数 | このフィールドには、インテントの複合スコアが含まれます。 |
| `intentID` | 文字列 | このフィールドには、システムで生成された一意の値が含まれます。 |
| `partitionDate` | 日付 | このフィールドにはパーティションの日付が格納されます。 これは、毎週、週末に `mm/dd/yyyy` 形式で行われます。 |
| `topicID` | 文字列 | このフィールドには、Bombora のインテント トピック ID が含まれています。 |
| `topicName` | 文字列 | このフィールドには、Bombora のインテント トピック名が含まれています。 |

{style="table-layout:auto"}

>[!TIP]
>
>スキーマに対する変更は、事前にAdobeに通知されます。 シームレスなスキーマ進化をサポートするには、後方互換性の維持が不可欠です。 Experience Platformでは、追加専用バージョン管理アプローチを採用し、スキーマの更新が非破壊的されるようにします。 つまり、重大な変更は厳しく禁止され、既存のスキーマを拡張または拡張する変更のみが許可されます。

## UI で [!DNL Bombora] アカウントをExperience Platformに接続する

前提条件の設定が完了したら、[ アカウントのExperience Platformへの接続  [!DNL Bombora]  に関するチュートリアルを読み ](../../tutorials/ui/create/data-partners/bombora.md) 統合を開始します。

## よくある質問 {#faq}

[!DNL Bombora] ソースに関するよくある質問への回答については、この節を参照してください。

### Real-Time CDP B2B editionでアカウントインテントデータを使用するには、[!DNL Bombora] との既存の契約が必要ですか？

+++回答

はい。Experience PlatformおよびReal-Time CDP B2B edition内のインテント データにアクセスして利用するには、[!DNL Bombora] とのアクティブな契約が必要です。 この統合では、[!DNL Bombora] との既存の契約を活用して、Experience PlatformおよびReal-Time CDPでアカウントインテント信号を取り込み、アクティブ化します。

+++

### [!DNL Bombora] のカスタムフィールドはこの統合でサポートされていますか？

+++回答

現在、取り込みとアクティベーションに使用できるのは、標準の [!DNL Bombora] フィールドのみです。 サポートされるフィールドのリストを表示するには、[[!DNL Bombora]  スキーマガイド ](#schema) で使用可能なフィールドの詳細を参照してください。

+++

### [!DNL Bombora] からExperience Platformにデータをアドホックベースで取り込むことができますか？

+++回答

はい、アドホックベースで [!DNL Bombora] からデータを取り込むことができます。 新しいデータフローを作成して、最新のインテントデータを取り込むことができま [!DNL Bombora] （アドビからの新しいデータがある場合）。 ただし、一度にアクティブにできるデータフローは 1 つだけです。 したがって、新しいデータフローを作成する前に、既存のデータフローを削除するようにしてください。

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

### [!DNL Bombora] からExperience Platformへのアカウントの照合に使用するフィールドは何ですか？

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

インテントデータを [ アカウントオーディエンス ](../../../segmentation/types/account-audiences.md) で利用して、ターゲティング、セグメント化およびパーソナライゼーションを強化できます。 インテントシグナルを活用することで、企業は特定のトピックに高い関心を示すアカウントを特定して関与し、マーケティングとセールスアウトリーチを最適化できます。

+++
