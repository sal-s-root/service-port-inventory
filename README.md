# service-port-inventory

Find services that claim the same transport protocol and port.

## Run

Python 3.8 or later; no third-party packages or cloud credentials are required.

```sh
python3 tool.py examples/input.json
cat examples/input.json | python3 tool.py -
python3 -m unittest -v
```

## Example

Input:

```json
{
  "services": [
    {
      "name": "api",
      "port": 8080
    },
    {
      "name": "admin",
      "port": 8080
    }
  ]
}
```

Result:

```json
{
  "conflicts": [
    {
      "protocol": "tcp",
      "port": 8080,
      "services": [
        "admin",
        "api"
      ]
    }
  ]
}
```

The command writes JSON to standard output. Exit 0 means evaluation completed;
a finding in the output may still require attention. Invalid input returns exit 2,
with an error on standard error and no result on standard output.

## Operational scope

Binding conflicts also depend on addresses, namespaces, and reuse settings; these are not modeled.

The tool evaluates supplied data locally. It does not change hosts, deploy
resources, or establish production compliance. Example identifiers and observations
are test fixtures. Keep credentials and private inventory out of Git.

## Design and verification

See [NOTES.md](NOTES.md) for the input contract, verification, and integration notes.
Related utilities: [process-snapshot-sorter](https://github.com/sal-s-root/process-snapshot-sorter), [memory-pressure-checker](https://github.com/sal-s-root/memory-pressure-checker).
