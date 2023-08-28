<aside>
💡 **Prerequisites**:

* **Azure AD
* On-premise AD**
* **Accounts**: You must have an **Azure AD Global Administrator** account or **Hybrid Identity Administrator** account for the Azure AD tenant you want to integrate with. This account must be a *school or organization account* and can't be a *Microsoft account. You must have an **Enterprise Administrator** account for your on-premises Active Directory*

</aside>

**Steps**:

1. Before you start installing Azure AD Connect, [download Azure AD Connect](https://go.microsoft.com/fwlink/?LinkId=615771), and be sure to complete the prerequisite steps in [Azure AD Connect: Hardware and prerequisites](https://learn.microsoft.com/en-us/azure/active-directory/hybrid/connect/how-to-connect-install-prerequisites).
2. Go to *AzureADConnect.msi* and double-click to open the installation file.
3. In **Connect to Azure AD**, enter the username and password of the Hybrid Identity Administrator account, and then select **Next**.

[![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/92c437dc-675d-4e55-b50d-7a838cc6fa41/Untitled.png)](https://www.notion.so/ducle97/Tutorial-Azure-AD-Connect-2dd6b7f36aad4c4d88866201a65e9487?pvs=4#bfbc621b285e468a83cbdb1e071a1365)

4. Connect to Azure AD using **Azure AD Global Administrator** account or **Hybrid Identity Administrator** account for the Azure AD tenant you want to integrate with.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/fd29bacf-864a-499d-8bd6-054a065ce4b7/Untitled.png)

1. Add Alternative UPN suffixes leduc.name.vn

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6c086952-8d64-481e-a386-465951076816/Untitled.png)

1. Connect to AD DS using ***Enterprise Administrator** account for your on-premises Active Directory*

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b0dc54c7-8a69-4146-8182-06b3156bfbf3/Untitled.png)

1. If you see this page, review each domain that's marked **Not Added** or **Not Verified**. Make sure that those domains have been verified in Azure AD. When you've verified your domains, select the **Refresh** icon.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e1251de2-bd57-4223-8863-5aec0d54be6c/Untitled.png)

1. In **Ready to configure**, select **Install**.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ad90c7f9-09ea-4282-bb1d-31461191fdf9/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c04cfad1-34fc-4f23-a9f9-5f660d7dac33/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/555bfc79-a940-457d-80f9-fa6e9f63dc8c/Untitled.png)

Finish the configuration

1. Check the status on AAD Connect

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0621af63-da84-46d0-8f93-042e66c71bae/Untitled.png)

***Reference link:***

[Azure AD Connect: Get started by using express settings - Microsoft Entra](https://learn.microsoft.com/en-us/azure/active-directory/hybrid/connect/how-to-connect-install-express)
