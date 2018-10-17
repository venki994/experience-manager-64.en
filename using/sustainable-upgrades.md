---
title: Sustainable Upgrades
seo-title: Sustainable Upgrades
description: null
seo-description: Learn about sustainable upgrades in AEM 6.4.
uuid: 1b9a86a5-09ec-485e-9f9c-67a4ddde34cf
content-encoding: ISO-8859-1
aemsrcnodepath: /content/help/en/experience-manager/6-4/sites/deploying/using/sustainable-upgrades
contentOwner: sarchiz
cq-designpath: /etc/designs/help
cq-lastmodified: 2018-04-30T03 29 11.259-0400
cq-lastmodifiedby: carlino
cq-lastreplicated: 2018-04-03T10 34 19.185-0400
cq-lastreplicatedby: carlino
cq-lastreplicationaction: Activate
products: SG_EXPERIENCEMANAGER/6.4/SITES
content-type: reference
topic-tags: upgrading
cq-template: /apps/help/templates/article-3
discoiquuid: 55f4cee2-773d-400c-804d-6c50a883af56
firstPublishExternalDate: 2018-04-03T10:34:18.826-0400
isreadyforlocalization: false
jcr-created: 2018-03-23T07 28 51.325-0400
jcr-createdby: admin
jcr-ischeckedout: true
jcr-language: en_us
lastPublishExternalDate: 2018-04-03T10:34:18.826-0400
lochandoffdate: 2018-04-30T03 29 11.258-0400
lr-lastreplicatedby: carlino@adobe.com
moreHelpPaths: /content/help/en/experience-manager/6-4/sites/deploying/morehelp/upgrading;/content/help/en/experience-manager/6-4/sites/deploying/morehelp/upgrading
pagecreatedat: en
publishexternaldate: 2018-04-03T10 34 18.826-0400
publishExternalURL: https://helpx.adobe.com/experience-manager/6-4/sites/deploying/using/sustainable-upgrades.html
sha1: 97a056c323c7fa6124b5d7276943974fea62908d
topicBrowsingSortDate: 2018-04-03T10:34:18.826-0400
unpublishinternaldate: 2018-03-30T04 56 09.414-0400
index: y
internal: n
snippet: y
---

# Sustainable Upgrades



## Customization Framework

### Architecture (Functional / Infrastructure / Content / Application)
The Customization Framework feature is designed to help reduce the violations in non-extensible areas of the code (like APIS) or content (like overlays) that are not upgrade friendly.

There are two components of the customization framework: the **API Surface** and the **Content Classification**.

#### API Surface
In previous releases of AEM many APIs were exposed via the Uber Jar. Some of these APIs were not intended to be used by customers, but were exposed to support AEM functionality across bundles. Going forward, the Java APIs will be marked as Public or Private to indicate to customers which APIs are safe to use in the context of upgrades. Other specifics include:

* Java APIs marked as `Public` can be used and referenced by custom implementation bundles.  

* The Public APIs will be backwards compatible with the installation of a compatibility package.  
* The compatibility package will contain a compatibility Uber JAR to ensure backwards compatability  
* Java APIs marked as `Private` are intended to only be used by AEM internal bundles and should not be used by custom bundles.

>[!NOTE]
>
>The concept of `Private` and `Public` in this context should not be confused with Java notions of public and private classes.

![](assets/image2018-2-12_23-52-48.png) 

#### Content Classifications
AEM has long used the principal of overlays and Sling Resource Merger to allow customers to extend and customize AEM functionality. Predefined functionality that powers the AEM consoles and UI are stored in **/libs**. Customers are never to modify anything beneath **/libs** but could add additional content beneath **/apps** to overlay and extend the functionality defined in **/libs** (See Developing with Overlays for more information). This still caused numerous issues when upgrading AEM as the content in **/libs** might change causing the overlay funcationlity to break in unexpected ways. Customers could also extend AEM components through inheritance via `sling:resourceSuperType`, or simply reference a component in **/libs** directly via sling:resourceType. Similar upgrade issues could occur with reference and override use cases.

In order to make it safer and easier for customers to understand what areas of **/libs** are safe to use and overlay the content in **/libs** has been classified with the following mixins:

* **Public (granite:PublicArea)** - Defines a node as public so that it can overlaid, inherited ( `sling:resourceSuperType`) or used directly ( `sling:resourceType`). Nodes beneath /libs marked as Public will be safe to upgrade with the addition of a Compatibility Package. In general customers should only leverage nodes marked as Public.  

* **Abstract (granite:AbstractArea)** - Defines a node as abstract. Nodes can be overlaid or inherited ( `sling:resourceSupertype`) but must not be used directly ( `sling:resourceType`).  

* **Final (granite:FinalArea)** - Defines a node as final. Nodes classified as final cannot be overlaid or inherited. Final nodes can be used directly via `sling:resourceType`. Subnodes under final node are considered internal by default  

* **Internal (granite:InternalArea)** - Defines a node as internal. Nodes classified as internal cannot be overlaid, inherited, or used directly. These nodes are meant only for internal functionality of AEM  

* **No Annotation** - Nodes inherit classification based on the tree hierachy. The / root is by default Public. **Nodes with a parent classified as Internal or Final are also to be treated as Internal.**

>[!NOTE]
>
>These policies are only enforced against Sling search path based mechanisms. Other areas of **/libs** like a client-side library may be marked as `Internal`, but could still be used with standard clientlib inclusion. It is important that a customer continues to respect the Internal classification in these cases.

#### CRXDE Lite Content Type Indicators
Mixins applied in CRXDE Lite will show content nodes and trees that are marked as `INTERNAL` as being greyed out. For `FINAL` only the icon is greyed out. The children of these nodes will also appear grey. The Overlay Node functionality is disabled in both these cases.

**Public**

![](assets/image2018-2-8_23-34-5.png)

**Final**

![](assets/image2018-2-8_23-34-56.png)

**Internal**

![](assets/image2018-2-8_23-38-23.png)

**Content Health Check**

AEM 6.4 will ship with a health check to alert customers if overlaid or referenced content is used in a way inconsistent with the content classification.

The** Sling/Granite Content Access Check** is a new health check that monitors the repository to see if customer code is improperly accessing protected nodes in AEM.

This will scan **/apps** and typically takes several seconds to complete.

In order to access this new health check, you need to do the following:

1. From the AEM Home Screen, navigate to **Tools &gt; Operations &gt; Health Reports**
1. Click on the **Sling/Granite Content Access Check** as shown below:

   ![](assets/screen_shot_2017-12-14at55648pm.png)

After the scan is complete, a list of warnings will appear notifiying an end user of the protected node that is being improperly referenced:

![](assets/screenshot-2018-2-5healthreports.png)

After fixing the violations it will return to green state:

![](assets/screenshot-2018-2-5healthreports-violations.png)

The health check displays information collected by a background service that asynchronously checks whenever an overlay or resource type is used across all Sling search paths. If content mixins are used incorrectly it reports a violation.