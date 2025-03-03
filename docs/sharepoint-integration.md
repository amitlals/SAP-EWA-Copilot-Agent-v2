# SharePoint Integration Guide for SAP & Oracle Reports

This guide provides detailed instructions for integrating your SharePoint site with Microsoft Copilot Studio to enable analysis of SAP EWA and Oracle AWR reports.

## Overview

SharePoint serves as the document repository for your technical reports. By connecting Microsoft Copilot Studio to SharePoint, you enable the AI to access, analyze, and extract insights from your SAP and Oracle reports automatically as they are uploaded.

## Prerequisites

Before beginning integration:

- SharePoint site with appropriate permissions
- Document libraries set up for storing reports
- Admin access to Microsoft Power Automate
- SAP EWA and Oracle AWR reports in PDF or HTML format

## Step 1: Prepare Your SharePoint Environment

### Create Dedicated Document Libraries

1. Navigate to your SharePoint site
2. Create the following document libraries:
   - `SAP-EWA-Reports`: For SAP Early Watch Alert reports
   - `Oracle-AWR-Reports`: For Oracle Automatic Workload Repository reports
   - `Processed-Reports`: For storing reports after analysis (optional)

### Configure Document Libraries

For each document library:

1. Add custom columns:
   - `SystemID`: Text field for system identifier
   - `ReportDate`: Date field for report generation date
   - `ProcessStatus`: Choice field (Pending, Processing, Completed, Error)
   - `AnalysisURL`: Hyperlink field for linking to analysis results

2. Create views:
   - "Pending Analysis" - Filtered by ProcessStatus = Pending
   - "Completed Analysis" - Filtered by ProcessStatus = Completed
   - "Error Reports" - Filtered by ProcessStatus = Error

### Set Up Permissions

1. Create a dedicated security group for report processing
2. Grant this group the following permissions:
   - Read and Write access to report document libraries
   - Contribute permission level

## Step 2: Create Power Automate Connection

### Register SharePoint Connection

1. In Microsoft Power Automate, navigate to "Data" → "Connections"
2. Click "New Connection" and select "SharePoint"
3. Authenticate with admin credentials
4. Verify connection status shows "Connected"

### Configure Connection Settings

1. In Power Automate connection settings:
   - Enable "Connection Approval Policy"
   - Set timeout to 120 seconds (for large reports)
   - Enable pagination for large document libraries

## Step 3: Create Main Integration Flow

### Build the Power Automate Flow

Create a flow that triggers when new reports are added to SharePoint:

```
When a new file is created in SharePoint
→ Get file properties
→ Condition: Check if file is a report (based on naming convention or metadata)
   ├── If Yes:
   │   └── Update file properties (Set ProcessStatus = "Processing")
   │   └── Convert file to PDF (if not already)
   │   └── Get file content
   │   └── Call Microsoft Copilot Studio (HTTP request)
   │   └── Update file properties (Set ProcessStatus = "Completed")
   │   └── Send email notification (optional)
   └── If No:
       └── Skip processing
```

Use the template in `templates/power-automate/sharepoint-connector.json` as a starting point.

### Configure Error Handling

Add parallel branches for error handling:

```
When an error occurs in any step
→ Log error details
→ Update file properties (Set ProcessStatus = "Error")
→ Send alert to admin
→ Create retry task (optional)
```

## Step 4: Connect Flow to Microsoft Copilot Studio

### Create Custom Connector

1. In Power Automate, navigate to "Data" → "Custom connectors"
2. Click "New custom connector"
3. Name it "Copilot Studio Report Analyzer"
4. Configure connector:
   - Base URL: Your Copilot Studio API endpoint
   - Authentication: OAuth 2.0
   - Actions: Define "AnalyzeReport" action

### Configure Data Mapping

1. In your flow, add "Parse JSON" action after fetching file content
2. Define schema for SAP and Oracle reports
3. Map the following fields to your Copilot Studio request:
   - `reportType`: Determined from file name or metadata
   - `systemId`: From SharePoint column
   - `reportDate`: From SharePoint column
   - `reportContent`: Base64 encoded file content
   - `analysisType`: "full" (or "summary" for large reports)

## Step 5: Configure Bidirectional Updates

### Report Status Updates

Create a flow that updates SharePoint when Copilot Studio completes analysis:

```
When Microsoft Copilot Studio completes analysis (webhook)
→ Get analysis results
→ Update SharePoint file properties:
   - ProcessStatus = "Completed"
   - AnalysisURL = Link to results
   - Add any extracted metadata
```

### Document Indexing for Knowledge Base

Create a schedule-based flow to keep Copilot's knowledge base updated:

```
Recurrence: Daily at 2:00 AM
→ Get changes in document libraries (since last run)
→ For each changed document:
   → Check if document is already in knowledge base
   → If new/updated:
      → Add/Update in Copilot Studio knowledge base
   → If deleted:
      → Remove from knowledge base
→ Store last run timestamp
```

## Step 6: Test the Integration

1. Upload a sample SAP EWA report to the `SAP-EWA-Reports` library
2. Monitor the flow execution in Power Automate
3. Verify that:
   - The flow triggers correctly
   - The file status updates through the process
   - The Copilot Studio receives the report
   - Analysis results are stored correctly

## Step 7: Set Up Monitoring and Alerts

### Create a Dashboard

1. In Power BI or SharePoint, create a dashboard showing:
   - Reports pending analysis
   - Recently completed analyses
   - Failed processes
   - Average processing time

### Configure Alerts

Set up alerts for:
- Processing failures
- Reports with critical findings
- Processing delays (reports pending > 1 hour)
- System capacity issues

## Best Practices

- **File Naming Convention**: Implement a consistent naming pattern (e.g., `SAP_EWA_SYST01_20250301.pdf`)
- **Version Control**: Use SharePoint versioning for tracking report changes
- **Batching**: Process reports in batches during off-hours for optimal performance
- **Security**: Use service principals instead of user accounts for automation
- **Logging**: Implement comprehensive logging for troubleshooting

## Troubleshooting Common Issues

| Issue | Solution |
|-------|----------|
| Flow fails to trigger | Check SharePoint webhook settings and permissions |
| File content extraction fails | Verify file format support and size limits |
| Connection timeout | Increase timeout settings and implement chunking for large files |
| Permission errors | Verify service account has appropriate SharePoint permissions |
| Duplicate processing | Implement deduplication logic using report metadata |

## Advanced Configuration

### Implementing Delta Processing

For large report repositories:

1. Track the last processed timestamp
2. Use SharePoint's change tracking API
3. Only process new or modified files

### Multi-Environment Support

For organizations with development, test, and production environments:

1. Create environment variables in Power Automate
2. Configure connection references for each environment
3. Use solution packages for deployment between environments

By following this guide, you'll establish a robust integration between SharePoint and Microsoft Copilot Studio, enabling automated analysis of your SAP and Oracle reports.