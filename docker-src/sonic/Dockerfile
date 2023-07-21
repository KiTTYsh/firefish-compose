FROM valeriansaliou/sonic:latest

# Sonic uses distroless base, add busybox as ash for startup script
COPY --from=busybox:uclibc /bin/busybox /bin/ash

COPY docker_init /

CMD [ "/docker_init" ]

