FROM mcr.microsoft.com/devcontainers/javascript-node:1-20-bookworm

ENV PATH=/usr/local/bin:${PATH}
# Install latest pnpm
RUN npm install -g pnpm@9.0.2

# [Optional] Uncomment this section to install additional OS packages.
# RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
#     && apt-get -y install --no-install-recommends <your-package-list-here>

# [Optional] Uncomment if you want to install an additional version of node using nvm
# ARG EXTRA_NODE_VERSION=10
# RUN su node -c "source /usr/local/share/nvm/nvm.sh && nvm install ${EXTRA_NODE_VERSION}"

# [Optional] Uncomment if you want to install more global node modules
# RUN su node -c "npm install -g <your-package-list-here>"

COPY library-scripts/*.sh /tmp/library-scripts/
COPY postinstall.sh /tmp/library-scripts/

ENV DOCKER_BUILDKIT=1
RUN apt-get update && apt-get install -y dos2unix \
    && rm -rf /var/lib/apt/lists/*

# Normalize script line endings (fixes CRLF -> LF issues when building on Windows hosts)
RUN set -eux; \
    for f in /tmp/library-scripts/*.sh; do \
    if [ -f "$f" ]; then dos2unix "$f" || true; fi; \
    done

RUN /bin/bash /tmp/library-scripts/docker-in-docker-debian.sh
RUN /bin/bash /tmp/library-scripts/postinstall.sh
