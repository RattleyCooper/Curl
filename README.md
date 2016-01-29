# Curler


A fluent API wrapper for libcurl in php.  Setting options and headers is done using
method chaining instead of setting options explicitly using the libcurl constants.

Debugging cURL commands in php using the Curler class is insanely simple as well.
Just chain the `dryRun()` method onto the end of your method chain instead of
the `go()` method and it will dump out all of the cURL request information
without making the request.
 
See http://php.net/manual/en/book.curl.php for information on libcurl.

Note: This repo is in its infancy and may not be suitable for all
applications, however it is great for simple and semi-advanced 
requests.

Documentation is located in the `Curler.php` file for now, but examples on usage will be added.

## Examples

#### Getting a web-page
Grab the HTML for a web page to be displayed in the web browser without rendering the HTML.
```php
$curler = new Curler('https://github.com/');

$curler->followRedirects() // Will follow redirects option set to true.
    ->header('Connection', 'keep-alive')  // Set some headers
    ->header('Accept', 'text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8')
    ->header('Accept-Language', 'en-US,en;q=0.5')
    ->header('Host', 'github.com')
    ->header('Content-Type', 'application/x-www-form-urlencoded')
    ->cookieJar('gitCookie') // Set a file to use as cookie jar.
    ->suppressOutput() // Sets the RETURNTRANSFER option to true so that output is fetched as string instead of displayed automatically.
    ->suppressRender() // Display in browser as HTML text instead of rendering the HTML.  Great for debugging!
;

$html = $curler->go(); // Replace go() with dryRun() to get debug info on the request without executing it.
var_dump($html);
```

#### Preforming a multi-request
You can send asynchronous requests as well!  This can be accomplished through the use of curler by
adding URLs to the request(this retains any options that have been set in Curler up to this point),
or by adding cURL handles to the request.

```php
$curler = new Curler('https://github.com/');

$urls = [
    'http://pastebin.com/',
    'https://google.com/',
    'http://yahoo.com/'
];

$headers = [
    'Connection'        =>      'keep-alive',
    'Accept'            =>      'text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8',
    'Accept-Language'   =>      'en-US,en;q=0.5',
    'Content-Type'      =>      'application/x-www-form-urlencoded'
];

$cj = '/home/parker/testCookie';

$curler->followRedirects()
    ->headerArray($headers)
    ->cookieJar($cj)
    ->multiCookie()
    ->suppressOutput()
    ->suppressRender()
    ->setHeaders()
    ->setPostFields()
;

foreach ( $urls as $url )
{
    $curler->addUrl($url); // $curler->addHandle($validCurlHandle); would work as well.
}

$html = $curler->goMulti()->multi_response;
 ```

#### Debugging a request
Debug requests with ease by chaining the `dryRun()` method to the end of your method chain and dumping the result.

This will output something similar to this(consider using something like Symfony's `VarDumper` or Laravels dump and die - `dd()`):  **Note that this is not yet implimented for multi-requests!!!**

```php
 [
   "url" => "https://github.com/"
   "cookieJarFile" => "/home/parker/gitCookie"
   "headers" => [
     "Connection" => "keep-alive"
     "Accept" => "text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8"
     "Accept-Language" => "en-US,en;q=0.5"
     "Host" => "github.com"
     "Content-Type" => "application/x-www-form-urlencoded"
   ]
   "postfields" => []
   "poststring" => ""
   "handles" => []
   "options" => [
     "CURLOPT_FOLLOWLOCATION" => true
     "CURLOPT_COOKIEFILE" => "/home/parker/gitCookie"
     "CURLOPT_COOKIEJAR" => "/home/parker/gitCookie"
     "CURLOPT_RETURNTRANSFER" => true
     "CURLOPT_HTTPHEADER" => [
       0 => "Connection: keep-alive"
       1 => "Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8"
       2 => "Accept-Language: en-US,en;q=0.5"
       3 => "Host: github.com"
       4 => "Content-Type: application/x-www-form-urlencoded"
     ]
   ]
   "curl_getinfo" => [
     "url" => "https://github.com/"
     "content_type" => null
     "http_code" => 0
     "header_size" => 0
     "request_size" => 0
     "filetime" => 0
     "ssl_verify_result" => 0
     "redirect_count" => 0
     "total_time" => 0.0
     "namelookup_time" => 0.0
     "connect_time" => 0.0
     "pretransfer_time" => 0.0
     "size_upload" => 0.0
     "size_download" => 0.0
     "speed_download" => 0.0
     "speed_upload" => 0.0
     "download_content_length" => -1.0
     "upload_content_length" => -1.0
     "starttransfer_time" => 0.0
     "redirect_time" => 0.0
     "redirect_url" => ""
     "primary_ip" => ""
     "certinfo" => []
     "primary_port" => 0
     "local_ip" => ""
     "local_port" => 0
   ]
 ]
```
