---
Creation Time: Sunday, February 23rd 2025
Modified Time: Sunday, February 23rd 2025
---
[[TIME SERIES DB]]

### Setup on mac: 
Download using cURL
curl --location -O "https://download.influxdata.com/influxdb/releases/influxdb2-2.7.11_darwin_amd64.tar.gz"
tar zxvf ./influxdb2-2.7.11_darwin_amd64.tar.gz
sudo cp influxdb2-2.7.11/influxd /usr/local/bin/
start using: influxd
open client ui on: http://localhost:8086/orgs/e190e41dfcd7fedb/new-user-setup/cli

_Example for using Influx DB

Storing driver continuous location updates  to track user location on applications like UBER.

InfluxDB measurement name: `driver_location
Tags: `driverID or vehicle Type or region for filters`
fields: `lat, long, speed, acuracy`
timestamp: for each event received at time series db, time is automatically captured.

Ex: driver_location,driverId=driver123,vehicleType=sedan latitude=37.7749,longitude=-122.4194,speed=45,accuracy=5 1631548400000000000

