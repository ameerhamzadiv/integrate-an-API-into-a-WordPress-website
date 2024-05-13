# integrate-an-API-into-a-WordPress-website
<br>
To integrate an API into a WordPress website built with Elementor, you can follow these general steps:
<br><br>
Step 1: Understand Your API
Let's assume your Laravel API has an endpoint /api/data which returns JSON data containing information you want to display on your WordPress site.
<br><br>
Step 2: Create a Custom WordPress Plugin
Create a new directory in your WordPress installation's wp-content/plugins/ directory. Let's call it custom-api-integration.
Inside this directory, create a PHP file named custom-api-integration.php. This will be the main file of your custom plugin.
<br><br>
Step 3: Plugin Activation: After Create Directory and php file Activate plugin from plugins page in wordpress go to pligin page you see custom plugin name.
<br><br>
Step 4: Make API Calls in WordPress
<br><br>
Open custom-api-integration.php and add the following code:
<br><br>
<code>
<?php
/*
Plugin Name: Custom API Integration
*/

// Function to retrieve data from the API
function fetch_api_data() {
    $baseurl = 'https://example.com';
    $response = wp_remote_get($baseurl.'/api/get-all-courses');
    
     if (!is_wp_error($response) && $response['response']['code'] == 200) {
        $data = json_decode($response['body']);
        
        return $data;
    } else {
        return false;
    }
}
</code>
<br><br>
Step 5: Create a shortcode in your plugin file (custom-api-integration.php):
<br><br>

<div class="snippet-clipboard-content notranslate position-relative overflow-auto"><pre class="notranslate">
<code> // Function to fetch API data and return HTML
function display_api_data() {
    $data = fetch_api_data();
    //var_dump($data);
    if ($data && isset($data->data)) {
        $courses = $data->data;
        $baseurl = 'https://lms.whiteboardprojectconsult.com';
    	$html = '
    	<style>



/* Float four columns side by side */
.column {
  float: left;
  width: 25%;
  padding: 10px 10px;
}

/* Remove extra left and right margins, due to padding */
.row {margin: 0 -5px;}

/* Clear floats after the columns */
.row:after {
  content: "";
  display: table;
  clear: both;
}

/* Responsive columns */
@media screen and (max-width: 600px) {
  .column {
    width: 100%;
    display: block;
    margin-bottom: 20px;
  }
}

/* Style the counter cards */
.card {
  box-shadow: 0px 24px 120px -24px rgba(0, 0, 0, 0.24);
  padding: 16px;
  text-align: center;
  background-color: #fff;
}
.regimg{
width:100%;
height:150px !important;
}
.h3api{
    font-size: 16px !important;
    padding-top: 20px !important;
    margin-bottom: 0px !important;
}
</style>
    	
    	<div class="row">';
		foreach ($courses as $course) {
		    if ($course->status == 1) {
		    $image = isset($course->image) ? 'https://example.com/'.$course->image : 'https://example.com/wp-content/uploads/2024/05/unnamed.jpg'; 
		    
		    // Check if the "about" parameter exists and if it contains the English version
            $about = isset($course->about->en) ? $course->about->en : '';

            // Limit the number of words in the "about" text to 20
            $about_short = '';
            if (!empty($about)) {
                $words = explode(' ', $about); // Split the text into words
                $about_short = implode(' ', array_slice($words, 0, 20)); // Take the first 20 words
                if (count($words) > 20) {
                    $about_short .= '...'; // Add ellipsis if there are more than 20 words
                }
            }
            
            
            $html .= '<!---loop start-----> <div class="column">
            <a href="'.$baseurl.'/courses-details/'. $course->slug . '">
    <div class="card">
    	
     <img class="regimg" src="'. $image . '" />
     
      <h3 class="h3api">' . $course->title->en . '</h3>
      
    </div>
    </a>
  </div> <!---loop end----->';
		    
	
		    }
    }
    	$html .= '</div>';
			  } else {
        $html = 'Failed to retrieve data from the API.';
    }

	 // Log data and errors
   // error_log('API Data: ' . print_r($data, true));
//    error_log('HTML Output: ' . $html);

    return $html;
}

// Register shortcode
add_shortcode('api_data_custom', 'display_api_data');
</code></pre><div class="zeroclipboard-container">
    <clipboard-copy aria-label="Copy" class="ClipboardButton btn btn-invisible js-clipboard-copy m-2 p-0 tooltipped-no-delay d-flex flex-justify-center flex-items-center" data-copy-feedback="Copied!" data-tooltip-direction="w" value="// Function to fetch API data and return HTML
function display_api_data() {
    $data = fetch_api_data();
    //var_dump($data);
    if ($data && isset($data->data)) {
        $courses = $data->data;
        $baseurl = 'https://lms.whiteboardprojectconsult.com';
    	$html = '
    	<style>



/* Float four columns side by side */
.column {
  float: left;
  width: 25%;
  padding: 10px 10px;
}

/* Remove extra left and right margins, due to padding */
.row {margin: 0 -5px;}

/* Clear floats after the columns */
.row:after {
  content: "";
  display: table;
  clear: both;
}

/* Responsive columns */
@media screen and (max-width: 600px) {
  .column {
    width: 100%;
    display: block;
    margin-bottom: 20px;
  }
}

/* Style the counter cards */
.card {
  box-shadow: 0px 24px 120px -24px rgba(0, 0, 0, 0.24);
  padding: 16px;
  text-align: center;
  background-color: #fff;
}
.regimg{
width:100%;
height:150px !important;
}
.h3api{
    font-size: 16px !important;
    padding-top: 20px !important;
    margin-bottom: 0px !important;
}
</style>
    	
    	<div class="row">';
		foreach ($courses as $course) {
		    if ($course->status == 1) {
		    $image = isset($course->image) ? 'https://example.com/'.$course->image : 'https://example.com/wp-content/uploads/2024/05/unnamed.jpg'; 
		    
		    // Check if the "about" parameter exists and if it contains the English version
            $about = isset($course->about->en) ? $course->about->en : '';

            // Limit the number of words in the "about" text to 20
            $about_short = '';
            if (!empty($about)) {
                $words = explode(' ', $about); // Split the text into words
                $about_short = implode(' ', array_slice($words, 0, 20)); // Take the first 20 words
                if (count($words) > 20) {
                    $about_short .= '...'; // Add ellipsis if there are more than 20 words
                }
            }
            
            
            $html .= '<!---loop start-----> <div class="column">
            <a href="'.$baseurl.'/courses-details/'. $course->slug . '">
    <div class="card">
    	
     <img class="regimg" src="'. $image . '" />
     
      <h3 class="h3api">' . $course->title->en . '</h3>
      
    </div>
    </a>
  </div> <!---loop end----->';
		    
	
		    }
    }
    	$html .= '</div>';
			  } else {
        $html = 'Failed to retrieve data from the API.';
    }

	 // Log data and errors
   // error_log('API Data: ' . print_r($data, true));
//    error_log('HTML Output: ' . $html);

    return $html;
}

// Register shortcode
add_shortcode('api_data_custom', 'display_api_data');" tabindex="0" role="button">
      <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-copy js-clipboard-copy-icon">
    <path d="M0 6.75C0 5.784.784 5 1.75 5h1.5a.75.75 0 0 1 0 1.5h-1.5a.25.25 0 0 0-.25.25v7.5c0 .138.112.25.25.25h7.5a.25.25 0 0 0 .25-.25v-1.5a.75.75 0 0 1 1.5 0v1.5A1.75 1.75 0 0 1 9.25 16h-7.5A1.75 1.75 0 0 1 0 14.25Z"></path><path d="M5 1.75C5 .784 5.784 0 6.75 0h7.5C15.216 0 16 .784 16 1.75v7.5A1.75 1.75 0 0 1 14.25 11h-7.5A1.75 1.75 0 0 1 5 9.25Zm1.75-.25a.25.25 0 0 0-.25.25v7.5c0 .138.112.25.25.25h7.5a.25.25 0 0 0 .25-.25v-7.5a.25.25 0 0 0-.25-.25Z"></path>
</svg>
      <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-check js-clipboard-check-icon color-fg-success d-none">
    <path d="M13.78 4.22a.75.75 0 0 1 0 1.06l-7.25 7.25a.75.75 0 0 1-1.06 0L2.22 9.28a.751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018L6 10.94l6.72-6.72a.75.75 0 0 1 1.06 0Z"></path>
</svg>
    </clipboard-copy>
  </div></div>

<br><br>
Step 6: In your Elementor HTML widget, use the shortcode [api_data_custom] to display the data:
<br><br>
<div class="snippet-clipboard-content notranslate position-relative overflow-auto"><pre class="notranslate"><code> 
	[api_data_custom]
</code></pre><div class="zeroclipboard-container">
    <clipboard-copy aria-label="Copy" class="ClipboardButton btn btn-invisible js-clipboard-copy m-2 p-0 tooltipped-no-delay d-flex flex-justify-center flex-items-center" data-copy-feedback="Copied!" data-tooltip-direction="w" value=" [api_data_custom] " tabindex="0" role="button">
      <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-copy js-clipboard-copy-icon">
    <path d="M0 6.75C0 5.784.784 5 1.75 5h1.5a.75.75 0 0 1 0 1.5h-1.5a.25.25 0 0 0-.25.25v7.5c0 .138.112.25.25.25h7.5a.25.25 0 0 0 .25-.25v-1.5a.75.75 0 0 1 1.5 0v1.5A1.75 1.75 0 0 1 9.25 16h-7.5A1.75 1.75 0 0 1 0 14.25Z"></path><path d="M5 1.75C5 .784 5.784 0 6.75 0h7.5C15.216 0 16 .784 16 1.75v7.5A1.75 1.75 0 0 1 14.25 11h-7.5A1.75 1.75 0 0 1 5 9.25Zm1.75-.25a.25.25 0 0 0-.25.25v7.5c0 .138.112.25.25.25h7.5a.25.25 0 0 0 .25-.25v-7.5a.25.25 0 0 0-.25-.25Z"></path>
</svg>
      <svg aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="octicon octicon-check js-clipboard-check-icon color-fg-success d-none">
    <path d="M13.78 4.22a.75.75 0 0 1 0 1.06l-7.25 7.25a.75.75 0 0 1-1.06 0L2.22 9.28a.751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018L6 10.94l6.72-6.72a.75.75 0 0 1 1.06 0Z"></path>
</svg>
    </clipboard-copy>
  </div></div>


