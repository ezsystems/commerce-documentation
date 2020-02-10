#  Varnish VCL 

The Original VCL file was extended in order to support Ajax calls (e.g. used for addtobasket feature):

``` 
   if (req.method != "GET" && req.method != "HEAD") {
        return (pass);
    } 
```

``` 
// Varnish 4 style - eZ 5.4+ / 2014.09+
// Complete VCL example extend for silver.eShop 3.3.3
 
vcl 4.0;
 
// Our Backend - Assuming that web server is listening on port 80
// Replace the host to fit your setup
backend ezpublish {
    .host = "127.0.0.1";
    .port = "80";
}
// ACL for debuggers IP
acl debuggers {
    "127.0.0.1";
    "192.168.0.0"/16;
}
 
// Called at the beginning of a request, after the complete request has been received
sub vcl_recv {
 
    // Set the backend
    set req.backend_hint = ezpublish;
 
    // ...
 
    // Retrieve client user hash and add it to the forwarded request.
    call ez_user_hash;
 
    // If it passes all these tests, do a lookup anyway.
    return (hash);
}
 
// Called when the requested object has been retrieved from the backend
sub vcl_backend_response {
    if (bereq.http.accept ~ "application/vnd.fos.user-context-hash"
        && beresp.status >= 500
    ) {
        return (abandon);
    }
    
    // ...
}
 
// Sub-routine to get client user hash, for context-aware HTTP cache.
sub ez_user_hash {
 
    // Prevent tampering attacks on the hash mechanism
    if (req.restarts == 0
        && (req.http.accept ~ "application/vnd.fos.user-context-hash"
            || req.http.x-user-hash
        )
    ) {
        return (synth(400));
    }
    // Don't cache requests other than GET and HEAD.
    // silver.eShop - required for ajax calls!!!!
    if (req.method != "GET" && req.method != "HEAD") {
        return (pass);
    } 
    if (req.restarts == 0 && (req.method == "GET" || req.method == "HEAD")) {
        // Get User (Context) hash, for varying cache by what user has access to.
        // https://doc.ez.no/display/EZP/Context+aware+HTTP+cache
 
        // Anonymous user w/o session => Use hardcoded anonymous hash to avoid backend lookup for hash
        if (req.http.Cookie !~ "eZSESSID" && !req.http.authorization) {
            // You may update this hash with the actual one for anonymous user
            // to get a better cache hit ratio across anonymous users.
            // Note: You should then update it every time anonymous user rights change.
            set req.http.X-User-Hash = "38015b703d82206ebc01d17a39c727e5";
        }
        // Pre-authenticate request to get shared cache, even when authenticated
        else {
            set req.http.x-fos-original-url    = req.url;
            set req.http.x-fos-original-accept = req.http.accept;
            set req.http.x-fos-original-cookie = req.http.cookie;
            // Clean up cookie for the hash request to only keep session cookie, as hash cache will vary on cookie.
            set req.http.cookie = ";" + req.http.cookie;
            set req.http.cookie = regsuball(req.http.cookie, "; +", ";");
            set req.http.cookie = regsuball(req.http.cookie, ";(eZSESSID[^=]*)=", "; \1=");
            set req.http.cookie = regsuball(req.http.cookie, ";[^ ][^;]*", "");
            set req.http.cookie = regsuball(req.http.cookie, "^[; ]+|[; ]+$", "");
            set req.http.accept = "application/vnd.fos.user-context-hash";
            set req.url = "/_fos_user_context_hash";
 
            // Force the lookup, the backend must tell how to cache/vary response containing the user hash
            return (hash);
        }
    }
 
    // Rebuild the original request which now has the hash.
    if (req.restarts > 0
        && req.http.accept == "application/vnd.fos.user-context-hash"
    ) {
        set req.url         = req.http.x-fos-original-url;
        set req.http.accept = req.http.x-fos-original-accept;
        set req.http.cookie = req.http.x-fos-original-cookie;
        unset req.http.x-fos-original-url;
        unset req.http.x-fos-original-accept;
        unset req.http.x-fos-original-cookie;
 
        // Force the lookup, the backend must tell not to cache or vary on the
        // user hash to properly separate cached data.
 
        return (hash);
    }
}
 
sub vcl_deliver {
    // On receiving the hash response, copy the hash header to the original
    // request and restart.
    if (req.restarts == 0
        && resp.http.content-type ~ "application/vnd.fos.user-context-hash"
    ) {
        set req.http.x-user-hash = resp.http.x-user-hash;
        return (restart);
    }
 
    // If we get here, this is a real response that gets sent to the client.
    // Remove the vary on context user hash, this is nothing public. Keep all
    // other vary headers.
    set resp.http.Vary = regsub(resp.http.Vary, "(?i),? *x-user-hash *", "");
    set resp.http.Vary = regsub(resp.http.Vary, "^, *", "");
    if (resp.http.Vary == "") {
        unset resp.http.Vary;
    }
 
    // Sanity check to prevent ever exposing the hash to a client.
    unset resp.http.x-user-hash;
    if (client.ip ~ debuggers) {
        if (obj.hits > 0) {
            set resp.http.X-Cache = "HIT";
            set resp.http.X-Cache-Hits = obj.hits;
        } else {
            set resp.http.X-Cache = "MISS";
        }
    }
}
```
