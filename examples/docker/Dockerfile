ARG BuildImage=ekidd/rust-musl-builder:stable 

# Build
FROM $BuildImage AS Build
ADD --chown=rust:rust . ./
RUN cargo build --release

FROM alpine:3 AS transform 
ARG BinName=transform

RUN echo building ${BinName}
LABEL "project.namespace"="dolysis" 
LABEL "dolysis.binary"=${BinName}

WORKDIR /home/${BinName}
COPY --from=Build /home/rust/src/target/x86_64-unknown-linux-musl/release/${BinName} .

ENTRYPOINT [./${BinName}]

FROM alpine:3 AS extract
ARG BinName=extract

RUN echo building ${BinName}
LABEL "project.namespace"="dolysis" 
LABEL "dolysis.binary"=${BinName}

WORKDIR /home/${BinName}
COPY --from=Build /home/rust/src/target/x86_64-unknown-linux-musl/release/${BinName} .

ENTRYPOINT [./${BinName}]

FROM alpine:3 AS load
ARG BinName=load

RUN echo building ${BinName}
LABEL "project.namespace"="dolysis"
LABEL "dolysis.binary"=${BinName}

WORKDIR /home/${BinName}
COPY --from=Build /home/rust/src/target/x86_64-unknown-linux-musl/release/$BinName .

ENTRYPOINT [./${BinName}]
