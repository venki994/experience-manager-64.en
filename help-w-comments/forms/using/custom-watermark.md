---
title: Custom watermark in letter PDF preview
seo-title: Custom watermark in letter PDF preview
description: Learn how to create custom watermark in letter PDF preview.
seo-description: Learn how to create custom watermark in letter PDF preview.
uuid: 9b106b7c-e128-4b2b-bb84-55a02e880c9c
content-type: reference
products: SG_EXPERIENCEMANAGER/6.4/FORMS
topic-tags: correspondence-management
discoiquuid: 308da011-b8b1-4ab2-814d-0048ffad8bbe
index: y
internal: n
snippet: y
---

# Custom watermark in letter PDF preview{#custom-watermark-in-letter-pdf-preview}

## Overview {#overview}

In the Create Correspondence UI, agent users preview the correspondence in final shape in which it is sent to post processing, such as for e-mailing or printing.

To prevent unauthorized use of this data, organizations can impose a watermark on the preview PDF. The default watermark is "PREVIEW", which appears across the PDF.

To enable the watermark in preview PDF, select the **[!UICONTROL Apply Watermark]** During Preview option in **[!UICONTROL Correspondence Management Configurations]** at http://[server]:[port]/system/console/configMgr.

![](assets/default-watermark.png)

You can use the following steps to customize the text and appearance of the watermark:

## Customize the watermark in PDF preview in Create Correspondence UI {#customizewatermark-}

1. Go to `http://[server]:[port]/[ContextPath]/crx/de` and login as Administrator.
1. In the apps folder, create a folder named **[!UICONTROL previewwatermark]** with path/structure similar to the previewwatermark folder in the libs folder:

    1. Right-click the **previewwatermark **folder at the following path and select **Overlay Node**:
    1. Ensure that the Overlay Node dialog has the following values:

       **Path:** /libs/fd/cm/configFiles/previewwatermark

       **Overlay Location:** /apps/

       **Match Node Types:** Checked

       >[!NOTE]
       >
       >Do not make changes in the /libs branch. Any changes you do make may be lost, because this branch is liable to changes whenever you:
       >
       >    
       >    
       >    * Upgrade on your instance
       >    * Apply a hot fix
       >    * Install a feature pack
       >    
       >

    1. Click **OK** and then click **Save All**. The **[!UICONTROL previewwatermark]** folder is created in the specified path.

1. Copy and paste the ddx file from "/libs/fd/cm/configFiles/previewwatermark" folder to "/apps/fd/cm/configFiles/previewwatermark" folder and click **[!UICONTROL Save All]**.
1. Make the desired changes in the ddx file under /apps/fd/cm/configFiles/previewwatermark/.

   ```
   <DDX xmlns="http://ns.adobe.com/DDX/1.0/">
    <PDF result="output.pdf">
     <PDF source="input.pdf"/>
           <Watermark opacity="15%" rotation="45">
            <StyledText>
                     <p font-family="Georgia" font-size="70pt" color="black" font-weight="bold">
                         PREVIEW
                    </p>
            </StyledText>
           </Watermark>
    </PDF>
   </DDX>
   ```

   For information on customizing the watermark appearance, text, and alignment, see Adding and removing watermarks and backgrounds in the [Assembler Service and DDX Reference](http://help.adobe.com/en_US/livecycle/11.0/ddxRef.pdf) document.

   >[!NOTE]
   >
   >In the ddx file, the references to result and source should remain unchaged to output.pdf and input.pdf. Name of the file ddx should also not be changed.

1. Click **Save All**.

>[!MORE_LIKE_THIS]
>
>* [Add custom action to Asset Listing view](../../forms/using/add-custom-action-asset-listing-view.md)
>* [Add Custom Action/Button in Create Correspondence UI](../../forms/using/add-action-button-in-create-correspondence-ui.md)
>* [Add custom properties to Correspondence Management assets](../../forms/using/add-custom-properties-cm-assets.md)