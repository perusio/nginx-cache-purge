# Bash script for deleting an item or set of items from Nginx cache

## Introduction 

This simple script **removes** an item or set of items from
[Nginx](http://nginx.org) cache, be it
[`fastcgi`](http://wiki.nginx.org/HttpFcgiModule#fastcgi_cache) or
[`proxy`](http://wiki.nginx.org/HttpProxyModule#proxy_cache).

It accepts a
[`grep` pattern](http://www.gnu.org/software/grep/manual/grep.html#Fundamental-Structure)
as argument to search for cached items in the given cache directory.

This script uses `grep`
[**basic**](http://www.gnu.org/software/grep/manual/grep.html#Basic-vs-Extended)
regular expressions. Pressuposes the use of
[GNU `grep`](http://www.gnu.org/software/grep/manual/grep.html).

The script **requires** `rw` (read-write) access to the cache
directory.

## Usage

 1. Delete `foobar.css` from the `/var/cache/nginx/baz` cache.
 
        nginx-cache-purge "foobar.cs" /var/cache/nginx/baz
    
 2. Delete all JPEG files from the `/var/cache/nginx/img` cache.
 
        nginx-cache-purge "\.jpe*g" /var/cache/nginx/img 

## Installation 

 1. Clone the repo:
 
        git clone git://github.com/perusio/nginx-cache-purge.git
    
 2. Place the script in a convenient place.
 
 3. Done.

## See also 

There's another [script](https://github.com/perusio/nginx-cache-inspector)
on github for **inspecting** cache items.
