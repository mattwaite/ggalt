
> **Fork notice:** This is a maintained fork of [hrbrmstr/ggalt](https://github.com/hrbrmstr/ggalt) by Bob Rudis and contributors. All original authorship and credit belongs to them. This fork exists solely to support data courses at the **University of Nebraska-Lincoln** — it updates the package to work with current versions of R and ggplot2 after the original was removed from CRAN.

---

`ggalt` : Extra Coordinate Systems, Geoms, Statistical Transformations,
Scales & Fonts for 'ggplot2'

A compendium of 'geoms', 'coords', 'stats', scales and fonts for
'ggplot2', including splines, 1d and 2d densities, univariate average
shifted histograms, a new map coordinate system based on the
'PROJ.4'-library and the 'StateFace' open source font 'ProPublica'.

### Installation

Install from GitHub (not available on CRAN):

``` r
# install.packages("remotes")
remotes::install_github("mattwaite/ggalt")
```

### What's in the package

  - `geom_ubar` : Uniform width bar charts

  - `geom_horizon` : Horizon charts

  - `coord_proj` : Like `coord_map`, only better (use with `geom_cartogram`)

  - `geom_xspline` / `stat_xspline` : Connect control points with an X-spline

  - `geom_bkde` / `stat_bkde` : Smooth density estimate (uses `KernSmooth::bkde`)

  - `geom_bkde2d` / `stat_bkde2d` : 2D density contours (uses `KernSmooth::bkde2D`)

  - `stat_ash` : Univariate averaged shifted histogram (uses `ash::ash1`)

  - `geom_encircle` : Automatically enclose points in a polygon

  - `geom_lollipop` : Lollipop charts (horizontal or vertical)

  - `geom_dumbbell` : Dumbbell plots

  - `geom_spikelines` : Spike lines from points to axes

  - `geom_stateface` : ProPublica StateFace font in plots

  - `stat_stepribbon` : Step ribbons

  - `annotation_ticks` : Minor ticks on log/identity axes

  - `byte_format` : Format numbers as bytes (e.g. `10000` → `10 Kb`)

  - `position_dodgev` : Vertical dodging

  - plotly integration for several of the above geoms

### Usage

``` r
library(ggplot2)
library(ggalt)
```

#### Dumbbell plots

``` r
df <- data.frame(trt=LETTERS[1:5], l=c(20, 40, 10, 30, 50), r=c(70, 50, 30, 60, 80))

ggplot(df, aes(y=trt, x=l, xend=r)) +
  geom_dumbbell(size=3, color="#e3e2e1",
                colour_x = "#5b8124", colour_xend = "#bad744",
                dot_guide=TRUE, dot_guide_size=0.25) +
  labs(x=NULL, y=NULL, title="ggplot2 geom_dumbbell with dot guide") +
  theme_minimal()
```

#### Lollipop charts

``` r
df <- data.frame(trt=LETTERS[1:10], value=seq(100, 10, by=-10))

ggplot(df, aes(trt, value)) + geom_lollipop()

ggplot(df, aes(value, trt)) + geom_lollipop(horizontal=TRUE)
```

#### Smooth density estimates

``` r
data(geyser, package="MASS")

ggplot(geyser, aes(x=duration)) +
  stat_bkde(bandwidth=0.25)

ggplot(geyser, aes(x=duration)) +
  geom_bkde(bandwidth=0.25)
```

#### 2D density contours

``` r
m <- ggplot(faithful, aes(x=eruptions, y=waiting)) +
  geom_point() + xlim(0.5, 6) + ylim(40, 110)

m + geom_bkde2d(bandwidth=c(0.5, 4))

m + stat_bkde2d(bandwidth=c(0.5, 4), aes(fill=after_stat(level)), geom="polygon")
```

#### Encircling points

``` r
gg <- ggplot(mpg, aes(displ, hwy))
gg + geom_encircle(data=subset(mpg, hwy>40)) + geom_point()
```

#### Spike lines

``` r
p <- ggplot(data=mtcars, aes(x=mpg, y=disp)) + geom_point()
p + geom_spikelines(data=mtcars[mtcars$carb==4,], linetype=2)
```

#### Horizon charts

``` r
set.seed(1)
dat <- data.frame(x=1:100, y=cumsum(rnorm(100)))
ggplot(dat, aes(x, y)) + geom_horizon(bandwidth=2)
```

### Changes in this fork

This fork updates the package for compatibility with R >= 3.5.0 and ggplot2 >= 3.3.0:

- Fixed `stat_bkde2d` which was broken by a ggplot2 4.x API change to `StatContour`
- Updated deprecated `..density..` / `..band..` aesthetic syntax to `after_stat()`
- Updated `size` to `linewidth` in geom default aesthetics for line-based geoms
- Removed dependency on `plyr` (replaced with base R equivalents)
- Moved `maps` from Imports to Suggests (only needed for mapping examples)
- Eliminated "S3 methods overwritten" startup warnings caused by duplicate `absoluteGrob` method registration

### Credits

Original package by [Bob Rudis](https://github.com/hrbrmstr) and contributors:
Ben Bolker, Ben Marwick, Jan Schulz, Rosen Matev, Aditya Kothari, Jonathan Sidi, Tarcisio Fedrizzi, and the AtherEnergy team (horizon plots).

Original repository: <https://github.com/hrbrmstr/ggalt>

### Code of Conduct

Please note that this project is released with a [Contributor Code of
Conduct](CONDUCT.md). By participating in this project you agree to
abide by its terms.
