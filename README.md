icinga2_api

python library and command line utility to support [icinga2 api](http://docs.icinga.org/icinga2/snapshot/doc/module/icinga2/chapter/icinga2-api)

### Pre-requisites

* A working icinga2 API setup through docker/docker/your_machine

### Description

* The python library and the command line utility's aim is to abstract out repetitive data like host, port, credentials, headers, etc. Strip
out that layer and you have a 1:1 mapping to how the icinga2 API is written. See Examples mapping from command line curl calls to the command line utlity to the python library.

### Examples

* Check API status

  - Without icinga2_api, through the command line

    ```bash
    $ curl -u $ICINGA2_API_USER:$ICINGA2_API_PASSWORD  -k "https://$ICINGA2_HOST:$ICINGA2_API_PORT/v1/status" | python -m json.tool
    ```

  - With the icinga2_api command line utility

    ```bash
    $ python icinga2_api -p docker
    ```

    No connection parameters are required as the user can specify connection parameters for a profile in a profile file. 
    Default location: ~/.icinga2/api.yml, default profile: default

    ```bash
    $ python icinga2_api --help
    ```

    to view all options

  - With the icinga2_api library

    ```python
    from icinga2_api import api
    obj = api.Api(profile='docker')
    obj.read()
    ```

* Create a dummy hostgroup:

  - Without icinga2_api, through the command line

    ```bash
    $ curl -u $ICINGA2_API_USER:$ICINGA2_API_PASSWORD \
         -H 'Accept: application/json' -X PUT \
         -k "https://$ICINGA2_HOST:$ICINGA2_API_PORT/v1/objects/hostgroups/api_dummy_hostgroup" \
         -d '{ "attrs": { "display_name": "api_dummy_hostgroup" } }' | python -m json.tool
    ```

  - With the icinga2_api command line utility

    ```bash
    $ python icinga2_api -p docker -a create -u '/v1/objects/hostgroups/api_dummy_hostgroup' -d '{ "attrs": { "display_name": "api_dummy_hostgroup" } }' 
    ```

  - With the icinga2_api library

    ```python
    from icinga2_api import api
    obj = api.Api(profile='docker')
    uri = '/v1/objects/hostgroups/api_dummy_hostgroup'
    data = { "attrs": { "display_name": "api_dummy_hostgroup" } }
    obj.create(uri, data)
    ```
