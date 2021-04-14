---
product: experience-platform
audience: user
user-guide-title: エクスペリエンスデータモデル（XDM）システムヘルプ
breadcrumb-title: Experience Data Model（XDM）ガイド
user-guide-description: エクスペリエンスデータモデル（XDM）クラスと Mixin を使用して、エクスペリエンスデータを標準化します。
feature: スキーマ
translation-type: tm+mt
source-git-commit: 8b88a828f8680ac4d064f7f84e0db9e315526833
workflow-type: tm+mt
source-wordcount: '280'
ht-degree: 49%

---


# エクスペリエンスデータモデル（XDM）システム {#xdm}

* [XDM システムの概要](home.md)
* スキーマ {#schema}
   * [スキーマ構成の基本](schema/composition.md)
   * [データモデリングのベストプラクティス](schema/best-practices.md)
   * [XDMフィールド型の制約](schema/field-constraints.md)
   * [XDM フィールド辞書](schema/field-dictionary.md)
   * 業界データモデル{#industries}
      * [概要](./schema/industries/overview.md)
      * [小売データモデルERD](./schema/industries/retail.md)
      * [金融サービスデータモデルERD](./schema/industries/financial.md)
      * [旅行および接客のデータモデルERD](./schema/industries/travel-hospitality.md)
* クラス {#classes}
   * [XDM 個人プロファイル](./classes/individual-profile.md)
   * [XDM ExperienceEvent](./classes/experienceevent.md)
   * [セグメント定義](./classes/segment-definition.md)
* Mixin {#mixins}
   * プロファイルミックスイン{#profile}
      * [IdentityMap](./mixins/profile/identitymap.md)
      * [人口統計の詳細](./mixins/profile/person-details.md)
      * [個人の連絡先の詳細](./mixins/profile/personal-details.md)
      * [プライバシー/パーソナライゼーション/マーケティングの環境設定（同意）](./mixins/profile/consents.md)
      * [セグメントのメンバーシップの詳細](./mixins/profile/segmentation.md)
      * [勤務先担当者の詳細](./mixins/profile/work-details.md)
   * イベントミックスイン{#event}
      * [エンドユーザーIDの詳細](./mixins/event/enduserids.md)
      * [環境の詳細](./mixins/event/environment-details.md)
   * [Mixin名の更新](./mixins/name-updates.md)
* データタイプ {#data-types}
   * [アプリケーション](./data-types/application.md)
   * [Beacon](./data-types/beacon.md)
   * [ブラウザーの詳細](./data-types/browser-details.md)
   * [コマース](./data-types/commerce.md)
   * [同意と環境設定](./data-types/consents.md)
   * [デバイス](./data-types/device.md)
   * [電子メールアドレス](./data-types/email-address.md)
   * [環境](./data-types/environment.md)
   * [汎用同意フィールド](./data-types/consent-field.md)
   * [汎用マーケティング基本設定フィールド](./data-types/marketing-field.md)
   * [購読付きの汎用マーケティング環境設定フィールド](./data-types/marketing-field-subscriptions.md)
   * [汎用パーソナライゼーション設定フィールド](./data-types/personalization-field.md)
   * [地域](./data-types/geo.md)
   * [地域サークル](./data-types/geo-circle.md)
   * [地理座標](./data-types/geo-coordinates.md)
   * [地域とのやり取りの詳細](./data-types/geo-interaction-details.md)
   * [ジオシェイプ](./data-types/geo-shape.md)
   * [ID](./data-types/identity.md)
   * [測定](./data-types/measure.md)
   * [Order](./data-types/order.md)
   * [支払項目](./data-types/payment-item.md)
   * [ユーザー](./data-types/person.md)
   * [個人名](./data-types/person-name.md)
   * [電話番号](./data-types/phone-number.md)
   * [配置コンテキスト](./data-types/place-context.md)
   * [POIの詳細](./data-types/poi-details.md)
   * [POI相互作用](./data-types/poi-interaction.md)
   * [住所](./data-types/postal-address.md)
   * [検索](./data-types/search.md)
   * [購読](./data-types/subscription.md)
   * [Webインタラクション](./data-types/web-interactions.md)
   * [ウェブページの詳細](./data-types/webpage-details.md)
*  SchemasUI  {#ui}
   * [概要](./ui/overview.md)
   * [XDMリソースの参照](./ui/explore.md)
   * リソースの作成と編集{#resources}
      * [スキーマ](./ui/resources/schemas.md)
      * [クラス](./ui/resources/classes.md)
      * [Mixin](./ui/resources/mixins.md)
      * [データタイプ](./ui/resources/data-types.md)
   * フィールドの定義{#fields}
      * [概要](./ui/fields/overview.md)
      * [必須フィールド](./ui/fields/required.md)
      * [オブジェクトフィールド](./ui/fields/object.md)
      * [配列フィールド](./ui/fields/array.md)
      * [列挙フィールド](./ui/fields/enum.md)
      * [IDフィールド](./ui/fields/identity.md)
      * [関係フィールド](./ui/fields/relationship.md)
   * [サンプルXDMデータの生成](./ui/sample.md)
   * [XDMスキーマの書き出し](./ui/export.md)
* スキーマレジストリ API {#api}
   * [概要](api/overview.md)
   * [はじめに](api/getting-started.md)
   * [スキーマ](api/schemas.md)
   * [動作](api/behaviors.md)
   * [クラス](api/classes.md)
   * [Mixin](api/mixins.md)
   * [データタイプ](api/data-types.md)
   * [記述子](api/descriptors.md)
   * [和集合](api/unions.md)
   * [書き出し/読み込み](api/export-import.md)
   * [サンプルデータ](api/sample-data.md)
   * [監査ログ](api/audit-log.md)
   * [アドホックスキーマ](api/ad-hoc.md)
   * [付録](api/appendix.md)
* チュートリアル {#tutorials}
   * [スキーマの作成（UI）](tutorials/create-schema-ui.md)
   * [スキーマの作成（API）](tutorials/create-schema-api.md)
   * [2 つのスキーマ間の関係の定義（UI）](tutorials/relationship-ui.md)
   * [2 つのスキーマ間の関係の定義（API）](tutorials/relationship-api.md)
   * [アドホックスキーマの作成（API）](tutorials/ad-hoc.md)
* [トラブルシューティングガイド](troubleshooting-guide.md)
* [API リファレンス](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/schema-registry.yaml)
* [プラットフォームのリリースノート](https://docs.adobe.com/content/help/ja-JP/experience-platform/release-notes/latest.html)