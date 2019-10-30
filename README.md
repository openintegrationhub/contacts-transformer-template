# contacts-transformer-template
> Contacts Node.js transformer template for OIH platform

This is a template for creating an OIH transformer. We recommend using it as the first step of development. The adapter comes with a very a basic architecture which can be used on OIH. Just clone it and get started!

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
└── test
    ├── seed
    │   └── person.js
    └── transform.test.js
```

All Node.js transformers get build by NPM `run-script` which checks the configuration in `package.json` first, then starts initialising the `node` and `npm` versions and builds them. As a next step, the dependencies get downloaded and build. All Node.js transformers must use the following dependencies:

```json
"dependencies": {
  "elasticio-sailor-nodejs": "^2.2.0",
  "elasticio-node": "^0.0.8"
}
```

Sailor is the Node.js SDK of the OIH platform which main role is to make the transformer part of OIH platform and it ensures a smooth communication with the platform.

### component.json

The file acts as an transformer descriptor which is interpreted by OIH platform to gather all required information. For instance, you could define the transformer's title and description. Here is the only place where the transformer's functionality could be listed. In this case we only have **actions** in both directions `from` and `to` OIH.

```json
{
  "title": "Contacts Transformation",
  "description": "Generic Data Transformation Component",
  "actions": {
    "transformPersonFromOih": {
      "title": "Transform a person from OIH",
      "description": "Transform the incoming data from OIH",
      "main": "./lib/actions/transformPersonFromOih.js",
      "metadata": {
        "out": {}
      }
    },
    "transformPersonToOih": {
      "title": "Transform a person to OIH",
      "description": "Transform the incoming data to OIH",
      "main": "./lib/actions/transformPersonToOih.js",
      "metadata": {
        "out": {}
      }
    }
  }
}
```

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

## Actions
This template **transformer** supports the following **actions**:

#### Actions:
  - Transform a person **from** OIH (```transformPersonFromOih.js```)
  - Transform a person **to** OIH (```transformPersonToOih.js```)

### Logo

If you have a logo for your adapter, you can place a file called `logo.png` in the root directory of your adapter. In case you don't provide a logo, a generic one will be shown.

### Spec

This directory consists of test files and seed data for testing purposes. Tests are not mandatory, but we highly recommend you to test the functionality before you deploy the transformer.

## Deployment

When you are ready to deploy your transformer, you should first build a [Docker image](https://docs.docker.com/v17.09/engine/userguide/storagedriver/imagesandcontainers/) of your transformer and then push it to [Dockerhub repository](https://hub.docker.com/u/openintegrationhub) of Openintegrationhub.
The first command creates the image and the second one pushes it to [Dockerhub](https://hub.docker.com/u/openintegrationhub).

```Dockefile
docker build -t openintegrationhub/[NAME] .
docker push openintegrationhub/NAME[:TAG]
```
Within the Open Integration Hub framework, the [Component Repository](https://openintegrationhub.github.io/docs/Services/ComponentRepository.html) is the place which lists all components your user can access, by referencing the docker images.

## Getting Started

This transformer example is developed and tested with [Snazzy Contacts API](https://snazzycontacts.com). If you want to build a flow using this example, you could use it in combination with [contacts-adapter-template](https://github.com/openintegrationhub/contacts-adapter-template).


To run the tests locally just run `npm test`.

## License

Apache-2.0 © [Wice GmbH](https://wice.de/)
