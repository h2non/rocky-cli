# rocky [![Build Status](https://api.travis-ci.org/h2non/rocky-cli.svg?branch=master&style=flat)](https://travis-ci.org/h2non/rocky-cli) [![NPM](https://img.shields.io/npm/v/rocky-cli.svg)](https://www.npmjs.org/package/rocky-cli)

Featured **command-line** interface for **[rocky](https://github.com/h2non/rocky)** `+0.2` programmatic HTTP/S proxy.

With `rocky-cli` you can easily setup a versatile HTTP proxy from a TOML [configuration file](#configuration-file)

## Installation

```bash
npm install -g rocky
```

### Standalone binaries

- [linux-x64](https://github.com/h2non/rocky-cli/releases/latest)
- [darwin-x64](https://github.com/h2non/rocky-cli/releases/latest)

Packaged using [nar](https://github.com/h2non/nar). Shipped with node.js `0.12.7`

##### Usage

```
chmod +x rocky-0.2.1-linux-x64.nar
```

```
./rocky-0.2.1-linux-x64.nar exec --port 3000 --config rocky.toml
```

## Usage

```bash
Start rocky HTTP proxy server
Usage: rocky [options]

Options:
  --help, -h     Show help                                             [boolean]
  --config, -c   File path to TOML config file
  --port, -p     rocky HTTP server port
  --forward, -f  Default forward server URL
  --replay, -r   Define a replay server URL
  --route, -t    Define one or multiple routes, separated by commas
  --key, -k      Path to SSL key file
  --cert, -e     Path to SSL certificate file
  --secure, -s   Enable SSL certification validation
  --balance, -b  Define server URLs to balance between, separated by commas
  --mute, -m     Disable HTTP traffic log in stdout                    [boolean]
  --debug, -d    Enable debug mode                                     [boolean]
  -v, --version  Show version number                                   [boolean]

Examples:
  rocky -c rocky.toml \
  -f http://127.0.0.1:9000 \
  -r http://127.0.0.1
```

#### Examples

Passing the config file:
```
rocky --config rocky.toml --port 8080
```

Reading config from `stdin`:
```
cat rocky.toml | rocky --port 8080
```

Transparent `rocky.toml` file discovery in current and higher directories:
```
rocky --port 8080
```

Alternatively `rocky` can find the config file passing the `ROCKY_CONFIG` environment variable:
```
ROCKY_CONFIG=path/to/rocky.toml rocky --port 8080
```

Or for simple configurations you can setup a proxy without a config file, defining the routes via flag:
```
rocky --port --forward http://server --route "/download/*, /images/*, /*"
```

## Configuration file

Default configuration file name: `rocky.toml`

For an up-to-date list of supported configuration params, see [rocky documentation](https://github.com/h2non/rocky#configuration)

The configuration file must be declared in [TOML](https://github.com/toml-lang/toml) language
```toml
port = 8080
forward = "http://google.com"
replay = ["http://duckduckgo.com"]

[ssl]
cert = "server.crt"
key  = "server.key"

[/users/:id]
method = "all"
forward = "http://new.server"

[/oauth]
method = "all"
forward = "http://auth.server"

[/*]
method = "GET"
forward = "http://old.server"

[/download/:file]
method = "GET"
timeout = 5000
balance = ["http://1.file.server", "http://2.file.server"]

[/photo/:name]
method = "GET"
replayAfterForward = true
[[replay]]
  target = "http://old.server"
  forwardHost = true
[[replay]]
  target = "http://backup.server"
```

## License

MIT - Tomas Aparicio
