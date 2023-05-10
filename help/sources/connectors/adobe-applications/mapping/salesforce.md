---
keywords: Experience Platform;ホーム;人気のトピック;Salesforce;Salesforce;フィールドマッピング;フィールドマッピング;マッピング;marketo;B2B;b2b
title: Salesforce マッピングフィールド
description: 次の表には、Salesforce ソースフィールドと、対応する XDM フィールドとのマッピングが含まれています。
exl-id: 33ee76f2-0495-4acd-a862-c942c0fa3177
source-git-commit: 5e93a86d6bdbf66e6b4991e0e2bc4d3dfe90d2b5
workflow-type: tm+mt
source-wordcount: '479'
ht-degree: 100%

---

# [!DNL Salesforce] フィールドマッピング

次の表には、[!DNL Salesforce] ソースフィールドと、対応するエクスペリエンスデータモデル（XDM）フィールドとのマッピングが含まれています。

## 連絡先 {#contact}

XDM クラスについて詳しくは、[XDM 個人プロファイルの概要](../../../../xdm/classes/individual-profile.md)を参照してください。 XDM フィールドグループについて詳しくは、[XDM ビジネスユーザー詳細スキーマフィールドグループ](../../../../xdm/field-groups/profile/business-person-details.md)ガイドと [XDM ビジネスユーザーコンポーネントスキーマフィールドグループ](../../../../xdm/field-groups/profile/business-person-components.md)ガイドを参照してください。

| ソースフィールド | ターゲット XDM フィールドのパス | メモ |
| --- | --- | --- |
| `AccountId` | `b2b.accountKey.sourceID` |
| `iif(AccountId != null && AccountId != "", to_object("sourceType", "Salesforce", "sourceInstanceID", "${CRM_ORG_ID}", "sourceKey", concat(AccountId,"@${CRM_ORG_ID}.Salesforce")), null)` | `b2b.accountKey` |
| `iif(AccountId != null && AccountId != "", to_object("sourceType", "Salesforce", "sourceInstanceID", "${CRM_ORG_ID}","sourceID", AccountId, "sourceKey", concat(AccountId,"@${CRM_ORG_ID}.Salesforce")), null)` | `personComponents.sourceAccountKey` |
| `"Salesforce"` | `b2b.personKey.sourceType` |
| `"${CRM_ORG_ID}"` | `b2b.personKey.sourceInstanceID` | `"${CRM_ORG_ID}"` の値は自動的に置き換えられます。 |
| `concat(Id,"@${CRM_ORG_ID}.Salesforce")` | `b2b.personKey.sourceKey` | プライマリ ID。`"${CRM_ORG_ID}"` の値は自動的に置き換えられます。 |
| `AssistantName` | `extendedWorkDetails.assistantDetails.name.fullName` |
| `AssistantPhone` | `extendedWorkDetails.assistantDetails.phone.number` |
| `Birthdate` | `person.birthDate` |
| `CreatedDate` | `extSourceSystemAudit.createdDate` |
| `Department` | `extendedWorkDetails.departments` |
| `Email` | `workEmail.address` | セカンダリ ID。 |
| `Email` | `personComponents.workEmail.address` |
| `Fax` | `faxPhone.number` |
| `FirstName` | `person.name.firstName` |
| `HomePhone` | `homePhone.number` |
| `isDeleted` | `isDeleted` |
| `Id` | `b2b.personKey.sourceID` |
| `"Salesforce"` | `personComponents.sourcePersonKey.sourceType` |
| `"${CRM_ORG_ID}"` | `personComponents.sourcePersonKey.sourceInstanceID` |
| `Id` | `personComponents.sourcePersonKey.sourceID` |
| `concat(Id,"@${CRM_ORG_ID}.Salesforce")` | `personComponents.sourcePersonKey.sourceKey` |
| `LastActivityDate` | `extSourceSystemAudit.lastActivityDate` |
| `LastModifiedDate` | `extSourceSystemAudit.lastUpdatedDate` |
| `LastName` | `person.name.lastName` |
| `LastReferencedDate` | `extSourceSystemAudit.lastReferencedDate` |
| `LastViewedDate` | `extSourceSystemAudit.lastViewedDate` |
| `LeadSource` | `b2b.personSource` |
| `LeadSource` | `personComponents.personSource` |
| `MailingCity` | `workAddress.city` |
| `MailingCountry` | `workAddress.country` |
| `MailingLatitude` | `workAddress._schema.latitude` |
| `MailingLongitude` | `workAddress._schema.longitude` |
| `MailingPostalCode` | `workAddress.postalCode` |
| `MailingState` | `workAddress.state` |
| `MailingStreet` | `workAddress.street1` |
| `MobilePhone` | `mobilePhone.number` |
| `Name` | `person.name.fullName` |
| `OtherCity` | `otherAddress.city` |
| `OtherCountry` | `otherAddress.country` |
| `OtherLatitude` | `otherAddress._schema.latitude` |
| `OtherLongitude` | `otherAddress._schema.longitude` |
| `OtherPhone` | `otherPhone.number` |
| `OtherPostalCode` | `otherAddress.postalCode` |
| `OtherState` | `otherAddress.state` |
| `OtherStreet` | `otherAddress.street1` |
| `Phone` | `workPhone.number` |
| `ReportsToId` | `extendedWorkDetails.reportsToID` |
| `Salutation` | `person.name.courtesyTitle` |
| `Title` | `extendedWorkDetails.jobTitle` |
| `"Contact"` | `b2b.personType` |

{style="table-layout:auto"}

## リード {#lead}

XDM クラスについて詳しくは、[XDM 個人プロファイルの概要](../../../../xdm/classes/individual-profile.md)を参照してください。XDM フィールドグループについて詳しくは、[XDM ビジネスユーザー詳細スキーマフィールドグループ](../../../../xdm/field-groups/profile/business-person-details.md)ガイドと [XDM ビジネスユーザーコンポーネントスキーマフィールドグループ](../../../../xdm/field-groups/profile/business-person-components.md)ガイドを参照してください。

| ソースフィールド | ターゲット XDM フィールドのパス | メモ |
| --- | --- | --- |
| `City` | `workAddress.city` |
| `ConvertedDate` | `b2b.convertedDate` |
| `Country` | `workAddress.country` |
| `Email` | `workEmail.address` | セカンダリ ID。 |
| `Email` | `personComponents.workEmail.address` |
| `Fax` | `faxPhone.number` |
| `FirstName` | `person.name.firstName` |
| `IsConverted` | `b2b.isConverted` |
| `isDeleted` | `isDeleted` |
| `"Salesforce"` | `b2b.personKey.sourceType` |
| `"${CRM_ORG_ID}"` | `b2b.personKey.sourceInstanceID` |
| `Id` | `b2b.personKey.sourceID` |
| `concat(Id,"@${CRM_ORG_ID}.Salesforce")` | `b2b.personKey.sourceKey` | プライマリ ID。`"${CRM_ORG_ID}"` の値は自動的に置き換えられます。 |
| `"Salesforce"` | `personComponents.sourcePersonKey.sourceType` |
| `"${CRM_ORG_ID}"` | `personComponents.sourcePersonKey.sourceInstanceID` |
| `Id` | `personComponents.sourcePersonKey.sourceID` |
| `concat(Id,"@${CRM_ORG_ID}.Salesforce")` | `personComponents.sourcePersonKey.sourceKey` |
| `LastActivityDate` | `extSourceSystemAudit.lastActivityDate` |
| `LastModifiedDate` | `extSourceSystemAudit.lastUpdatedDate` |
| `LastName` | `person.name.lastName` |
| `LastReferencedDate` | `extSourceSystemAudit.lastReferencedDate` |
| `LastViewedDate` | `extSourceSystemAudit.lastViewedDate` |
| `LeadSource` | `b2b.personSource` |
| `LeadSource` | `personComponents.personSource` |
| `Latitude` | `workAddress._schema.latitude` |
| `Longitude` | `workAddress._schema.longitude` |
| `Name` | `person.name.fullName` |
| `PostalCode` | `workAddress.postalCode` |
| `Salutation` | `person.name.courtesyTitle` |
| `State` | `workAddress.state` |
| `Status` | `b2b.personStatus` |
| `Status` | `personComponents.personStatus` |
| `Street` | `workAddress.street1` |
| `Title` | `extendedWorkDetails.jobTitle` |
| `Suffix` | `person.name.suffix` |
| `Company` | `b2b.companyName` |
| `Website` | `b2b.companyWebsite` |
| `ConvertedContactId` | `b2b.convertedContactKey.sourceID` |
| `iif(ConvertedContactId != null && ConvertedContactId != "", to_object("sourceType", "Salesforce", "sourceInstanceID", "${CRM_ORG_ID}", "sourceKey", concat(ConvertedContactId,"@${CRM_ORG_ID}.Salesforce")), null)` | `b2b.convertedContactKey` |
| `CreatedDate` | `extSourceSystemAudit.createdDate` |
| `"Lead"` | `b2b.personType` |
| `iif(ConvertedContactId != null && ConvertedContactId != "", to_object("sourceType", "Salesforce", "sourceInstanceID", "${CRM_ORG_ID}", "sourceID", ConvertedContactId, "sourceKey", concat(ConvertedContactId,"@${CRM_ORG_ID}.Salesforce")), null)` | `personComponents.sourceConvertedContactKey` |

{style="table-layout:auto"}

## アカウント {#account}

XDM クラスについて詳しくは、[XDM ビジネスアカウントの詳細の概要](../../../../xdm/classes/b2b/business-account.md)を参照してください。

| ソースフィールド | ターゲット XDM フィールドのパス | メモ |
| --- | --- | --- |
| `"Salesforce"` | `accountKey.sourceType` |
| `"${CRM_ORG_ID}"` | `accountKey.sourceInstanceID` | `"${CRM_ORG_ID}"` の値は自動的に置き換えられます。 |
| `AccountNumber` | `accountNumber` |
| `AccountSource` | `accountSourceType` |
| `AnnualRevenue` | `accountOrganization.annualRevenue.amount` |
| `BillingCity` | `accountBillingAddress.city` |
| `BillingCountry` | `accountBillingAddress.country` |
| `BillingLatitude` | `accountBillingAddress._schema.latitude` |
| `BillingLongitude` | `accountBillingAddress._schema.longitude` |
| `BillingPostalCode` | `accountBillingAddress.postalCode` |
| `BillingState` | `accountBillingAddress.state` |
| `BillingStreet` | `accountBillingAddress.street1` |
| `CreatedDate` | `extSourceSystemAudit.createdDate` |
| `Description` | `accountDescription` |
| `DunsNumber` | `accountOrganization.DUNSNumber` | data.com の機能 |
| `Fax` | `accountFax.number` |
| `isDeleted` | `isDeleted` |
| `Id` | `accountKey.sourceID` |
| `concat(Id,"@${CRM_ORG_ID}.Salesforce")` | `accountKey.sourceKey` | プライマリ ID。`"${CRM_ORG_ID}"` の値は自動的に置き換えられます。 |
| `Industry` | `accountOrganization.industry` |
| `Jigsaw` | `accountOrganization.jigsaw` |
| `LastActivityDate` | `extSourceSystemAudit.lastActivityDate` |
| `LastModifiedDate` | `extSourceSystemAudit.lastUpdatedDate` |
| `LastReferencedDate` | `extSourceSystemAudit.lastReferencedDate` |
| `LastViewedDate` | `extSourceSystemAudit.lastViewedDate` |
| `NaicsCode` | `accountOrganization.NAICSCode` | data.com の機能 |
| `NaicsDesc` | `accountOrganization.NAICSDescription` | data.com の機能 |
| `Name` | `accountName` |
| `NumberOfEmployees` | `accountOrganization.numberOfEmployees` |
| `Ownership` | `accountOwnership` |
| `ParentId` | `accountParentKey.sourceID` |
| `iif(ParentId != null && ParentId != "", to_object("sourceType", "Salesforce", "sourceInstanceID", "${CRM_ORG_ID}", "sourceKey", concat(ParentId,"@${CRM_ORG_ID}.Salesforce")), null)` | `accountParentKey` |
| `Phone` | `accountPhone.number` |
| `ShippingCity` | `accountShippingAddress.city` |
| `ShippingCountry` | `accountShippingAddress.country` |
| `ShippingLatitude` | `accountShippingAddress._schema.latitude` |
| `ShippingLongitude` | `accountShippingAddress._schema.longitude` |
| `ShippingPostalCode` | `accountShippingAddress.postalCode` |
| `ShippingState` | `accountShippingAddress.state` |
| `ShippingStreet` | `accountShippingAddress.street1` |
| `Sic` | `accountOrganization.SICCode` |
| `SicDesc` | `accountOrganization.SICDescription` |
| `Site` | `accountSite` |
| `TickerSymbol` | `accountOrganization.tickerSymbol` |
| `Tradestyle` | `accountTradeStyle` | data.com の機能 |
| `Type` | `accountType` |
| `Website` | `accountOrganization.website` |

{style="table-layout:auto"}

## Opportunity {#opportunity}

XDM クラスについて詳しくは、[XDM Business Opportunity の概要](../../../../xdm/classes/b2b/business-opportunity.md)を参照してください。

| ソースフィールド | ターゲット XDM フィールドのパス | メモ |
| --- | --- | --- |
| `"Salesforce"` | `opportunityKey.sourceType` |
| `"${CRM_ORG_ID}"` | `opportunityKey.sourceInstanceID` | `"${CRM_ORG_ID}"` の値は自動的に置き換えられます。 |
| `concat(Id,"@${CRM_ORG_ID}.Salesforce")` | `opportunityKey.sourceKey` | プライマリ ID。`"${CRM_ORG_ID}"` の値は自動的に置き換えられます。 |
| `AccountId` | `accountKey.sourceID` |
| `iif(AccountId != null && AccountId != "", to_object("sourceType", "Salesforce", "sourceInstanceID", "${CRM_ORG_ID}", "sourceKey", concat(AccountId,"@${CRM_ORG_ID}.Salesforce")), null)` | `accountKey` | 関係。 |
| `Amount` | `opportunityAmount.amount` |
| `CampaignId` | `campaignKey.sourceID` |
| `iif(CampaignId != null && CampaignId != "", to_object("sourceType", "Salesforce", "sourceInstanceID", "${CRM_ORG_ID}", "sourceKey", concat(CampaignId,"@${CRM_ORG_ID}.Salesforce")), null)` | `campaignKey` |
| `CloseDate` | `expectedCloseDate` |
| `CreatedDate` | `extSourceSystemAudit.createdDate` |
| `Description` | `opportunityDescription` |
| `ExpectedRevenue` | `expectedRevenue.amount` |
| `FiscalQuarter` | `fiscalQuarter` |
| `FiscalYear` | `fiscalYear` |
| `ForecastCategory` | `forecastCategory` |
| `ForecastCategoryName` | `forecastCategoryName` |
| `Id` | `opportunityKey.sourceID` |
| `IsClosed` | `isClosed` |
| `isDeleted` | `isDeleted` |
| `IsWon` | `isWon` |
| `LastActivityDate` | `extSourceSystemAudit.lastActivityDate` |
| `LastModifiedDate` | `extSourceSystemAudit.lastUpdatedDate` |
| `LastReferencedDate` | `extSourceSystemAudit.lastReferencedDate` |
| `LastViewedDate` | `extSourceSystemAudit.lastViewedDate` |
| `LeadSource` | `leadSource` |
| `Name` | `opportunityName` |
| `NextStep` | `nextStep` |
| `Probability` | `probabilityPercentage` |
| `StageName` | `opportunityStage` |
| `TotalOpportunityQuantity` | `opportunityQuantity` |
| `Type` | `opportunityType` |
| `CurrencyIsoCode` | `opportunityAmount.currencyCode` |

{style="table-layout:auto"}

## 商談連絡先の役割 {#opportunity-contact-role}

XDM クラスについて詳しくは、[XDM Business Opportunity ユーザー関係クラスの概要](../../../../xdm/classes/b2b/business-opportunity-person-relation.md)を参照してください。

| ソースフィールド | ターゲット XDM フィールドのパス | メモ |
| --- | --- | --- |
| `"Salesforce"` | `opportunityPersonKey.sourceType` |
| `"${CRM_ORG_ID}"` | `opportunityPersonKey.sourceInstanceID` | `"${CRM_ORG_ID}"` の値は自動的に置き換えられます。 |
| `"Salesforce"` | `personKey.sourceType` |
| `"${CRM_ORG_ID}"` | `personKey.sourceInstanceID` |
| `ContactId` | `personKey.sourceID` |
| `concat(ContactId,"@${CRM_ORG_ID}.Salesforce")` | `personKey.sourceKey` |
| `CreatedDate` | `extSourceSystemAudit.createdDate` |
| `Id` | `opportunityPersonKey.sourceID` |
| `concat(Id,"@${CRM_ORG_ID}.Salesforce")` | `opportunityPersonKey.sourceKey` | プライマリ ID。`"${CRM_ORG_ID}"` の値は自動的に置き換えられます。 |
| `isDeleted` | `isDeleted` |
| `IsPrimary` | `isPrimary` |
| `LastModifiedDate` | `extSourceSystemAudit.lastUpdatedDate` |
| `"Salesforce"` | `opportunityKey.sourceType` |
| `"${CRM_ORG_ID}"` | `opportunityKey.sourceInstanceID` |
| `OpportunityId` | `opportunityKey.sourceID` |
| `concat(OpportunityId,"@${CRM_ORG_ID}.Salesforce")` | `opportunityKey.sourceKey` |
| `Role` | `personRole` |

{style="table-layout:auto"}

## Campaign {#campaign}

XDM クラスについて詳しくは、[XDM ビジネスキャンペーンクラスの概要](../../../../xdm/classes/b2b/business-campaign.md)を参照してください。 XDM フィールドグループについて詳しくは、[XDM ビジネスキャンペーン詳細スキーマフィールドグループ](../../../../xdm/field-groups/b2b-campaign/details.md)ガイドを参照してください。

| ソースフィールド | ターゲット XDM フィールドのパス | メモ |
| --- | --- | --- |
| `"Salesforce"` | `campaignKey.sourceType` |
| `"${CRM_ORG_ID}"` | `campaignKey.sourceInstanceID` | `"${CRM_ORG_ID}"` の値は自動的に置き換えられます。 |
| `isDeleted` | `isDeleted` |
| `Id` | `campaignKey.sourceID` |
| `concat(Id,"@${CRM_ORG_ID}.Salesforce")` | `campaignKey.sourceKey` | プライマリ ID。`"${CRM_ORG_ID}"` の値は自動的に置き換えられます。 |
| `Name` | `campaignName` |
| `ParentId` | `parentCampaignKey.sourceID` |
| `iif(ParentId != null && ParentId != "", to_object("sourceType", "Salesforce", "sourceInstanceID", "${CRM_ORG_ID}", "sourceKey", concat(ParentId,"@${CRM_ORG_ID}.Salesforce")), null)` | `parentCampaignKey` |
| `Type` | `campaignType` |
| `Status` | `campaignStatus` |
| `StartDate` | `campaignStartDate` |
| `EndDate` | `campaignEndDate` |
| `ExpectedRevenue` | `expectedRevenue.amount` |
| `BudgetedCost` | `budgetedCost.amount` |
| `ActualCost` | `actualCost.amount` |
| `ExpectedResponse` | `expectedResponse` |
| `IsActive` | `isActive` |
| `Description` | `campaignDescription` |
| `CreatedDate` | `extSourceSystemAudit.createdDate` |
| `LastModifiedDate` | `extSourceSystemAudit.lastUpdatedDate` |
| `LastActivityDate` | `extSourceSystemAudit.lastActivityDate` |
| `LastViewedDate` | `extSourceSystemAudit.lastViewedDate` |
| `LastReferencedDate` | `extSourceSystemAudit.lastReferencedDate` |
| `CurrencyIsoCode` | `actualCost.currencyCode` |

## キャンペーンメンバー {#campaign-member}

XDM クラスについて詳しくは、[XDM ビジネスキャンペーンメンバーの概要](../../../../xdm/classes/b2b/business-campaign-members.md)を参照してください。XDM フィールドグループについて詳しくは、[XDM ビジネスキャンペーンメンバー詳細スキーマフィールドグループ](../../../../xdm/field-groups/b2b-campaign/details.md)ドキュメントを参照してください。

| ソースフィールド | ターゲット XDM フィールドのパス | メモ |
| --- | --- | --- |
| `"Salesforce"` | `campaignMemberKey.sourceType` |
| `"${CRM_ORG_ID}"` | `campaignMemberKey.sourceInstanceID` | `"${CRM_ORG_ID}"` の値は自動的に置き換えられます。 |
| `isDeleted` | `isDeleted` |
| `Id` | `campaignMemberKey.sourceID` |
| `concat(Id,"@${CRM_ORG_ID}.Salesforce")` | `campaignMemberKey.sourceKey` | プライマリ ID。`"${CRM_ORG_ID}"` の値は自動的に置き換えられます。 |
| `"Salesforce"` | `campaignKey.sourceType` |
| `${CRM_ORG_ID}` | `campaignKey.sourceInstanceID` |
| `CampaignId` | `campaignKey.sourceID` |
| `concat(CampaignId,"@${CRM_ORG_ID}.Salesforce")` | `campaignKey.sourceKey` |
| `"Salesforce"` | `personKey.sourceType` |
| `${CRM_ORG_ID}` | `personKey.sourceInstanceID` |
| `LeadOrContactId` | `personKey.sourceID` |
| `concat(LeadOrContactId,"@${CRM_ORG_ID}.Salesforce")` | `personKey.sourceKey` |
| `Status` | `memberStatus` |
| `HasResponded` | `hasResponded` |
| `CreatedDate` | `extSourceSystemAudit.createdDate` |
| `LastModifiedDate` | `extSourceSystemAudit.lastUpdatedDate` |
| `FirstRespondedDate` | `firstRespondedDate` |
| `Type` | `b2b.personType` |

## アカウント連絡先の関係 {#account-contact-relation}

XDM クラスについて詳しくは、[XDM ビジネスアカウントユーザー関係クラス](../../../../xdm/classes/b2b/business-account-person-relation.md)を参照してください。

| ソースフィールド | ターゲット XDM フィールドのパス | メモ |
| --- | --- | --- |
| `AccountId` | `accountKey.sourceID` |
| `iif(AccountId != null && AccountId != "", to_object("sourceType", "Salesforce", "sourceInstanceID", "${CRM_ORG_ID}", "sourceKey", concat(AccountId,"@${CRM_ORG_ID}.Salesforce")), null)` | `accountKey` |
| `ContactId` | `personKey.sourceID` |
| `iif(ContactId != null && ContactId != "", to_object("sourceType", "Salesforce", "sourceInstanceID", "${CRM_ORG_ID}", "sourceKey", concat(ContactId,"@${CRM_ORG_ID}.Salesforce")), null)` | `personKey` |
| `CreatedById` | `extSourceSystemAudit.createdBy` |
| `CreatedDate` | `extSourceSystemAudit.createdDate` |
| `EndDate` | `relationEndDate` |
| `IsDeleted` | `isDeleted` |
| `Id` | `accountPersonKey.sourceID` |
| `"Salesforce"` | `accountPersonKey.sourceType` |
| `"${CRM_ORG_ID}"` | `accountPersonKey.sourceInstanceID` |
| `concat(Id, "@${CRM_ORG_ID}.Salesforce")` | `accountPersonKey.sourceKey` | プライマリ ID。 |
| `IsActive` | `IsActive` |
| `IsDirect` | `IsDirect` |
| `LastModifiedById` | `extSourceSystemAudit.lastUpdatedBy` |
| `LastModifiedDate` | `extSourceSystemAudit.lastUpdatedDate` |
| `explode(Roles,";")` | `personRoles[]` |
| `StartDate` | `relationStartDate` |

## 次の手順

このドキュメントでは、[!DNL Salesforce] ソースフィールドとそれに対応する XDM フィールドとのマッピング関係について説明しました。詳しくは、[ [!DNL Salesforce] ソース接続の作成](../../../connectors/crm/salesforce.md)に関するドキュメントを参照してください。
