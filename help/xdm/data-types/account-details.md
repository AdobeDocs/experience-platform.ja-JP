---
title: アカウントの詳細データタイプ
description: アカウント詳細エクスペリエンスデータモデル（XDM）データタイプについて説明します。
exl-id: 17254393-263e-4000-9bd2-815a9e842533
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '409'
ht-degree: 22%

---

# [!UICONTROL &#x200B; アカウントの詳細 &#x200B;] データタイプ

[!UICONTROL &#x200B; アカウントの詳細 &#x200B;] は、ビジネス組織に関連する詳細を記述する標準の Experience Data Model （XDM）データタイプです。

![ データタイプ構造 ](../images/data-types/account-details.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `annualRevenue` | [[!UICONTROL 通貨]](./currency.md) | 組織の年間売上高の推定金額。 |
| `DUNSNumber` | 文字列 | 組織の Dun &amp; Bradstreet D-U-N-S 番号。 これは独立して個別に動作する独自の Dun &amp; Bradstreet データベース内の各事業拠点に割り当てられた 9 桁の番号で、Dun &amp; Bradstreet によってのみ管理されます。 |
| `NAICSCode` | 文字列 | 組織の北米産業分類システム内の分類。 |
| `NAICSDescription` | 文字列 | NAICS コードに基づいた、組織の業種の簡単な説明。 |
| `SICCode` | 文字列 | 組織の標準産業分類（SIC） コード。 これは、ビジネス アクティビティに基づいて会社が属する業界を分類する 4 桁のコードです。 |
| `SICDescription` | 文字列 | SIC コードに基づいた、組織の業種の簡単な説明。 |
| `companyProductAndServices` | 文字列 | 組織が扱っている、またはビジネスを行っている製品およびサービス。 |
| `facebookPageUrl` | 文字列 | 組織のFacebook アカウントへの web サイトリンク。 |
| `industry` | 文字列 | この組織が属する業界。 これは自由形式のフィールドで、クエリやプロパティの使用には、構造化された値または `xdm:classifier` を使用することをお勧めします。 |
| `jigsaw` | 文字列 | 組織のData.com キー。 |
| `linkedinPageUrl` | 文字列 | 組織のLinkedIn アカウントへの web サイトリンク。 |
| `logoUrl` | 文字列 | 組織に関連付けられたソーシャルネットワークプロファイル画像をリクエストする URL を生成するために、Salesforce インスタンスの URL （例：`https://yourInstance.salesforce.com/`）と組み合わせるパス。 生成された URL は、組織のソーシャルネットワークプロファイル画像への HTTP リダイレクト（コード 302）を返します。 |
| `marketSegment` | 文字列 | 組織が参加する名前付きのマーケットオーディエンス。 これは自由形式のフィールドで、クエリやプロパティの使用には、構造化された値または `xdm:identifier` を使用することをお勧めします。 |
| `numberOfEmployees` | 整数 | 組織の従業員数。 |
| `organizationType` | 文字列 | 組織のタイプを説明するラベル。 |
| `primaryEmailDomain` | 文字列 | 組織が人物に使用するプライマリ E メールドメイン。 |
| `rating` | Double | この組織に対して計算されたスコアまたは星評価。 `1` は可能な最大評価を示し、`0` は可能な最小評価です。 |
| `tickerSymbol` | 文字列 | このアカウントの株式市場のシンボル。最大 20 文字。 |
| `twitterHandleUrl` | 文字列 | 組織のtwitterハンドルへの web サイトリンク。 |
| `website` | 文字列 | 組織の web サイトの URL。 |

{style="table-layout:auto"}

データタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/components/datatypes/b2b/account-organization.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/components/datatypes/b2b/account-organization.schema.json)
