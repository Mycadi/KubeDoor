# 多阶段构建支持不同架构的二进制文件
FROM --platform=$BUILDPLATFORM debian:12.8 AS base

WORKDIR /

ARG TARGETPLATFORM

# 将所有二进制文件复制到镜像中
COPY kubedoor-agent-amd64 kubedoor-agent-arm64 ./

# 使用 TARGETPLATFORM 环境变量选择正确的二进制文件
RUN case "$TARGETPLATFORM" in \
        "linux/amd64") mv kubedoor-agent-amd64 kubedoor-agent ;; \
        "linux/arm64") mv kubedoor-agent-arm64 kubedoor-agent ;; \
        *) echo "Unsupported platform: $TARGETPLATFORM" && exit 1 ;; \
    esac

# 构建最终镜像
FROM  debian:12.8

WORKDIR /
COPY --from=base /kubedoor-agent /kubedoor-agent
# USER 65532:65532

ENTRYPOINT ["/kubedoor-agent"]