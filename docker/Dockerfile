FROM registry.access.redhat.com/ubi8/ubi-minimal



ARG UNAME=myuser



ENV UID=100000000
ENV GID=100000000
ENV APP_FOLDER=/site



RUN microdnf update --disablerepo=* --enablerepo=ubi-8-appstream --enablerepo=ubi-8-baseos --disableplugin=subscription-manager -y && \
microdnf install shadow-utils.x86_64 -y && \
microdnf clean all && rm -rf /var/cache/yum && \
groupadd -g $GID -o $UNAME && \
# user home under /home/runtime
useradd -u $UID -g $GID $UNAME && \
# create app folder and grant relevant access
mkdir -p $APP_FOLDER && chown $UNAME $APP_FOLDER && chmod a+rwx $APP_FOLDER



# copy artisan cli into the image
COPY ./content/art /usr/bin/



# copy the mkdocs website into the image
COPY ./content/site /site/



WORKDIR /site/



# grant permissions to artisan CLI
RUN chmod ug+x /usr/bin/art



# launch artisan as a web server on port 8080 serving the content of the /site folder
#CMD ["sh", "-c", "art serve --default-root /intro -p 8080 /site"]
CMD ["sh", "-c", "art serve -p 8006 /site"]


USER $UNAME