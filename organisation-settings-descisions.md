# Github Org Security Setup

## Prelude
https://best.openssf.org/SCM-BestPractices/github/

## Decisions

// Organisation Level
### 2FA Enforced
**Threat Example**  
If an attacker gets the valid credentials for one of the organization’s users they can authenticate to your GitHub organization.

**Remidiation** 
1. Make sure you have admin permissions
2. Go to the organization settings page
3. Enter “Authentication security” tab
4. Under “Two-factor authentication”
5. Toggle on “Require two-factor authentication for everyone in the organization”
### SSO Required
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

### Default Permissions set to none
**Threat Example**  
Organization members can see the content of freshly created repositories, even if they should be restricted.

**Remidiation**  
1. Make sure you have admin permissions
2. Go to the organization settings page
3. Enter “Member privileges” tab
4. Under “Base permissions”
5. Set permissions to “No permissions”
6. Click “Save”

### Only admins can create public repositories
**Threat Example**  
A member of the organization could inadvertently or maliciously make public an internal repository exposing confidential data.

**Remidiation**
1. Make sure you have admin permissions
2. Go to the organization settings page
3. Enter “Member privileges” tab
4. Under “Repository creation”
5. Toggle off “Public”
6. Click “Save”

### Org should have maximum 3 owner
**Threat Example**  
The more accounts have admin rights on the org the higher the risk of mismanagement.

**Remidiation**
1. Visit the `People` tab
2. Filter `Membership` for `Owner`
3. Audit the list (demote anyone who doesn't need full ownership)

// CI / CD
### Workflows cannot approve pull requests
**Threat Example**  
Attackers can exploit this misconfiguration to bypass code-review restrictions by creating a workflow that approves their own pull request and then merging the pull request without anyone noticing, introducing malicious code that would go straight ahead to production.

**Remidiation**
1. Make sure you have admin permissions
2. Go to the org’s settings page
3. Enter “Actions - General” tab
4. Under ‘Workflow permissions’
5. Uncheck ‘Allow GitHub actions to create and approve pull requests.
6. Click ‘Save’

### Actions restricted to selected repositories
**Threat Example**  
This misconfiguration could lead to the following attack:  
Prerequisite: the attacker is part of your GitHub organization  
Attacker creates new repository in the organization  
Attacker creates a workflow file that reads all organization secrets and exfiltrate them  
Attacker trigger the workflow  
Attacker receives all organization secrets and uses them maliciously  

**Remidiation**  
1. Make sure you have admin permissions
2. Go to the org’s settings page
3. Enter the “Actions - General” tab
4. Under “Policies”
5. Change “All repositories” to “Selected repositories” and select repositories that should be able to run actions
6. Click “Save”

### Actions limited to verified / trusted publishers
**Threat Example**  
This misconfiguration could lead to the following attack:

1. Attacker creates a repository with a tempting but malicious custom GitHub Action
2. An innocent developer / DevOps engineer uses this malicious action
3. The malicious action has access to the developer repository and could steal its secrets or modify its content

**Remidiation**
1. Make sure you have admin permissions
2. Go to the org’s settings page
3. Enter “Actions - General” tab
4. Under “Policies”
5. Select “Allow enterprise, and select non-enterprise, actions and reusable workflows”
6. Check “Allow actions created by GitHub” and “Allow actions by Marketplace verified creators”
7. Set any other used trusted actions under “Allow specified actions and reusable workflows”
8. Click “Save”

### Default workflow token permission is Read-only
**Threat Example**  
In case of token compromise (due to a vulnerability or malicious third-party GitHub actions), an attacker can use this token to sabotage various assets in your CI/CD pipeline, such as packages, pull-requests, deployments, and more.

**Remidiation**  
1. Make sure you have admin permissions
2. Go to the org’s settings page
3. Enter “Actions - General” tab
4. Under ‘Workflow permissions’
5. Select ‘Read repository contents permission’
6. Click ‘Save’

// Repositories
### Repository Defaults (Org wide)
**Threat Example**  
Mainly to prevent deletion, alteration or malicious pushes to main branches.

**Remidiation**  
- Require pull request before merging
  - At least 1 Approval
  - Dismiss stale approvals
  - Require review from a Code Owner
  - Require converstion resolution
- Require status checks to pass
  - Require branches to be up to date before merging
- Restrict force pushes
- Restrict deletions

### No invitation of outside collaborators
**Threat Example**  
Inviting external collaborators could result in a loss of control over proprietary information and potentially expose the organization to security risks, such as data leaks.

**Remidiation**  
1. Make sure you are an enterprise owner
2. Go to the policies page
3. Under the “Repository outside collaborators” section - choose the “Enterprise Owners Only” or the “Organization Owners Only” option
