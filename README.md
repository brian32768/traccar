I am using Traccar as an integration in Home Assistant to track
the location of two smart phones. I usually use a MySQL database
to hold the data.

https://hub.docker.com/r/traccar/traccar

== Configuration

Set up an XML file that will contain secrets.
I have sample copies here, you can use H2 or MySQL but this project uses MySQL.
There is a password embedded in the mysql version that you need to change for your set up.

You also need to put the same secrets into "mysql_root_password" and
"mysql_user_password" so that the MySQL container can use them.

You edit traccar XML  per instructions here:
https://www.traccar.org/configuration-file/

== Use OpenCage Geocoder

    <entry key='geocoder.enable'>true</entry>
    <entry key='geocoder.type'>opencage</entry>
    <entry key='geocoder.url'>http://api.opencagedata.com/geocode/v1</entry>
    <entry key='geocoder.key'>API KEY GOES HERE</entry>

== Other set up notes

When creating devices for phones, set the map icon in "Category"

Home in Astoria location to use is 46.1854 -123.8309

In Account
Zoom 14
Permissions Admin
Attributes
Speed Unit, Distance unit, Volume unit, Timezone

Geofences
Astoria Co-op
800 Exchange
Home

Server:
  Basemap
  46.18, -123.83 10

Users: Julie, Brian

## Deploy

    docker stack deploy -c docker-compose.yml traccar
