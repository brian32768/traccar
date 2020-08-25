I am using Traccar as an integration in Home Assistant to track
the location of two smart phones.

https://hub.docker.com/r/traccar/traccar

You have to set up an XML file that will be traccar.xml in the config volume.
I have sample copies here, you can use H2 or MySQL. There is a password
embedded in there that you need to change for your set up.

You edit it per instructions here:
https://www.traccar.org/configuration-file/

I included the h2 file to allow using the default H2 database, which I know nothing about.
I made the traccar_mysql.xml file to use MySQL, which I actually understand.
Then I switched back to using H2, because I got tired of fighting with the XML file
when migrating to SWARM.

== Use OpenCage Geocoder

    <entry key='geocoder.enable'>true</entry>
    <entry key='geocoder.type'>opencage</entry>
    <entry key='geocoder.url'>http://api.opencagedata.com/geocode/v1</entry>
    <entry key='geocoder.key'>API KEY GOES HERE</entry>

== Use MySQL

Using MySQL instead of H2- if you set the variables correctly in .env then on the first
startup, the database and user will be created in the mysql container. This means the
container will restart itself once. The traccar container depends on the mysql container
so it will delay starting until mysql is ready. 

   <entry key='database.driver'>com.mysql.jdbc.Driver</entry>
   <entry key='database.url'>jdbc:mysql://traccar-mysql:3306/traccar_database?serverTimezone=UTC&amp;useSSL=false&amp;allowMultiQueries=true&amp;autoReconnect=true&amp;useUnicode=yes&amp;characterEncoding=UTF-8&amp;sessionVariables=sql_mode=''</entry>
   <entry key='database.user'>traccar_user</entry>
   <entry key='database.password'>[PASSWORD]</entry>

== Default H2 settings

    <entry key='database.driver'>org.h2.Driver</entry>
    <entry key='database.url'>jdbc:h2:./data/database</entry>
    <entry key='database.user'>sa</entry>
    <entry key='database.password'></entry>

== Set up notes

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

