# YellowPages2OpenStreetMap

⚠️⚠️⚠️
**You can find the content of this repository on [CodeBerg.org](https://codeberg.org/jmizv/yp2osm).
The following is just a copy of the actual README.md.**
⚠️⚠️⚠️

Not sure whether the [OpenStreetMap](https://www.openstreetmap.org) in your vicinity is up-to-date?

Let's make a cross-check with your [Yellow Pages](https://www.gelbeseiten.de). This tool does not offer a UI but rather
generates some output
data that can be read (simple document) or used in other applications like OsmAnd.

**⚠️ Note:** Do not use directly the data provided by the Yellow Pages. Use it as hint and visit the place on the ground
to ensure to add only valid data to OpenStreetMap. Of course, even Yellow Pages can have wrong data.

⁉ Any questions or comments?

## Description

This tool is based on Java 17, Gradle and the Spring Boot framework. Having at least Java 17 and Gradle installed, you
can run the program from the command line.

The main process consists of two query parts:

- query the content of the [Gelbe Seiten](https://www.gelbeseiten.de/) (Germany's Yellow Pages) for a
  certain industry like dentist in a certain German city like Dortmund.
- query (with the help of Overpass Turbo) the data of OpenStreetMap for a certain node in a certain German city.

The cities are configured in the `application.yaml`. For now, there is no way to pass the city data via the CLI.

After having this data, the approach is to map between each via the street and number. If a dentist is listed for
Ulmenstraße 59 in both Gelbe Seiten and OpenStreetMap, it is very likely that there is also a dentist on the ground.
For now there is no check by the name.

The most important information is though when there is no match:

- either Gelbe Seiten lists a dentist for Käthe-Kollwitz-Straße 42 but OpenStreetMap not:
    - we need to check on the ground if we can add a new `healthcare=dentist` node to OSM
- or OpenStreetMap has data for a dentist in the Friedenstraße 72 but Gelbe Seiten not:
    - we need to check on the ground if we can remove an obsolete `healthcare=dentist` node in OSM

Again: never add or correct data in OpenStreetMap only with the help of Gelbe Seiten. We have to check the (non-)
existence
of a dentists or other amenities [on the ground](https://wiki.openstreetmap.org/wiki/Ground_truth).

## Usage

`gradle boot:run`

## Result

Currently, there are three different output formats:

* Markdown document
* OsmAnd file for markers on the map
* GPX file for opening and editing in JOSM 

## Mapped Points of interest

Currently supported points of interest:

| Name               | Yellow Pages                                              | OpenStreetMap                            |
|--------------------|-----------------------------------------------------------|------------------------------------------|
| Endokrinolog*in    | `endokrinologen`                                          | `healthcare:speciality=endocrinology`    |
| Gastroenterolog*in | `ärzte%3a innere medizin und gastroenterologie fachärzte` | `healthcare:speciality=gastroenterology` |
| Gynäkolog*in       | `gynäkologen`                                             | `healthcare:speciality=gynaecology`      |
| Hausärzt*in        | `hausarzt`                                                | `healthcare:speciality=general`          |
| Hautärzt*in        | `hautarzt`                                                | `healthcare:speciality=dermatology`      |
| Kardiolog*in       | `kardiologen`                                             | `healthcare:speciality=cardiology`       |
| Zahnärzt*in        | `zahnarzt`                                                | `healthcare=dentist`                     |
