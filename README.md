# utl-compute-the-area-inside-a-latitude-and-longitude-polygon-in-square-km-gis-graph
Compute the area inside a latitude and longidure polygon gis graph   
    %let pgm=utl-compute-the-area-inside-a-latitude-and-longitude-polygon-in-square-km-gis-graph;

    %stop_submission;

    Compute the area inside a latitude and longidure polygon gis graph

               SOLUTION (SIMPLE SPECIAL CASE  R CAN HANDLE ANY POLYGON?)

                  1 sas geodist
                  2 r sf package
                  3 related repos

    github
    https://tinyurl.com/bdd7b9fk
    https://github.com/rogerjdeangelis/utl-compute-the-area-inside-a-latitude-and-longitude-polygon-in-square-km-gis-graph

    /*               _     _
     _ __  _ __ ___ | |__ | | ___ _ __ ___
    | `_ \| `__/ _ \| `_ \| |/ _ \ `_ ` _ \
    | |_) | | | (_) | |_) | |  __/ | | | | |
    | .__/|_|  \___/|_.__/|_|\___|_| |_| |_|
    |_|
    */

    /**************************************************************************************************************************/
    /*                                                     |                                                    |             */
    /*                          INPUT                      |     PROCESS                                        |  OUTPUT     */
    /*                         =======                     |     =======                                        |  ======     */
    /*                                                     |                                                    |             */
    /*                        LONGITUDE                    | 1  SAS SOLUTION                                    | POUKEEPSIE  */
    /*                                                     |                                                    | QUADRANGEL  */
    /*      -74.05 -74.00 -73.95 -73.90 -73.85 -73.80      |                                                    | AREA KM^2   */
    /*         -------+------+------+------+------- LAT    |  data distance;                                    |           2 */
    /*         |      |      |      |      |      |        |                                                    | 211.172 km  */
    /*         |      |      |      |      |      |        |    lon = -73.85;                                   |             */
    /*   40.90 +------+------+------+------+------+ 0.90   |    lat1 = 40.7;                                    |             */
    /*         |      |      |      |      |      |        |    lat2 = 40.85;                                   |             */
    /* L       |      |      |      |      |      |      L |    latKm = geodist(lat1, lon, lat2, lon, 'K');     |             */
    /* A 40.85 +------+------+------+------+------+ 0.85 A |                                                    |             */
    /* T       |      |                    |      |      T |    lat = 40.7;                                     |             */
    /* I       |      |                    |      |      I |    lon1 = -74;                                     |             */
    /* T 40.80 +------+                    +------+ 0.80 T |    lon2 = -73.85;                                  |             */
    /* U       |      |   Poukeepsie NY    |      |      U |    lonKm = geodist(lat, lon1, lat, lon2, 'K');     |             */
    /* D       |      |   Quadrangle       |      |      D |                                                    |             */
    /* E 40.75 +------+                    +------+ 0.75 E |    area = latKm * LonKm;                           |             */
    /*         |      |                    |      |        |    Poukeepsie_Quadrangel_Area = area;              |             */
    /*         |      |                    |      |        |    put Poukeepsie_Quadrangel_Area = 8.3;           |             */
    /*   40.70 +------+------+------+------+------+ 0.70   |                                                    |             */
    /*         |      |      |      |      |      |        |  run;quit;                                         |             */
    /*         |      |      |      |      |      |        |                                                    |             */
    /*   40.65 +------+------+------+------+------+ 0.65   |                                                    |             */
    /*         |      |      |      |      |      |        | 2 R PACKAGE SF (WORKS WITH ANY POLOGON?)           | POUKEEPSIE  */
    /*         |      |      |      |      |      |        |                                                    | QUADRANGEL  */
    /*   40.60 -------+------+------+------+-------        |  %utl_rbeginx;                                     | AREA KM^2   */
    /*      -74.05 -74.00 -73.95 -73.90 -73.85 -73.80      |  parmcards4;                                       | 210.674     */
    /*                                                     |  library(haven)                                    |             */
    /*                      LONGITUDE                      |  source("c:/oto/fn_tosas9x.R")                     |             */
    /*                                                     |  have<-as.matrix(read_sas("d:/sd1/have.sas7bdat")) |             */
    /*                                                     |  print(have)                                       |             */
    /*   options validvarname=upcase                       |  library(sf)                                       |             */
    /*   libname sd1 "d:/sd1";                             |                                                    |             */
    /*   data sd1.have;                                    |  polygon <- st_polygon(list(have))                 |             */
    /*     input lon lat;                                  |  # Convert to an sf object                         |             */
    /*   cards4;                                           |  sf_polygon <- st_sfc(polygon, crs = 4326)         |             */
    /*   -74.0 40.7                                        |                                                    |             */
    /*   -73.85 40.7                                       |  # Calculate the area                              |             */
    /*   -73.85 40.85                                      |  area <- st_area(sf_polygon)                       |             */
    /*   -74.0 40.85                                       |                                                    |             */
    /*   -74.0 40.7                                        |  # Convert to square kilometers                    |             */
    /*   ;;;;                                              |  area_km2 <- units::set_units(area, "km^2")        |             */
    /*   run;quit;                                         |  writeClipboard(as.character(area_km2))            |             */
    /*                                                     |  print(area_km2)                                   |             */
    /*                                                     |  ;;;;                                              |             */
    /*                                                     |  %utl_rendx(return=area);                          |             */
    /*                                                     |                                                    |             */
    /*                                                     |  %put Poughkeepsi Quadrangle a Area(KM**2) = &area;|             */
    /*                                                     |                                                    |             */
    /**************************************************************************************************************************/


     /*                   _
    (_)_ __  _ __  _   _| |_
    | | `_ \| `_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    */

    options validvarname=upcase;
    libname sd1 "d:/sd1";
    data sd1.have;
      input lon lat;
    cards4;
    -74.0 40.7
    -73.85 40.7
    -73.85 40.85
    -74.0 40.85
    -74.0 40.7
    ;;;;
    run;quit;

    options ls=72 ps=32;
    proc plot data=sd1.have(rename=lat=lat12345678901234567890);
     plot lat12345678901234567890*lon='+' /box
       vaxis= 40.6  to  40.9  by .05 vref= 40.6  to  40.9  by .05
       haxis=-74.05 to -73.80 by .05 href=-74.10 to -73.80 by .05;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*     SD1HAVE                                                                                                            */
    /*                                                                                                                        */
    /*        LON      LAT            LAT12345678901234567890*LON.  Symbol used is '+'.                                       */
    /*                                                                                                                        */
    /*      -74.00    40.70                    -+--------+--------+--------+--------+--------+-                               */
    /*      -73.85    40.70           34567890 ||        |        |        |        |        ||                               */
    /*      -73.85    40.85                    ||        |        |        |        |        ||                               */
    /*      -74.00    40.85              40.90 ++--------+--------+--------+--------+--------++                               */
    /*      -74.00    40.70                    ||        |        |        |        |        ||                               */
    /*                                         ||        |        |        |        |        ||                               */
    /*                                   40.85 ++--------+--------+--------+--------+--------++                               */
    /*                                         ||        |        |        |        |        ||                               */
    /*                                         ||        |        |        |        |        ||                               */
    /*                                   40.80 ++--------+--------+--------+--------+--------++                               */
    /*                                         ||        |        |        |        |        ||                               */
    /*                                         ||        |        |        |        |        ||                               */
    /*                                   40.75 ++--------+--------+--------+--------+--------++                               */
    /*                                         ||        |        |        |        |        ||                               */
    /*                                         ||        |        |        |        |        ||                               */
    /*                                   40.70 ++--------+--------+--------+--------+--------++                               */
    /*                                         ||        |        |        |        |        ||                               */
    /*                                         ||        |        |        |        |        ||                               */
    /*                                   40.65 ++--------+--------+--------+--------+--------++                               */
    /*                                         ||        |        |        |        |        ||                               */
    /*                                         ||        |        |        |        |        ||                               */
    /*                                   40.60 ++--------+--------+--------+--------+--------++                               */
    /*                                         ||        |        |        |        |        ||                               */
    /*                                         -+--------+--------+--------+--------+--------+-                               */
    /*                                       -74.05   -74.00   -73.95   -73.90   -73.85  -73.80                               */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*                                        _ _     _
    / |  ___  __ _ ___    __ _  ___  ___   __| (_)___| |_
    | | / __|/ _` / __|  / _` |/ _ \/ _ \ / _` | / __| __|
    | | \__ \ (_| \__ \ | (_| |  __/ (_) | (_| | \__ \ |_
    |_| |___/\__,_|___/  \__, |\___|\___/ \__,_|_|___/\__|
                         |___/
    */

    data distance;

      lon = -73.85;
      lat1 = 40.7;
      lat2 = 40.85;
      latKm = geodist(lat1, lon, lat2, lon, 'K');

      lat = 40.7;
      lon1 = -74;
      lon2 = -73.85;
      lonKm = geodist(lat, lon1, lat, lon2, 'K');

      area = latKm * LonKm;
      Poukeepsie_Quadrangel_Area = area;
      put Poukeepsie_Quadrangel_Area = 8.3;

    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  SAS VARIABLE                                                                                                          */
    /*                                                                                                                        */
    /*  POUKEEPSIE_QUADRANGEL_AREA=211.172                                                                                    */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*___                __                     _
    |___ \   _ __   ___ / _|  _ __   __ _  ___ (_) __ _  __ _  ___
      __) | | `__| / __| |_  | `_ \ / _` |/ __|| |/ _` |/ _` |/ _ \
     / __/  | |    \__ \  _| | |_) | (_| | (__ | | (_| | (_| |  __/
    |_____| |_|    |___/_|   | .__/ \__,_|\___|/ |\__,_|\__, |\___|
                             |_|             |__/       |___/
    */


    /*----
    CRS Code EPSG code 4326, representing the WGS84 coordinate
    system commonly used for longitude and latitude coordinates
    ----*/


    %utl_rbeginx;
    parmcards4;
    library(haven)
    source("c:/oto/fn_tosas9x.R")
    have<-as.matrix(read_sas("d:/sd1/have.sas7bdat"))
    print(have)
    library(sf)

    polygon <- st_polygon(list(have))
    # Convert to an sf object
    sf_polygon <- st_sfc(polygon, crs = 4326)

    # Calculate the area
    area <- st_area(sf_polygon)

    # Convert to square kilometers
    area_km2 <- units::set_units(area, "km^2")
    writeClipboard(as.character(area_km2))
    print(area_km2)
    ;;;;
    %utl_rendx(return=area);

    %put Poughkeepsi Quadrangle a Area(KM**2) = %sysfunc(putn(&area,8.3));

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  R VARAIBLE AREA_KM2                         SAS MACRO VARIABLE AREA                                                   */
    /*                                                                                                                        */
    /*  area_km2 <- units::set_units(area, "km^2")  put Poughkeepsi Quadrangle a Area(KM**2) = %sysfunc(putn(&area,8.3))      */
    /*                                                                                                                        */
    /*  print(area_km2)                                                                                                       */
    /*                                                                                                                        */
    /*  210.6736 [km^2]                             Poughkeepsi Quadrangle a Area(KM**2) =  210.674                           */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*____
    |___ /   _ __ ___ _ __   ___  ___
      |_ \  | `__/ _ \ `_ \ / _ \/ __|
     ___) | | | |  __/ |_) | (_) \__ \
    |____/  |_|  \___| .__/ \___/|___/
                     |_|
    */

    https://github.com/rogerjdeangelis/utl-R-AI-igraph-list-connections-in-a-non-directed-graph-for-a-subset-of-vertices
    https://github.com/rogerjdeangelis/utl-add-new-siblings-to-existing-families-using-r-igraph-sql-sas-r-and-python-multi-languages
    https://github.com/rogerjdeangelis/utl-how-many-triangles-in-the-polygon-r-igraph-AI
    https://github.com/rogerjdeangelis/utl-linking-connected-nodes-in-a-network-graph-r-igraph
    https://github.com/rogerjdeangelis/utl-three-heat-maps-of-bivariate-normal-wps-r-graph-plot
    https://github.com/rogerjdeangelis/utl-web-html-graphics-and-charts-with-free-r-googlevis-package
    https://github.com/rogerjdeangelis/utl_javascript_and_classic_map_graphics_with_mouseovers_and_multiple_drilldowns
    https://github.com/rogerjdeangelis/utl_graphics_zipcode_boundary_maps
    https://github.com/rogerjdeangelis/utl_proc_gmap_classic_graphics_grid_containing_four_states
    https://github.com/rogerjdeangelis/utl_remove_isolated_nodes_from_an_network_r_igraph

    https://github.com/rogerjdeangelis/utl-draw-polygons-around-clusters-of-points-r-convex-hulls
    https://github.com/rogerjdeangelis/utl_convex_hull_maximum-distance-between-two-points-in-a-scatter-plot
    https://github.com/rogerjdeangelis/utl_convex_hull_polygons_encompassing_a_three_dimensional_scatter_plot
    https://github.com/rogerjdeangelis/utl_simple_convex_hull_polygon_envelop_for_a_scatter_plot

    https://github.com/rogerjdeangelis/utl-area-of-intersection-of-arbitrary-polygons-R-AI
    https://github.com/rogerjdeangelis/utl-area-under-y-equal-x-squared-from-0-to-1-trapezoid-auc-clinical
    https://github.com/rogerjdeangelis/utl-calculate-the-area-within-a-latittude-longitude-polygon
    https://github.com/rogerjdeangelis/utl-find-area-under-curve-and-compute-regression-slope-and-intercept-using-sqllite-r-python
    https://github.com/rogerjdeangelis/utl-r-compute-the-area--of-an-image-which-is-under-a-curve-AI-image-processing-AI
    https://github.com/rogerjdeangelis/utl-r-python-compute-the-area-between-two-curves-AI-sympy-trapezoid
    https://github.com/rogerjdeangelis/utl_calculating_the_area_between_a_curve_and_a_straight_line
    https://github.com/rogerjdeangelis/utl_monte_carlo_simulation_to_determine_the_area_under_a_non_integrable_function

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
