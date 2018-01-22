FROM jbonjean/volumio

RUN apt-get update && apt-get install -y minizip wget

COPY files/ /files

RUN dpkg -i /files/snapserver_0.12.0_amd64.deb && \
    apt-get install -f -y && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
    rm -f rm -f /var/cache/debconf/config.dat-old /var/cache/debconf/templates.dat-old && \
    find /var/log -type f -delete && \
    rm -f /snapserver_0.12.0_amd64.deb && \
    mv files/snapcast /etc/service/snapcast && \
    mv files/mpd.conf /etc/mpd.conf && \
    echo 'VOLUMIO_VARIANT="volumio"' >> /etc/os-release
