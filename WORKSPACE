workspace(
    name = "bazel_nodejs",
    managed_directories = {"@npm": ["portal/node_modules"]},
)

load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")

http_archive(
    name = "build_bazel_rules_nodejs",
    sha256 = "d0c4bb8b902c1658f42eb5563809c70a06e46015d64057d25560b0eb4bdc9007",
    urls = [
        "http://deploy.i.brainpp.cn/library/bazelbuild/rules_nodejs/rules_nodejs-1.5.0.tar.gz",
        "https://github.com/bazelbuild/rules_nodejs/releases/download/1.5.0/rules_nodejs-1.5.0.tar.gz",
    ],
)

GOPROXY = "https://goproxy.cn"

# copy from bazelbuild/rules_go 0.20.1 by using goproxy
http_archive(
    name = "bazel_skylib",
    # 1.0.2, latest as of 2019-10-09
    urls = ["{}/github.com/bazelbuild/bazel-skylib/@v/v0.0.0-20191009164321-e59b620b392a.zip".format(GOPROXY)],
    sha256 = "5c0268703657f508891311f51e8abe61bcc5bf5322ff435202935c780338919e",
    strip_prefix = "github.com/bazelbuild/bazel-skylib@v0.0.0-20191009164321-e59b620b392a",
    type = "zip",
)

load("@build_bazel_rules_nodejs//:index.bzl", "node_repositories", "yarn_install")

node_repositories(
    package_json = ["//portal:package.json"],
    node_version = "12.16.1",
    yarn_version = "1.19.1",
    node_repositories = {
        "12.16.1-darwin_amd64": ("node-v12.16.1-darwin-x64.tar.gz", "node-v12.16.1-darwin-x64", "34895bce210ca4b3cf19cd480e6563588880dd7f5d798f3782e3650580d35920"),
        "12.16.1-linux_amd64": ("node-v12.16.1-linux-x64.tar.gz", "node-v12.16.1-linux-x64", "b2d9787da97d6c0d5cbf24c69fdbbf376b19089f921432c5a61aa323bc070bea"),
        "12.16.1-windows_amd64": ("node-v12.16.1-win-x64.zip", "node-v12.16.1-win-x64", "b93b73572c5e495154a9823d494de5729c77d1c83b041171154c4b5f3f76b590"),
    },
    node_urls = [
        "https://nodejs.org/dist/v{version}/{filename}",
    ],
    yarn_urls = [
        "https://github.com/yarnpkg/yarn/releases/download/v{version}/{filename}",
    ],
)

yarn_install(
    name = "npm",
    package_json = "//portal:package.json",
    yarn_lock = "//portal:yarn.lock",
    quiet = False,
    manual_build_file_contents = """filegroup(
name = "bin_files",
srcs = glob(["node_modules/.bin/*"]),
)""",
)
