# Github Org Security Setup

## Prelude


## Decisions

### Org Level
#### 2FA Enforced
**Threat Example**  
If an attacker gets the valid credentials for one of the organization’s users they can authenticate to your GitHub organization.

**Remidiation** 
1. Make sure you have admin permissions
2. Go to the organization settings page
3. Enter “Authentication security” tab
4. Under “Two-factor authentication”
5. Toggle on “Require two-factor authentication for everyone in the organization”
#### SSO Required
**Threat Example**  
Not using an SSO solution makes it more difficult to track a potentially compromised user’s actions across different systems, prevents the organization from defining a common password policy, and makes it challenging to audit different aspects of the user’s behavior.

**Remidiation**
1. Make sure you have admin permissions
2. Go to the organization settings page
3. Enter “Authentication security” tab
4. Toggle on “Enable SAML authentication”
5. Order an "Enterprise App" on the [Backend Catalog](https://srgssr.easyvista.com/). You find an example [here](samlsso-order.md)
6. Enter Information ("Sign on URL" ends on `*/saml2`, Issuer starts with `*sts.windows.net*`)
7. Test SAML configuration
8. Once Setup, check "Require SAML SSO authentication..."

#### Default Permissions set to none
**Threat Example**  
Organization members can see the content of freshly created repositories, even if they should be restricted.

**Remidiation**  
1. Make sure you have admin permissions
2. Go to the organization settings page
3. Enter “Member privileges” tab
4. Under “Base permissions”
5. Set permissions to “No permissions”
6. Click “Save”

#### Only admins can create public repositories
**Threat Example**  
A member of the organization could inadvertently or maliciously make public an internal repository exposing confidential data.

**Remidiation**
1. Make sure you have admin permissions
2. Go to the organization settings page
3. Enter “Member privileges” tab
4. Under “Repository creation”
5. Toggle off “Public”
6. Click “Save”

#### Org should have maximum 3 owner
**Threat Example**  
The more accounts have admin rights on the org the higher the risk of mismanagement.

**Remidiation**
1. Visit the `People` tab
2. Filter `Membership` for `Owner`
3. Audit the list (demote anyone who doesn't need full ownership)

### CI / CD
#### Workflows cannot approve pull requests
