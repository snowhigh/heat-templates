^^^^^^^^^^^^^^^^^^^^
hot/autoscaling.yaml
^^^^^^^^^^^^^^^^^^^^

example::

  heat stack-create AutoscalingWordpress -f autoscaling.yaml -P image=centosq \
  -P key=simon -P flavor=m1.small \
  -P database_flavor=m1.small \
  -P subnet_id=<internal network subnet> \
  -P database_name=wordpress \
  -P database_user=wordpress \
  -P external_network_id=4bddd327-c05a-4d6a-9c04-e9416ba26e2f \
  -P network=<internal network>

Here we measure avg cpu utilization in this stack for auto scaling criteria::

  cpu_alarm_high:
      type: OS::Ceilometer::Alarm
      properties:
        description: Scale-up if the average CPU > 50% for 1 minute
        meter_name: cpu_util
        statistic: avg
        period: 60
        evaluation_periods: 1
        threshold: 50
        alarm_actions:
          - {get_attr: [web_server_scaleup_policy, alarm_url]}
        matching_metadata: {'metadata.user_metadata.stack': {get_param: "OS::stack_id"}}
        comparison_operator: gt

The meter_name can be cpu usage or network bandwidht...etc. More meters can be found here::

http://docs.openstack.org/admin-guide-cloud/content/section_telemetry-compute-meters.html
