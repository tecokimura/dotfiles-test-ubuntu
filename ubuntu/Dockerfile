FROM ubuntu:22.04

ENV USER diveto
ENV USERPW diveto

# translate Japanese
RUN apt-get update && apt-get install -y language-pack-ja

# for dotfiles test
RUN apt-get install -y git curl wget zsh vim sed

RUN zsh
ENV SHELL /usr/bin/zsh
RUN sed -i.bak "s|$HOME:|$HOME:$SHELL|" /etc/passwd

ENV HOME /home/$USER

# 一般ユーザーアカウントを追加
RUN useradd -m $USER
# 一般ユーザーにsudo権限を付与
RUN gpasswd -a $USER sudo
# 一般ユーザーのパスワード設定
RUN echo "$USER:$USERPW" | chpasswd


USER $USER
WORKDIR $HOME

RUN echo "export LANG=ja_JP.UTF-8" >> $HOME/.zshrc
RUN dircolors -p > $HOME/.dircolors
RUN sed -i "s|DIR 01;34|DIR 01;36|" $HOME/.dircolors \
    && sed -i "s|LINK 01;36|LINK 01;33|" $HOME/.dircolors \
    && sed -i "s|EXEC 01;32|EXEC 01;34|" $HOME/.dircolors

RUN sh -c "$(curl -fsLS get.chezmoi.io)" -- -b $HOME/.local/bin
ENV PATH $HOME/.local/bin:$PATH

