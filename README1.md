# Setup Commands
> Infrastructure-level monitoring

## 1. server setup
```
git clone https://github.com/hpcfapix/phantom_monitoring_server.git
cd phantom_monitoring_server
./setup.sh
```

## 2. run the server
```
cd phantom_monitoring_server
./start.sh
curl localhost:3033
```

## 3. client setup
```
git clone https://github.com/hpcfapix/phantom_monitoring_client.git
cd phantom_monitoring_client
./setup.sh

```
## 4. client configure and build

Define the **unique name** of the monitored platform and provide it as platform_id option in the config file .src/mf_config.ini.

In the configure file make sure that the **server** url is setup correctly. By default infrastructure monitoring, the **:application_id** is **infrastructure**.

Please find in the following link the dependencies for each plug-in. Make sure that the hardware is available for using the specific plug-in.

https://github.com/phantom-monitoring-framework/phantom_monitoring_client/blob/master/src/plugins/README.md

Please follow the commands below for configure and build:
```
cd phantom_monitoring_client
vim src/mf_config.ini
make clean-all
make all
make install
```

## 5. register the application by the server
replace the **:platform_id** by the one in your case.

curl -H "Content-Type: application/json" -XPUT localhost:3033/v1/phantom_mf/workflows/infrastructure -d '{"application":"infrastructure","author":"Random Guy","optimization":"Time","tasks":[{"name":":"**:platform_id**","exec":"mf_client","cores_nr": "1"}]}'

by default infrastructure monitoring, the **:platform_id** is given in `src/mf_config.ini`.


## 6. configure for the specific platform the required parameters
replace the **:platform_id** by the one in your case.

curl -H "Content-Type: application/json" -XPUT localhost:3033/v1/phantom_rm/configs/**:platform_id** -d '{"parameters":{"MAX_CPU_POWER":"24.5","MIN_CPU_POWER":"6.0","MEMORY_POWER":"2.016","L2CACHE_MISS_LATENCY":"59.80","L2CACHE_LINE_SIZE":"128","E_DISK_R_PER_KB":"0.0556","E_DISK_W_PER_KB":"0.0438","E_NET_SND_PER_KB":"0.14256387","E_NET_RCV_PER_KB":"0.24133936"}}'


## 7. start the monitoring client
```
cd phantom_monitoring_client/scripts
sudo ./start.sh
```

## 8. check the application_id, task_id, and experiment_id in the log file
```
sudo cat phantom_monitoring_client/dist/bin/log/xxx
```

## 9. use these IDs to query the metrics
details in: https://github.com/phantom-monitoring-framework/phantom_monitoring_server/blob/master/README.md
