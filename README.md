# contacts-transformer-template
> Contacts Node.js transformer template for OIH platform

This is a template for creating an OIH transformer which we recommend using it as the first step of the development. The transformer comes with a very a basic architecture which can be used on OIH. You can just clone it and use it.

## Transformer Architecture

``` bash
├── component.json
├── Dockerfile
├── lib
│   ├── actions
│   │   ├── transform.js
│   │   ├── transformPersonFromOih.js
│   │   └── transformPersonToOih.js
│   └── expressions
│       ├── personFromOih.js
│       └── personToOih.js
├── logo.png
├── package.json
└── spec
    ├── seed
    │   └── person.js
    └── transform.spec.js
```

All Node.js transformers for OIH  platform get build by NPM `run-script` which actually first checks the configuration in `package.json` and starts initialising the `node` and `npm` versions and build them. As a next step, the dependencies get downloaded and build. All Node.js transformers must use the following dependencies:

```json
"dependencies": {
  "elasticio-sailor-nodejs": "^2.2.0",
  "elasticio-node": "^0.0.8"
}
```

Sailor is the Node.js SDK of the OIH platform which main role is to make the transformer part of OIH platform and it ensures a smooth communication with the platform.

### component.json

The file acts as an transformer descriptor which is interpreted by OIH platform to gather all required information. For instance, you could define the transformer's title and description. Here is the only place where the transformer's functionality could be listed. In this case we only have **actions** in both directions `from` and `to` OIH.

### Dockerfile

Your transformer should be build as a [Docker image](https://docs.docker.com/v17.09/engine/userguide/storagedriver/imagesandcontainers/). That is why this file plays a significant role in the whole process of deploying the transformer on OIH. An **important** part here is the `ENTRYPOINT` where you start your transformer. For this purpose you should have a `start` script in your `package.json` file which specifies the path to the script which runs the sailor:

```json
  "start": "./node_modules/elasticio-sailor-nodejs/run.js"
```

> NOTE: Never use `root` as user in a Dockerfile

### lib

This directory contains sub-directories such as **actions** and **expressions**.
The Node.js sources are in the sub-directories `lib/actions` and `lib/expressions`.
In `lib/actions` you will find the functions which are responsible for making the real transformation. On the other hand in directory `lib/expressions` are the models or expressions which make the real mapping.

### Logo

If you have a logo for your transformer, you can place a file called `logo.png` in the root directory of your transformer. In case you don't provide a logo, the generic one will be showed.

### Spec

This directory consists of test files and seed data which is user for testing purposes. Tests are not required, but we recommend you to test the functionality before you deploy the transformer.

## Deployment

When you are ready to deploy your transformer, you should first build a [Docker image](https://docs.docker.com/v17.09/engine/userguide/storagedriver/imagesandcontainers/) of your transformer and then push it to [Dockerhub repository](https://hub.docker.com/u/openintegrationhub) of Openintegrationhub.
The first command creates the image and the second one pushes it to [Dockerhub](https://hub.docker.com/u/openintegrationhub).

```Dockefile
docker build -t openintegrationhub/[NAME] .
docker push openintegrationhub/NAME[:TAG]
```

## Getting Started

This transformer example is developed and tested with [Snazzy Contacts API](https://snazzycontacts.com). If you want to build a flow using this example, you could use it in combination with [contacts-adapter-template](https://github.com/openintegrationhub/contacts-adapter-template).


To run the tests locally just run `npm test`.

## Actions
This template **transformer** supports the following **actions**:

#### Actions:
  - Transform a person **from** OIH (```transformPersonFromOih.js```)
  - Transform a person **to** OIH (```transformPersonToOih.js```)

## License

Apache-2.0 © [Wice GmbH](https://wice.de/)
