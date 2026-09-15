# How to setup SAML SSO for a Github Organisation

>[!NOTE] 
> [Single sign-on with Microsoft Entra ID](https://learn.microsoft.com/en-us/entra/identity/saas-apps/github-tutorial)  
> [Entra SSO](https://learn.microsoft.com/en-us/entra/identity/saas-apps/github-tutorial#configure-microsoft-entra-sso)

## Backend Catalog
1. Order a "AD Azure Enterprise App"
2. Choose Organisational Unit
3. `Application Name` is the name, which the application will have in Entra (choose a name that identifies the need)
4. `Application owner` is a shared mailbox updates will get sent to
5. `Application Admin` is the user who can edit the application in Entra using his / her `adm-` account.
6. `Application Security Group` is the group that is allowed to access the App. In other words the group that will have access to the Github org.
7. `Single-sign-on configuration needed` Yes  
  7.1 `Application Identifier Entity` is `https://github.com/orgs/<YOUR ORG NAME>`
  7.2 `Reply URL` is `https://github.com/orgs/ORG/saml/consume`
  7.3 `Sign On URL` is `https://github.com/orgs/ORG/sso`
  7.4 `Relay State` leave empty
  7.5 `Logout URL` leave empty
  7.6 `Attributes & claims` leave empty
8. `Provisioning` No
9. `Kommentar/Ergänzende Informationen` Man kann den Admin auf die Entra Gruppe `srg-cyde-github-organisation` verweisen als Referenz.
