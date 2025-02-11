FROM texlive/texlive:latest


# KeTCindyの動作に必要なパッケージをインストール
RUN apt-get update && apt-get install -y \
	wget \
	tar \
	jq \
	r-base \
	maxima \
	gcc \
	evince \
	pkexec \
	sudo

# Cinderellaのアーカイブをダウンロードして解凍
RUN wget https://beta.cinderella.de/Cinderella-3.0b.2089.tar.gz -O /tmp/Cinderella-3.0b.2089.tar.gz \
	&& tar -xzf /tmp/Cinderella-3.0b.2089.tar.gz -C /opt \
	&& rm /tmp/Cinderella-3.0b.2089.tar.gz

# /opt/cinderellaにパスを通す
ENV PATH="/opt/cinderella:$PATH"

# ketcindyの最新リリースをダウンロードして解凍
RUN LATEST_RELEASE=$(wget -qO- https://api.github.com/repos/ketpic/ketcindy/releases/latest | jq -r '.tag_name') \
	&& wget https://github.com/ketpic/ketcindy/archive/$LATEST_RELEASE.tar.gz -O /tmp/ketcindy-$LATEST_RELEASE.tar.gz \
	&& mkdir -p /home/ketcindy \
	&& tar -xzf /tmp/ketcindy-$LATEST_RELEASE.tar.gz -C /home/ketcindy \
	&& rm /tmp/ketcindy-$LATEST_RELEASE.tar.gz