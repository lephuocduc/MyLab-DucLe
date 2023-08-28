<aside>
💡 **Prerequisites**:

1. A working Azure AD tenant with at least an Azure AD free or trial license enabled. In the Free tier, SSPR only works for cloud users in Azure AD. Password change is supported in the Free tier, but password reset is not. For later tutorials in this series, you'll need an **Azure AD Premium P1** or trial license for on-premises password writeback.
2. An account with ***Global Administrator*** or ***Authentication Policy Administrator*** privileges.
3. Enable **password writeback** in Azure AD Connect
4. Enabled **MFA. MFA** can be enabled by one of these methods
    1. Enable MFA per user
    2. Azure Identity Protection
    3. Conditional Access Policy
5. **Minimum password age** is set to **0** day in GPO
</aside>



For later tutorials in this series, you'll need an Azure AD Premium P1 or trial license for on-premises password writeback.



1. **Enable SSRP for all users or for specific groups**

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/58922466-0068-4f7b-94ae-f6575f0be9c6/Untitled.png)

1. **Select authentication methods and registration options*:*** When users need to unlock their account or reset their password, they're prompted for another confirmation method. This extra authentication factor makes sure that Azure AD finished only approved SSPR events

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1b8c99ec-f925-4576-816a-43bf2a298c03/Untitled.png)

Before users can unlock their account or reset a password, they must register their contact information. Azure AD uses this contact information for the different authentication methods set up in the previous steps

1. Make sure you enabled **password writeback** in Azure AD Connect

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/04bb06a1-6e97-414b-b166-ffc36c1c4dfc/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5c2bcf8c-9e14-427c-969f-2b0d3eb57911/Untitled.png)

1. Set **Minimum password age** to **0** day

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/bc4ab88a-549b-46aa-bb35-d4867205e927/Untitled.png)

**TESTING SSRP**

1. Sign in a test user to implement SSRP or access *https://aka.ms/ssprsetup* to ***s*etup some information for the MFA**

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ef25fcfc-c49f-49c2-9eba-c522a57cbd3a/Untitled.png)

1. Access to *https://aka.ms/sspr* to self reset password. Enter your non-administrator test users' account information, like *testuser*, the characters from the CAPTCHA, and then select **Next**.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/099a9367-89bd-4f7a-b901-751f246b3887/Untitled.png)

1. **Follow the verification steps to reset your password**. When finished, you'll receive an email notification that your password was reset.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3edab29f-4a91-456b-92b9-cd490d1cbbc7/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c07ac4b3-9224-4dc2-a12f-17ed2c9ce0a7/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/262efe25-7a32-4e84-81ce-2a5dd80b0b2a/Untitled.png)

***Reference link:***

[Enable Azure Active Directory self-service password reset - Microsoft Entra](https://learn.microsoft.com/en-us/azure/active-directory/authentication/tutorial-enable-sspr?fbclid=IwAR3QrE5bPx9X_qMf3FCinaHEYGVZqq2sve8pwvVDQWbLUFohKd0_1VFvpg8)
