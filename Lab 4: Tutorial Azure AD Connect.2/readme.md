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

![image](https://github.com/lephuocduc/MyLab-DucLe/assets/37317309/cf161476-7e62-4e67-9abb-c6882b9c6b7d)

4. Connect to Azure AD using **Azure AD Global Administrator** account or **Hybrid Identity Administrator** account for the Azure AD tenant you want to integrate with.

![image](https://github.com/lephuocduc/MyLab-DucLe/assets/37317309/770c9aa6-7f9f-4f42-bed4-e9aeb41fcfa8)

5. Add Alternative UPN suffixes leduc.name.vn

![image](https://github.com/lephuocduc/MyLab-DucLe/assets/37317309/66d3bef8-c8f6-4edc-87e3-ba781a76db02)

6. Connect to AD DS using ***Enterprise Administrator** account for your on-premises Active Directory*

![image](https://github.com/lephuocduc/MyLab-DucLe/assets/37317309/704b0f97-fa8f-4712-ba01-80b2f94a3746)

7. If you see this page, review each domain that's marked **Not Added** or **Not Verified**. Make sure that those domains have been verified in Azure AD. When you've verified your domains, select the **Refresh** icon.

![image](https://github.com/lephuocduc/MyLab-DucLe/assets/37317309/3bde7aec-39b6-4396-ba33-184de15b9992)

8. In **Ready to configure**, select **Install**.

![image](https://github.com/lephuocduc/MyLab-DucLe/assets/37317309/28f2c836-ea6f-4587-a4af-aa30647022c1)

![image](https://github.com/lephuocduc/MyLab-DucLe/assets/37317309/0c11909f-cf32-466a-b28d-c3ddea6463ab)

![image](https://github.com/lephuocduc/MyLab-DucLe/assets/37317309/3d88dfb1-7af3-43ad-af6e-8f57e296aa17)

Finish the configuration

1. Check the status on AAD Connect

![image](https://github.com/lephuocduc/MyLab-DucLe/assets/37317309/95320d7f-5a2e-4998-bf3d-54e388cfa0ce)

***Reference link:***

[Azure AD Connect: Get started by using express settings - Microsoft Entra](https://learn.microsoft.com/en-us/azure/active-directory/hybrid/connect/how-to-connect-install-express)
