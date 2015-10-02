# Curler
libcurl wrapper using a fluent API for building and executing requests.

Documentation is located in the `Curler.php` file for now.  Examples are coming soon!

 * A fluent API wrapper for libcurl in php.  Setting options and headers is done using
 * method chaining instead of setting options explicitly using the libcurl constants.
 *
 * Debugging cURL commands in php using the Curler class is insanely simple as well.
 * Just chain the `dryRun()` method onto the end of your method chain instead of
 * the `go()` method and it will dump out all of the cURL request information.
 *
 * See http://php.net/manual/en/book.curl.php for information on libcurl.
 *
 * Note: This repo is in its infancy and may not be suitable for all
 * applications, however it is great for simple requests!  Multi-
 * requests will be coming soon!  Hopefully ;)
