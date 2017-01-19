# Sensu SNMP Trap Extension

A SNMP trap listener for the Sensu client process. This check
extension creates a SNMPv2 trap listener, loads & imports MIB files,
and attempts to translate SNMP traps into Sensu check results. Sensu
proxy clients are created for SNMP devices, using the device hostname
as the client name, when a SNMP trap is received.

The SNMP trap to Sensu check result translation logic is currently
being improved. Failure to translate the SNMP trap will produce a
check result (and event) like the following (addresses have been
redacted):

![screenshot](https://raw.github.com/sensu-extensions/sensu-extensions-snmp-trap/master/iflinkdown.png)

## Inspiration

This SNMP trap extension was inspired by the work done by Toby Jackson
on [SNMPTrapHandler](https://github.com/warmfusion/sensu-extension-snmptrap).

## Installation

This extension requires Sensu version >= 0.26.

On a Sensu client machine.

```
sensu-install -e snmp-trap:0.0.33
```

Edit `/etc/sensu/conf.d/extensions.json` to load it.

``` json
{
  "extensions": {
    "snmp-trap": {
      "version": "0.0.33"
    }
  }
}
```

## Configuration

Edit `/etc/sensu/conf.d/snmp_trap.json` to change its configuration.

``` json
{
  "snmp_trap": {
    "community": "secret",
    "result_attributes": {
      "datacenter": "DC01"
    },
    "result_status_map": [
      ["authenticationFailure", 0]
    ]
  }
}
```

|attribute|type|default|description|
|----|----|----|---|
|bind|string|0.0.0.0|IP to bind the SNMP trap listener to|
|port|integer|1062|Port to bind the SNMP trap listener to|
|community|string|"public"|SNMP community string to use|
|mibs_dir|string|"/etc/sensu/mibs"|MIBs directory to import and load MIBs from|
|imported_dir|string|"$TMPDIR/sensu_snmp_imported_mibs"|Directory to store imported MIB data in|
|handlers|array|["default"]|Handlers to specify in Sensu check results|
|result_attributes|hash|{}|Custom check result attributes to add to every SNMP trap Sensu check result|
|result_map|array|[]|SNMP trap varbind to Sensu check result translation mappings|
|result_status_map|array|[]|SNMP trap OID to Sensu check result status mappings|
