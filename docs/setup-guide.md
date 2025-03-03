# Microsoft Copilot Studio Setup Guide for SAP & Oracle Report Analysis

This comprehensive guide walks you through the process of setting up Microsoft Copilot Studio to analyze SAP Early Watch Alert (EWA) and Oracle Automatic Workload Repository (AWR) reports.

## Prerequisites

Before you begin:

1. **Microsoft Copilot Studio Access**:
   - Ensure you have a Microsoft Copilot Studio license or trial
   - Admin access to your Microsoft 365 tenant

2. **Azure Subscription**:
   - Active Azure subscription
   - Permissions to create resources and register applications

3. **Data Sources**:
   - SharePoint site for storing reports
   - Access to SAP and/or Oracle systems generating reports

4. **Technical Knowledge**:
   - Basic understanding of Power Platform
   - Familiarity with SAP EWA and Oracle AWR reports

## Step 1: Set Up Microsoft Copilot Studio Environment

### Create a Copilot Environment

1. Navigate to [Microsoft Copilot Studio](https://copilotstudio.microsoft.com/)
2. Sign in with your Microsoft account
3. Click "Create a Copilot"
4. Enter details:
   - Name: "SAP-Oracle Report Analyzer"
   - Description: "AI-powered analysis of SAP EWA and Oracle AWR reports"
   - Language: Select your preferred language
5. Click "Create"

### Configure Base Settings

1. In your newly created Copilot, navigate to "Settings"
2. Under "General", configure:
   - Session timeout: Recommend 30 minutes for lengthy report analysis
   - User identification: Enable for personalized insights
   - Analytics: Enable to track usage patterns and improve the Copilot

## Step 2: Set Up Knowledge Base

The knowledge base is crucial for your Copilot to understand the structure and content of SAP and Oracle reports.

### Create Knowledge Base

1. In your Copilot, navigate to "Knowledge Base" → "Add"
2. Enter details:
   - Name: "Technical Reports Knowledge Base"
   - Description: "Knowledge source for SAP EWA and Oracle AWR reports"
3. Click "Create"

### Add Sample Documents

1. Upload sample SAP EWA and Oracle AWR reports from the `templates/sample-reports/` directory
2. Configure parsing settings:
   - Document chunking: Enable with medium chunk size
   - Language detection: Automatic
   - OCR processing: Enable for scanned PDF documents

### Configure AI Model Settings

1. In Knowledge Base settings, navigate to "AI Model"
2. Configure:
   - Response length: Medium to long (for detailed analyses)
   - Temperature: 0.3-0.5 (lower for more precise technical responses)
   - Top P: 0.95 (recommended for technical content)
   - Frequency penalty: 0.5 (balanced for technical reporting)
   - Presence penalty: 0.5 (balanced for consistent responses)

## Step 3: Connect to SharePoint for Live Reports

### Create SharePoint Connection

1. In Microsoft Copilot Studio, navigate to "Data" → "Add data source"
2. Select "SharePoint" as the data source
3. Follow the connection wizard:
   - Enter SharePoint site URL where reports are stored
   - Authenticate with appropriate credentials
   - Select document libraries containing reports
4. Configure refresh settings:
   - Schedule: Daily (or based on your report generation schedule)
   - Notification: Enable for refresh failures

### Set Up Power Automate Flow

For dynamic report processing, create a Power Automate flow:

1. Navigate to [Power Automate](https://flow.microsoft.com)
2. Create a new automated flow
3. Use the template from `templates/power-automate/sharepoint-connector.json`
4. Configure triggers:
   - When a new file is added to SharePoint
   - When a file is modified in SharePoint
5. Add actions:
   - Parse document based on file type
   - Extract key report sections
   - Trigger Copilot Studio knowledge base refresh

## Step 4: Create Topics and Entities

### Define Core Entities

1. In Copilot Studio, navigate to "Entities"
2. Create the following entities:
   - "ReportType": SAP EWA, Oracle AWR
   - "SystemIdentifier": For system IDs
   - "TimeFrame": For report time periods
   - "PerfomanceMetric": For specific metrics (CPU, Memory, I/O, etc.)
   - "ThresholdStatus": Critical, Warning, Normal

### Create Main Topics

1. Navigate to "Topics"
2. Create essential topics:
   - "Analyze Latest Report"
   - "Find Performance Issues"
   - "Compare Time Periods"
   - "System Recommendations"
   - "Alert Configuration"
   
3. For each topic:
   - Add trigger phrases
   - Configure entities
   - Set up conversations flows
   - Connect to knowledge base

### Create Workflows

1. Navigate to "Workflows"
2. Import the workflow templates from `templates/copilot-studio/workflow-templates.json`
3. Customize workflows for your specific environment:
   - Set up decision branches based on report types
   - Configure response templates for different scenarios
   - Add conditional logic for alert severity levels

## Step 5: Configure Authentication

For secure access to reports and systems:

1. Follow the detailed instructions in `docs/user-authentication.md`
2. Key steps include:
   - App registration in Azure AD
   - Setting up client credentials
   - Configuring API permissions
   - Implementing secure token handling

## Step 6: Test Your Implementation

1. Navigate to "Test your bot" in Copilot Studio
2. Try sample queries:
   - "Analyze the latest SAP EWA report for system PROD"
   - "What are the critical performance issues in last week's Oracle AWR report?"
   - "Compare memory usage between last month and current reports"
   - "Generate recommendations for improving database performance"
   
2. Validate responses against expected outcomes
3. Make adjustments to topics, workflows, and AI settings as needed

## Step 7: Deploy to Production

When testing is complete:

1. Navigate to "Publish"
2. Choose your deployment channels:
   - Microsoft Teams
   - SharePoint
   - Website
   - Mobile app
3. Configure channel-specific settings
4. Publish your Copilot

## Step 8: Monitor and Optimize

After deployment:

1. Review analytics in Copilot Studio dashboard
2. Track key metrics:
   - Usage patterns
   - Successful vs. failed interactions
   - Most common queries
3. Use insights to continuously improve your Copilot:
   - Add new topics for common questions
   - Refine AI responses for accuracy
   - Update knowledge base with new report templates

## Troubleshooting

| Issue | Possible Solution |
|-------|-------------------|
| Copilot not recognizing report format | Update knowledge base with more sample reports |
| Authentication failures | Verify Azure AD permissions and token configuration |
| Inaccurate answers | Adjust AI model parameters or add more specific topics |
| Performance issues | Optimize document chunking and processing settings |

## Next Steps

- Set up alerts for critical findings
- Implement scheduled report summaries
- Create custom dashboards for visualizing insights
- Train technical team on advanced interaction techniques

For additional assistance, refer to the other guides in the `docs/` directory or contact support through the Microsoft Copilot Studio portal.