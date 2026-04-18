# Cow Txt Interceptor Plugin

[![Build Status](https://github.com/InCogNiTo124/cow-txt-plugin/workflows/Main/badge.svg?branch=master)](https://github.com/InCogNiTo124/cow-txt-plugin/actions)

A [Traefik](https://traefik.io) middleware plugin written in [Go](https://golang.org) that intercepts requests hitting the exact path `/cow.txt` and immediately serves a predefined ASCII cow, returning a standard `200 OK` and adjusting the `Content-Type` header to `text/plain; charset=utf-8`. 

Any requests that do not match the `/cow.txt` path will pass through uninterrupted to the next middleware or service in the chain.

## Usage & Configuration

For the plugin to be active on a given Traefik instance, it must be declared in both your static and dynamic configuration.

### Static Configuration

Define the plugin explicitly in your static configuration file (e.g., `traefik.yml`):

```yaml
experimental:
  plugins:
    cow-txt-plugin:
      moduleName: github.com/InCogNiTo124/cow-txt-plugin
      version: v0.1.0 # Or whichever version tag you are using
```

### Dynamic Configuration

Register the active middleware to a given Traefik HTTP router in your dynamic configuration setup:

```yaml
http:
  routers:
    my-router:
      rule: host(`example.localhost`)
      service: service-foo
      entryPoints:
        - web
      middlewares:
        - cow-interceptor

  services:
    service-foo:
      loadBalancer:
        servers:
          - url: http://127.0.0.1:5000
  
  middlewares:
    cow-interceptor:
      plugin:
        cow-txt-plugin:
          # This plugin currently accepts an empty configuration struct, but the structure is mandated by Traefik.
          {}
```

## Credits 

Compliant with the cow.txt RFC specification defined at https://moooo.farm/.

Initial configuration setup, tooling modernization, and namespace migrations were authored with the assistance of **Gemini 3.1 Pro**.
