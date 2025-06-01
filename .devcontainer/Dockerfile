FROM node:22

# disable all npm funding messags
ENV NPM_CONFIG_FUND=false

# Install basic development tools
RUN --mount=type=cache,mode=0755,id=apt-cache,target=/var/cache/apt,sharing=locked \
    --mount=type=cache,mode=0755,id=apt-data,target=/var/lib/apt,sharing=locked \
    apt update \
    && apt install -y less man-db sudo ca-certificates gnupg2 \
    && npm install -g corepack \
    && npm update -g npm yarn \
    && mkdir -p /home/node/.yarn /home/node/.cache/node/corepack \
    && chown node:node /home/node/.yarn /home/node/.cache/node/corepack \
    && mkdir -p /workspaces/modifiedmc.com \
    && chown node:node /workspaces/modifiedmc.com


# Ensure default `node` user has access to `sudo`
ARG USERNAME=node
RUN echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME

USER node
COPY package.json yarn.lock /workspaces/modifiedmc.com/
RUN cd /workspaces/modifiedmc.com \
    && yarn install --immutable \
    && rm /workspaces/modifiedmc.com/package.json /workspaces/modifiedmc.com/yarn.lock

COPY .devcontainer/bashrc /home/node/.bashrc
COPY .devcontainer/bin /usr/local/bin

# Set `DEVCONTAINER` environment variable to help with orientation
ENV DEVCONTAINER=true
WORKDIR /workspaces/modifiedmc.com
ENTRYPOINT [ "yarn-entrypoint" ]
