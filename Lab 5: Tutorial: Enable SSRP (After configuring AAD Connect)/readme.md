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

![image](https://github.com/lephuocduc/MyLab-DucLe/assets/37317309/c3ec4baa-70b9-4e4a-86d1-1ecf2d78dde5)

For later tutorials in this series, you'll need an Azure AD Premium P1 or trial license for on-premises password writeback.
![image](https://github.com/lephuocduc/MyLab-DucLe/assets/37317309/2c6e5093-a2d8-495f-9c65-9fabb5639ff2)

1. **Enable SSRP for all users or for specific groups**

![image](https://github.com/lephuocduc/MyLab-DucLe/assets/37317309/761fecca-5fd7-49e5-bb56-21b4f5acf8df)

2. **Select authentication methods and registration options*:*** When users need to unlock their account or reset their password, they're prompted for another confirmation method. This extra authentication factor makes sure that Azure AD finished only approved SSPR events

![image](https://github.com/lephuocduc/MyLab-DucLe/assets/37317309/7f09383b-25f5-4956-875f-f9d1c31d4ae6)

Before users can unlock their account or reset a password, they must register their contact information. Azure AD uses this contact information for the different authentication methods set up in the previous steps

3. Make sure you enabled **password writeback** in Azure AD Connect

![image](https://github.com/lephuocduc/MyLab-DucLe/assets/37317309/a146c71c-6732-4506-9ffa-7ba6699aea12)

![image](https://github.com/lephuocduc/MyLab-DucLe/assets/37317309/642a1b4a-89fb-495b-9c06-70f8b0257354)

4. Set **Minimum password age** to **0** day

![image](https://github.com/lephuocduc/MyLab-DucLe/assets/37317309/4b6d0e0b-1d60-4096-af55-a5629c586548)

**TESTING SSRP**

1. Sign in a test user to implement SSRP or access *https://aka.ms/ssprsetup* to ***s*etup some information for the MFA**

![image](https://github.com/lephuocduc/MyLab-DucLe/assets/37317309/2de5ff58-83a3-4bd3-8d50-536d61ebd7de)

2. Access to *https://aka.ms/sspr* to self reset password. Enter your non-administrator test users' account information, like *testuser*, the characters from the CAPTCHA, and then select **Next**.

![image](https://github.com/lephuocduc/MyLab-DucLe/assets/37317309/f3ce85b8-6ab5-4eb3-a499-6e46265392ad)

3. **Follow the verification steps to reset your password**. When finished, you'll receive an email notification that your password was reset.

![image](https://github.com/lephuocduc/MyLab-DucLe/assets/37317309/15b2f6cd-ba4e-4480-9721-be1726c98de7)

![image](https://github.com/lephuocduc/MyLab-DucLe/assets/37317309/32a99e1d-221e-485b-9fe8-65ed51ee74b1)

![Uploading image.png…]()

***Reference link:***

[Enable Azure Active Directory self-service password reset - Microsoft Entra](https://learn.microsoft.com/en-us/azure/active-directory/authentication/tutorial-enable-sspr?fbclid=IwAR3QrE5bPx9X_qMf3FCinaHEYGVZqq2sve8pwvVDQWbLUFohKd0_1VFvpg8)
