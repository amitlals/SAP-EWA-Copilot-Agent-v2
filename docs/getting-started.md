# Getting Started with PCF Controls

This guide will help you get started with using the PCF controls included in this repository.

## What are PCF Controls?

Power Apps Component Framework (PCF) controls allow developers to create custom components that can be used in model-driven apps and canvas apps in Power Apps. These controls extend the standard capabilities of Power Apps and provide enhanced functionality.

## Prerequisites

Before you begin working with PCF controls, ensure you have:

1. **Power Platform Environment**: Access to a Power Platform environment where you can import solutions
2. **Development Tools** (only needed for customization):
   - Node.js (LTS version recommended)
   - Power Apps CLI
   - Visual Studio Code
   - Power Platform Tools extension for VS Code

## Installing Controls

### Option 1: Using Pre-built Solutions

1. Navigate to the `solutions` folder in this repository
2. Download the solution ZIP file
3. Go to your Power Platform environment ([https://make.powerapps.com](https://make.powerapps.com))
4. Go to Solutions and click Import
5. Select the downloaded solution ZIP file
6. Follow the import wizard to complete the installation

### Option 2: Building from Source

1. Clone this repository
2. Navigate to the control folder you want to build
3. Run `npm install` to install dependencies
4. Run `npm run build` to build the control
5. Run `npm run package` to package the control
6. Import the generated solution file into your environment

## Using Controls in Canvas Apps

1. Create or open a Canvas App
2. Insert -> Custom -> Select the installed control
3. Configure the control properties in the property panel
4. Connect the control to your data sources as needed

## Using Controls in Model-driven Apps

1. Open your solution
2. Edit the form where you want to add the control
3. Add the field you want to attach the control to
4. In the field properties, select "Controls" and add the custom control
5. Configure the control properties
6. Save and publish

## Troubleshooting

If you encounter issues with the controls:

1. Check the browser console for any JavaScript errors
2. Verify that the solution was imported correctly
3. Ensure your Power Apps version supports PCF controls
4. For canvas apps, confirm that you've enabled PCF components

## Next Steps

- Explore the documentation for each specific control in the `docs` folder
- Check out the sample apps in the `samples` folder
- Try customizing the controls to fit your specific needs