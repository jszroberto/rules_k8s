# Copyright 2017 The Bazel Authors. All rights reserved.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#    http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
package(default_visibility = ["//visibility:public"])

licenses(["notice"])  # Apache 2.0

load("@examples_todocontroller_pip//:requirements.bzl", "requirement")
load("@io_bazel_rules_docker//python:image.bzl", "py_image")

py_image(
    name = "controller",
    srcs = ["controller.py"],
    main = "controller.py",
    deps = [
        requirement("kubernetes"),
    ],
)

load("@k8s_deploy//:defaults.bzl", "k8s_deploy")

k8s_deploy(
    name = "controller-deployment",
    images = {
        "us.gcr.io/not-my-project/todo-controller:staging": ":controller",
    },
    template = "//examples/todocontroller:deployment.yaml",
)

load("@io_bazel_rules_k8s//k8s:objects.bzl", "k8s_objects")

k8s_objects(
    name = "staging",
    objects = [
        "//examples/todocontroller:todo-crd",
        ":controller-deployment",
    ],
)
