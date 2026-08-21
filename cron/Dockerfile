ARG BASE=ghcr.io/cloudnative-pg/postgresql:18-minimal-trixie
ARG PG_MAJOR=18
ARG EXT_VERSION=1.6.7-3.pgdg13+1
FROM $BASE AS builder

ARG PG_MAJOR
ARG EXT_VERSION

USER 0

RUN apt-get update
RUN apt-get install -y --no-install-recommends "postgresql-${PG_MAJOR}-cron=${EXT_VERSION}"

FROM scratch

ARG PG_MAJOR

# Licenses
COPY --from=builder /usr/share/doc/postgresql-${PG_MAJOR}-cron/copyright /licenses/postgresql-${PG_MAJOR}-pg_cron/

# Libraries
COPY --from=builder /usr/lib/postgresql/${PG_MAJOR}/lib/pg_cron.so /lib/
COPY --from=builder /usr/lib/postgresql/${PG_MAJOR}/lib/bitcode/ /lib/bitcode

# Share
COPY --from=builder /usr/share/postgresql/${PG_MAJOR}/extension/pg_cron* /share/extension/

USER 65532:65532
