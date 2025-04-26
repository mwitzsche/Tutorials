# Deploying Company Portal App and Integrating with Autopilot ESP

## Introduction

The Microsoft Company Portal app is a critical component of the Microsoft Intune device management ecosystem. It provides users with a self-service portal to install approved applications, manage their devices, and access company resources. When combined with Windows Autopilot and the Enrollment Status Page (ESP), you can create a seamless deployment experience that ensures devices are properly configured before users can access them.

This tutorial will guide you through the process of deploying the Company Portal app from the Windows App Store and integrating it with Windows Autopilot ESP, including the White Glove pre-provisioning mode.

![Company Portal Autopilot](Media/Img/Tutorial5_CompanyPortal/company_portal_autopilot.png)

## Prerequisites

Before you begin, ensure you have:

- Microsoft Intune subscription with administrative access
- Windows 10/11 devices registered for Windows Autopilot
- Appropriate licenses for users
- Access to the Microsoft Intune admin center
- Basic understanding of Windows Autopilot and Intune app deployment

## Understanding the Company Portal App

The Company Portal app serves several important functions in the Intune ecosystem:

1. **Application Management**: Allows users to install company-approved applications
2. **Device Management**: Enables users to enroll and manage their devices
3. **Resource Access**: Provides access to company resources and support
4. **Compliance Status**: Shows users their device compliance status

When deployed as part of the Windows Autopilot process, the Company Portal app can be installed before the user even signs in, ensuring it's available immediately after setup.

## Part 1: Adding the Company Portal App to Intune

### Step 1: Access the Microsoft Intune Admin Center

1. Sign in to the [Microsoft Intune admin center](https://intune.microsoft.com) with your admin account.
2. Select **Apps** from the left navigation menu.
3. In the **Apps** page, select **All apps**.

### Step 2: Add the Company Portal App

1. In the **All apps** page, select **Add**.
2. In the **Select app type** pane, under **Store app**, select **Microsoft Store app (new)**.
3. Click **Select** at the bottom of the page.
4. Select **Search the Microsoft Store app (new)**.
5. In the search box, enter **Company Portal**.
6. Select **Company Portal** from the search results.
7. Click **Select** at the bottom of the page.

### Step 3: Configure App Settings

1. In the **App information** tab, review the app details:
   - Name: Company Portal
   - Publisher: Microsoft Corporation
   - Category: Business
2. Change **Install behavior** to **System** to ensure the app is installed in the system context rather than per-user.
3. Set **Show this as a featured app in the Company Portal** to **Yes** if you want it to be prominently displayed.
4. Click **Next** to continue.

### Step 4: Configure Scope Tags (Optional)

1. Add any scope tags if you use them to organize your Intune resources.
2. Click **Next** to continue.

### Step 5: Assign the App

1. In the **Assignments** tab, under **Required**, select **Add group**.
2. Select the groups to which you want to assign the Company Portal app.
   - For Autopilot deployment, assign it to your Autopilot device groups.
   - Consider creating a dynamic device group that includes all your Windows 10/11 devices.
3. Click **Select** to add the groups.
4. Click **Next** to continue.

### Step 6: Review and Create

1. Review all the settings you've configured.
2. Click **Create** to add the Company Portal app to Intune.

## Part 2: Configuring the Enrollment Status Page (ESP)

The Enrollment Status Page (ESP) displays the provisioning status to users during device setup and can be configured to block device use until all required policies and applications, including the Company Portal, are installed.

![Enrollment Status Page](Media/Img/Tutorial5_CompanyPortal/enrollment_status_page.png)

### Step 1: Access ESP Settings

1. Sign in to the [Microsoft Intune admin center](https://intune.microsoft.com).
2. Navigate to **Devices** > **Enrollment** > **Windows enrollment**.
3. Select the **Windows** tab.
4. Under **Windows Autopilot**, select **Enrollment Status Page**.

### Step 2: Create a New ESP Profile

1. Click **Create** to create a new ESP profile.
2. In the **Basics** tab:
   - Enter a descriptive **Name** for the profile (e.g., "Standard ESP with Company Portal").
   - Add an optional **Description**.
3. Click **Next** to continue.

### Step 3: Configure ESP Settings

1. In the **Settings** tab, configure the following:
   - **Show app and profile configuration progress**: Set to **Yes** to display installation progress.
   - **Show an error when installation takes longer than specified number of minutes**: Set an appropriate timeout (e.g., 60 minutes).
   - **Show custom message when time limit or error occur**: Optionally set to **Yes** and provide a custom message.
   - **Turn on log collection and diagnostics page for end users**: Set to **Yes** for troubleshooting purposes.
   - **Only show page to devices provisioned by out-of-box experience (OOBE)**: Choose based on your requirements.
   - **Block device use until all apps and profiles are installed**: Set to **Yes** to ensure all required apps are installed.
   - **Allow users to reset device if installation error occurs**: Set based on your organization's policies.
   - **Allow users to use device if installation error occurs**: Set based on your organization's policies.

2. For **Block device use until these required apps are installed if they are assigned to the user/device**:
   - Select **Selected**.
   - Click **Select apps**.
   - In the app list, select **Company Portal** and any other critical apps.
   - Click **Select** to add the apps to the blocking list.

3. If you're using pre-provisioning (White Glove), set **Only fail selected blocking apps in technician phase** based on your requirements:
   - **No**: All apps must be installed during pre-provisioning.
   - **Yes**: Only blocking apps must be installed during pre-provisioning.

4. Click **Next** to continue.

### Step 4: Assign the ESP Profile

1. In the **Assignments** tab, select **Add groups**.
2. Select the groups that should receive this ESP profile.
   - For Autopilot deployment, assign it to your Autopilot device groups.
   - Consider creating a dynamic device group that includes all your Windows 10/11 devices.
3. Click **Select** to add the groups.
4. Click **Next** to continue.

### Step 5: Add Scope Tags (Optional)

1. Add any scope tags if you use them to organize your Intune resources.
2. Click **Next** to continue.

### Step 6: Review and Create

1. Review all the settings you've configured.
2. Click **Create** to create the ESP profile.

## Part 3: Integrating with Windows Autopilot

Now that you've added the Company Portal app and configured the ESP, you need to ensure they're properly integrated with your Windows Autopilot deployment.

### Step 1: Configure Windows Autopilot Deployment Profile

1. In the Microsoft Intune admin center, navigate to **Devices** > **Enrollment** > **Windows enrollment**.
2. Under **Windows Autopilot**, select **Deployment Profiles**.
3. Select an existing profile or create a new one.
4. Ensure the profile is configured for your desired deployment scenario:
   - User-driven mode (standard)
   - Self-deploying mode
   - Pre-provisioning (White Glove)
5. Make sure the profile is assigned to the appropriate device groups.

### Step 2: Verify ESP Assignment

1. In the deployment profile, ensure that **Show the Enrollment Status Page** is set to **Yes**.
2. Verify that the ESP profile you created is assigned to the same groups as your Autopilot deployment profile.

## Part 4: Using White Glove Pre-Provisioning

White Glove (pre-provisioning) allows IT staff, partners, or OEMs to pre-configure devices before delivering them to end users. This process installs device-targeted apps and policies, including the Company Portal, before the user receives the device.

### Step 1: Prepare for White Glove Provisioning

1. Ensure your devices are registered for Windows Autopilot.
2. Verify that your Autopilot deployment profile is configured correctly.
3. Make sure the Company Portal app is assigned as a required app to your Autopilot device groups.
4. Confirm that your ESP profile is properly configured and assigned.

### Step 2: Initiate White Glove Provisioning

1. Boot the device to be provisioned.
2. At the first OOBE screen, press **Windows key + Shift + F10** to access the pre-provisioning options.
3. Press the **Windows key** five times to open the Windows Autopilot pre-provisioning screen.
4. Select **Windows Autopilot provisioning**.
5. On the authentication screen, sign in with a technician account that has appropriate permissions.
6. Select **Pre-provision** to begin the process.

### Step 3: Monitor the Pre-Provisioning Process

1. The device will connect to your tenant and begin downloading and applying policies and apps.
2. The ESP will display the progress of app installations, including the Company Portal.
3. Once all required apps and policies are installed, the pre-provisioning process will complete.
4. The device will be ready to be shipped to the end user.

### Step 4: End-User Experience

When the end user receives the device:

1. They will power on the device and begin the OOBE process.
2. They will sign in with their credentials.
3. The ESP will display, showing the progress of user-specific configurations.
4. Once complete, the user will have access to the desktop with the Company Portal already installed.
5. The user can open the Company Portal to access additional apps and resources.

## Part 5: Testing the Deployment

Before rolling out to all users, it's important to test the deployment process to ensure everything works as expected.

### Step 1: Test with a Pilot Device

1. Select a test device that is registered for Windows Autopilot.
2. Assign it to your Autopilot deployment profile and ESP profile.
3. Reset the device to begin the Autopilot process.
4. Monitor the deployment to ensure:
   - The ESP displays correctly
   - The Company Portal app installs successfully
   - The device is properly configured before the user gains access

### Step 2: Verify Company Portal Functionality

1. After the device setup is complete, sign in as a test user.
2. Locate and open the Company Portal app.
3. Verify that the app launches correctly and displays the expected content.
4. Test installing an app from the Company Portal to ensure it works properly.

### Step 3: Gather Feedback and Make Adjustments

1. Collect feedback from test users about the deployment experience.
2. Make any necessary adjustments to your ESP profile or app assignments.
3. Test again with the updated configuration before rolling out to all users.

## Part 6: Monitoring and Troubleshooting

### Monitoring App Installation Status

1. In the Microsoft Intune admin center, navigate to **Apps** > **Monitor** > **App install status**.
2. Select the Company Portal app to view its installation status across your devices.
3. Review the status to identify any devices where installation failed.

### Troubleshooting Common Issues

#### Company Portal Not Installing During ESP

1. Verify that the Company Portal app is assigned as a required app to the device group.
2. Check that the app is included in the blocking apps list in your ESP profile.
3. Ensure the ESP timeout is long enough to allow for app installation.
4. Review the device's enrollment status in Intune to identify any issues.

#### ESP Timeout or Errors

1. Increase the timeout value in your ESP profile if installations are taking too long.
2. Enable log collection in the ESP profile to gather diagnostic information.
3. Review the logs to identify the specific issue causing the failure.
4. Check network connectivity and ensure the device can reach all required endpoints.

#### White Glove Pre-Provisioning Issues

1. Ensure the technician account has appropriate permissions.
2. Verify that the device meets the hardware requirements for pre-provisioning.
3. Check network connectivity during the pre-provisioning process.
4. Review the ESP logs to identify any specific issues.

## Best Practices

1. **Assign the Company Portal as a System App**: Always set the install behavior to "System" to ensure it's installed for all users on the device.

2. **Include in ESP Blocking Apps**: Add the Company Portal to your ESP blocking apps list to ensure it's installed before the user can access the device.

3. **Test Thoroughly**: Always test your deployment process with a small group of devices before rolling out to all users.

4. **Monitor Deployment**: Regularly check the installation status of the Company Portal app to identify and address any issues.

5. **Update Regularly**: Keep the Company Portal app updated to ensure users have access to the latest features and security improvements.

6. **Document Your Configuration**: Maintain documentation of your ESP and Autopilot configurations for future reference and troubleshooting.

7. **Use Dynamic Groups**: Create dynamic device groups based on device properties to automatically assign profiles and apps to new devices.

8. **Optimize Pre-Provisioning**: For White Glove scenarios, carefully select which apps are critical to install during pre-provisioning to minimize provisioning time.

## Conclusion

Deploying the Company Portal app through Windows Autopilot with ESP integration provides a seamless and controlled deployment experience for your users. By ensuring the Company Portal is installed during the device setup process, you give users immediate access to the tools and resources they need to be productive.

The White Glove pre-provisioning option further enhances this experience by allowing IT staff to prepare devices before they reach end users, reducing the time users spend waiting for device setup to complete.

By following the steps and best practices outlined in this tutorial, you can successfully deploy the Company Portal app as part of your Windows Autopilot process, creating a consistent and efficient deployment experience for your organization.

## Additional Resources

- [Add and assign the Windows Company Portal app for Intune managed devices](https://learn.microsoft.com/en-us/mem/intune-service/apps/store-apps-company-portal-autopilot)
- [Set up the Enrollment Status Page in the admin center](https://learn.microsoft.com/en-us/intune/intune-service/enrollment/windows-enrollment-status)
- [Windows Autopilot Enrollment Status Page](https://learn.microsoft.com/en-us/autopilot/enrollment-status)
- [Windows Autopilot for pre-provisioned deployment](https://learn.microsoft.com/en-us/autopilot/pre-provision)
- [Troubleshoot the Enrollment Status Page](https://learn.microsoft.com/en-us/troubleshoot/mem/intune/device-enrollment/troubleshoot-windows-enrollment-status-page)
