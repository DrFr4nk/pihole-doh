# Derived from the work of Elton Kuzniewski
# https://github.com/eltonk/pihole-doh

FROM pihole/pihole:latest

LABEL maintainer="Francesco Loprete"
LABEL url="https://github.com/DrFr4nk/pihole-doh"

# Install and configure cloudflared
RUN /bin/bash -c 'cd /tmp; \
apt-get update; \
apt-get install wget -y; \
wget https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-arm64; \
mv ./cloudflared-linux-arm64 /usr/local/bin/cloudflared; \
chmod +x /usr/local/bin/cloudflared;'

COPY config.yml /etc/cloudflared/config.yml

RUN /bin/bash -c 'mkdir -p /etc/services.d/cloudflared; \
echo "#!/usr/bin/env bash" > /etc/services.d/cloudflared/run; \
echo "s6-echo Starting cloudflared" >> /etc/services.d/cloudflared/run; \
echo "/usr/local/bin/cloudflared --config /etc/cloudflared/config.yml" >> /etc/services.d/cloudflared/run; \
echo "#!/usr/bin/env bash" > /etc/services.d/cloudflared/finish; \
echo "s6-echo Stopping cloudflared" >> /etc/services.d/cloudflared/finish; \
echo "killall -9 cloudflared" >> /etc/services.d/cloudflared/finish;'

RUN chmod +x /etc/services.d/cloudflared/run; \
chmod +x /etc/services.d/cloudflared/finish;
