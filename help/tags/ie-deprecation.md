---
title: Internet Explorer 10 および 11 でのタグのサポートの終了
description: Adobe Experience Platformでは、Internet Explorer 10 および 11 でタグの更新サポートが提供されなくなりました。
source-git-commit: 0103f1af37dc202087d3c81d495de88d3de7c377
workflow-type: tm+mt
source-wordcount: '354'
ht-degree: 0%

---

# Internet Explorer 10 および 11 でのタグのサポートの終了

オンラインデジタルエクスペリエンスの状況の発展に伴い、Adobeは、Adobe Experience Platformのタグに Internet Explorer 10(IE10) と Internet Explorer 11(IE11) をサポートするためのリソースに投資しなくなりました。 Adobeが IE10 および IE11 のサポートを積極的に削除していない間、これらのブラウザーが新機能に与える影響は、今後のアップデートでは考慮されなくなります。

## IE10 と IE11 がサポートされなくなった理由を教えてください。

タグが IE10 および IE11 をサポートしなくなる理由は次の 4 つです。

* **Microsoftは IE10 および IE11 のサポートを停止しています**:2020 年 1 月現在 [Microsoftは IE10 のサポートを停止しました](https://docs.microsoft.com/en-us/lifecycle/announcements/internet-explorer-10-end-of-support) セキュリティおよび非セキュリティの更新の場合。 2022 年 6 月現在 [Microsoftは IE11 のサポートを停止しました](https://docs.microsoft.com/en-us/lifecycle/announcements/internet-explorer-11-end-of-support) （Windows の特定のバージョン）
* **IE10 および IE11 のサポートを停止している業界が広がっています**:IE10 および IE11 のサポートが広範な業界で廃止されるにつれ、タグによるこれらのテクノロジーとの後方互換性の維持機能はますます妨げられています。
* **最新のテクノロジーでは、IE10 および IE11 をサポートしていません**:タグが最新のテクノロジーを引き続きサポートするには、IE10 および IE11 のサポートを終了する必要があります。これらの最新のテクノロジーは、これらの Web ブラウザーに互換性がないからです。
* **IE10 と IE11 をサポートすると、全体的な機能の開発が遅くなります。**:タグ用にリリースされた多くの新機能は、IE10 と IE11 を慎重に考慮する必要があります。 この検討の結果、IE10 および IE11 で動作する難しいテストツールを入手し、ネイティブサポートを持たない IE10 および IE11 で機能させるコードを追加し、機能が期待どおりに動作するかどうかを調べる作業が数時間増えます。 IE10 および IE11 のサポートを停止すると、Adobeは新しい機能をより迅速に提供できます。

## IE10 および IE11 の廃止の影響

サポートが廃止されると、次の効果が発生します。

* 新しい機能は、IE10 および IE11 をサポートしていない可能性があります。
* IE10 および IE11 を使用した、現在および新しい機能のサポートのテストは停止します。
* IE10 および IE11 をサポートしていた機能は、これらのブラウザーをサポートしなくなる場合があります。
