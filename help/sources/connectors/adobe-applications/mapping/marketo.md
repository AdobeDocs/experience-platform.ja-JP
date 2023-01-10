---
keywords: Experience Platform;ホーム;人気のトピック;Marketo Engage;Marketo;マッピング
solution: Experience Platform
title: Marketo Engage ソースのマッピングフィールド
description: Marketo データセットのフィールドとそれに対応する XDM フィールドとのマッピングを次の表に示します。
exl-id: 2b217bba-2748-4d6f-85ac-5f64d5e99d49
source-git-commit: 59dfa862388394a68630a7136dee8e8988d0368c
workflow-type: tm+mt
source-wordcount: '952'
ht-degree: 100%

---

# [!DNL Marketo Engage] フィールドマッピング {#marketo-engage-field-mappings}

>[!CONTEXTUALHELP]
>id="platform_sources_marketo_mapping"
>title="Marketo ソースフィールドのマッピング"
>abstract="Marketo と Platform の間にソース接続を確立するには、Platform に取り込まれる前に、Marketo ソースデータフィールドを適切なターゲット XDM フィールドにマッピングする必要があります。"

9 個の [!DNL Marketo] データセットのフィールドとそれに対応するエクスペリエンスデータモデル（XDM）フィールドとのマッピングを次の表に示します。

>[!TIP]
>
>`Activities` を除くすべての [!DNL Marketo] データセットで `isDeleted` をサポートするようになりました。既存のデータフローには、`isDeleted` が自動的に含まれますが、新しく取り込んだデータのフラグのみが取り込まれます。すべての履歴データにフラグを適用する場合は、既存のデータフローを停止し、新しいマッピングで再作成する必要があります。 なお、`isDeleted` を削除すると、機能にアクセスできなくなります。自動入力後もマッピングが保たれることが重要です。

## アクティビティ {#activities}

[!DNL Marketo] ソースでは、追加の標準アクティビティをサポートするようになりました。 標準のアクティビティを使用するには、[スキーマ自動生成ユーティリティ](../marketo/marketo-namespaces.md)を使用してスキーマを更新する必要があります。スキーマを更新せずに新しい `activities` データフローを作成すると、新しいターゲットフィールドがスキーマに存在しないので、マッピングテンプレートが機能しなくなるからです。スキーマの更新を選択しない場合でも、新しいデータフローを作成しエラーを解除できます。 ただし、新しいフィールドや更新されたフィールドは、Platform には取り込まれません。

XDM クラスと XDM フィールドについて詳しくは、[XDM エクスペリエンスイベントクラス](../../../../xdm/classes/experienceevent.md)に関するドキュメントを参照してください。

| ソースデータセット | XDM ターゲットフィールド | メモ |
| -------------- | ---------------- | ----- |
| `_id` | `_id` |
| `"Marketo"` | `personKey.sourceType` |
| `"${MUNCHKIN_ID}"` | `personKey.sourceInstanceID` | `"${MUNCHKIN_ID}"` の値は自動的に置き換えられます。 |
| `personID` | `personKey.sourceID` |
| `concat(personID,"@${MUNCHKIN_ID}.Marketo")` | `personKey.sourceKey` | プライマリ ID。`"${MUNCHKIN_ID}"` の値は自動的に置き換えられます。 |
| `eventType` | `eventType` |
| `producedBy` | `producedBy` |
| `timestamp` | `timestamp` |
| `web.webPageDetails.URL` | `web.webPageDetails.URL` |
| `environment.browserDetails.userAgent` | `environment.browserDetails.userAgent` |
| `environment.ipV4` | `environment.ipV4` |
| `search.keywords` | `search.keywords` |
| `search.searchEngine` | `search.searchEngine` |
| `web.webPageDetails.webPageID` | `web.webPageDetails.webPageID` |
| `web.webPageDetails.name` | `web.webPageDetails.name` |
| `web.webPageDetails.isPersonalizedURL` | `web.webPageDetails.isPersonalizedURL` |
| `web.webPageDetails.queryParameters` | `web.webPageDetails.queryParameters` |
| `web.webReferrer.URL` | `web.webReferrer.URL` |
| `iif(${listOperations\.listID} != null && ${listOperations\.listID} != "", to_object("sourceType", "Marketo", "sourceInstanceID", "${MUNCHKIN_ID}","sourceID", ${listOperations\.listID}, "sourceKey", concat(${listOperations\.listID},"@${MUNCHKIN_ID}.Marketo")), null)` | `listOperations.listKey` |
| `opportunityEvent.isPrimary` | `opportunityEvent.isPrimary` |
| `iif(${opportunityEvent\.opportunityID} != null && ${opportunityEvent\.opportunityID} != "", to_object("sourceType", "Marketo", "sourceInstanceID", "${MUNCHKIN_ID}","sourceID",${opportunityEvent\.opportunityID}, "sourceKey", concat(${opportunityEvent\.opportunityID},"@${MUNCHKIN_ID}.Marketo")), null)` | `opportunityEvent.opportunityKey` |
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
| `directMarketing.testVariantName` | `directMarketing.testVariantName` |
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
| `json_to_object(${opportunityEvent\.dataValueChanges})` | `opportunityEvent.dataValueChanges` |
| `iif(${leadOperation\.campaignProgression\.campaignID} != null && ${leadOperation\.campaignProgression\.campaignID} != "" , to_object("sourceType", "Marketo", "sourceInstanceID", "${MUNCHKIN_ID}", "sourceID", ${leadOperation\.campaignProgression\.campaignID}, "sourceKey", concat(${leadOperation\.campaignProgression\.campaignID},"@${MUNCHKIN_ID}.Marketo")), null)` | `leadOperation.campaignProgression.campaignKey` |
| `leadOperation.campaignProgression.campaignID` | `leadOperation.campaignProgression.campaignID` |
| `leadOperation.campaignProgression.isAcquiredBy` | `leadOperation.campaignProgression.isAcquiredBy` |
| `leadOperation.campaignProgression.isSuccessful` | `leadOperation.campaignProgression.isSuccessful` |
| `leadOperation.campaignProgression.newStatusID` | `leadOperation.campaignProgression.newStatusID` |
| `leadOperation.campaignProgression.newStatusName` | `leadOperation.campaignProgression.newStatusName` |
| `leadOperation.campaignProgression.oldStatusID` | `leadOperation.campaignProgression.oldStatusID` |
| `leadOperation.campaignProgression.oldStatusName` | `leadOperation.campaignProgression.oldStatusName` |
| `leadOperation.campaignProgression.reason` | `leadOperation.campaignProgression.reason` |
| `leadOperation.interestingMoment.date` | `leadOperation.interestingMoment.date` |
| `leadOperation.interestingMoment.description` | `leadOperation.interestingMoment.description` |
| `leadOperation.interestingMoment.source` | `leadOperation.interestingMoment.source` |
| `leadOperation.interestingMoment.type` | `leadOperation.interestingMoment.type` |
| `iif(${leadOperation\\.callWebhook\\.webhookKey\\.sourceID} != null && ${leadOperation\\.callWebhook\\.webhookKey\\.sourceID} != \"\", to_object(\"sourceType\", \"Marketo\", \"sourceInstanceID\", \"${MUNCHKIN_ID}\", \"sourceID\", ${leadOperation\\.callWebhook\\.webhookKey\\.sourceID}, \"sourceKey\", concat(${leadOperation\\.callWebhook\\.webhookKey\\.sourceInstanceID},\"@${MUNCHKIN_ID}.Marketo\")), null)"` | `leadOperation.callWebhook.webhookKey` |
| `leadOperation.callWebhook.webhookName` | `leadOperation.callWebhook.webhookName` |
| `leadOperation.callWebhook.responseCode` | `leadOperation.callWebhook.responseCode` |
| `iif(${leadOperation\\.changeCampaignCadence\\.campaignKey\\.sourceID} != null && ${leadOperation\\.changeCampaignCadence\\.campaignKey\\.sourceID} != \"\", to_object(\"sourceType\", \"Marketo\", \"sourceInstanceID\", \"${MUNCHKIN_ID}\", \"sourceID\", ${leadOperation\\.changeCampaignCadence\\.campaignKey\\.sourceID}, \"sourceKey\", concat(${leadOperation\\.changeCampaignCadence\\.campaignKey\\.sourceInstanceID},\"@${MUNCHKIN_ID}.Marketo\")), null)"` | `leadOperation.changeCampaignCadence.campaignKey` |
| `leadOperation.changeCampaignCadence.newCadence` | `leadOperation.changeCampaignCadence.newCadence` |
| `leadOperation.changeCampaignCadence.previousCadence` | `leadOperation.changeCampaignCadence.previousCadence` |
| `iif(${leadOperation\.addToCampaign\.campaignID} != null && ${leadOperation\.addToCampaign\.campaignID} != "", to_object("sourceType", "Marketo", "sourceInstanceID", "${MUNCHKIN_ID}","sourceID",${leadOperation\.addToCampaign\.campaignID}, "sourceKey", concat(${leadOperation\.addToCampaign\.campaignID},"@${MUNCHKIN_ID}.Marketo")), null)` | `leadOperation.addToCampaign.campaignKey` |
| `iif(${leadOperation\.addToCampaign\.streamID} != null && ${leadOperation\.addToCampaign\.streamID} != "", to_object("sourceType", "Marketo", "sourceInstanceID", "${MUNCHKIN_ID}","sourceID",${leadOperation\.addToCampaign\.streamID}, "sourceKey", concat(${leadOperation\.addToCampaign\.streamID},"@${MUNCHKIN_ID}.Marketo")), null)` | `leadOperation.addToCampaign.streamKey` |
| `leadOperation.addToCampaign.streamName` | `leadOperation.addToCampaign.streamName` |
| `iif(${leadOperation\.changeCampaignStream\.campaignID} != null && ${leadOperation\.changeCampaignStream\.campaignID} != "", to_object("sourceType", "Marketo", "sourceInstanceID", "${MUNCHKIN_ID}","sourceID",${leadOperation\.changeCampaignStream\.campaignID}, "sourceKey", concat(${leadOperation\.changeCampaignStream\.campaignID},"@${MUNCHKIN_ID}.Marketo")), null)` | `leadOperation.changeCampaignStream.campaignKey` |
| `iif(${leadOperation\.changeCampaignStream\.newStreamID} != null && ${leadOperation\.changeCampaignStream\.newStreamID} != "", to_object("sourceType", "Marketo", "sourceInstanceID", "${MUNCHKIN_ID}","sourceID",${leadOperation\.changeCampaignStream\.newStreamID}, "sourceKey", concat(${leadOperation\.changeCampaignStream\.newStreamID},"@${MUNCHKIN_ID}.Marketo")), null)` | `leadOperation.changeCampaignStream.newStreamKey` |
| `leadOperation.changeCampaignStream.newStreamName` | `leadOperation.changeCampaignStream.newStreamName` |
| `iif(${leadOperation\.changeCampaignStream\.previousStreamID} != null && ${leadOperation\.changeCampaignStream\.previousStreamID} != "", to_object("sourceType", "Marketo", "sourceInstanceID", "${MUNCHKIN_ID}","sourceID",${leadOperation\.changeCampaignStream\.previousStreamID}, "sourceKey", concat(${leadOperation\.changeCampaignStream\.previousStreamID},"@${MUNCHKIN_ID}.Marketo")), null)` | `leadOperation.changeCampaignStream.previousStreamKey` |
| `leadOperation.changeCampaignStream.previousStreamName` | `leadOperation.changeCampaignStream.previousStreamName` |
| `iif(${leadOperation\.changeRevenueStage\.modelID} != null && ${leadOperation\.changeRevenueStage\.modelID} != "", to_object("sourceType", "Marketo", "sourceInstanceID", "${MUNCHKIN_ID}","sourceID",${leadOperation\.changeRevenueStage\.modelID}, "sourceKey", concat(${leadOperation\.changeRevenueStage\.modelID},"@${MUNCHKIN_ID}.Marketo")), null)` | `leadOperation.changeRevenueStage.modelKey` |
| `leadOperation.changeRevenueStage.modelName` | `leadOperation.changeRevenueStage.modelName` |
| `iif(${leadOperation\.changeRevenueStage\.newStageID} != null && ${leadOperation\.changeRevenueStage\.newStageID} != "", to_object("sourceType", "Marketo", "sourceInstanceID", "${MUNCHKIN_ID}","sourceID",${leadOperation\.changeRevenueStage\.newStageID}, "sourceKey", concat(${leadOperation\.changeRevenueStage\.newStageID},"@${MUNCHKIN_ID}.Marketo")), null)` | `leadOperation.changeRevenueStage.newStageKey` |
| `leadOperation.changeRevenueStage.newStageName` | `leadOperation.changeRevenueStage.newStageName` |
| `iif(${leadOperation\\.changeRevenueStage\\.previousStageID} != null && ${leadOperation\\.changeRevenueStage\\.previousStageID} != \"\", to_object(\"sourceType\", \"Marketo\", \"sourceInstanceID\", \"${MUNCHKIN_ID}\",\"sourceID\",${leadOperation\\.changeRevenueStage\\.previousStageID}, \"sourceKey\", concat(${leadOperation\\.changeRevenueStage\\.previousStageID},\"@${MUNCHKIN_ID}.Marketo\")), null)` | `leadOperation.changeRevenueStage.previousStageKey` |
| `leadOperation.changeRevenueStage.previousStageName` | `leadOperation.changeRevenueStage.previousStageName` |
| `leadOperation.changeRevenueStage.reason` | `leadOperation.changeRevenueStage.reason` |
| `iif(${leadOperation\.mergeLeads\.sourceIDs} != null && ${leadOperation\.mergeLeads\.sourceIDs} != "", to_object("sourceType", "Marketo", "sourceInstanceID", "${MUNCHKIN_ID}","sourceID",${leadOperation\.mergeLeads\.sourceIDs}, "sourceKey", concat(${leadOperation\.mergeLeads\.sourceIDs},"@${MUNCHKIN_ID}.Marketo")), null)` | `leadOperation.mergeLeads.sourceKeys` |
| `leadOperation.mergeLeads.targetUpdated` | `leadOperation.mergeLeads.targetUpdated` |
| `leadOperation.mergeLeads.mergedInCRM` | `leadOperation.mergeLeads.mergedInCRM` |
| `leadOperation.mergeLeads.mergeSource` | `leadOperation.mergeLeads.mergeSource` |
| `iif(${directMarketing\\.emailSent\\.mailingID} != null && ${directMarketing\\.emailSent\\.mailingID} != \"\", to_object(\"sourceType\", \"Marketo\", \"sourceInstanceID\", \"${MUNCHKIN_ID}\",\"sourceID\",${directMarketing\\.emailSent\\.mailingID}, \"sourceKey\", concat(${directMarketing\\.emailSent\\.mailingID},\"@${MUNCHKIN_ID}.Marketo\")), null)"` | `directMarketing.emailSent.mailingKey` |
| `directMarketing.emailSent.mailingName` | `directMarketing.emailSent.mailingName` |
| `directMarketing.emailSent.testVariantID` | `directMarketing.emailSent.testVariantID` |
| `directMarketing.emailSent.testVariantName` | `directMarketing.emailSent.testVariantName` |
| `directMarketing.emailSent.automationRunID` | `directMarketing.emailSent.automationRunID` |

{style=&quot;table-layout:auto&quot;}

## プログラム {#programs}

XDM クラスについて詳しくは、[XDM ビジネスキャンペーンの概要](../../../../xdm/classes/b2b/business-campaign.md)を参照してください。 XDM フィールドグループについて詳しくは、[ビジネスキャンペーン詳細スキーマフィールドグループ](../../../../xdm/field-groups/b2b-campaign/details.md)ガイドを参照してください。

| ソースデータセット | XDM ターゲットフィールド | メモ |
| -------------- | ---------------- | ----- |
| `"Marketo"` | `campaignKey.sourceType` |
| `"${MUNCHKIN_ID}"` | `campaignKey.sourceInstanceID` | `"${MUNCHKIN_ID}"` の値は自動的に置き換えられます。 |
| `id` | `campaignKey.sourceID` |
| `concat(id,"@${MUNCHKIN_ID}.Marketo")` | `campaignKey.sourceKey` | プライマリ ID。`"${MUNCHKIN_ID}"` の値は自動的に置き換えられます。 |
| `iif(sfdcId != null && sfdcId != "", to_object("sourceType", "${CRM_TYPE}", "sourceInstanceID", "${CRM_ORG_ID}","sourceID", sfdcId, "sourceKey", concat(sfdcId,"@${CRM_ORG_ID}.${CRM_TYPE}")), null)` | `extSourceSystemAudit.externalKey` | `extSourceSystemAudit.externalKey` はセカンダリ ID です。 `{CRM_ORG_ID}` と `{CRM_TYPE}` の値は自動的に置き換えられます。 |
| `name` | `campaignName` |
| `description` | `campaignDescription` |
| `type` | `campaignType` |
| `status` | `campaignStatus` |
| `channel` | `channelName` |
| `createdAt` | `extSourceSystemAudit.createdDate` |
| `updatedAt` | `extSourceSystemAudit.lastUpdatedDate` |
| `cost` | `actualCost.amount` |
| `iif(parentProgramId != null && parentProgramId != "", to_object("sourceType", "Marketo", "sourceInstanceID", "${MUNCHKIN_ID}","sourceID", parentProgramId, "sourceKey", concat(parentProgramId,"@${MUNCHKIN_ID}.Marketo")), null)` | `parentCampaignKey` |
| `integrationPartner` | `integrationPartnerName` |
| `webinarSessionName` | `webinarSessionName` |
| `webinarSessionDescription` | `webinarSessionDescription` |
| `webinarHistorySyncStatus` | `webinarHistorySyncStatus` |
| `webinarHistorySyncDate` | `webinarHistorySyncDate` |
| `startDate` | `campaignStartDate` |
| `endDate` | `campaignEndDate` |
| `marketoIsDeleted` | `isDeleted` |

{style=&quot;table-layout:auto&quot;}

## プログラムのメンバーシップ {#program-memberships}

XDM クラスについて詳しくは、[XDM ビジネスキャンペーンメンバーの概要](../../../../xdm/classes/b2b/business-campaign-members.md)を参照してください。 XDM フィールドグループについて詳しくは、[XDM ビジネスキャンペーンメンバー詳細スキーマフィールドグループ](../../../../xdm/field-groups/b2b-campaign-members/details.md)ガイドを参照してください。

| ソースデータセット | XDM ターゲットフィールド | メモ |
| -------------- | ---------------- | ----- |
| `"Marketo"` | `campaignMemberKey.sourceType` |
| `"${MUNCHKIN_ID}"` | `campaignMemberKey.sourceInstanceID` | `"${MUNCHKIN_ID}"` の値は自動的に置き換えられます。 |
| `id` | `campaignMemberKey.sourceID` |
| `concat(id,"@${MUNCHKIN_ID}.Marketo")` | `campaignMemberKey.sourceKey` | プライマリ ID。`"${MUNCHKIN_ID}"` の値は自動的に置き換えられます。 |
| `iif(programId != null && programId != "", to_object("sourceType", "Marketo", "sourceInstanceID", "${MUNCHKIN_ID}","sourceID", programId, "sourceKey", concat(programId,"@${MUNCHKIN_ID}.Marketo")), null)` | `campaignKey` | 関係 |
| `iif(leadId != null && leadId != "", to_object("sourceType", "Marketo", "sourceInstanceID", "${MUNCHKIN_ID}","sourceID", leadId, "sourceKey", concat(leadId,"@${MUNCHKIN_ID}.Marketo")), null)` | `personKey` | 関係 |
| `iif(acquiredByCampaignID != null && acquiredByCampaignID != "", to_object("sourceType", "Marketo", "sourceInstanceID", "${MUNCHKIN_ID}","sourceID", acquiredByCampaignID, "sourceKey", concat(acquiredByCampaignID,"@${MUNCHKIN_ID}.Marketo")), null)` | `acquiredByCampaignKey` |
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
| `iif(sfdc.crmId != null && sfdc.crmId != "", to_object("sourceType", "${CRM_TYPE}", "sourceInstanceID", "${CRM_ORG_ID}","sourceID", sfdc.crmId, "sourceKey", concat(sfdc.crmId,"@${CRM_ORG_ID}.${CRM_TYPE}")), null)` | `extSourceSystemAudit.externalKey` | `extSourceSystemAudit.externalKey` はセカンダリ ID です。 `{CRM_ORG_ID}` と `{CRM_TYPE}` の値は自動的に置き換えられます。 |
| `sfdc.lastStatus` | `lastStatus` |
| `sfdc.hasResponded` | `hasResponded` |
| `sfdc.firstRespondedDate` | `firstRespondedDate` |
| `createdAt` | `extSourceSystemAudit.createdDate` |
| `updatedAt` | `extSourceSystemAudit.lastUpdatedDate` |
| `marketoIsDeleted` | `isDeleted` |

{style=&quot;table-layout:auto&quot;}

## 会社 {#companies}

XDM クラスについて詳しくは、[XDM ビジネスアカウントの概要](../../../../xdm/classes/b2b/business-account.md)を参照してください。

| ソースデータセット | XDM ターゲットフィールド | メモ |
| -------------- | ---------------- | ----- |
| `"Marketo"` | `accountKey.sourceType` |
| `"${MUNCHKIN_ID}"` | `accountKey.sourceInstanceID` | `"${MUNCHKIN_ID}"` の値は自動的に置き換えられます。 |
| `concat(id, ".mkto_org")` | `accountKey.sourceID` |
| `concat(id, ".mkto_org@${MUNCHKIN_ID}.Marketo")` | `accountKey.sourceKey` | プライマリ ID。`"${MUNCHKIN_ID}"` の値は自動的に置き換えられます。 |
| <ul><li>`iif(mktoCdpExternalId != null && mktoCdpExternalId != "", to_object("sourceType", "${CRM_TYPE}", "sourceInstanceID", "${CRM_ORG_ID}", "sourceID", mktoCdpExternalId, "sourceKey", concat(mktoCdpExternalId,"@${CRM_ORG_ID}.${CRM_TYPE}")), null)`</li><li>`iif(msftCdpExternalId != null && msftCdpExternalId != "", to_object("sourceType", "${CRM_TYPE}", "sourceInstanceID", "${CRM_ORG_ID}","sourceID", msftCdpExternalId, "sourceKey", concat(msftCdpExternalId,"@${CRM_ORG_ID}.${CRM_TYPE}")), null)`</li></ul> | `extSourceSystemAudit.externalKey` | `extSourceSystemAudit.externalKey` はセカンダリ ID です。 `{CRM_ORG_ID}` と `{CRM_TYPE}` の値は自動的に置き換えられます。 |
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
| `iif(mktoCdpParentOrgId != null && mktoCdpParentOrgId != "", to_object("sourceType", "Marketo", "sourceInstanceID", "${MUNCHKIN_ID}","sourceID", concat(mktoCdpParentOrgId, ".mkto_org"), "sourceKey", concat(mktoCdpParentOrgId, ".mkto_org@${MUNCHKIN_ID}.Marketo")), null)` | `accountParentKey` |
| `marketoIsDeleted` | `isDeleted` |

{style=&quot;table-layout:auto&quot;}

## 静的リスト {#static-lists}

XDM クラスについて詳しくは、[XDM ビジネスマーケティングリストの概要](../../../../xdm/classes/b2b/business-marketing-list.md)を参照してください。

| ソースデータセット | XDM ターゲットフィールド | メモ |
| -------------- | ---------------- | ----- |
| `"Marketo"` | `marketingListKey.sourceType` |
| `"${MUNCHKIN_ID}"` | `marketingListKey.sourceInstanceID` | `"${MUNCHKIN_ID}"` は Explore API の一部として置き換えられます。 |
| `id` | `marketingListKey.sourceID` |
| `concat(id,"@${MUNCHKIN_ID}.Marketo")` | `marketingListKey.sourceKey` | プライマリ ID。`"${MUNCHKIN_ID}"` の値は自動的に置き換えられます。 |
| `name` | `marketingListName` |
| `description` | `marketingListDescription` |
| `createdAt` | `extSourceSystemAudit.createdDate` |
| `updatedAt` | `extSourceSystemAudit.lastUpdatedDate` |
| `marketoIsDeleted` | `isDeleted` |

{style=&quot;table-layout:auto&quot;}

## 静的リストのメンバーシップ {#static-list-memberships}

XDM クラスについて詳しくは、[XDM ビジネスマーケティングリストメンバーの概要](../../../../xdm/classes/b2b/business-marketing-list-members.md)を参照してください。

| ソースデータセット | XDM ターゲットフィールド | メモ |
| -------------- | ---------------- | ----- |
| `"Marketo"` | `marketingListMemberKey.sourceType` |
| `"${MUNCHKIN_ID}"` | `marketingListMemberKey.sourceInstanceID` | `"${MUNCHKIN_ID}"` の値は自動的に置き換えられます。 |
| `staticListMemberID` | `marketingListMemberKey.sourceID` |
| `concat(staticListMemberID,"@${MUNCHKIN_ID}.Marketo")` | `marketingListMemberKey.sourceKey` | プライマリ ID。`"${MUNCHKIN_ID}"` の値は自動的に置き換えられます。 |
| `iif(staticListID != null && staticListID != "", to_object("sourceType", "Marketo", "sourceInstanceID", "${MUNCHKIN_ID}","sourceID", staticListID, "sourceKey", concat(staticListID,"@${MUNCHKIN_ID}.Marketo")), null)` | `marketingListKey` | 関係 |
| `iif(personID != null && personID != "", to_object("sourceType", "Marketo", "sourceInstanceID", "${MUNCHKIN_ID}","sourceID", personID, "sourceKey", concat(personID,"@${MUNCHKIN_ID}.Marketo")), null)` | `personKey` | 関係 |
| `createdAt` | `extSourceSystemAudit.createdDate` |
| `marketoIsDeleted` | `isDeleted` |

{style=&quot;table-layout:auto&quot;}

## 重点顧客 {#named-accounts}

>[!IMPORTANT]
>
>重点顧客データセットは、Marketo の Account-Based Marketing（ABM）機能でのみ必要です。 ABM を使用しない場合は、重点顧客のマッピングを設定する必要はありません。

XDM クラスについて詳しくは、[XDM ビジネスアカウントの概要](../../../../xdm/classes/b2b/business-account.md)を参照してください。

| ソースデータセット | XDM ターゲットフィールド | メモ |
| -------------- | ---------------- | ----- |
| `"Marketo"` | `accountKey.sourceType` |
| `"${MUNCHKIN_ID}"` | `accountKey.sourceInstanceID` | `"${MUNCHKIN_ID}"` の値は自動的に置き換えられます。 |
| `concat(id, ".mkto_acct")` | `accountKey.sourceID` |
| `concat(id, ".mkto_acct@${MUNCHKIN_ID}.Marketo")` | `accountKey.sourceKey` | プライマリ ID。`"${MUNCHKIN_ID}"` の値は自動的に置き換えられます。 |
| `iif(crmGuid != null && crmGuid != "", to_object("sourceType", "${CRM_TYPE}", "sourceInstanceID", "${CRM_ORG_ID}","sourceID", crmGuid, "sourceKey", concat(crmGuid,"@${CRM_ORG_ID}.${CRM_TYPE}")), null)` | `extSourceSystemAudit.externalKey` | `extSourceSystemAudit.externalKey` はセカンダリ ID です。 `{CRM_ORG_ID}` と `{CRM_TYPE}` の値は自動的に置き換えられます。 |
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
| `iif(parentAccountId != null && parentAccountId != "", to_object("sourceType", "Marketo", "sourceInstanceID", "${MUNCHKIN_ID}", "sourceID", concat(parentAccountId, ".mkto_acct"), "sourceKey", concat(parentAccountId, ".mkto_acct@${MUNCHKIN_ID}.Marketo")), null)` | `accountParentKey` |
| `sourceType` | `accountSourceType` |
| `marketoIsDeleted` | `isDeleted` |

{style=&quot;table-layout:auto&quot;}

## 商談 {#opportunities}

XDM クラスについて詳しくは、[XDM ビジネス商談の概要](../../../../xdm/classes/b2b/business-opportunity.md)を参照してください。

| ソースデータセット | XDM ターゲットフィールド | メモ |
| -------------- | ---------------- | ----- |
| `"Marketo"` | `opportunityKey.sourceType` |
| `"${MUNCHKIN_ID}"` | `opportunityKey.sourceInstanceID` | `"${MUNCHKIN_ID}"` の値は自動的に置き換えられます。 |
| `id` | `opportunityKey.sourceID` |
| `concat(id,"@${MUNCHKIN_ID}.Marketo")` | `opportunityKey.sourceKey` | プライマリ ID。`"${MUNCHKIN_ID}"` の値は自動的に置き換えられます。 |
| `iif(externalOpportunityId != null && externalOpportunityId != "", to_object("sourceType", "${CRM_TYPE}", "sourceInstanceID", "${CRM_ORG_ID}","sourceID", externalOpportunityId, "sourceKey", concat(externalOpportunityId,"@${CRM_ORG_ID}.${CRM_TYPE}")), null)` | `extSourceSystemAudit.externalKey.sourceKey` | セカンダリ ID。 `{CRM_ORG_ID}` と `{CRM_TYPE}` の値は自動的に置き換えられます。 |
| `iif(mktoCdpAccountOrgId != null && mktoCdpAccountOrgId != "", to_object("sourceType", "Marketo", "sourceInstanceID", "${MUNCHKIN_ID}","sourceID", concat(mktoCdpAccountOrgId, ".mkto_org"), "sourceKey", concat(mktoCdpAccountOrgId, ".mkto_org@${MUNCHKIN_ID}.Marketo")), null)` | `accountKey` | 関係 |
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
| `iif(mktoCdpAccountOrgId != null && mktoCdpAccountOrgId != "", to_object("sourceType", "Marketo", "sourceInstanceID", "${MUNCHKIN_ID}","sourceID", concat(mktoCdpAccountOrgId, ".mkto_org"), "sourceKey", concat(mktoCdpAccountOrgId, ".mkto_org@${MUNCHKIN_ID}.Marketo")), null)` | `accountKey` | このソースデータセットは、[!DNL Salesforce] 統合のユーザーのみが使用できます。 |
| `lastActivityDate` | `lastActivityDate` |
| `leadSource` | `leadSource` |
| `nextStep` | `nextStep` |
| `marketoIsDeleted` | `isDeleted` |

{style=&quot;table-layout:auto&quot;}

## 商談連絡先の役割 {#opportunity-contact-roles}

XDM クラスについて詳しくは、[XDM ビジネス商談担当者関係の概要](../../../../xdm/classes/b2b/business-account-person-relation.md)を参照してください。

| ソースデータセット | XDM ターゲットフィールド | メモ |
| -------------- | ---------------- | ----- |
| `"Marketo"` | `opportunityPersonKey.sourceType` |
| `"${MUNCHKIN_ID}"` | `opportunityPersonKey.sourceInstanceID` | `"${MUNCHKIN_ID}"` の値は自動的に置き換えられます。 |
| `id` | `opportunityPersonKey.sourceID` |
| `concat(id,"@${MUNCHKIN_ID}.Marketo")` | `opportunityPersonKey.sourceKey` | プライマリ ID。 `"${MUNCHKIN_ID}"` の値は Explore API の一部として置き換えられます。 |
| `iif(mktoCdpSfdcId != null && mktoCdpSfdcId != "", to_object("sourceType", "${CRM_TYPE}", "sourceInstanceID", "${CRM_ORG_ID}","sourceID", mktoCdpSfdcId, "sourceKey", concat(mktoCdpSfdcId,"@${CRM_ORG_ID}.${CRM_TYPE}")), null)` | `extSourceSystemAudit.externalKey` | `extSourceSystemAudit.externalKey` はセカンダリ ID です。 `{CRM_ORG_ID}` と `{CRM_TYPE}` の値は自動的に置き換えられます。 |
| `iif(mktoCdpOpptyId != null && mktoCdpOpptyId != "", to_object("sourceType", "Marketo", "sourceInstanceID", "${MUNCHKIN_ID}","sourceID", mktoCdpOpptyId, "sourceKey", concat(mktoCdpOpptyId,"@${MUNCHKIN_ID}.Marketo")), null)` | `opportunityKey` | 関係 |
| `iif(leadId != null && leadId != "", to_object("sourceType", "Marketo", "sourceInstanceID", "${MUNCHKIN_ID}","sourceID", leadId, "sourceKey", concat(leadId,"@${MUNCHKIN_ID}.Marketo")), null)` | `personKey` | 関係 |
| `role` | `personRole` |
| `isPrimary` | `isPrimary` |
| `createdAt` | `extSourceSystemAudit.createdDate` |
| `updatedAt` | `extSourceSystemAudit.lastUpdatedDate` |
| `marketoIsDeleted` | `isDeleted` |

{style=&quot;table-layout:auto&quot;}

## ユーザー {#persons}

XDM クラスについて詳しくは、[XDM 個人プロファイルの概要](../../../../xdm/classes/individual-profile.md)を参照してください。 XDM フィールドグループについて詳しくは、[XDM ビジネスユーザー詳細スキーマフィールドグループ](../../../../xdm/field-groups/profile/business-person-details.md)ガイドと [XDM ビジネスユーザーコンポーネントスキーマフィールドグループ](../../../../xdm/field-groups/profile/business-person-components.md)ガイドを参照してください。

| ソースデータセット | XDM ターゲットフィールド | メモ |
| -------------- | ---------------- | ----- |
| `"Marketo"` | `b2b.personKey.sourceType` |
| `"${MUNCHKIN_ID}"` | `b2b.personKey.sourceInstanceID` | `"${MUNCHKIN_ID}"` の値は自動的に置き換えられます。 |
| `id` | `b2b.personKey.sourceID` |
| `concat(id,"@${MUNCHKIN_ID}.Marketo")` | `b2b.personKey.sourceKey` | プライマリ ID。`"${MUNCHKIN_ID}"` の値は自動的に置き換えられます。 |
| `iif(unsubscribed == 'true', 'n', 'y' ))` | `consents.marketing.email.val` | unsubscribed が `true`（例：value = `1`）の場合は、`consents.marketing.email.val` を `n` と設定します。unsubscribed が `false`（例：value = `0`）の場合は、`consents.marketing.email.val` を `null` と設定します。 |
| `iif(unsubscribedReason != null && unsubscribedReason != "", substr(unsubscribedReason, 0, 100), null)` | `consents.marketing.email.reason` |
| `iif(contactCompany != null && contactCompany != "", to_object("sourceType", "Marketo", "sourceInstanceID", "${MUNCHKIN_ID}","sourceID", concat(contactCompany, ".mkto_org"), "sourceKey", concat(contactCompany, ".mkto_org@${MUNCHKIN_ID}.Marketo")), null)` | `b2b.accountKey` |
| `marketingSuspended` | `b2b.isMarketingSuspended` |
| `marketingSuspendedCause` | `b2b.marketingSuspendedCause` |
| `leadScore` | `b2b.personScore` |
| `leadSource` | `b2b.personSource` |
| `leadStatus` | `b2b.personStatus` |
| `personType` | `b2b.personType` |
| `leadPartitionId` | `b2b.personGroupID` |
| `mktoCdpIsConverted` | `b2b.isConverted` |
| `mktoCdpConvertedDate` | `b2b.convertedDate` |
| <ul><li>`iif(decode(sfdcType, "Contact", sfdcContactId, "Lead", sfdcLeadId , null) != null, to_object("sourceType", "${CRM_TYPE}", "sourceInstanceID", "${CRM_ORG_ID}","sourceID", decode(sfdcType, "Contact", sfdcContactId, "Lead", sfdcLeadId , null), "sourceKey", concat(decode(sfdcType, "Contact", sfdcContactId, "Lead", sfdcLeadId , null),"@${CRM_ORG_ID}.${CRM_TYPE}")), null)`</li><li>`iif(decode(msftType, "Contact", msftContactId, "Lead", msftLeadId , null) != null, to_object("sourceType", "${CRM_TYPE}", "sourceInstanceID", "${CRM_ORG_ID}","sourceID", decode(msftType, "Contact", msftContactId, "Lead", msftLeadId , null), "sourceKey", concat(decode(msftType, "Contact", msftContactId, "Lead", msftLeadId , null),"@${CRM_ORG_ID}.${CRM_TYPE}")), null)`</li></ul> | `extSourceSystemAudit.externalKey` | `extSourceSystemAudit.externalKey` はセカンダリ ID です。 |
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
| `company` | `b2b.companyName` |
| `website` | `b2b.companyWebsite` |
| `leadScore` | `personComponents.personScore` |
| `leadSource` | `personComponents.personSource` |
| `leadStatus` | `personComponents.personStatus` |
| `personType` | `personComponents.personType` |
| `leadPartitionId` | `personComponents.personGroupID` |
| `iif(contactCompany != null && contactCompany != "", to_object("sourceType", "Marketo", "sourceInstanceID", "${MUNCHKIN_ID}", "sourceID", concat(contactCompany, ".mkto_org"), "sourceKey", concat(contactCompany, ".mkto_org@${MUNCHKIN_ID}.Marketo")), null)` | `personComponents.sourceAccountKey` |
| <ul><li>`iif(decode(sfdcType, "Contact", sfdcContactId, "Lead", sfdcLeadId , null) != null, to_object("sourceType", "${CRM_TYPE}", "sourceInstanceID", "${CRM_ORG_ID}", "sourceID", decode(sfdcType, "Contact", sfdcContactId, "Lead", sfdcLeadId , null), "sourceKey", concat(decode(sfdcType, "Contact", sfdcContactId, "Lead", sfdcLeadId , null),"@${CRM_ORG_ID}.${CRM_TYPE}")), null)`</li><li>`iif(decode(msftType, "Contact", msftContactId, "Lead", msftLeadId , null) != null, to_object("sourceType", "${CRM_TYPE}", "sourceInstanceID", "${CRM_ORG_ID}","sourceID", decode(msftType, "Contact", msftContactId, "Lead", msftLeadId , null), "sourceKey", concat(decode(msftType, "Contact", msftContactId, "Lead", msftLeadId , null),"@${CRM_ORG_ID}.${CRM_TYPE}")), null)`</li></ul> | `personComponents.sourceExternalKey` |
| `iif(id != null && id != "", to_object("sourceType", "Marketo", "sourceInstanceID", "${MUNCHKIN_ID}","sourceID", id, "sourceKey", concat(id,"@${MUNCHKIN_ID}.Marketo")), null)` | `personComponents.sourcePersonKey` |
| `email` | `personComponents.workEmail.address` |
| `email` | `workEmail.address` |
| `iif(ecids != null, to_object('ECID',arrays_to_objects('id',explode(ecids))), null)` | `identityMap` | これは計算フィールドです。 |
| `marketoIsDeleted` | `isDeleted` |
| `iif(mktoCdpCnvContactPersonId != null && mktoCdpCnvContactPersonId != \"\", to_object(\"sourceType\", \"Marketo\", \"sourceInstanceID\", \"${MUNCHKIN_ID}\", \"sourceID\", mktoCdpCnvContactPersonId, \"sourceKey\", concat(mktoCdpCnvContactPersonId,\"@${MUNCHKIN_ID}.Marketo\")), null)` | `b2b.convertedContactKey` | これは計算フィールドです。 |
| `iif(mktoCdpCnvContactPersonId != null && mktoCdpCnvContactPersonId != \"\", to_object(\"sourceType\", \"Marketo\", \"sourceInstanceID\", \"${MUNCHKIN_ID}\", \"sourceID\", mktoCdpCnvContactPersonId, \"sourceKey\", concat(mktoCdpCnvContactPersonId,\"@${MUNCHKIN_ID}.Marketo\")), null)` | `personComponents.sourceConvertedContactKey` | これは計算フィールドです。 |

{style=&quot;table-layout:auto&quot;}

>[!NOTE]
>
>`to_object('ECID',arrays_to_objects('id',explode(ecids)))` ソースフィールドは、Platform UI の「[!UICONTROL 計算フィールドを追加] 」オプションを使用して追加する必要がある計算フィールドです。詳しくは、[計算フィールドの追加](../../../../data-prep/ui/mapping.md#calculated-fields)に関するチュートリアルを参照してください。

## 次の手順

このドキュメントでは、[!DNL Marketo] データセットとそれに対応する XDM フィールドとのマッピング関係について説明しました。[!DNL Marketo] データフローを完成させるには、[ [!DNL Marketo] ソース接続の作成](../../../tutorials/ui/create/adobe-applications/marketo.md)に関するチュートリアルを参照してください。
