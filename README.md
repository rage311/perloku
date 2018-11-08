Perloku
=======

Deploy Perl applications in seconds.

## Step 1

Write an app (or `mojo generate lite_app`):

```perl
#!/usr/bin/env perl
use Mojolicious::Lite;

get '/' => sub {
  my $c = shift;
  $c->render(template => 'index');
};

app->start;
__DATA__

@@ index.html.ep
% layout 'default';
% title 'Welcome';
<h1>Welcome to the Mojolicious real-time web framework!</h1>

@@ layouts/default.html.ep
<!DOCTYPE html>
<html>
  <head><title><%= title %></title></head>
  <body><%= content %></body>
</html>
```

## Step 2

Create a Makefile.PL with your dependencies (or `mojo generate makefile`):

```perl
use strict;
use warnings;

use ExtUtils::MakeMaker;

WriteMakefile(
  VERSION   => '0.01',
  PREREQ_PM => {'Mojolicious' => '8.05'},
  test      => {TESTS => 't/*.t'}
);
```

Alternately, you may create a
[cpanfile](https://metacpan.org/pod/distribution/Module-CPANfile/lib/cpanfile.pod)
that lists dependencies instead:

```
requires 'Mojolicious', '8.05';
```

## Step 3

Create an executable file called Procfile which runs a server on the port
given as an enviroment variable:

```sh
web: ./myapp.pl daemon --listen http://*:$PORT --mode production
```


Test that you can start the server:

```sh
export PORT=3000 && ./myapp.pl daemon --listen http://*:$PORT --mode production
```

## Step 4

Deploy:

```sh
git init
git add .
git update-index --chmod=+x myapp.pl (only if using Windows)
git commit -m "Initial version"
heroku create -s cedar --buildpack https://github.com/judofyr/perloku.git
git push heroku master
```


Watch:

```
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 8 threads
Compressing objects: 100% (5/5), done.
Writing objects: 100% (5/5), 767 bytes | 767.00 KiB/s, done.
Total 5 (delta 0), reused 0 (delta 0)
remote: Compressing source files... done.
remote: Building source:
remote:
remote: -----> Perloku app detected
remote: -----> Vendoring Perl
remote:        Using Perl 5.26.2
remote: -----> Installing dependencies
remote:        --> Working on /tmp/build_e78a53b852d3889ea231fc4062b0debb
remote:        Configuring /tmp/build_e78a53b852d3889ea231fc4062b0debb ... OK
remote:        ==> Found dependencies: Mojolicious
remote:        --> Working on Mojolicious
remote:        Fetching http://www.cpan.org/authors/id/S/SR/SRI/Mojolicious-8.05.tar.gz ... OK
remote:        Configuring Mojolicious-8.05 ... OK
remote:        ==> Found dependencies: List::Util, IO::Socket::IP
remote:        --> Working on List::Util
remote:        Fetching http://www.cpan.org/authors/id/P/PE/PEVANS/Scalar-List-Utils-1.50.tar.gz ... OK
remote:        Configuring Scalar-List-Utils-1.50 ... OK
remote:        Building Scalar-List-Utils-1.50 ... OK
remote:        Successfully installed Scalar-List-Utils-1.50 (upgraded from 1.27)
remote:        --> Working on IO::Socket::IP
remote:        Fetching http://www.cpan.org/authors/id/P/PE/PEVANS/IO-Socket-IP-0.39.tar.gz ... OK
remote:        Configuring IO-Socket-IP-0.39 ... OK
remote:        Building IO-Socket-IP-0.39 ... OK
remote:        Successfully installed IO-Socket-IP-0.39
remote:        Building Mojolicious-8.05 ... OK
remote:        Successfully installed Mojolicious-8.05
remote:        <== Installed dependencies for /tmp/build_e78a53b852d3889ea231fc4062b0debb. Finishing.
remote:        3 distributions installed
remote:        Dependencies installed
remote: -----> Discovering process types
remote:        Procfile declares types     -> (none)
remote:        Default types for buildpack -> web
remote:
remote: -----> Compressing...
remote:        Done: 14.2M
remote: -----> Launching...
remote:        Released v4
remote:        https://perloku-example.herokuapp.com/ deployed to Heroku
remote:
remote: Verifying deploy... done.
To https://git.heroku.com/perloku-example.git
 * [new branch]      master -> master
```

[Enjoy!](http://perloku-example.herokuapp.com)
