
This repo contains ideas, code, and example data to estimate
‘cyclability’ on transport networks.

## What is cyclability?

There are at least three definitions of how conducive to cycling
different places, routes and segments of travel networks are:

- [Level of Traffic
  Stress](https://docs.conveyal.com/learn-more/traffic-stress) (LTS)
- [Bikeability](https://www.britishcycling.org.uk/cycletraining/article/ct20110111-cycletraining-What-is-Bikeability-0)
  levels, which rates infrastructure based on the level of training
  needed to feel comfortable:
  - Level 1 teaches basic bike-handling skills in a controlled
    traffic-free environment.
  - Level 2 teaches trainees to cycle planned routes on minor roads,
    offering a real cycling experience.
  - Level 3 ensures trainees are able to manage a variety of traffic
    conditions and is delivered on busier roads with advanced features
    and layouts
- CycleStreets’s [Quietness
  rating](https://www.cyclestreets.net/help/journey/howitworks/#quietness)
  from 1 (very unpleasant for cycling) to 100 (the quietest)
  - The [BNA tool](https://bna.peopleforbikes.org/#/) which builds on
    the concept of traffic stress to classify segments as Low Stress or
    High Stress.

## Example data

### Data from Leeds, UK

Datasets containing estimates of ‘quietness’ and ‘cyclability’ for
Leeds, UK, are available from the a separate
[repo](https://github.com/ITSLeeds/cyclability/). These datasets were
taken from an area with the following bounding box:

         xmin      ymin      xmax      ymax 
    -1.571467 53.797790 -1.541108 53.815759 

This area, representing a 1 km boundary around the University of Leeds
(-1.556288, 53.80677) can be seen in OSM at the following URL:
https://www.openstreetmap.org/#map=16/53.8068/-1.5563

#### OSM data

OSM data was downloaded from overpass with the following command which
uses `wget` to query the API for the bounding box:

::: {.cell}

``` bash
wget -O example-data/leeds.osm "https://overpass-api.de/api/interpreter?data=[out:xml][timeout:25];(way[highway](53.797790,-1.571467,53.815759,-1.541108);node(w););out body;>;out skel qt;"
```

:::

The output of the command above can be found in the `example-data`
folder of this repo.

#### Quietness

A GeoJSON file with quietness estimates for each road segment in Leeds
is available at
https://github.com/ITSLeeds/cyclability/raw/main/cyclestreets/leeds_quietness.geojson
and is illustrated below:

``` r
leeds_quietness = sf::read_sf("https://github.com/ITSLeeds/cyclability/raw/main/cyclestreets/leeds_quietness.geojson")
plot(leeds_quietness["quietness"])
```

![](README_files/figure-commonmark/unnamed-chunk-3-1.png)

``` r
leeds_quietness |>
  sf::st_drop_geometry() |>
  dplyr::slice(1:3) |>
  knitr::kable()
```

| name              | ridingSurface      |      id | cyclableText | quietness | speedMph | speedKmph | pause | color    |
|:------------------|:-------------------|--------:|:-------------|----------:|---------:|----------:|------:|:---------|
| Hanover Way       | Minor road         | 1709456 | Yes          |        40 |       16 |        26 |     0 | \#9295FF |
| Hyde Place        | Residential street | 1709460 | Yes          |        60 |       15 |        24 |     0 | \#B06840 |
| Buckingham Avenue | Residential street | 2956857 | Yes          |        40 |       15 |        24 |     0 | \#9295FF |

## Live examples

### Network Planning Tool

![](https://user-images.githubusercontent.com/122299965/236216704-72d7a546-6d69-4a8b-97fd-eb29f4f51115.png)

Source: https://nptscot.github.io/#14.75/55.94993/-3.19227

## Prior methodological work and implementations

- The Bike Network Analysis (BNA) tool methodology:
  https://cityratings.peopleforbikes.org/about/methodology
- Discussion of cyclability in A/B Street issue tracker:
  https://github.com/a-b-street/abstreet/issues/600
- Gist in Python on GitHub calculating cyclability from OSM:
  https://gist.github.com/aroche/d6fd03e51869c3e554f908bc14b5750b
- Methods described in by CycleStreets: [Quietness
  rating](https://www.cyclestreets.net/help/journey/howitworks/#quietness)

## Thoughts on next steps (draft)

- [ ] Prototype code to generate plausible quietness ratings from
  OpenStreetMap data
- [ ] Wire up to a web interface
- [ ] Create a frontent to allow people to tweak the parameters
  affecting cyclability
- [ ] Develop default settings e.g. for different types of users
  (e.g. novice, experienced, confident) and implementations of different
  metrics (e.g. Bikeability 1 to 3 or LTS 1 to 4)
- [ ] Encode the settings that lead to these implementations in a human
  readable and easy-to-edit format, e.g. JSON
