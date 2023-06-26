FROM mcr.microsoft.com/devcontainers/base:jammy

RUN set -ex; echo 'alias k=kubectl' >> /home/vscode/.bashrc && echo 'complete -F __start_kubectl k' >> /home/vscode/.bashrc && \
    echo 'export PATH=${KREW_ROOT:-$HOME/.krew}/bin:$PATH' >> /home/vscode/.bashrc && \
    curl -sL https://raw.githubusercontent.com/crossplane/crossplane/master/install.sh | sh && \
    # Move the crossplane kubectl extension to the bin
    sudo mv kubectl-crossplane /usr/local/bin && \
    sudo curl -Lo /usr/local/bin/tk https://github.com/grafana/tanka/releases/latest/download/tk-linux-amd64 && \
    sudo chmod a+x /usr/local/bin/tk && \
    sudo curl -Lo /usr/local/bin/jb https://github.com/jsonnet-bundler/jsonnet-bundler/releases/latest/download/jb-linux-amd64 && \
    sudo chmod a+x /usr/local/bin/jb && \
    sudo apt update ; sudo apt install jsonnet && \
    sudo mkdir -p /etc/apt/keyrings && \
    curl -fsSL https://repo.charm.sh/apt/gpg.key | sudo gpg --dearmor -o /etc/apt/keyrings/charm.gpg && \
    echo "deb [signed-by=/etc/apt/keyrings/charm.gpg] https://repo.charm.sh/apt/ * *" | sudo tee /etc/apt/sources.list.d/charm.list && \
    sudo apt update && sudo apt install gum

USER vscode
RUN cd "$(mktemp -d)" && \
    OS="$(uname | tr '[:upper:]' '[:lower:]')" && \
    ARCH="$(uname -m | sed -e 's/x86_64/amd64/' -e 's/\(arm\)\(64\)\?.*/\1\2/' -e 's/aarch64$/arm64/')" && \
    KREW="krew-${OS}_${ARCH}" && \
    curl -fsSLO "https://github.com/kubernetes-sigs/krew/releases/latest/download/${KREW}.tar.gz" && \
    tar zxvf "${KREW}.tar.gz" && \
    ./"${KREW}" install krew

USER root
