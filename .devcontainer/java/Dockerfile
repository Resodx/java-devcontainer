# Use an official OpenJDK runtime as a parent image
FROM openjdk:11-jdk-slim

USER root

RUN echo "[-] Install dependencies [-]" \
    && apt-get update && apt-get install -y \
    curl \
    git \
    wget \
    zsh \
    fontconfig \
    && rm -rf /var/lib/apt/lists/*

RUN echo "[-] Create developer user [-]" \
    && if ! id -u developer > /dev/null 2>&1; then \
    adduser --home /home/developer \
    --shell /bin/zsh \
    --uid 1000 \
    --disabled-password \
    -q developer; \
    fi

USER developer

RUN if [ ! -d "/home/developer/.oh-my-zsh" ]; then \
    echo "[-] Install oh-my-zsh [-]" \
    && sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)" \
    && git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-/home/developer/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting \
    && git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-/home/developer/.oh-my-zsh/custom}/plugins/zsh-autosuggestions \
    && git clone https://github.com/zsh-users/zsh-completions ${ZSH_CUSTOM:-${ZSH:-/home/developer/.oh-my-zsh}/custom}/plugins/zsh-completions \
    && git clone https://github.com/romkatv/powerlevel10k.git /home/developer/.oh-my-zsh/custom/themes/powerlevel10k; \
    fi

COPY --chown=developer:developer .p10k.zsh .zshrc /home/developer/

RUN if [ ! -d "/home/developer/.fonts" ]; then \
    echo "[-] Download fonts [-]" \
    && mkdir -p ~/.fonts && cd  ~/.fonts \
    && wget https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS%20NF%20Regular.ttf \
    && wget https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS%20NF%20Bold.ttf \
    && wget https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS%20NF%20Italic.ttf \
    && wget https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS%20NF%20Bold%20Italic.ttf; \
    fi  

# Clear fontconfig cache
RUN fc-cache -fv

# Set the working directory to /app
WORKDIR /app

# Copy the current directory contents into the container at /app
COPY . /app
