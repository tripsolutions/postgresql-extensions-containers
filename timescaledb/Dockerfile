ARG BASE=ghcr.io/cloudnative-pg/postgresql:18-minimal-trixie
ARG PG_MAJOR=18
ARG EXT_VERSION=2.29.2
FROM $BASE AS builder

ARG PG_MAJOR
ARG EXT_VERSION

USER 0

RUN apt-get update
RUN apt-get install -y --no-install-recommends curl
RUN curl -s https://packagecloud.io/install/repositories/timescale/timescaledb/script.deb.sh | bash
RUN apt-get install -y --no-install-recommends "timescaledb-2-${EXT_VERSION}-postgresql-${PG_MAJOR}"

FROM scratch

ARG PG_MAJOR
ARG EXT_VERSION

# Licenses
COPY --from=builder /usr/share/doc/timescaledb-2-${EXT_VERSION}-postgresql-${PG_MAJOR}/copyright /licenses

# Libraries
COPY --from=builder /usr/lib/postgresql/${PG_MAJOR}/lib/timescaledb-*.so /lib/

# Share
COPY --from=builder /usr/share/postgresql/${PG_MAJOR}/extension/timescaledb* /share/extension/

USER 65532:65532
