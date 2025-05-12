# LimaCharlie External Adapter Configuration


## Step 1: Configure Permissions

The enable the External Adapter, an administrator will need to enable permissions, go to Groups -> Permissions and give these permissions to the organization: 

![External Adapter Permissions](https://github.com/user-attachments/assets/f576422f-a8a9-41b8-9661-19004839a88d)

Once the permissions are enabled and applied to the group for an organization, you will see "External Adapter" appear in the left navigation under "Sensors":

![Sensors -> External Adapters](https://github.com/user-attachments/assets/f72c96b4-5273-4896-aa4d-bede93eb3a09)

## Step 2: Create An External Adapter

First we will click on the "Add External Adapter" button to create a new External Adapter: 
![Add External Adatper](https://github.com/user-attachments/assets/51d5b36a-9c2e-4927-b49a-f4b485c81393)

The configuration for External Adapters is similar to regular adapters, with the main difference being the configuration and processing is done in the cloud vs on the system running the adapter. You can see more information on adapter configuraion in the [LimaCharlie Docs](https://docs.limacharlie.io/v2/docs/adapter-usage). In this example we will be routing syslog from a Linux system into LimaCharlie. You will need an ```ORG ID ``` to define the tenant/org the data will be sent to, as well as an [```installation key```](https://docs.limacharlie.io/docs/installation-keys) created. 

In this configuration, we setup the External Adapter as a syslog forwarder, we provide an ```installation_key``` and ```OID```, then set our mappings along with a regex that will convert syslog events to JSON, followed by the port we want the adapter to communicate on.



```yaml
sensor_type: syslog
syslog:
  client_options:
    hostname: test-syslog
    identity:
      installation_key: [YOUR INSTALLATION KEY]
      oid:[YOUR ORG ID]
    mappings:
      - event_time_path: timestamp
        event_type_path: app
        parsing_re: >-
          ^(?:\<(?P<priority>\d+)\>(?P<severity>\d+) )?(?P<timestamp>\S+\s\d{2}
          \d{2}:\d{2}:\d{2})\s+(?P<host>\S+)\s(?P<app>\S+)\[(?P<pid>\d+)\]:\s(?P<msg>.*)
        transform:
          +dvc_product: '{{ $.app }}'
    platform: text
    sensor_seed_key: syslog-test
  port: 4242

```

## Step 3: Run Adapter 

We download and execute the adapter on a Linux host, in this case a Debian system. You can download the agent using wget: 

``` wget https://downloads.limacharlie.io/adapter/linux/64 ```



![image](https://github.com/user-attachments/assets/22dbf1a4-4930-4f7c-9947-ad1dae99a8b9)


![image](https://github.com/user-attachments/assets/fabfec70-d518-42da-bbb6-79a75d27f8d1)



