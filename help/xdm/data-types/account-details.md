---
title: アカウント詳細データタイプ
description: このドキュメントでは、アカウント詳細のエクスペリエンスデータモデル (XDM) データタイプの概要を説明します。
exl-id: 17254393-263e-4000-9bd2-815a9e842533
source-git-commit: 2fd35c4ac29f43391f9dc03c636d20558b701be7
workflow-type: tm+mt
source-wordcount: '435'
ht-degree: 19%

---

# [!UICONTROL アカウントの詳細] データタイプ

[!UICONTROL アカウントの詳細] は、ビジネス組織に関連する詳細を記述する標準の Experience Data Model(XDM) データ型です。

![データタイプの構造](../images/data-types/account-details.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `annualRevenue` | [[!UICONTROL 通貨]](./currency.md) | 組織の年間売上高の推定金額。 |
| `DUNSNumber` | 文字列 | 組織の Dun &amp; Bradstreet D-U-N-S 番号。 これは、Dun &amp; Bradstreet データベース内の各ビジネスロケーションに割り当てられた 9 桁の非目安番号で、一意で独立した個別の操作を持ち、Dun &amp; Bradstreet によってのみ管理されます。 |
| `NAICSCode` | 文字列 | 北米産業分類システムにおける組織の分類。 |
| `NAICSDescription` | 文字列 | NAICS コードに基づく、組織の事業部門の簡単な説明。 |
| `SICCode` | 文字列 | 組織の標準産業分類 (SIC) コード。 これは、会社が属する業界をビジネス活動に基づいて分類する 4 桁のコードです。 |
| `SICDescription` | 文字列 | SIC コードに基づく、組織の事業部門の簡単な説明。 |
| `companyProductAndServices` | 文字列 | 組織が取引またはビジネスを行っている製品およびサービス。 |
| `facebookPageUrl` | 文字列 | 組織のFacebookアカウントへの Web サイトリンク。 |
| `industry` | 文字列 | この組織が属する業界。 これは自由形式のフィールドで、クエリやプロパティの使用には、構造化された値または `xdm:classifier` を使用することをお勧めします。 |
| `jigsaw` | 文字列 | 組織の Data.com キー。 |
| `linkedinPageUrl` | 文字列 | 組織のLinkedInアカウントへの Web サイトリンク。 |
| `logoUrl` | 文字列 | Salesforce インスタンスの URL と組み合わせるパス ( 例： `https://yourInstance.salesforce.com/`) をクリックして、組織に関連付けられたソーシャルネットワークプロファイル画像をリクエストする URL を生成します。 生成された URL は、組織のソーシャルネットワークプロファイル画像への HTTP リダイレクト（コード 302）を返します。 |
| `marketSegment` | 文字列 | 組織が参加する名前付きのマーケットセグメント。これは自由形式のフィールドで、クエリやプロパティの使用には、構造化された値または `xdm:identifier` を使用することをお勧めします。 |
| `numberOfEmployees` | 整数 | 組織の従業員数。 |
| `organizationType` | 文字列 | 組織のタイプを説明するラベル。 |
| `primaryEmailDomain` | 文字列 | 組織がその担当者に使用する主な E メールドメイン。 |
| `rating` | Double | この組織の計算されたスコアまたは星評価。 `1` は可能な最大定格を示し、 `0` は、可能な最小定格です。 |
| `tickerSymbol` | 文字列 | このアカウントの株式市場のシンボル。 最大 20 文字。 |
| `twitterHandleUrl` | 文字列 | 組織のtwitterハンドルへの Web サイトリンク。 |
| `website` | 文字列 | 組織の web サイトの URL。 |

{style=&quot;table-layout:auto&quot;}

データタイプについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力された例](https://github.com/adobe/xdm/blob/master/components/datatypes/b2b/account-organization.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/b2b/account-organization.schema.json)
