# docker-munin-grafana
A docker stack for displaying munin data via grafana

This is not a dashboard. We use grafana-rrd-server to feed rrd data from munin
to grafana.

Background: we use munin for system metrics, which works nicely. We put
considerable efforts into customization and writing of munin plugins, so we
want to keep these data. All we are missing is a more comfortable way to
display the RRD data delivered. This is, where `grafana` springs to mind.



### Prerequisites

We need `docker-compose`.

We need some RRD data to display (for instance an already running `munin`
instance).


### Setup

Clone or copy this repository:

    $ clone https://github.com/waeup/docker-munin-grafana

Then, adjust settings in `docker-compose.yml`. Especially the grafana password
should be changed before starting.

This is also a good time to connect your local RRD source with the
``/rrd-data`` dir of the grafana-rrd-server (via shared volume). On a system,
where munin is installed in standard paths, this path would be `/var/lib/munin`
which could be connected like this:

    ...
    rrd:
    ...
        volumes:
        - /var/lib/munin:/rrd-data
    ...

If you want to play around a bit first, then copy some rrd files into the local
``rrd-data`` directory and keep the respective setting in `docker-compose.yml`
untouched.

Afterwards you can bring the whole stuff up:

    $ docker-compose up

will first build the `grafana-rrd-server` image, fetch downloadable images and
copy everything into the correct location. You should now be able to login into
the site on `http://127.0.0.1:3000`.

This is also the backend URL you should pass to your web frontend (apache,
nginx, or whatever you use for proxying).

After logging in, you should change the password (if not done beforehand).


### Create a Data Source

Click on "Create a data source" and pick "SimpleJSON" from the list.

In the settings screen apearing next, set something sensible as `name` and fill
in the URL of the freshly started grafana-rrd-server. If you have not fiddled
around too much with the `docker-compose.yml`, the URL will be

    http://rrd:9000

as this is the hostname provided by docker-compose for the rrd-server.

Leave everything else untouched and click "Save & Test". This should give you a
green message saying everything is allright.


### Create a munin dashboard

There is a munin dashboard already made available at the grafana.com website.
You can import it at any time by clicking on the big "plus" on the left, then
hovering over to "Create" and "Import". In the presented form enter number
``9627``, click on "Load", select your freshly created data source in the last
row of the form and finally click on "Import".

The raw Munin dashboard will most probably look empty after import. That is,
because the data sources of all the panels will most likely have a different
name than the given ones from the imported template.

Therefore, we have to go through all the elements and have to fix these
sources.
