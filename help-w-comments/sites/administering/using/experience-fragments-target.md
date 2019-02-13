---
title: Target Integration with Experience Fragments
seo-title: Target Integration with Experience Fragments
description: Target Integration with Experience Fragments
seo-description: Target Integration with Experience Fragments
uuid: 993c3f81-9af5-44d3-b0ca-2d9b79f1c4ce
contentOwner: carlino
products: SG_EXPERIENCEMANAGER/6.4/SITES
topic-tags: integration
content-type: reference
discoiquuid: 87b0e90e-c36d-4b9b-a5cd-c0c895814164
index: y
internal: n
snippet: y
---

# Target Integration with Experience Fragments{#target-integration-with-experience-fragments}

>[!NOTE]
>
>This functionality requires the application of [AEM 6.4 Service Pack 2 (6.4.2.0)](../../../release-notes/sp-release-notes.md) or later.

You can export [Experience Fragments](../../../sites/authoring/using/experience-fragments.md), created in Adobe Experience Manager (AEM), to Adobe Target. They can then be used as offers in Target activities, to test and personalize experiences at scale. This lets you combine the ease-of-use and power of AEM, with the powerful Automated Intelligence (AI) and Machine Learning (ML) capabilities in Target.

## Prerequisites {#prerequisites}

Various actions are required:

1. You have to integrate AEM with Target. See [Integrating with Adobe Target](../../../sites/administering/using/target.md) for more information.

   <!--
   Comment Type: remark
   Last Modified By: Alison Heimoz (aheimoz)
   Last Modified Date: 2019-01-10T03:37:44.571-0500
   <p>for how much longer - Cloud Services config being deprecated - need to use Adobe Launch<br /> </p>
   <p><a href="https://jira.corp.adobe.com/browse/CQ-4248189">https://jira.corp.adobe.com/browse/CQ-4248189</a></p>
   -->

1. Experience Fragments are exported from the author instance, so you need to [Configure the Link Externalizer](../../../sites/developing/using/externalizer.md) on the author instance to ensure that any links are externalized for the publish instance.

## Add the Cloud Configuration {#add-the-cloud-configuration}

Before exporting a fragment you need to add the **Cloud Configuration** for **Adobe Target** to the fragment, or folder:

1. Navigate to the **Experience Fragments** console.
1. Open **Page Properties** for the appropriate folder or fragment.

   >[!NOTE]
   >
   >If you add the cloud configuration to the Experience Fragment parent folder, the configuration is inherited by all the children.
   >
   >
   >If you add the cloud configuration to the Experience Fragment itself, the configuration is inherited by all varations.

1. Select the **Cloud Services** tab.  

1. Under **Cloud Service Configuration**, select **Adobe Target** from the drop down list.
1. Under **Adobe Target**, select the appropriate configuration.  

1. **Save & Close**.

## Exporting an Experience Fragment to Target {#exporting-an-experience-fragment-to-target}

>[!NOTE]
>
>For media assets, such as images, only a reference is exported to Target. The asset itself remains stored in AEM Assets and is delivered from the AEM publish instance.
>
>Due to this the Experience Fragment, with all related assets, needs to be published before exporting to Target.

To export an experience fragment from AEM to Target (after specifying the Cloud Configuration):

1. Navigate to the Experience Fragment console.
1. Select the Experience Fragment you would like to export to target.

   >[!NOTE]
   >
   >It has to be an Experience Fragment Web variation.

1. Tap/click **Export to Adobe Target**.

   >[!NOTE]
   >
   >If the Experience Fragment has already been exported, select **Update in Adobe Target**.

1. Tap/click **Export without publishing** or **Publish** as required.

   >[!NOTE]
   >
   >Selecting** Publish** will publish the experience fragment right away and send it to Target.

1. Tap/click **OK** in the confirmation dialog.

   Your experience fragment should now be in Target.

>[!NOTE]
>
>Alternatively, you can perform the export from the page editor, using comparable commands in the [Page Information](../../../sites/authoring/using/author-environment-tools.md#main-pars-title-21) menu.

## Using your Experience Fragments in Target {#using-your-experience-fragments-in-target}

After performing the preceding tasks, the experience fragment displays on the Offers page in Target. Please have a look at the [specific Target documentation](https://experiencecloud.adobe.com/resources/help/en_US/target/target/aem-experience-fragments.html) to learn about what you can achieve there.

## Deleting an Experience Fragment already exported to Target {#deleting-an-experience-fragment-already-exported-to-target}

Deleting an Experience Fragment that has already been exported to Target may cause problems if the fragment is already being used in an offer in Target. Deleting the fragment would render the offer unusable as the fragment content is being delivered by AEM.

To avoid such situations:

* If the Experience Fragment is not being currently used in an activity, AEM allows the user to delete the fragment without a warning message.
* If the Experience Fragment is currently in use by an activity in Target, an error message warns the AEM user about possible consequences that deleting the fragment will have on the activity.

  The error message in AEM does not prohibit the user from (force-)deleting the Experience Fragment. If the Experience Fragment is deleted:

    * The Target offer with AEM Experience Fragment may show undesired behavior

        * The offer will likely still render, as the Experience Fragment HTML was pushed to Target
        * Any references in the Experience Fragment may not work correctly if referenced assets were deleted in AEM as well.

    * Of course, any further modifications to the Experience Fragment are impossible as the Experience Fragment does not exist anymore in AEM.
