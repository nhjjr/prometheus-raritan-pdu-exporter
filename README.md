# Prometheus Raritan PDU Exporter
Python-based Raritan PDU exporter for [prometheus.io](https://prometheus.io/).

This exporter uses the Raritan JSON-RPC API to find inlet, outlet, pole, and 
device sensors to expose their readings to the Prometheus monitoring system.

We have purposely opted not to use the Raritan PDU Python API since direct
calls to the JSON-RPC API are very straight-forward. Furthermore, only a
handful of methods are used (`getInlets`, `getOutlets`, `getMetaData`, 
`getDeviceSlots`, `getDevice`, `getReading` and `getState`) on the `/bulk`
endpoint, ignoring most of the methods included in the Python API. As a result
we do not have to bundle the Raritan PDU Python API with this project. 

## Installation
```commandline
git clone git@jugit.fz-juelich.de:inm7/infrastructure/prometheus_raritan_pdu_exporter.git
cd prometheus_raritan_pdu_exporter
pip install -r requirements.txt
```

## Usage for PDU collection

    prometheus_raritan_pdu_exporter.py [-h] -c config [-w listen_address] [-k]
        
    optional arguments:
      -h, --help            show this help message and exit
      -c config, --config config
                            configuration json file containing PDU addresses and
                            login info
      -w listen_address, --web.listen-address listen_address
                            Address and port to listen on (default = :9840)
      -k, --insecure        allow a connection to an insecure raritan API
      -l, --log-level       Logging level (default = warning)

### Example

```commandline
python3 prometheus_raritan_pdu_exporter.py --web.listen-address :9840 -k
```

### Debugging
To enable debugging, set `-l debug` to log debug messages. Note that this will 
provide a lot of additional information and is therefore not a recommended 
setting for long-term use in production.

## Testing
Install test requirements using:

```commandline
pip3 install -r test-requirements.txt
```

Run tests by executing the following in the package root directory:

```commandline
pytest tests
```

### Test Requirements
The tests use `vcrpy`, and can hence be run using recorded data rather than 
live data. To re-establish the recorded data, remove the 
`tests/fixtures/vcr_cassettes/` folder and its contents. Make sure to include 
a `tests/fixtures/config.json` that, unlike the `config.json-example` file has 
real login credentials for the Raritan PDUs to be tested.
