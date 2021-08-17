---
keywords: Experience Platform；ホーム；人気のあるトピック；Marketo Engage;marketo engage;Marketo；マッピング
solution: Experience Platform
title: Marketo Engage・ソースのフィールドのマッピング
topic-legacy: overview
description: 次の表に、Marketoデータセットのフィールドと、対応するXDMフィールドのマッピングを示します。
exl-id: 2b217bba-2748-4d6f-85ac-5f64d5e99d49
source-git-commit: 0af9290a3143b85311fbbd8d194f4799b0c9a873
workflow-type: tm+mt
source-wordcount: '333'
ht-degree: 13%

---

# （ベータ版） [!DNL Marketo Engage]フィールドマッピング

>[!IMPORTANT]
>
>[!DNL Marketo Engage]ソースは現在ベータ版です。 機能とドキュメントは変更される場合があります。

次のテーブルには、9つの[!DNL Marketo]データセット内のフィールドと、対応するExperience Data Model(XDM)フィールド間のマッピングが含まれています。

## アクティビティ {#activities}

| ソースデータセット | XDMターゲットフィールド | 備考 |
| -------------- | ---------------- | ----- |
| `_id` | `_id` |
| `personID` | `personID` | プライマリID |
| `eventType` | `eventType` |
| `timestamp` | `timestamp` |
| `web.webPageDetails._marketo.URL` | `web.webPageDetails._marketo.URL` |
| `environment.browserDetails.userAgent` | `environment.browserDetails.userAgent` |
| `environment.ipV4` | `environment.ipV4` |
| `search.keywords` | `search.keywords` |
| `search.searchEngine` | `search.searchEngine` |
| `web.webPageDetails.webPageID` | `web.webPageDetails.webPageID` |
| `web.webPageDetails.name` | `web.webPageDetails.name` |
| `web.webPageDetails.isPersonalizedURL` | `web.webPageDetails.isPersonalizedURL` |
| `web.webPageDetails.queryParameters` | `web.webPageDetails.queryParameters` |
| `web.webReferrer.URL` | `web.webReferrer.URL` |
| `listOperations.listID` | `listOperations.listID` |
| `opportunityEvent.isPrimary` | `opportunityEvent.isPrimary` |
| `opportunityEvent.opportunityID` | `opportunityEvent.opportunityID` |
| `opportunityEvent.role` | `opportunityEvent.role` |
| `leadOperation.newLead.createdDate` | `leadOperation.newLead.createdDate` |
| `leadOperation.newLead.formName` | `leadOperation.newLead.formName` |
| `leadOperation.newLead.leadSource` | `leadOperation.newLead.leadSource` |
| `leadOperation.newLead.listName` | `leadOperation.newLead.listName` |
| `leadOperation.newLead.sfdcType` | `leadOperation.newLead.sfdcType` |
| `leadOperation.newLead.sourceType` | `leadOperation.newLead.sourceType` |
| `leadOperation.convertLead.assignTo` | `leadOperation.convertLead.assignTo` |
| `leadOperation.convertLead.convertedStatus` | `leadOperation.convertLead.convertedStatus` |
| `leadOperation.convertLead.isSentNotificationEmail` | `leadOperation.convertLead.isSentNotificationEmail` |
| `directMarketing.mailingID` | `directMarketing.mailingID` |
| `directMarketing.mailingName` | `directMarketing.mailingName` |
| `directMarketing.testVariantID` | `directMarketing.testVariantID` |
| `directMarketing.emailBouncedCode` | `directMarketing.emailBouncedCode` |
| `directMarketing.emailBouncedDetails` | `directMarketing.emailBouncedDetails` |
| `directMarketing.email` | `directMarketing.email` |
| `device.isMobileDevice` | `device.isMobileDevice` |
| `device.model` | `device.model` |
| `environment.operatingSystem` | `environment.operatingSystem` |
| `directMarketing.linkURL` | `directMarketing.linkURL` |
| `web.webInteraction.linkID` | `web.webInteraction.linkID` |
| `web.fillOutForm.webFormID` | `web.fillOutForm.webFormID` |
| `web.fillOutForm.webFormName` | `web.fillOutForm.webFormName` |
| `web.webInteraction.linkURL` | `web.webInteraction.linkURL` |
| `leadOperation.changeScore.changeValue` | `leadOperation.changeScore.changeValue` |
| `leadOperation.changeScore.newValue` | `leadOperation.changeScore.newValue` |
| `leadOperation.changeScore.oldValue` | `leadOperation.changeScore.oldValue` |
| `leadOperation.changeScore.priority` | `leadOperation.changeScore.priority` |
| `leadOperation.changeScore.reason` | `leadOperation.changeScore.reason` |
| `leadOperation.changeScore.relativeScore` | `leadOperation.changeScore.relativeScore` |
| `leadOperation.changeScore.relativeUrgency` | `leadOperation.changeScore.relativeUrgency` |
| `leadOperation.changeScore.scoreAttributeID` | `leadOperation.changeScore.scoreAttributeID` |
| `leadOperation.changeScore.scoreAttributeName` | `leadOperation.changeScore.scoreAttributeName` |
| `leadOperation.changeScore.urgency` | `leadOperation.changeScore.urgency` |
| `opportunityEvent.dataValueChanges.attributeName` | `opportunityEvent.dataValueChanges.attributeName` |
| `opportunityEvent.dataValueChanges.newValue` | `opportunityEvent.dataValueChanges.newValue` |
| `opportunityEvent.dataValueChanges.oldValue` | `opportunityEvent.dataValueChanges.oldValue` |
| `opportunityEvent.opportunityID` | `opportunityEvent.opportunityID` |
| `leadOperation.campaignProgression.campaignID` | `leadOperation.campaignProgression.campaignID` |
| `leadOperation.campaignProgression.isAcquiredBy` | `leadOperation.campaignProgression.isAcquiredBy` |
| `leadOperation.campaignProgression.isSuccessful` | `leadOperation.campaignProgression.isSuccessful` |
| `leadOperation.campaignProgression.newStatusID` | `leadOperation.campaignProgression.newStatusID` |
| `leadOperation.campaignProgression.newStatusName` | `leadOperation.campaignProgression.newStatusName` |
| `leadOperation.campaignProgression.oldStatusID` | `leadOperation.campaignProgression.oldStatusID` |
| `leadOperation.campaignProgression.oldStatusName` | `leadOperation.campaignProgression.oldStatusName` |
| `leadOperation.campaignProgression.reason` | `leadOperation.campaignProgression.reason` |

{style=&quot;table-layout:auto&quot;}

## プログラム {#programs}

| ソースデータセット | XDMターゲットフィールド | 備考 |
| -------------- | ---------------- | ----- |
| `id` | `campaignID` | プライマリID |
| `sfdcId` | `extSourceSystemAudit.externalID` | セカンダリID |
| `name` | `campaignName` |
| `description` | `campaignDescription` |
| `type` | `campaignType` |
| `status` | `campaignStatus` |
| `channel` | `channelName` |
| `createdAt` | `extSourceSystemAudit.createdDate` |
| `updatedAt` | `extSourceSystemAudit.lastUpdatedDate` |
| `cost` | `actualCost.amount` |
| `parentProgramId` | `parentCampaignID` |
| `integrationPartner` | `integrationPartnerName` |
| `startDate` | `campaignStartDate` |
| `endDate` | `campaignEndDate` |

{style=&quot;table-layout:auto&quot;}

## プログラムメンバーシップ {#program-memberships}

| ソースデータセット | XDMターゲットフィールド | 備考 |
| -------------- | ---------------- | ----- |
| `id` | `campaignMemberID` | プライマリID |
| `programId` | `campaignID` | 関係 |
| `leadId` | `personID` | 関係 |
| `acquiredByCampaignID` | `acquiredByCampaignID` |
| `reachedSuccess` | `hasReachedSuccess` |
| `isExhausted` | `isExhausted` |
| `statusName` | `memberStatus` |
| `statusReason` | `memberStatusReason` |
| `membershipDate` | `membershipDate` |
| `nurtureCadence` | `nurtureCadence` |
| `trackName` | `nurtureTrackName` |
| `webinarUrl` | `webinarConfirmationUrl` |
| `registrationCode` | `webinarRegistrationID` |
| `reachedSuccessDate` | `reachedSuccessDate` |
| `sfdc.crmId` | `extSourceSystemAudit.externalID` |
| `sfdc.lastStatus` | `lastStatus` |
| `sfdc.hasResponded` | `hasResponded` |
| `sfdc.firstRespondedDate` | `firstRespondedDate` |
| `createdAt` | `extSourceSystemAudit.createdDate` |
| `updatedAt` | `extSourceSystemAudit.lastUpdatedDate` |

{style=&quot;table-layout:auto&quot;}

## 会社 {#companies}

| ソースデータセット | XDMターゲットフィールド | 備考 |
| -------------- | ---------------- | ----- |
| `id` | `accountID` | プライマリID |
| `mktoCdpExternalId` | `extSourceSystemAudit.externalID` | セカンダリID |
| `createdAt` | `extSourceSystemAudit.createdDate` |
| `updatedAt` | `extSourceSystemAudit.lastUpdatedDate` |
| `billingCity` | `accountBillingAddress.city` |
| `billingCountry` | `accountBillingAddress.country` |
| `billingPostalCode` | `accountBillingAddress.postalCode` |
| `billingState` | `accountBillingAddress.state` |
| `billingStreet` | `accountBillingAddress.street1` |
| `annualRevenue` | `accountOrganization.annualRevenue.amount` |
| `sicCode` | `accountOrganization.SICCode` |
| `industry` | `accountOrganization.industry` |
| `numberOfEmployees` | `accountOrganization.numberOfEmployees` |
| `website` | `accountOrganization.website` |
| `mainPhone` | `accountPhone.number` |
| `company` | `accountName` |
| `companyNotes` | `accountDescription` |
| `site` | `accountSite` |
| `mktoCdpParentOrgId` | `accountParentID` |

{style=&quot;table-layout:auto&quot;}

## 静的リスト {#static-lists}

| ソースデータセット | XDMターゲットフィールド | 備考 |
| -------------- | ---------------- | ----- |
| `id` | `marketingListID` | プライマリID |
| `name` | `marketingListName` |
| `description` | `marketingListDescription` |
| `createdAt` | `extSourceSystemAudit.createdDate` |
| `updatedAt` | `extSourceSystemAudit.lastUpdatedDate` |

{style=&quot;table-layout:auto&quot;}

## 静的リストのメンバーシップ {#static-list-memnberships}

| ソースデータセット | XDMターゲットフィールド | 備考 |
| -------------- | ---------------- | ----- |
| `staticListMemberID` | `marketingListMemberID` | プライマリID |
| `staticListID` | `marketingListID` | 関係 |
| `personID` | `personID` | 関係 |
| `createdAt` | `extSourceSystemAudit.createdDate` |

{style=&quot;table-layout:auto&quot;}

## 名前付きアカウント {#named-accounts}

>[!IMPORTANT]
>
>名前付きアカウントデータセットは、Marketoのアカウントベースマーケティング(ABM)機能でのみ必要です。 ABMを使用しない場合は、名前付きアカウントのマッピングを設定する必要はありません。

| ソースデータセット | XDMターゲットフィールド | 備考 |
| -------------- | ---------------- | ----- |
| `id` | `accountID` | プライマリID |
| `crmGuid` | `extSourceSystemAudit.externalID` | セカンダリID |
| `createdAt` | `extSourceSystemAudit.createdDate` |
| `updatedAt` | `extSourceSystemAudit.lastUpdatedDate` |
| `city` | `accountBillingAddress.city` |
| `country` | `accountBillingAddress.country` |
| `state` | `accountBillingAddress.state` |
| `annualRevenue` | `accountOrganization.annualRevenue.amount` |
| `sicCode` | `accountOrganization.SICCode` |
| `industry` | `accountOrganization.industry` |
| `logoUrl` | `accountOrganization.logoUrl` |
| `numberOfEmployees` | `accountOrganization.numberOfEmployees` |
| `name` | `accountName` |
| `parentAccountId` | `accountParentID` |
| `sourceType` | `accountSourceType` |

{style=&quot;table-layout:auto&quot;}

## 機会 {#opportunities}

| ソースデータセット | XDMターゲットフィールド | 備考 |
| -------------- | ---------------- | ----- |
| `id` | `opportunityID` | プライマリID |
| `externalOpportunityId` | `extSourceSystemAudit.externalID` | セカンダリID |
| `mktoCdpAccountOrgId` | `accountID` | 関係 |
| `description` | `opportunityDescription` |
| `name` | `opportunityName` |
| `stage` | `opportunityStage` |
| `type` | `opportunityType` |
| `createdAt` | `extSourceSystemAudit.createdDate` |
| `updatedAt` | `extSourceSystemAudit.lastUpdatedDate` |
| `expectedRevenue` | `expectedRevenue.amount` |
| `amount` | `opportunityAmount.amount` |
| `closeDate` | `expectedCloseDate` |
| `fiscalQuarter` | `fiscalQuarter` |
| `fiscalYear` | `fiscalYear` |
| `forecastCategory` | `forecastCategory` |
| `forecastCategoryName` | `forecastCategoryName` |
| `isClosed` | `isClosed` |
| `isWon` | `isWon` |
| `quantity` | `opportunityQuantity` |
| `probability` | `probabilityPercentage` |
| `mktoCdpSourceCampaignId` | `campaignID` | Salesforce統合を使用する場合にのみ推奨されます。 |
| `lastActivityDate` | `lastActivityDate` |
| `leadSource` | `leadSource` |
| `nextStep` | `nextStep` |

{style=&quot;table-layout:auto&quot;}

## 営業案件の連絡先の役割 {#opportunity-contact-roles}

| ソースデータセット | XDMターゲットフィールド | 備考 |
| -------------- | ---------------- | ----- |
| `id` | `opportunityPersonID` | プライマリID |
| `mktoCdpSfdcId` | `extSourceSystemAudit.externalID` | セカンダリID |
| `mktoCdpOpptyId` | `opportunityID` | 関係 |
| `leadId` | `personID` | 関係 |
| `role` | `personRole` |
| `isPrimary` | `isPrimary` |
| `createdAt` | `extSourceSystemAudit.createdDate` |
| `updatedAt` | `extSourceSystemAudit.lastUpdatedDate` |

{style=&quot;table-layout:auto&quot;}

## 人 {#persons}

| ソースデータセット | XDMターゲットフィールド | 備考 |
| -------------- | ---------------- | ----- |
| `id` | `personID` | プライマリID |
| `contactCompany` | `b2b.accountID` |
| `marketingSuspended` | `b2b.isMarketingSuspended` |
| `marketingSuspendedCause` | `b2b.marketingSuspendedCause` |
| `leadScore` | `b2b.personScore` |
| `leadSource` | `b2b.personSource` |
| `leadStatus` | `b2b.personStatus` |
| `personType` | `b2b.personType` |
| `leadPartitionId` | `b2b.personGroupID` |
| `mktoCdpCnvContactPersonId` | `b2b.convertedContactID` |
| `mktoCdpIsConverted` | `b2b.isConverted` |
| `mktoCdpConvertedDate` | `b2b.convertedDate` |
| `sfdcLeadId` | `extSourceSystemAudit.externalID` | セカンダリ |
| `createdAt` | `extSourceSystemAudit.createdDate` |
| `updatedAt` | `extSourceSystemAudit.lastUpdatedDate` |
| `title` | `extendedWorkDetails.jobTitle` |
| `fax` | `faxPhone.number` |
| `mobilePhone` | `mobilePhone.number` |
| `firstName` | `person.name.firstName` |
| `lastName` | `person.name.lastName` |
| `middleName` | `person.name.middleName` |
| `salutation` | `person.name.courtesyTitle` |
| `dateOfBirth` | `person.birthDate` |
| `city` | `workAddress.city` |
| `country` | `workAddress.country` |
| `postalCode` | `workAddress.postalCode` |
| `state` | `workAddress.state` |
| `address` | `workAddress.street1` |
| `phone` | `workPhone.number` |
| `company` | `organizations` |
| `leadScore` | `personComponents.personScore` |
| `leadSource` | `personComponents.personSource` |
| `leadStatus` | `personComponents.personStatus` |
| `personType` | `personComponents.personType` |
| `leadPartitionId` | `personComponents.personGroupID` |
| `mktoCdpCnvContactPersonId` | `personComponents.sourceConvertedContactID` |
| `contactCompany` | `personComponents.sourceAccountID` |
| `sfdcContactId` | `personComponents.sourceExternalID` | Salesforce統合を使用する場合にのみ推奨されます。 |
| `id` | `personComponents.sourcePersonID` |
| `email` | `personComponents.workEmail.address` |
| `email` | `workEmail.address` |
| `to_object('ECID',arrays_to_objects('id',explode(ecids)))` | `identityMap` |

{style=&quot;table-layout:auto&quot;}

>[!NOTE]
>
>`to_object('ECID',arrays_to_objects('id',explode(ecids)))`ソースフィールドは、Platform UIの「[!UICONTROL 計算済みフィールド]を追加」オプションを使用して追加する必要がある計算済みフィールドです。 詳しくは、[計算フィールド](../../../../data-prep/calculated-fields.md)の追加に関するチュートリアルを参照してください。

## 次の手順

このドキュメントでは、[!DNL Marketo]データセットと対応するXDMフィールドの間のマッピング関係に関する洞察を得ました。 [ [!DNL Marketo] ソース接続](../../../tutorials/ui/create/adobe-applications/marketo.md)の作成に関するチュートリアルを参照して、[!DNL Marketo]データフローを完了してください。
