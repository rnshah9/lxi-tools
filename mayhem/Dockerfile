FROM --platform=linux/amd64 ubuntu:22.04 as builder

RUN apt-get update
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y build-essential cmake lua5.3 pkg-config meson liblua5.3 liblxi-dev libgtk-4-dev git desktop-file-utils clang

COPY . /repo
WORKDIR /repo/build
RUN meson .. --prefix=/install
RUN ninja install
RUN clang /repo/mayhem/fuzz_strip_trailing_space.cpp -fsanitize=fuzzer,address /repo/src/misc.c -o /fuzz

FROM ubuntu:22.04 as package
RUN apt-get update
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y liblua5.3 liblxi-dev libreadline-dev
COPY --from=builder /install /install
COPY --from=builder /fuzz /fuzz_strip_trailing_space
