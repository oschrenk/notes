# R #

## Installation ##

	$ brew info gfortran
	$ brew install r

	R.framework was installed to:
	  /usr/local/Cellar/r/2.15.1/R.framework

	To use this Framework with IDEs such as RStudio, it must be linked
	to the standard OS X location:
	  ln -s "/usr/local/Cellar/r/2.15.1/R.framework" /Library/Frameworks

	To enable rJava support, run the following command:
	  R CMD javareconf JAVA_CPPFLAGS=-I/System/Library/Frameworks/JavaVM.framework/Headers

The installation from source took quite a while.

## Usage ##

Open a REPL by calling `r` in the terminal

## Usage ##

The easiest way to interact with `r` is via the REPL. Just open a terminal and run the command`r`.

- `> demo()` opens some demos
- `> help()` opens online help
- `> q()` quits r REPL

You can use packages by calling

	> library(<package-name>)

## Package Managment ##

Open a REPL.

- List installed packages `> installed.packages()`
- Infos about specific package `> packageDescription("MASS")`

To get a list fo the packages visit a mirror of the CRAN archive site eg [this](http://mirrors.softliste.de/cran/)

As an example install the [geosphere](http://cran.r-project.org/web/packages/geosphere/index.html) package

	> install.packages("geosphere")

Choose a mirror near your location.

## Examples

### Computing Cross Track Distance using geosphere package ###

Compute the distance of a point to a great-circle path (also referred to as the cross track distance or cross track error). The great circle is defined by `p1` and `p2`, while `p3` is the point away from the path.

    > dist2gc(p1, p2, p3, r=6378137)

with

- `p1` Start of great circle path. longitude/latitude of point(s). Can be a vector of two numbers, a matrix of 2 columns (first one is longitude, second is latitude) or a SpatialPoints* object
- `p2` End of great circle path. As above
- `p3` Point away from the great cicle path.
- `r` radius of the earth; default = 6378137

#### Example

	> library(gepsphere)
	> dist2gc(c(0,0),c(90,90),c(80,80))

### Computing Along Track Distance using geosphere package ###

The "along track distance" is the distance from the start point (`p1`) to the closest point on the path to a third point (`p3`), following a great circle path defined by points `p1` and `p2`.

    > alongTrackDistance(p1, p2, p3, r=6378137)

with

- `p1`, `p2`, `p3` longitude/latitude of point
- `r` radius of the earth; default = `6378137m`

#### Example

	> library(gepsphere)
	> alongTrackDistance(c(0,0),c(60,60),c(50,40))

