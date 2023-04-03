# Change Group Mapping

<p align="center">
<img height="75dp" src="https://github.com/goauthentik/authentik/raw/3ecc715e91ed0bb8b019d2a8fe42d0eb6531a341/web/icons/icon_left_brand.svg"/><br><br>
<img src="https://img.shields.io/badge/Importance-High-critical?style=for-the-badge"/>
</p>

Since authentik allows defining groups as members of other groups it is possible
to create subgroups which inherit the parent group.
The OpenID mapping for the groups does not reflect this and therefore the 
mapping of this user attribute needs to be changed.

## Instructions
1. Open the administrative UI of the authentik installation<br>
`https://<your-authentik-binding>/if/admin/` <br>

2. Navigate to the _Property Mappings_ which are located under the _Customization_
   entry in the sidebar.

3. Now enable the display of the managed Mappings by flipping the switch labeled
   `Hide managed mappings`

4. Now edit the mapping named `authentik default OAuth Mapping: OpenID 'profile'`

5. Replace the mapping with the following code
    ```python
    # This function will help resolving the parents of a group
    def resolve_parents(group, parentList):
        parentList.append(group)
        if group.parent is not None:
            parentList.append(group.parent)
            resolve_parents(group.parent, parentList)
    
    # Now get all groups the user is directly assigned to
    userGroups = [g.name for g in user.ak_groups.all()]

    # Now resolve all parents of a group if the group has a parent
    for group in user.ak_groups.all():
        if group.parent is not None:
            parents = []
            resolve_parents(group, parents)
            for parent in parents:
                userGroups.append(parent.name)

    return {
        # Because authentik only saves the user's full name, and has no concept of first and last names,
        # the full name is used as given name.
        # You can override this behaviour in custom mappings, i.e. `request.user.name.split(" ")`
        "name": request.user.name,
        "given_name": request.user.name,
        "preferred_username": request.user.username,
        "nickname": request.user.username,
        # groups is not part of the official userinfo schema, but is a quasi-standard
        "groups": list(set(userGroups)),
    }
    ```

    <details>
    <summary>Original Mapping</summary>

    ```python
    return {
        # Because authentik only saves the user's full name, and has no concept of first and last names,
        # the full name is used as given name.
        # You can override this behaviour in custom mappings, i.e. `request.user.name.split(" ")`
        "name": request.user.name,
        "given_name": request.user.name,
        "preferred_username": request.user.username,
        "nickname": request.user.username,
        # groups is not part of the official userinfo schema, but is a quasi-standard
        "groups": [group.name for group in request.user.ak_groups.all()],
    }
    ```
    </details>

6. Save the edited mapping
