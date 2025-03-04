# WordPress Integration Guide for Demokratie Leben! Funding Application Form

This guide will help you integrate the Funding Application Form into your WordPress website.

## Overview

The application form consists of 7 HTML pages:
1. Index page (landing page)
2. Page 1: General information
3. Page 2: Project description (SMART criteria)
4. Page 3: Measures and target groups
5. Page 4: Partners and public relations
6. Page 5-6: Cost and financing plan
7. Page 7: Signature and data protection
8. Preview page
9. Thank you page

## Integration Options

### Option 1: Using HTML Custom Fields (Recommended)

1. **Create a new page for each form part**
   - Create 9 new WordPress pages (one for each HTML file)
   - Give them appropriate titles like "Funding Application Form", "Funding Application Form - Page 1", etc.

2. **Install a Custom HTML plugin**
   - Install a plugin like "Custom HTML Block" or "Advanced Custom Fields"
   - This allows you to insert raw HTML code into your WordPress pages

3. **Add the HTML content**
   - For each page, paste the corresponding HTML code into a Custom HTML block
   - Make sure to include all the HTML content between the `<body>` tags (excluding the `<body>` tags themselves)

4. **Add CSS to your theme**
   - Go to Appearance → Customize → Additional CSS
   - Paste the CSS from each page (the content between the `<style>` tags)
   - Alternatively, create a custom CSS file in your theme

5. **Link the pages together**
   - Update the navigation links in each page to point to the correct WordPress page URLs
   - For example, change `window.location.href = 'funding-application-page2.html';` to `window.location.href = '/funding-application-form-page-2/';`

### Option 2: Using a Form Plugin

1. **Install a form plugin**
   - Install a comprehensive form plugin like Gravity Forms, WPForms, or Formidable Forms
   - These plugins offer multi-page form capabilities and file uploads

2. **Create a multi-step form**
   - Build the form using the plugin's form builder
   - Split it into multiple pages/steps matching our 7-page structure
   - Add conditional logic where needed

3. **Style the form**
   - Most form plugins allow custom CSS
   - Apply styles to match the original design

4. **Configure form submissions**
   - Set up email notifications
   - Configure form data storage in the WordPress database
   - Set up PDF generation (if available in your plugin)

## WordPress Plugin Method (For Developers)

If you have development experience, you can also create a simple WordPress plugin:

1. Create a folder named `demokratie-leben-application-form` in your `wp-content/plugins/` directory

2. Create a main plugin file `demokratie-leben-application-form.php`:

```php
<?php
/**
 * Plugin Name: Demokratie Leben Application Form
 * Description: Interactive application form for the Demokratie Leben program
 * Version: 1.0
 * Author: Your Name
 */

// Exit if accessed directly
if (!defined('ABSPATH')) exit;

// Enqueue scripts and styles
function dl_application_form_scripts() {
    wp_enqueue_style('dl-form-style', plugin_dir_url(__FILE__) . 'assets/css/form-style.css');
    wp_enqueue_script('dl-form-script', plugin_dir_url(__FILE__) . 'assets/js/form-script.js', array('jquery'), '1.0', true);
}
add_action('wp_enqueue_scripts', 'dl_application_form_scripts');

// Add shortcode for the form
function dl_application_form_shortcode($atts) {
    $atts = shortcode_atts(array(
        'page' => 'index',
    ), $atts);

    ob_start();
    include plugin_dir_path(__FILE__) . 'templates/form-' . $atts['page'] . '.php';
    return ob_get_clean();
}
add_shortcode('dl_application_form', 'dl_application_form_shortcode');
```

3. Create folders for assets and templates:
   - `assets/css/form-style.css` - Combined CSS from all pages
   - `assets/js/form-script.js` - Combined JavaScript from all pages
   - `templates/` - Put the HTML content for each page in separate PHP files

4. Use the shortcode in WordPress pages:
   - `[dl_application_form page="index"]`
   - `[dl_application_form page="page1"]`
   - etc.

## Form Submission Handling

To handle form submissions, you'll need to:

1. Create a server-side script to process the form data (PHP)
2. Store the form data in the WordPress database
3. Generate a PDF of the completed form
4. Send email notifications to administrators and applicants

You can either use a WordPress form plugin that handles these features or implement them in your custom solution.

## Data Privacy Considerations

Since this form collects personal data:

1. Make sure your privacy policy mentions this form and how data is processed
2. Ensure secure data transmission (use HTTPS)
3. Implement data retention policies
4. Comply with GDPR requirements for EU users

## Testing

Before going live, thoroughly test the form to ensure:

1. All navigation between pages works correctly
2. Form validation works as expected 
3. Data is correctly saved and retrieved from localStorage
4. The form can be successfully submitted
5. All emails and notifications are sent correctly
6. The form works well on different devices (mobile, tablet, desktop)

## Support and Maintenance

Plan for ongoing maintenance:
1. Regularly test the form for functionality
2. Update the form when program requirements change
3. Provide a contact method for users who experience issues

## Additional Features to Consider

1. **Application tracking system**
   - Allow applicants to check the status of their application
   - Require login credentials for accessing applications

2. **Admin dashboard**
   - Create a backend interface for reviewing applications
   - Add approval/rejection functionality

3. **Data export**
   - Add functionality to export application data to CSV or Excel for further processing

4. **Automated reminders**
   - Send reminders about deadlines or missing information
   - Follow-up emails for incomplete applications