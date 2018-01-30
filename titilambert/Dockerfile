FROM jbonjean/volumio

COPY files/packages /files/packages

RUN apt-get update && \
    rm -f /etc/shairport-sync.conf && \
    apt-get install -y minizip wget && \
    (wget -q -O - http://apt.mopidy.com/mopidy.gpg | apt-key add -) && \
    wget -q -O /etc/apt/sources.list.d/mopidy.list https://apt.mopidy.com/stretch.list && \
    apt-get update && \
    apt-get install -y libspotify-dev cmake pkgconf libjson-glib-1.0-0 libsoup2.4 libnotify4 libsox2 shairport-sync && \
    apt-get install -y libjson-glib-dev libao-dev libdbus-glib-1-dev libnotify-dev libsoup2.4-dev libsox-dev && \
    dpkg -i /files/packages/snapserver_0.12.0_amd64.deb && \
    apt-get install -f -y && \
    mkdir -p ~/.config/spop && \
    git clone git://github.com/Schnouki/spop.git && \
    mkdir /spop/build && \
    cd /spop/build && \
    cmake -Wdev -DCMAKE_BUILD_TYPE=Debug -DCMAKE_INSTALL_PREFIX=/usr .. && \
    make all && make install && \
    apt-get purge -y cmake  libjson-glib-dev libao-dev libdbus-glib-1-dev libnotify-dev libsoup2.4-dev libsox-dev libspotify-dev && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
    rm -rf /var/cache/debconf/config.dat-old /var/cache/debconf/templates.dat-old && \
    find /var/log -type f -delete && \
    rm -rf /files/packages && \
    echo 'VOLUMIO_VARIANT="volumio"' >> /etc/os-release

COPY files/ files

RUN mv files/service/* /etc/service/ && \
    mv files/mpd.conf /etc/mpd.conf && \
    cp files/volumio /usr/local/bin/volumio && \
    mkdir -p /root/.config/spop/ && \
    mkdir -p /home/volumio/.config/spop && \
    chown -R volumio: /home/volumio && \
    cp /files/spopd.conf /etc/spopd.conf && \
    chown volumio:volumio /etc/spopd.conf && \
    cp /files/entrypoint /usr/bin/entrypoint && \
    mv /files/shairport-sync.conf /etc/shairport-sync.conf

WORKDIR /
#RUN chmod 777 /etc/spopd.conf
HEALTHCHECK --interval=5s --timeout=3s --retries=3 CMD curl -f http://localhost:3000 || exit 1
