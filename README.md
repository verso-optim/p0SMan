# pOSMan

## About

pOSMan is a work in progress to solve the [chinese postman
problem](https://en.wikipedia.org/wiki/Route_inspection_problem) on
the road network derived from OpenStreetMap data.

## Demo

Going through all edges in a small town (click on the marker to start
the animation):

- [example 1](https://verso-optim.com/vrp-optim/chinese-postman-poc/monnieres/)
- [example 2](https://verso-optim.com/vrp-optim/chinese-postman-poc/saint-lumine-de-clisson/)
- [example 3](https://verso-optim.com/vrp-optim/chinese-postman-poc/montreuil-en-touraine/)
- [example 4](https://verso-optim.com/vrp-optim/chinese-postman-poc/vieillevigne/)

## Requirements

Parsing OpenStreetMap data to generate a graph of the road network is
currently done using
[`osm4routing`](https://github.com/Tristramg/osm4routing).


## Getting started

### Build

Clone the repo and build from source.

```
git clone https://github.com/verso-optim/pOSMan.git
cd pOSMan/src
make
```

### Usage

Pass relevant files/information to the executable at `bin/posman`.

```
posman -n nodes.csv -e edges.csv -s START -w WAYS -o OUTPUT
```

- `nodes.csv` and `edges.csv` are files generated by running `osm4routing` on your favorite OpenStreetMap extract.
- `START` is the id of the OSM node to use as start of the route
- `WAYS` is the list of ids for the OSM ways that needs visiting
- `OUTPUT` is the name of the `json` file to write the solution to

### Output format

The computed solution is formatted as follow.

| Key         | Description |
| ----------- | ----------- |
| `length` | total length of the route (cm) |
| `legs` | an array of `leg` objects describing the trip |

#### Leg

A `leg` has the following properties:

| Key         | Description |
| ----------- | ----------- |
| `length` | leg length (cm) |
| `way_id` | the OSM way id this leg is part of |
| `nodes` | a list of notable visited nodes with their OSM id and coordinates |
| `geometry` | an array of coordinates providing the full leg geometry |

## Current limitations

- Only handle the undirected case at the moment
- Don't handle one-way streets so only suitable for use by foot (bike?)
- No fine control on what ways are included or not, no notion of "profile"
- A sub-optimal matching step can cause detours in some cases (the `experiment/blossom5` branch fixes that, albeit only for research purpose as it uses a non-free third party library)
- This is really a prototype and error handling is far from perfect, e.g. for connectivity issues in data