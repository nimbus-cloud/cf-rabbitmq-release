# Cloud Foundry RabbitMQ Service

This repository contains the release for RabbitMQ for Cloud Foundry.
It is deployable by BOSH in the usual way.

This release is now using BOSH v2 [job links](https://bosh.io/docs/links.html) and [cloud config](https://bosh.io/docs/cloud-config.html) and requires at least BOSH Director v255.5

## Dependencies

- [bundler](http://bundler.io/)

- [phantomjs](http://phantomjs.org/): In order to run the integration tests you need to have `phantomjs` available in the `$PATH`. `phantomjs` is required by `capybara` at runtime.

## Updating

Clone the repository and run `./scripts/update-release` to update submodules and install dependencies.

## Deploying

Once you have a [BOSH Lite up and running locally](https://github.com/cloudfoundry/bosh-lite), run `scripts/deploy-bosh-lite`.

To deploy the release into BOSH you will need a deployment manifest. You can generate a deployment manifest using the following command:
```bash
bosh interpolate --vars-file=manifests/lite-vars-file.yml --var=director-uuid=$(bosh status --uuid) manifests/cf-rabbitmq-server-only-template.yml > manifests/cf-rabbitmq.yml
```

## Testing

### Unit Tests

To run the unit tests locally, just run: `bundle exec rake spec:unit`.

### Integration Tests
Integration tests require this release to be deployed using BOSH director.

To run integration tests to `bundle exec rake spec:integration`.

Use `SKIP_SYSLOG=true bundle exec rake spec:integration` to skip syslog tests if you don't have `PAPERTAIL_TOKEN` and `PAPERTRAIL_GROUP_ID` environment variables configured.

### System Tests
System tests require this release and other releases to be deployed alongside with the same BOSH director.

Ensure you have deployed the release to BOSH Lite. Set `BOSH_MANIFEST` env to `$PWD/manifests/cf-rabbitmq.yml` and run `bundle exec rake spec:system`

If you want to run tests on custom BOSH you need to set following environment variables:

```sh
export CF_DOMAIN='bosh-lite.com'
export CF_USERNAME='admin'
export CF_PASSWORD='admin'
export CF_API='api.bosh-lite.com'
export BOSH_TARGET='bosh-lite.com'
export BOSH_USERNAME='admin'
export BOSH_PASSWORD='admin'
```

## Documentation

 * [BOSH Installation](docs/bosh_install.md)
 * [Anatomy of RabbitMQ BOSH release](docs/bosh_rabbitmq.md)
 * [Service Broker](docs/service_broker.md)
