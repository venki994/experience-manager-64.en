---
title: Managing Users and User Groups
seo-title: Managing Users and User Groups
description: Users of AEM Communities can self-register and edit their profiles
seo-description: Users of AEM Communities can self-register and edit their profiles
uuid: ae5df404-4038-4584-b8ab-e114d1cc4aa0
contentOwner: Janice Kendall
products: SG_EXPERIENCEMANAGER/6.4/COMMUNITIES
topic-tags: administering
content-type: reference
discoiquuid: b4e0fe74-cc5c-4259-bc2b-669fafb6cb6e
index: y
internal: n
snippet: y
---

# Managing Users and User Groups{#managing-users-and-user-groups}

## Overview {#overview}

In AEM Communities, in the publish environment, users can self-register and edit their* *profiles. Given the appropriate permissions, they may also

* create sub-communities within the community site (see [community groups](../../communities/using/creating-groups.md))
* [moderate](../../communities/using/moderation.md) user generated content (UGC)
* be [enablement resource](../../communities/using/resources.md) contacts
* be [privileged](#privilegedmembersgroup) to create entries for blogs, calendars, QnA, and forums

Users registered in the publish environment are generally referred to as *community members (members)* to distinguish them from *users *in the author environment.

Permissions are granted by assigning members to one of the [member (user) groups](#publishgrouproles) dynamically created when the community site is [created](../../communities/using/sites-console.md) or [modified](../../communities/using/sites-console.md#modifyingsiteproperties) from the author environment. When working from the author environment, members are visible from the publish environment by means of the [tunnel service](#tunnelservice).

By design, members and member groups created in the publish environment should not appear in the author environment. Users and user groups created in the author environment are similarly intended to remain in the author environment.

When users on author and members on publish come from the same list of users, such as synchronized from the same LDAP directory, they are not considered the same user with the same permissions and group membership in both the author and publish environments. The role(s) of members and users must be established separately on publish and author, as appropriate.

For a [publish farm](../../communities/using/topologies.md), registration and modifications made on one publish instance need to be synchronized with other publish instances in order for them to have access to the same user data. For details see [User Synchronization](../../communities/using/sync.md), which includes a section describing [What Happens When...](../../communities/using/sync.md#whathappenswhen).

### Contribution Limits {#contribution-limits}

In order to protect against spam, it is possible to limit members' frequency of posting content. Further, it is possible to automatically limit the contributions of newly registered members.

For details, see [Member Contribution Limits](../../communities/using/limits.md).

### Dynamically Created User Groups {#dynamically-created-user-groups}

When a new community site is created, new user groups are dynamically created with uniquie ids (uid) and permissions appropriate for various administrative functions necessary to manage the community site either in the author environment (see [Author Group Roles](#authorgrouproles)) or the publish environment (see [Publish Group Roles](#publishgrouproles)).

The names of the groups are generated from the name given the site during [community site creation](../../communities/using/sites-console.md#step13asitetemplate). The unique ids avoid naming conflicts for similarly named community sites and community groups on the same server.

For example, if the site name were "*engage*" for a site titled "We.Retail Engage", then one of the user groups created would be:

* Community *Engage *Members

## Author Environment {#author-environment}

### Tunnel Service {#tunnel-service}

When using the author environment to [create sites](../../communities/using/sites-console.md), [modify site properties](../../communities/using/sites-console.md#modifyingsiteproperties) and [manage community members and member groups](../../communities/using/members.md), it is necessary to access users and user groups registered in the publish environment.

The tunnel service provides this access using the replication agent on author.

* for details, see [configuration instructions](../../communities/using/deploy-communities.md#tunnelserviceonauthor) on the deployment page

The [Communities Members and Groups consoles](../../communities/using/members.md) are for the sole purpose of managing users (members) and user groups (member groups) registered only in the publish environment.

To manage users and user groups registered in the author environment, use the [Security console](../../sites/administering/using/security.md)

### Author Group Roles {#author-group-roles}

| If Member of Group... |Primary Role |
|---|---|
| administrators |The administrators group consists of system administrators who have all the abilities of a Community Administrator as well as the ability to manage the Community Administrators group. |
| Community Administrators |The Community Administrators group automatically becomes a member of all community sites and any community groups created on the site. An initial member of the Community Administrators group is the administrators group. In the author environment, Community Administrators are able to create community sites, manage sites, manage members (they can ban members from the community), and moderate content. |
| Community &lt;*site name&gt; *Sitecontentmanager |The Community Site Content Manager is able to perform traditional AEM authoring, content creation, and modifying pages for a community site. |
| Community Enablement Managers |The Community Enablement Managers group consists of users who are available for assignment to manage a community site's Enablement Managers group. |
| Community &lt;*site name&gt; *Siteenablementmanagers |The Community Site Enablement Managers group consists of users who have been assigned to manage a community site's enablement [resources](../../communities/using/resources.md). |
| None |An anonymous site visitor may not access the author environment. |

### System Administrators {#system-administrators}

Members of the administrators group are system administrators who are able to perform the initial setup of an AEM installation for both the author and publish environments.

For demonstration and development purposes, the administrators group has a member whose userid is *admin* and password is *admin*.

For production environments, the default administrators group should be modified.

Be sure to follow the [Security Checklist](../../sites/administering/using/security-checklist.md).

## Publish Environment {#publish-environment}

### Becoming a Member {#becoming-a-member}

In the publish environment, depending on the [settings](../../communities/using/sites-console.md#usermanagement) of the community site, a site visitor may become a community member

* when the community site is private (closed) :

    * by invitation
    * by actions of an administrator

* when the community site is public (open) :

    * by self-registration
    * by social login with Facebook and Twitter

>[!NOTE]
>
>If a site visitor registers as a member of one open community site, they automatically become a member of other open community sites on the same publish environment.

### Publish Group Roles {#publish-group-roles}

| If Member of Group... |Primary Role |
|---|---|
| Community &lt;*site name&gt; *Members |A community site member is a registered user. They may login, modify their profile, join an open community group, post content to the community, send messages to other members, and follow site activities. |
| Community &lt;*site name&gt; *Moderators |A community site moderator is a trusted community member who is able to moderate UGC either in bulk, using the moderation console, or in-context, on the page where the content is posted. |
| Community &lt;*site name&gt; *&lt;*group name*&gt; Members |A community group member is a community member who has either joined an open community group or has been invited to a closed community group. They have the abilities of a member for that community group within the site. |
| Community &lt;*site name&gt; *Groupadministrators |A community site group administrator is a trusted community member who is assigned to create and manage sub-communities (groups) within a community site. Included is the ability to provide in-context moderation.  |
| *Privileged Members Security Group* |A manually created and maintained user group for the purpose of restricting content creation. See [Privileged Members Group](#privilegedmembersgroup). |
| None |An anonymous site visitor, who discovers the site, may view and search community sites that allow anonymous access. In order to participate and post content, the user must self-register (if allowed) and become a community member. |

### Assigning Members to Publish Group Roles {#assigning-members-to-publish-group-roles}

When [creating a community site](../../communities/using/sites-console.md) in the author environment, or when [modifying site properties,](../../communities/using/sites-console.md#modifyingsiteproperties) members may be assigned various roles performed in the publish environment, such as moderators, group administrators, resource contacts, or privileged members.

[Enabling the tunnel service](../../communities/using/sync.md#accessingpublishusersfromauthor) results in assignment choices being presented from members on publish instead of users on author.

The selected members will be automatically assigned to the [appropriate group](#publishgrouproles) and their memberships will be included when the community site is (re)published.

### Privileged Members Group {#privileged-members-group}

The purpose of a privileged members security group is to restrict the creation of content for certain community functions to a privileged subset of a community site's members.

The privileged members group is a member group created and managed using the [Communities Groups console](../../communities/using/members.md).

After a privileged members group is created, and with the [tunnel service enabled](../../communities/using/sync.md#accessingpublishusersfromauthor), an existing community site's structure may be [modified](../../communities/using/sites-console.md#modifystructure) to edit the configuration of its community functions to 'Allow Privileged Members' and add the created group.

The community functions which allow specification of one or more privileged members groups are :

* [blog function](../../communities/using/functions.md#blogfunction) - to restrict creation of new articles
* [calendar function](../../communities/using/functions.md#calendarfunction) - to restrict creation of new events
* [forum function](../../communities/using/functions.md#forumfunction) - to restrict creation of new topics
* [QnA function](../../communities/using/functions.md#qnafunction) - to restrict creation of new questions

When a community function is not secured (no privileged members group assigned), then all community site members are allowed to create feature content (articles, events, topics, questions).

>[!NOTE]
>
>Adding a user to a privileged members group for a community site will only grant them create privileges if they are also a member of that same community site.

## Creating Community Members {#creating-community-members}

### Repository Location {#repository-location}

In order for certain features to work properly, it is required to create users and user groups with the appropriate privileges.

When members are created in `/home/users/community`, they inherit the proper ACLs that give read privileges to members' profiles.

Similarly, custom community user groups (such as privileged members groups) should be created in `/home/groups/community`.

Using the [Communities Members and Groups consoles](../../communities/using/members.md) will create users and groups in these paths.

To specify a custom path requires use of the classic security UI, which is accessible at [http://&lt;server&gt;:&lt;port&gt;/useradmin](http://localhost:4503/useradmin).

To give read privileges for custom member paths, on all publish instances set ACLs similar to `/home/users/community` :

```xml
<allow
  jcr:primaryType="rep:GrantACE"
  rep:principalName="everyone"
  rep:privileges="{Name}[jcr:read]" >
  <rep:restrictions 
    jcr:primaryType="rep:Restrictions"  
    rep:glob="*/profile*" />
</allow>
```

To give the proper privileges for custom member group paths, such as /home/groups/mycompany, on all publish instances set ACLs similar to `/home/groups/community` :

```xml
<allow
  jcr:primaryType="rep:GrantACE"
  rep:principalName="community-administrators"
  rep:privileges="{Name}[jcr:read]"  />
```

### Consoles {#consoles}

There are four separate consoles available only in the author environment:

<table border="1" cellpadding="2" cellspacing="2" width="100%"> 
 <tbody> 
  <tr> 
   <td style="text-align: center;"><strong>console</strong></td> 
   <td style="text-align: center;">Tools, Security,<br /> Users</td> 
   <td style="text-align: center;">Tools, Security,<br /> Groups</td> 
   <td style="text-align: center;">Communities,<br /> Members</td> 
   <td style="text-align: center;">Communities,<br /> Groups</td> 
  </tr> 
  <tr> 
   <td style="text-align: center;"><strong>manages</strong></td> 
   <td style="text-align: center;">users on author</td> 
   <td style="text-align: center;">user groups on author</td> 
   <td style="text-align: center;">members on publish</td> 
   <td style="text-align: center;">member groups on publish</td> 
  </tr> 
  <tr> 
   <td style="text-align: center;"><strong>requires</strong></td> 
   <td style="text-align: center;">admin permission</td> 
   <td style="text-align: center;">admin permission</td> 
   <td style="text-align: center;">admin permission,<br /> tunnel service,<br /> user sync for publish farm</td> 
   <td style="text-align: center;">admin permission,<br /> tunnel service,<br /> user sync for publish farm</td> 
  </tr> 
 </tbody> 
</table>

### Community Enablement Manager Role {#community-enablement-manager-role}

The ability for a site visitor to self register is typically not allowed for an [enablement community](../../communities/using/overview.md#enablementcommunity) as there are costs associated with each member. Enablement learners and resources are managed by a user assigned the [role](#authorgrouproles) of `enablement manager` [during site creation](../../communities/using/sites-console.md#enablement) on author (added as member of group `Community <site-name> Siteenablementmanagers`). The `enablement manager` is also responsible for [assigning learning resources](../../communities/using/resources.md) to community members on author.

Only users who are members of the global `Community Enablement Managers` group may be selected as an `enablement manager` for a specific community site.

To create a user who may be assigned the role of `Community Site Enablement Manager`, use the classic UI security console in order to specify the path :

* on an author instance
* signed in with administrator privileges
* browse to the classic UI security console

    * for example, [http://localhost:4502/useradmin](http://localhost:4502/useradmin)

* from the Edit menu, select **Create User**
* fill in the `Create User` dialog

    * path must be `/home/users/community`

* select **Create**

![](assets/chlimage_1-130.png)

* In the left pane :

    * search for the newly created user and select to display in the right pane

![](assets/chlimage_1-131.png)

In the left pane :

* clear the search box and select **Hide Users**
* locate and drag `community-enablementmanagers` to the **Groups **tab of the new user displayed in the right pane

![](assets/chlimage_1-132.png) 

### Community Administrators Role {#community-administrators-role}

As stated in the [Author Group Roles](#authorgrouproles) chart, members of the Community Administrators group are able to create community sites, manage sites, manage members (they can ban members from the community), and moderate content.

Follow the same steps as creating and assigning a user to the role of [enablement manager](#communitysiteenablementmanagerrole), but add c `ommunity-administrators` group under the user's Groups tab.

### LDAP Integration {#ldap-integration}

AEM supports the use of LDAP for authentication of users as well as creation of user accounts. This is detailed in [Configuring LDAP with AEM 6](../../sites/administering/using/ldap-config.md).

Following are some configuration details specific for community members and member groups.

1. configure LDAP for each AEM publish instance
1. [the LDAP Identity Provider](../../sites/administering/using/ldap-config.md#configuringtheldapidentityprovider)

    * no special instructions

1. [the Synchronization Handler](../../sites/administering/using/ldap-config.md#configuringthesynchronizationhandler)

    * set the following properties :

        * **User auto membership**&#42; : `community-*<site name>-<uid>*-members`
        
        * **User Path Prefix** : `/community`
        
        * **Group Path Prefix** : `/community`

1. [the External Login Module](../../sites/administering/using/ldap-config.md#theexternalloginmodule)

    * no special instructions

This results in users automatically being assigned to the community site's members group and the repository location being `/home/users/community` and `/home/groups/community`, so that they inherit the appropriate permissions to see one another's profile.

&#42; The `User auto membership` value should be the `rep:authorizableId` property, not the `givenName` (display name) from the profile.

## Synchronizing Users Among AEM Instances {#synchronizing-users-among-aem-instances}

When using a [publish farm](../../communities/using/topologies.md), ensure users have the same path on each publish instance by importing the users first to one instance and [enabling user sync](../../communities/using/sync.md) to Sling distribute the users to the other publish instances.

If importing user groups, to ensure the user groups have the same path on each publish instance, import to one instance, then [create a package](../../sites/administering/using/package-manager.md#creatinganewpackage) for export, and install that package on all other publish instances.

While the syncing of user groups through user sync will be included in a future release, presently only the *membership *of a user group will sync when user sync runs.

## About Community Groups {#about-community-groups}

When discussing groups, there are two distinct topics :

* ** [community groups](../../communities/using/overview.md#communitygroups)** 
  These are sub-communities which may be created in the publish environment for a community site which supports creation of community groups. Creation of a community group results in more pages added to the website and are managed in a manner similar to their parent community site. For more information visit [Community Group Essentials](../../communities/using/essentials-groups.md) for developers and [Community Group](../../communities/using/creating-groups.md) for authors.

* ** [member groups](../../sites/administering/using/security.md)** 
  These are groups to which members may belong and are managed through the Groups console. Much of the discussion on this page has been devoted to member groups. The member groups automatically created for a community site, which are prefixed with * `Community`*, may be referred to as a community group, therefore the context of the discussion must be considered.
