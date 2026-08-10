FROM docker.io/texlive/texlive:TL2025-historic


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
	openssh-client \
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

ARG RTK_VERSION=0.45.0

RUN set -eux; \
	case "$(dpkg --print-architecture)" in \
	amd64) rtk_target="x86_64-unknown-linux-musl" ;; \
	arm64) rtk_target="aarch64-unknown-linux-gnu" ;; \
	*) echo "Unsupported architecture" >&2; exit 1 ;; \
	esac; \
	curl -fsSL "https://github.com/rtk-ai/rtk/releases/download/v${RTK_VERSION}/rtk-${rtk_target}.tar.gz" \
	| tar -xz -C /usr/local/bin rtk; \
	chmod +x /usr/local/bin/rtk

CMD ["sleep", "infinity"]