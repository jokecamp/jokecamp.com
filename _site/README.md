## Joe Kampschmidt's (Joe's Code) Personal Website/Blog


Source code for <https://www.jokecamp.com>

The website is a static site built with [Poole](https://github.com/poole/poole) and [Jekyll](http://jekyllrb.com).

To test change to the project directory and run the following command

    jekyll serve --watch

Then browse to <http://localhost:4000/>

Pull requests will be accepted for any typos or editing and your contribution will be credited.

## Asset Pipeline

https://github.com/matthodan/jekyll-asset-pipeline/blob/master/README.md

- gem install jekyll-asset-pipeline
- gem install yui-compressor

## Script to Compress static content

See <https://members.nearlyfreespeech.net/wiki/HowTo/GzipStatic>

Run on unix web server
```
#!/usr/local/bin/perl

# This script should be uploaded to the web server.

use warnings;
use strict;
use File::Find;
find (\&wanted, ("."));
sub wanted
{
 if (/(.*\.(?:html|htm|css|js)$)/i) {
     print "Compressing $File::Find::name\n";
     system ("gzip --keep --best --force $_");
 }
}
```

## To Deploy

```
jekyll serve
git commit -m "
git push origin master
./deploy.sh
```
