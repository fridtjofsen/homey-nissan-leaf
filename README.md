# Nissan Leaf

**⚠️ DEPRECATED: NissanConnect EV service shuts down March 30, 2026. This app will stop working then.**

Adds support for Nissan Leaf (pre-May 2019) and eNV200 (pre-2022).

## ⚠️ Important Notice

Nissan has announced that the NissanConnect EV service will be discontinued on **March 30, 2026**. After this date, this app will no longer work as it relies on Nissan's cloud service.

The following features will still work directly via the car's navigation system:
- Climate control timer
- Charging timer

## Local development

Prerequisites:
- [Node.js and NPM](https://nodejs.org/en/download)
- [git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
- [docker](https://docs.docker.com/engine/install/)
- [Homey](https://apps.developer.homey.app/the-basics/getting-started)

```sh
npm install
homey app run
```

Be sure to follow eslint
```sh
npm run lint-fix
```
