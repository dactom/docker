FROM alpine:latest
LABEL MAINTAINER="Peter Winter <peter@pwntr.com>" \
    Description="Simple and lightweight Samba docker container, based on Alpine Linux." \
    Version="1.1.0"

# upgrade base system and install samba and supervisord
RUN apk --no-cache upgrade && apk --no-cache add samba samba-common-tools supervisor vim

# create a dir for the config and the share
RUN mkdir /config /mydata

# copy config files from project folder to get a default config going for samba and supervisord
COPY *.conf /config/

# add a non-root user and group called "rio" with no password, no home dir, no shell, and gid/uid set to 1000
RUN addgroup -g 1000 USERNAME && adduser -D -H -G USERNAME -s /bin/false -u 1000 USERNAME

# create a samba user matching our user from above with a very simple password ("dedi")
RUN echo -e "PASSWORD\nPASWWORD" | smbpasswd -a -s -c /config/smb.conf USERNAME

# volume mappings
VOLUME /config /mydata

# exposes samba's default ports (135 for End Point Mapper [DCE/RPC Locator Service],
# 137, 138 for nmbd and 139, 445 for smbd)
EXPOSE 135/tcp 137/udp 138/udp 139/tcp 445/tcp

ENTRYPOINT ["supervisord", "-c", "/config/supervisord.conf"]
