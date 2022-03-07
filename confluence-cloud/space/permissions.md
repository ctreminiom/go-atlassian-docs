# 🛡 Permissions

## Add new permission to space

Adds new permission to space. If the permission to be added is a group permission, the group can be identified by its group name or group id.

{% hint style="info" %}
**Note**: Apps cannot access this REST resource - including when utilizing user impersonation.
{% endhint %}

{% embed url="https://developer.atlassian.com/cloud/confluence/rest/api-group-space-permissions#api-wiki-rest-api-space-spacekey-permission-post" %}
Official Documentation
{% endembed %}

## Add new custom content permission to space

Adds new custom content permission to space. If the permission to be added is a group permission, the group can be identified by its group name or group id.

{% hint style="info" %}
**Note**: Apps cannot access this REST resource - including when utilizing user impersonation.
{% endhint %}

{% embed url="https://developer.atlassian.com/cloud/confluence/rest/api-group-space-permissions#api-wiki-rest-api-space-spacekey-permission-custom-content-post" %}
Official Documentation
{% endembed %}

## Remove a space permission

Removes a space permission. Note that removing Read Space permission for a user or group will remove all the space permissions for that user or group.

{% hint style="info" %}
Note: Apps cannot access this REST resource - including when utilizing user impersonation.
{% endhint %}

{% embed url="https://developer.atlassian.com/cloud/confluence/rest/api-group-space-permissions#api-wiki-rest-api-space-spacekey-permission-id-delete" %}
Official Documentation
{% endembed %}
