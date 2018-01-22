#####################
Volumio2 Docker image
#####################


Based on https://hub.docker.com/r/jbonjean/volumio/

Thanks to `JBonjean <https://github.com/jbonjean>` ;)


Upstream projects
#################

https://github.com/volumio/Volumio2-UI.git
https://github.com/volumio/Volumio2

Usage
#####

docker run --restart=always -d --name volumio \
  -e HOST=http://192.168.1.1:3000 \
  -p 3000:3000 \
  -v ${MUSIC_DIR}:/var/lib/mpd/music/:ro \
  -v ${VOLUMIO_DATA_DIR}:/data \
  --device /dev/snd \
  jbonjean/volumio

With pulseaudio
###############

docker run --restart=always -d --name volumio \
    -e HOST=http://192.168.1.1:3000 \
    -p 3000:3000 \
    -v ${MUSIC_DIR}:/var/lib/mpd/music/:ro \
    -v ${VOLUMIO_DATA_DIR}:/data \
    -v /run/user/$(id -u)/pulse:/pulse:ro \
    -e PULSE_SERVER=unix:/pulse/native \
    -e PULSE_COOKIE_DATA="$(cat $HOME/.config/pulse/cookie)" \
    -e HOST_USER=$(id -u):$(id -g) \
    -e AUDIO_OUTPUT=pulse \
 jbonjean/volumio
