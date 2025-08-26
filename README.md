
# StreetSafe Docker

This repository contains the Docker Compose config to get up and running with your own instance of StreetSafe easily.

## Requirements
- Docker
- NodeJS
- Postgres database
  - This will need the [H3 extension](https://github.com/zachasme/h3-pg) enabled. We used [Neon](https://neon.com/) to host our database, as [they have H3 as an available extension](https://neon.com/docs/extensions/postgis-related-extensions#h3-and-h3-postgis), however hosting your own Postgres database is easy to do as well.

## Getting started
This project uses a custom fork of [Valhalla](https://github.com/solithcy/valhalla_safety/). To get started you will need to generate your own Valhalla config and tile data, then have it available in `./custom_files`. The files present in this directory should look similar to:
```bash
~/streetsafe-docker$ ls custom_files
admins.sqlite        duplicateways.txt  great-britain-latest.osm.pbf  transit_tiles  valhalla_tiles
default_speeds.json  file_hashes.txt    timezones.sqlite              valhalla.json  valhalla_tiles.tar
```
`great-britain-latest.osm.pbf` is the OSM data we used to initialise our Valhalla tiles. Since this is a custom fork of Valhalla, we recommend using the code in [our fork](https://github.com/solithcy/valhalla_safety/) to generate a configuration file. A dummy configuration file is available in this repository. Notably, there is a new field in `additional_data`, `db_connection_string`. Here you will need to put the connection URL to your Postgres database.

You must add a `.env` file to the root of this repository, populated with values as lined out in our [server README](https://github.com/cb671/streetsafe-server/blob/main/README.md). Optionally, you can choose which branches the server and client are built from by setting the `BUILD_BRANCH` env file. This will default to `main`, however for the latest (and possibly unstable) features, you can set this to `dev`.


## Database initialisation
You will need to initialise your database using the scripts in our repositories. Firstly, clone our [PoliceDataIngest](https://github.com/solithcy/PoliceDataIngest) repository and follow the instructions there to populate the database with crime data and population data. Then, clone our [server](https://github.com/cb671/streetsafe-server) repository and follow the instructions there to populate the database with the appropriate tables and data regarding emergency services.

## Running
Once you've followed the outlined steps, you're ready to get your own instance of StreetSafe running. To do so, run the following command:
```bash
$ docker compose up -d
```
