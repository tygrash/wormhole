FROM alluxio/alluxio:2.0.0

ENV USER=datamaster
ENV UID=12345
ENV GID=23456

RUN addgroup --gid "$GID" "$USER" \
    && adduser \
    --disabled-password \
    --gecos "" \
    --home "$(pwd)" \
    --ingroup "$USER" \
    --no-create-home \
    --uid "$UID" \
    "$USER"

USER root
RUN chown -R datamaster:datamaster /opt/

USER datamaster

WORKDIR /opt/alluxio
ADD configs/alluxio-site.properties.template /opt/alluxio/conf/alluxio-site.properties

CMD ["./bin/alluxio", "format"]
CMD ["./bin/alluxio-start.sh", "master"]