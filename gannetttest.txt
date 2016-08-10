<?php
///////////////CAN VIEW LIVE AT danny.marketingwerks-vec.com///////////////////////////////////////////////////////
//hit site and get profile ID
$url = "https://peaceful-springs-7920.herokuapp.com/profile/";

//load info from site into variable 
$getpid = file_get_contents($url);
//use JSON decode we are codind based on the fact we know the JSON layout
$cookie_value_json = json_decode($getpid, true);

//SET COOKIE IF NOT SET  ALSO NOTE 0 TO BE USED AS A VALID OPTION AND SET COOKIE FOR 365 DAYS DEBUG WITH DELETE COOKIE
$cookie_name = "gannetttest";
$cookie_value =  $cookie_value_json['profileId'];
$cookie_valid = ($_COOKIE[$cookie_name]);
if (!empty($cookie_valid) || $cookie_valid=="0" ) {
$cookie_value = $cookie_valid;
}
else {
setcookie($cookie_name, $cookie_value, time() + (86400 * 365), "/"); // 86400 = 1 day
}

////////////////////////////END COOKIE INFORMATION /////////////////////////////////////////////////////////////

echo "*********************************************DEBUGGING INFO***********************************************************************<br />";
///////check new id cookie id cooir id
echo "New ID " . ($cookie_valid). "<br />";
echo ($url) . "<br />";
echo ($getpid) . "<br />";

///////SET NEW URL TO GET ATRICLES BASED ON ID FROM COOKIE OR NEW ID 
$url = "https://peaceful-springs-7920.herokuapp.com/content/$cookie_value/";
echo ($url) . "<br />";
$getarticles = file_get_contents($url);

//////////////////ONCE AGAIN PULL DATA BASED ON SCHMEA OF JSON WE COULD BACK UP A STEP AND SEE WHAT FIELDS ARE BEING PASSED BUT THIS IS A SET STRUCTURE
//////////this is also using the boolean that when set as true, tells it to return the objects as associative arrays I CAN MAKE THIS AN RETURN OBJECT BUT IN THIS CASE SEEMS A BOT MUCH 
$articles_json = json_decode($getarticles, true);
echo ($articles_json['theme']). "<br />";
$theme = ($articles_json['theme']);
echo "Recursive count: " . sizeof($articles_json,1)."<br />";
echo "Article count: " . sizeof($articles_json['articles'])."<br />";
$articles_count = sizeof($articles_json['articles']);


echo "********************************************* END DEBUGGING INFO***********************************************************************<br />";

///////////////////////////////CHECK ON THEME///////////////////////////////////////////////////////////////////
if ($theme=="rare") {
$theme_color = "#DC143C";
$bground_color ="#F5F5DC";
}
else if ($theme=="well") {
$theme_color = "#8B4513";
$bground_color ="#F8F8FF";
}
//////////////////////////////////////START OUTPUT /////////////////////////////////////////////////////////
echo "<!DOCTYPE html>";	
echo "<html>\n";
echo "<head>\n";
echo "<meta http-equiv=\"Content-Type\" content=\"text/html; charset=utf-8\" />";
echo "<title>Gannett Developer Candidate Exercise</title>\n";
echo "<style>\n";
echo ".theme {color:$theme_color;}";
echo ".article {border-style: solid; background-color:$bground_color}";

echo "</style>\n";
echo "</head>\n";
echo "<body>\n";
echo "<button type=\"button\" onclick='document.cookie=\"gannetttest=;expires=Wed; 01 Jan 1970\"'>Delete Cookie </button>\n";
echo "<h2>My Delicious Articles</h2>\n";
echo "<div class=\"theme\">\n";
///////////////////////////////PULL ARTICLES /////////////////////////////////////////////////////////////////
for($x = 0; $x < $articles_count; $x++) {


   echo "<p class=\"article\">" . $articles_json['articles'][$x]['title'] . "<br /><br />\n"; 
   
    echo $articles_json['articles'][$x]['summary'] . "<br /><br />\n";
    
    echo $articles_json['articles'][$x]['href'] . "<br /><br />\n";

}
////////////////////////////END PULL//////////////////////////////////////////////////////////////////////////
echo "</div>\n";
echo "</body>\n";
echo "</html>\n";
?>
