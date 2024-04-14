# 阶段一： 构建WASM二进制文件
FROM golang:1.16 AS build

# 设置环境变量
ENV CGO_ENABLED=0 GOOS=linux GOARCH=amd64

# 复制源文件
COPY main.go .

# 用TinyGo编译WASM
RUN go get github.com/tinygo/net/http
RUN tinygo build -o /tmp/main.wasm -scheduler=coroutines -target wasi main.go

FROM secondstate/wasmedge:0.8.2

COPY --from=build /tmp/main.wasm .

ENTRYPOINT ["wasmedge", "/main.wasm"]