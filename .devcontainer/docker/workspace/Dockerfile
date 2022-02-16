ARG PHP_VERSION=7.4
FROM php:${PHP_VERSION}-cli

# [Option] Install zsh
ARG INSTALL_ZSH="true"
# [Option] Upgrade OS packages to their latest versions
ARG UPGRADE_PACKAGES="true"

ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID

COPY library-scripts/*.sh /tmp/library-scripts/

RUN apt-get update
RUN export DEBIAN_FRONTEND=noninteractive \
    && bash /tmp/library-scripts/common-debian.sh "${INSTALL_ZSH}" "${USERNAME}" "${USER_UID}" "${USER_GID}" "${UPGRADE_PACKAGES}" \
    && apt-get -y install --no-install-recommends lynx

# [Option] Install Node.js
ARG INSTALL_NODE="true"
ARG NODE_VERSION="none"
ARG UPDATE_RC="false"
ENV NVM_DIR=/usr/local/share/nvm
ENV NVM_SYMLINK_CURRENT=true \
    PATH=${NVM_DIR}/current/bin:${PATH}

RUN if [ "$INSTALL_NODE" = "true" ]; then /bin/bash /tmp/library-scripts/node-debian.sh "${NVM_DIR}" "${NODE_VERSION}" "${USERNAME}" "${UPDATE_RC}"; fi
RUN curl -f https://get.pnpm.io/v6.16.js | node - add --global pnpm

# Clean up
RUN apt-get autoremove -y && apt-get clean -y && rm -rf /var/lib/apt/lists/* /tmp/library-scripts/

COPY config/aliases.sh /home/${USERNAME}/aliases.sh

RUN sed -i 's/\r//' /home/${USERNAME}/aliases.sh && \
    chown ${USER_UID}:${USER_GID} /home/${USERNAME}/aliases.sh && \
    echo "" >> /home/${USERNAME}/.zshrc && \
    echo "# Load Custom Aliases" >> /home/${USERNAME}/.zshrc && \
    echo "source ~/aliases.sh" >> /home/${USERNAME}/.zshrc && \
	echo "" >> /home/${USERNAME}/.zshrc

CMD [ "sleep", "infinity" ]
