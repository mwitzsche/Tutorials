# Intune Reporting and Monitoring: CSV Export, Power BI Integration, and Data Warehouse

## Introduction

Microsoft Intune provides comprehensive reporting and monitoring capabilities that allow administrators to gain insights into their device management environment. These capabilities include built-in reports, CSV export functionality, Power BI integration, and access to the Intune Data Warehouse. This tutorial will guide you through the various reporting and monitoring features in Intune, with a focus on CSV export, Power BI integration, and the Intune Data Warehouse.

![Intune Reports Graph API](Media/Img/Tutorial3_Reporting/intune_reports_graph_api.png)

## Prerequisites

Before exploring Intune's reporting and monitoring capabilities, ensure you have:

- Microsoft Intune subscription
- Administrative access to the Microsoft Intune admin center
- Power BI Desktop (for Power BI integration)
- Basic understanding of Microsoft Graph API (for advanced reporting)
- Appropriate administrative permissions (at minimum, the Report Reader role)

## Understanding Intune Reporting and Monitoring

Intune offers several ways to monitor and report on your device management environment:

1. **Built-in reports**: Pre-configured reports available in the Microsoft Intune admin center
2. **CSV export**: Ability to export device and app data to CSV files
3. **Power BI integration**: Connect to the Intune Data Warehouse with Power BI for interactive reports
4. **Graph API**: Use Microsoft Graph API to programmatically access and export Intune data
5. **Monitoring dashboards**: Real-time monitoring of device and app status

## Accessing Built-in Reports in Intune

Intune provides several built-in reports that offer insights into your device management environment:

1. Sign in to the [Microsoft Intune admin center](https://intune.microsoft.com).
2. In the navigation pane, select **Reports**.
3. Browse through the available report categories:
   - **Organizational**: Overview of your Intune environment
   - **Operational**: Day-to-day management reports
   - **Device Configuration**: Configuration profile status
   - **Compliance**: Device compliance status
   - **Device Management**: Device enrollment and status
   - **Software Updates**: Update status for managed devices
   - **App Management**: App installation and status

## Monitoring User Activity

Intune allows you to monitor user activity, including sign-in logs and audit information:

1. Sign in to the [Microsoft Intune admin center](https://intune.microsoft.com).
2. Select **Users** from the navigation menu.
3. Select **Sign-in logs** to view user sign-in activity.
4. Select a specific user's sign-in entry to view detailed information, including:
   - Basic info
   - Location
   - Device info
   - Authentication details
   - Conditional Access

To view audit logs for administrative changes:

1. In the Users navigation pane, select **Audit logs**.
2. Browse through the audit entries to see administrative changes made to users.

## Monitoring Device Information

Intune provides detailed device information that can be monitored:

1. Sign in to the [Microsoft Intune admin center](https://intune.microsoft.com).
2. Select **Devices** from the navigation menu.
3. Select **Overview** to see a summary of device status.
4. Select **All devices** to view a list of all managed devices.
5. Select a specific device to view detailed information, including:
   - Hardware inventory
   - Discovered apps
   - Device configuration status
   - Compliance status
   - Security status

## CSV Export Functionality

Intune allows you to export device and app data to CSV files for further analysis:

### Exporting Device List

1. Sign in to the [Microsoft Intune admin center](https://intune.microsoft.com).
2. Select **Devices** > **All devices**.
3. Select **Export** from the top menu.
4. Choose the export format (CSV).
5. The export will be processed, and you'll receive a notification when it's ready to download.

### Exporting App Inventory

1. Sign in to the [Microsoft Intune admin center](https://intune.microsoft.com).
2. Select **Apps** > **Monitor** > **Discovered apps**.
3. Select **Export** from the top menu.
4. The export will be processed, and you'll receive a notification when it's ready to download.

### Exporting Device Compliance Reports

1. Sign in to the [Microsoft Intune admin center](https://intune.microsoft.com).
2. Select **Devices** > **Monitor** > **Device compliance**.
3. Select **Export** from the top menu.
4. Choose the export format (CSV).
5. The export will be processed, and you'll receive a notification when it's ready to download.

## Power BI Integration with Intune Data Warehouse

The Intune Data Warehouse provides a way to access your Intune data for reporting purposes. You can connect to the Data Warehouse using Power BI for interactive, dynamically generated reports.

![Intune Power BI Integration](Media/Img/Tutorial3_Reporting/intune_powerbi_integration.png)

### Prerequisites for Power BI Integration

- Power BI Desktop installed
- Appropriate permissions to access the Intune Data Warehouse
- Microsoft Entra ID credentials with access to Intune

### Using the Power BI Intune Compliance App

1. Navigate to the [Intune Compliance (Data Warehouse)](https://app.powerbi.com/groups/me/getapps/services/microsoftintune.inTuneComplianceDataWarehouse) app in AppSource.
2. Click **Get It Now**, and then click **Continue**.
3. When prompted to install the Power BI app, click **Install**.
4. After installation completes, click on the **Intune Compliance (Data Warehouse)** app tile.
5. Click the **Connect** button.
6. Sign in with a user account that has access to the Intune Data Warehouse.
7. To view the included dashboard, click the **Dashboards** tab, then click the **Compliance Overview** dashboard.
8. To view all available reports, click the **Reports** tab, then click the **Compliance V1.0** report.

### Connecting to the Data Warehouse from the Intune Admin Center

1. Sign in to the [Microsoft Intune admin center](https://intune.microsoft.com).
2. Select **Reports** > **Intune Data warehouse** > **Data warehouse**.
3. Select **Get Power BI App** to access and share pre-created Power BI reports for your tenant in the browser.

### Loading Data in Power BI Using the OData Link

1. Sign in to the [Microsoft Intune admin center](https://intune.microsoft.com).
2. Select **Reports** > **Intune Data warehouse** > **Data warehouse**.
3. Retrieve the custom feed URL from the reporting blade.
4. Open **Power BI Desktop**.
5. Choose **File** > **Get Data** > **OData feed**.
6. Choose **Basic**.
7. Paste the OData URL into the URL box.
8. Select **OK**.
9. Authenticate with Microsoft Entra ID for your tenant.
10. Select **Load**.

### Available Data in the Intune Data Warehouse

The Intune Data Warehouse provides access to the following types of data:

- Devices
- Enrollment
- App protection policy
- Compliance policy
- Device configuration profiles
- Software updates
- Device inventory logs

## Using Microsoft Graph API for Advanced Reporting

For more advanced reporting needs, you can use the Microsoft Graph API to programmatically access and export Intune data.

### Available Reports via Graph API

Microsoft Intune provides many reports that can be exported using Graph APIs. The following are some of the available reports:

- Device Compliance
- Device Non-Compliance
- All Devices List
- Feature Update Policy Failures
- Defender Agents
- Active Malware
- All Apps List
- App Install Status
- Device Install Status By App
- User Install Status By App
- Discovered Apps

### Exporting Reports Using Graph API

To export Intune reports using Graph API:

1. Use the following endpoint:
   ```
   https://graph.microsoft.com/beta/deviceManagement/reports/exportJobs
   ```

2. Create a POST request with the following JSON body:
   ```json
   {
     "reportName": "Devices",
     "filter": "",
     "select": ["DeviceName", "OSVersion", "LastContact"],
     "format": "csv"
   }
   ```

3. Replace `"reportName"` with the name of the report you want to export (e.g., "Devices", "DeviceCompliance", "AllAppsList").

4. Monitor the status of the export job using the returned job ID.

5. Once the export is complete, download the report using the provided URL.

## Intune Warehouse Data Model

The Intune Data Warehouse organizes data into several entities that can be used for reporting:

1. **Date Entity**: Represents dates for other entities in the data model
2. **User Entity**: Contains user properties
3. **Device Entity**: Contains device properties
4. **App Entity**: Contains app properties
5. **Policy Entity**: Contains policy properties
6. **Compliance Entity**: Contains compliance check results
7. **Configuration Entity**: Contains configuration profile results

Understanding this data model is essential for creating custom reports in Power BI.

## Creating Custom Reports in Power BI

Once you've connected to the Intune Data Warehouse, you can create custom reports in Power BI:

1. In Power BI Desktop, select the tables you want to use in your report.
2. Create relationships between tables if they don't already exist.
3. Create visualizations by dragging fields onto the report canvas.
4. Add filters to focus on specific data points.
5. Create calculated columns and measures for advanced analysis.
6. Format your report for better readability.
7. Save and publish your report to Power BI Service for sharing.

### Example: Creating a Device Compliance Report

1. Connect to the Intune Data Warehouse using the OData link.
2. Select the **devices** and **deviceComplianceStatus** tables.
3. Create a relationship between the tables using the **deviceId** field.
4. Create a pie chart showing the distribution of compliant vs. non-compliant devices.
5. Add a table showing details of non-compliant devices.
6. Add filters for device platform and ownership.
7. Save and publish the report.

## Monitoring Intune with Dashboards

Intune provides several dashboards for monitoring your environment:

### Device Overview Dashboard

1. Sign in to the [Microsoft Intune admin center](https://intune.microsoft.com).
2. Select **Devices** > **Overview**.
3. View the dashboard showing device enrollment status, compliance status, and platform distribution.

### App Overview Dashboard

1. Sign in to the [Microsoft Intune admin center](https://intune.microsoft.com).
2. Select **Apps** > **Overview**.
3. View the dashboard showing app installation status, app protection policy status, and app inventory.

### Compliance Dashboard

1. Sign in to the [Microsoft Intune admin center](https://intune.microsoft.com).
2. Select **Devices** > **Monitor** > **Device compliance**.
3. View the dashboard showing compliance status by platform, policy, and setting.

## Best Practices for Intune Reporting and Monitoring

1. **Regular Monitoring**: Set up a schedule to regularly review key reports and dashboards.
2. **Export Critical Data**: Regularly export critical data to CSV for historical tracking and backup.
3. **Create Custom Reports**: Use Power BI to create custom reports tailored to your organization's needs.
4. **Automate Report Generation**: Use Microsoft Graph API to automate the generation and distribution of reports.
5. **Monitor Trends**: Look for trends in device compliance, app installation, and user activity.
6. **Set Up Alerts**: Configure alerts for critical events, such as high numbers of non-compliant devices.
7. **Document Reporting Procedures**: Create documentation for your reporting procedures to ensure consistency.
8. **Limit Access to Reports**: Use role-based access control to limit access to sensitive reports.
9. **Validate Data**: Regularly validate the data in your reports to ensure accuracy.
10. **Update Reports**: Update your reports as your Intune environment evolves.

## Troubleshooting Reporting Issues

If you encounter issues with Intune reporting and monitoring, consider the following troubleshooting steps:

1. **Check Permissions**: Ensure you have the appropriate permissions to access reports and data.
2. **Verify Data Sync**: Check that data is syncing properly between Intune and the Data Warehouse.
3. **Check Export Status**: Verify the status of export jobs in the Microsoft Intune admin center.
4. **Update Power BI**: Ensure you're using the latest version of Power BI Desktop.
5. **Check OData URL**: Verify that the OData URL for the Data Warehouse is correct.
6. **Check Graph API Calls**: Ensure your Graph API calls are properly formatted.
7. **Review Documentation**: Consult the Microsoft documentation for updates or changes to reporting features.

## Conclusion

Intune's reporting and monitoring capabilities provide valuable insights into your device management environment. By leveraging built-in reports, CSV export functionality, Power BI integration, and the Intune Data Warehouse, you can gain a comprehensive understanding of your device and app landscape.

This tutorial has covered the essential aspects of Intune reporting and monitoring, with a focus on CSV export, Power BI integration, and the Intune Data Warehouse. By following the procedures and best practices outlined here, you can effectively monitor and report on your Intune environment, making informed decisions to improve your device management strategy.

## Additional Resources

- [Microsoft Intune documentation](https://learn.microsoft.com/en-us/mem/intune/)
- [Intune reports and properties using Graph API](https://learn.microsoft.com/en-us/intune/intune-service/fundamentals/reports-export-graph-available-reports)
- [Connect to the Data Warehouse with Power BI](https://learn.microsoft.com/en-us/intune/intune-service/developer/reports-proc-get-a-link-powerbi)
- [Power BI Desktop](https://powerbi.microsoft.com/desktop/)
- [Microsoft Graph API documentation](https://learn.microsoft.com/en-us/graph/api/overview)
