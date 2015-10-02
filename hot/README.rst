^^^^^^^^^^^^^^^^^^^^
hot/autoscaling.yaml
^^^^^^^^^^^^^^^^^^^^

example::

  neutron net-list

  heat stack-create auto_cpu -f autoscaling_cpu.yaml -P image=TestVM -P key=simon \
  -P external_network_id=91cc5b54-6dba-44b7-b6fe-0813234fa622 \
  -P network=9a0ededa-ddcd-41e7-9e50-0be1b3743406 \
  -P subnet_id=1df0af6a-ae49-40ac-ba72-c58c5fa470ea

^^^^^^^^^^^^
Auto scaling
^^^^^^^^^^^^

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

^^^^^^^^^^^^^
Load balancer
^^^^^^^^^^^^^

Session persistence can be defined in heat template. more resource can be found here
http://docs.openstack.org/developer/heat/template_guide/openstack.html
ex::

  OS::Neutron::Pool
  ...
    vip : Map
    ...
    session_persistence : Map
    type : String
      Allowed values: SOURCE_IP, HTTP_COOKIE, APP_COOKIE
  
  
