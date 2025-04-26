# Creating Conditional Access Policies for MFA and Emergency Accounts

## Introduction

Multi-factor authentication (MFA) is a critical security component for protecting your organization's resources. By requiring users to provide multiple forms of verification when signing in, you significantly reduce the risk of unauthorized access. However, there are scenarios where you need emergency access to your environment, even if normal authentication methods are unavailable. This tutorial will guide you through creating a conditional access policy that requires MFA for all users while also setting up emergency accounts that are exempted from these requirements.

![Emergency Access Accounts](Media/Img/Tutorial4_ConditionalAccess/emergency_access_accounts.png)

## Prerequisites

Before you begin, ensure you have:

- Administrative access to Microsoft Entra ID (formerly Azure AD)
- Global Administrator or Conditional Access Administrator role
- A mobile phone for testing MFA registration
- Basic understanding of Microsoft Entra ID and conditional access concepts

## Understanding Conditional Access and MFA

Conditional Access policies in Microsoft Entra ID act as if-then statements: if a user wants to access a resource, then they must complete an action. For example, if a user wants to access Microsoft 365, then they must complete multi-factor authentication.

Multi-factor authentication provides additional security by requiring two or more of the following verification methods:

1. Something you know (password)
2. Something you have (trusted device, security key)
3. Something you are (biometrics)

## Why Create Emergency Access Accounts?

Emergency access accounts (also known as "break glass" accounts) are necessary for scenarios where:

- Federation services are unavailable, preventing normal sign-in
- MFA services are unavailable
- The last Global Administrator has left the organization
- Natural disasters or other emergencies make normal authentication methods inaccessible
- Approval workflows for privileged roles are unavailable

These accounts should be carefully managed, monitored, and used only when absolutely necessary.

## Part 1: Creating a Conditional Access Policy for MFA

Let's start by creating a conditional access policy that requires MFA for all users:

### Step 1: Access the Microsoft Entra Admin Center

1. Sign in to the [Microsoft Entra admin center](https://entra.microsoft.com) as a Global Administrator or Conditional Access Administrator.
2. In the navigation pane, expand **Protection** and select **Conditional Access**.

### Step 2: Create a New Policy

1. On the **Conditional Access** page, select **Policies**, and then select **+ New policy**.
2. In the **Name** field, enter a descriptive name such as "Require MFA for All Users".

### Step 3: Configure Assignments

1. Under **Assignments**, select **Users and groups**.
2. On the **Include** tab, select **All users**.
3. (Optional) If you want to exclude specific users or groups temporarily during testing, you can use the **Exclude** tab.

### Step 4: Configure Target Resources

1. Under **Target resources**, select **Cloud apps**.
2. On the **Include** tab, select **All cloud apps**.
3. (Optional) If you want to exclude specific apps, you can use the **Exclude** tab.

### Step 5: Configure Access Controls

1. Under **Access controls**, in the **Grant** section, select **Grant access**.
2. Select the checkbox for **Require multi-factor authentication**.
3. Ensure that **Require all the selected controls** is selected.
4. Click **Select**.

### Step 6: Enable the Policy

1. Under **Enable policy**, set the toggle to **On**.
2. Click **Create** to create and enable the policy.

## Part 2: Creating Emergency Access Accounts

Now, let's create emergency access accounts that will be excluded from the MFA requirement:

### Step 1: Create Emergency Access Accounts

1. In the Microsoft Entra admin center, navigate to **Identity** > **Users** > **All users**.
2. Click **+ New user** > **Create new user**.
3. Enter a username following your naming convention for emergency accounts (e.g., `emergency-admin1@yourdomain.onmicrosoft.com`).
4. Enter a strong, complex password.
5. Under **Properties**, fill in required information.
6. Under **Assignments**, assign the **Global Administrator** role.
7. Click **Create** to create the account.
8. Repeat the process to create a second emergency access account.

### Step 2: Configure Strong Authentication for Emergency Accounts

While emergency accounts are excluded from conditional access MFA policies, they should still use strong authentication methods:

1. For each emergency account, consider using one of these methods:
   - FIDO2 security keys (recommended)
   - Certificate-based authentication
   - Hardware OATH tokens

2. To set up a FIDO2 security key:
   - Sign in with the emergency account
   - Navigate to **Security Info**
   - Add a security key as an authentication method
   - Register the physical security key following the prompts

### Step 3: Create an Exclusion Group for Emergency Accounts

1. In the Microsoft Entra admin center, navigate to **Groups** > **All groups**.
2. Click **+ New group**.
3. Set **Group type** to **Security**.
4. Enter a name such as "Emergency Access Accounts".
5. Set **Membership type** to **Assigned**.
6. Add your emergency access accounts to the group.
7. Click **Create**.

### Step 4: Update the MFA Conditional Access Policy

1. Return to **Protection** > **Conditional Access** > **Policies**.
2. Select the MFA policy you created earlier.
3. Under **Assignments**, select **Users and groups**.
4. On the **Exclude** tab, select **Users and groups**.
5. Select the "Emergency Access Accounts" group you created.
6. Click **Select** and then **Save** to update the policy.

![Conditional Access Exclusion](Media/Img/Tutorial4_ConditionalAccess/conditional_access_exclusion.png)

## Part 3: Testing the Configuration

### Testing Regular User MFA

1. Sign in to a Microsoft 365 service (like Outlook Web App) with a regular user account.
2. You should be prompted to set up MFA if not already configured.
3. Complete the MFA setup process:
   - Select a verification method (phone, authenticator app, etc.)
   - Register the method following the prompts
   - Verify that you can sign in successfully with MFA

### Testing Emergency Account Access

1. Sign in to the Microsoft Entra admin center with one of your emergency access accounts.
2. Verify that you can access the portal without being prompted for MFA (beyond the authentication method you configured directly for the account).
3. Sign out when testing is complete.

## Part 4: Monitoring and Managing Emergency Accounts

### Setting Up Monitoring for Emergency Accounts

1. In the Microsoft Entra admin center, navigate to **Monitoring** > **Sign-ins**.
2. Create a filter for your emergency access accounts.
3. Consider setting up alerts for when these accounts are used:
   - Navigate to **Monitoring** > **Audit logs**
   - Configure alerts for sign-in events from emergency accounts

### Regular Review and Validation

1. Schedule regular reviews of your emergency access accounts (at least quarterly).
2. Verify that:
   - The accounts are still accessible
   - The authentication methods still work
   - The accounts are still excluded from appropriate policies
   - The passwords or authentication methods are still secure

## Best Practices for Emergency Access Accounts

1. **Create at least two emergency access accounts** to ensure redundancy.
2. **Use cloud-only accounts** with the `.onmicrosoft.com` domain to avoid dependencies on on-premises systems.
3. **Use strong authentication methods** like FIDO2 security keys.
4. **Store credentials securely** in a physical safe or secure location.
5. **Make emergency access accounts permanent** rather than eligible for just-in-time access.
6. **Exclude emergency accounts from automated processes** that might disable or change them.
7. **Document the emergency access procedure** and ensure multiple administrators know how to use these accounts.
8. **Monitor usage** of emergency access accounts and investigate any unexpected sign-ins.
9. **Test emergency access accounts regularly** to ensure they work when needed.
10. **Review and rotate credentials** periodically according to your security policies.

## Advanced Configuration: Creating a Dedicated Emergency Access Policy

For more granular control, you can create a dedicated conditional access policy for your emergency access accounts:

1. Create a new conditional access policy named "Emergency Access Accounts Policy".
2. Under **Assignments** > **Users and groups**, include only your emergency access accounts group.
3. Under **Target resources**, select **All cloud apps**.
4. Under **Access controls** > **Grant**, select **Grant access** without requiring MFA.
5. Set the policy to **On** and create it.
6. Ensure this policy has a higher priority than your general MFA policy by using the **Reorder** function.

## Troubleshooting

### If Emergency Accounts Still Require MFA

1. Check if there are other conditional access policies affecting these accounts.
2. Verify that the accounts are correctly added to the exclusion group.
3. Check if there are per-user MFA settings enabled for these accounts.
4. Review the sign-in logs to identify which policy is triggering the MFA requirement.

### If Regular Users Aren't Prompted for MFA

1. Verify that the conditional access policy is enabled.
2. Check the policy assignments to ensure users are included.
3. Review the sign-in logs to see if the policy is being evaluated.
4. Test with a different user account or browser session.

## Conclusion

Implementing conditional access policies for MFA while maintaining emergency access accounts is a critical balance between security and operational resilience. By following this tutorial, you've created a robust authentication system that protects your organization's resources while ensuring you can still access your environment in emergency situations.

Remember that emergency access accounts should be used only when absolutely necessary, and their usage should be monitored and audited. Regular testing and validation of both your MFA policies and emergency access procedures will help ensure they work as expected when needed.

## Additional Resources

- [Microsoft Entra Conditional Access documentation](https://learn.microsoft.com/en-us/entra/identity/conditional-access/)
- [Manage emergency access accounts in Microsoft Entra ID](https://learn.microsoft.com/en-us/entra/identity/role-based-access-control/security-emergency-access)
- [Manage users excluded from Conditional Access policies](https://learn.microsoft.com/en-us/entra/id-governance/conditional-access-exclusion)
- [Plan a Microsoft Entra multifactor authentication deployment](https://learn.microsoft.com/en-us/entra/identity/authentication/howto-mfa-getstarted)
