# Service Packages build path

../BUILD/SERVICE/PARTS/PackageService/PackageService.1.0.5

## Collect folders path

#### Normal

```sh
//192.168.1.202/BSService/ManagementServices/packageservice/pools/collect/[service/web]
```

#### Force (without verify)

```sh
//192.168.1.202/BSService/ManagementServices/packageservice/pools/collect-force/[service/web]
```

## Link preview & update

*Local 202*: [http://192.168.1.202:4111](http://192.168.1.202:4111) 

*Sandbox (via VPN)*: [http://10.235.235.100:4111](http://10.235.235.100:4111)

*Pro (via VPN)*: [http://10.235.235.100:4112](http://10.235.235.100:4112)

## Format Version file (version.json)

Location: same level EXE(service) or Bin folder (web)

```json
{
    "Name": "Name of web/service",
    "Version": "5.3.10",
    "Description": "Fix bug...."
}
```