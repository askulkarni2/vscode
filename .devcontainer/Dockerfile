FROM jsii/superchain

RUN yum groupinstall -y 'Development Tools'
RUN yum install -y jq wget zsh sudo docker

RUN mv $(which aws) /usr/bin/aws_V1

ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=1000

# Setup user
RUN adduser $USERNAME -s /bin/zsh -u $USER_UID -U && \
  mkdir -p /etc/sudoers.d && \
  echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME && \
  chmod 0440 /etc/sudoers.d/$USERNAME

# install aws-cli v2
RUN curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip" && \
  unzip awscliv2.zip && \
  ./aws/install

# install kubectl
RUN curl -o kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/1.18.8/2020-09-18/bin/linux/amd64/kubectl && \
  chmod +x kubectl && \
  mv kubectl /usr/local/bin/kubectl

COPY ./bin/aws-sso-credential-process /usr/local/bin/

# Setup shell
USER $USERNAME
ENV TERM xterm \
  LANG=en_US.UTF-8 \
  ZSH_THEME agnoster
RUN sh -c "$(wget -O- https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)" "" --unattended &> /dev/null
RUN curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.37.2/install.sh | bash && \
    echo 'export NVM_DIR="$HOME/.nvm"' >> ~/.zshrc && \
    echo '[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"' >> ~/.zshrc && \
    echo '[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"' >> ~/.zshrc && \
    source ~/.zshrc && \
    nvm install 14

RUN sudo npm install --force -g yarn aws-cdk

RUN mkdir -p /home/${USERNAME}/.aws && aws configure set \
    credential_process /usr/local/bin/aws-sso-credential-process

# start zsh
CMD [ "zsh" ]