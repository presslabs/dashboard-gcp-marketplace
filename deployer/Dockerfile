ARG MARKETPLACE_TOOLS_TAG
FROM launcher.gcr.io/google/debian9 AS build

RUN apt-get update \
    && apt-get install -y --no-install-recommends gettext

ADD schema.yaml /tmp/schema.yaml

# Provide registry prefix and tag for default values for images.
ARG REGISTRY
ARG TAG
RUN cat /tmp/schema.yaml \
    | env -i "REGISTRY=$REGISTRY" "TAG=$TAG" envsubst \
    > /tmp/schema.yaml.new \
    && mv /tmp/schema.yaml.new /tmp/schema.yaml

FROM gcr.io/cloud-marketplace-tools/k8s/deployer_envsubst:$MARKETPLACE_TOOLS_TAG

COPY manifest/*.yaml.template /data/manifest/

COPY --from=build /tmp/schema.yaml /data/
COPY apptest/deployer /data-test/

ENV WAIT_FOR_READY_TIMEOUT 1800
