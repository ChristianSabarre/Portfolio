# Power BI Dashboard Integration Guide

## 🚀 How to Connect Your Power BI Dashboard

### Step 1: Prepare Your Power BI Report

1. **Publish your report to Power BI Service**
   - Open Power BI Desktop
   - Create/open your dashboard
   - Click "Publish" → Select your workspace
   - Note down the Report ID from the URL

2. **Enable Embedding**
   - Go to Power BI Service (app.powerbi.com)
   - Navigate to your report
   - Click "File" → "Embed" → "Website or portal"
   - Copy the embed code

### Step 2: Get Your Embed URL

Your Power BI embed URL will look like this:
```
https://app.powerbi.com/reportEmbed?reportId=YOUR_REPORT_ID&autoAuth=true&ctid=YOUR_TENANT_ID
```

**To find your Report ID:**
1. Open your report in Power BI Service
2. Look at the URL: `https://app.powerbi.com/groups/workspace-id/reports/REPORT_ID`
3. Copy the `REPORT_ID` part

**To find your Tenant ID:**
1. Go to Azure Active Directory admin center
2. Or use this URL format if you know your organization domain:
   ```
   https://login.microsoftonline.com/YOUR_DOMAIN.onmicrosoft.com/v2.0/.well-known/openid_configuration
   ```

### Step 3: Update the Website Code

#### Option A: Simple Iframe Embed (Public Reports)

Replace the placeholder in the HTML file:

```html
<!-- Find this section in index.html around line 490 -->
<div id="powerbi-container" class="w-full h-full rounded-lg overflow-hidden border border-gray-300 dark:border-gray-600">
    <!-- Replace the placeholder div with this iframe -->
    <iframe 
        title="Your Dashboard Name" 
        width="100%" 
        height="600" 
        src="https://app.powerbi.com/reportEmbed?reportId=YOUR_REPORT_ID&autoAuth=true&ctid=YOUR_TENANT_ID&config=eyJjbHVzdGVyVXJsIjoiaHR0cHM6Ly9XQUJJLVdFU1QtVVMtUkVESVJFQ1QuYW5hbHlzaXMud2luZG93cy5uZXQifQ%3d%3d" 
        frameborder="0" 
        allowFullScreen="true">
    </iframe>
</div>
```

#### Option B: Advanced Embed with Power BI JavaScript API

For better control and authentication, use the Power BI JavaScript API:

1. **Add the Power BI JavaScript library to your HTML head:**
```html
<script src="https://cdn.jsdelivr.net/npm/powerbi-client@2.22.1/dist/powerbi.min.js"></script>
```

2. **Replace the iframe section with a div:**
```html
<div id="powerbi-container" class="w-full h-full rounded-lg overflow-hidden border border-gray-300 dark:border-gray-600">
    <div id="reportContainer" style="height: 600px;"></div>
</div>
```

3. **Add JavaScript to embed the report:**
```javascript
// Add this to your app.js file
function embedPowerBIReport() {
    // Get models and embed the report
    const models = window['powerbi-client'].models;
    
    const embedConfig = {
        type: 'report',
        id: 'YOUR_REPORT_ID',
        embedUrl: 'https://app.powerbi.com/reportEmbed?reportId=YOUR_REPORT_ID',
        accessToken: 'YOUR_ACCESS_TOKEN', // You'll need to implement token generation
        tokenType: models.TokenType.Embed,
        settings: {
            panes: {
                filters: {
                    expanded: false,
                    visible: true
                }
            },
            background: models.BackgroundType.Transparent,
        }
    };

    const reportContainer = document.getElementById('reportContainer');
    const report = powerbi.embed(reportContainer, embedConfig);
}

// Call the function when the page loads
window.addEventListener('load', embedPowerBIReport);
```

### Step 4: Handle Authentication

#### For Public Reports (Easiest)
- Make sure your Power BI report is published to web
- Use the "Publish to web" feature in Power BI Service
- This generates a public URL that works without authentication

#### For Private Reports (More Secure)
You'll need to implement server-side authentication to generate access tokens. This requires:

1. **Azure App Registration**
2. **Power BI REST API integration**
3. **Token generation on your backend**

### Step 5: Responsive Design

The current implementation is already responsive. The iframe will adapt to different screen sizes using the container's CSS classes.

### Step 6: Testing Your Integration

1. Replace the placeholder content with your embed code
2. Open the website and navigate to the Dashboard section
3. Verify the dashboard loads correctly
4. Test on different devices and screen sizes
5. Check dark mode compatibility

## 🛠 Troubleshooting

### Common Issues:

1. **"Report not found" error**
   - Verify your Report ID is correct
   - Ensure the report is published and accessible

2. **Authentication errors**
   - Check if your report requires authentication
   - Consider using "Publish to web" for public access

3. **Iframe not loading**
   - Check browser console for errors
   - Verify the embed URL is correct
   - Ensure your browser allows iframes

4. **Responsive issues**
   - The current CSS should handle responsiveness
   - Test on mobile devices

### Security Considerations:

- **Public embedding:** Anyone can access your dashboard
- **Private embedding:** Requires proper authentication setup
- **Data sensitivity:** Consider what data you're exposing publicly

## 📝 Example Implementation

Here's a complete example of replacing the placeholder:

```html
<!-- Replace the entire placeholder div in index.html -->
<div id="powerbi-container" class="w-full h-full rounded-lg overflow-hidden border border-gray-300 dark:border-gray-600">
    <iframe 
        title="Sales Analytics Dashboard" 
        width="100%" 
        height="600" 
        src="https://app.powerbi.com/view?r=eyJrIjoiYjMzZTY0YWQtZmE2Yi00OGE2LWE4YTItYjY3ZGI2ZmE2M2EyIiwidCI6IjZlYjAzZGRiLWFjYWUtNDllNi1hMzE5LTE5YjNjYmY2MWY1ZCIsImMiOjZ9" 
        frameborder="0" 
        allowFullScreen="true">
    </iframe>
</div>
```

## 🎨 Customization Options

You can customize the dashboard section by:

1. **Changing the title and description**
2. **Modifying the feature icons and text**
3. **Adjusting the container height**
4. **Adding multiple dashboards with tabs**

The website is now ready for your Power BI integration! Simply follow the steps above to connect your dashboard.