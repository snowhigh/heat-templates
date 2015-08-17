==============
hot/autoscaling.yaml
====================
ex.
heat stack-create AutoscalingWordpress -f autoscaling.yaml -P image=centosq \
-P key=simon -P flavor=m1.small \
-P database_flavor=m1.small \
-P subnet_id=<internal network subnet> \
-P database_name=wordpress \
-P database_user=wordpress \
-P external_network_id=4bddd327-c05a-4d6a-9c04-e9416ba26e2f \
-P network=<internal network>
