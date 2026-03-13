# Copyright Amazon.com Inc. or its affiliates. All Rights Reserved.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#      http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

ARG BASE_IMAGE
ARG BUILDER_IMAGE
FROM quay.io/frrouting/frr:9.1.0 as frr-upstream

FROM $BASE_IMAGE

# Copy FRR binaries and libraries from upstream image
COPY --from=frr-upstream /usr/lib/frr/ /usr/lib/frr/
COPY --from=frr-upstream /usr/bin/vtysh /usr/bin/vtysh
COPY --from=frr-upstream /usr/sbin/frr* /usr/sbin/
COPY --from=frr-upstream /etc/frr/ /etc/frr/
COPY --from=frr-upstream /usr/lib64/libfrr* /usr/lib64/
COPY --from=frr-upstream /usr/lib64/frr/ /usr/lib64/frr/

# Copy shared library dependencies
COPY --from=frr-upstream /usr/lib64/libyang* /usr/lib64/
COPY --from=frr-upstream /usr/lib64/libjson-c* /usr/lib64/
COPY --from=frr-upstream /usr/lib64/libpcre* /usr/lib64/

# FRR runtime directories
RUN mkdir -p /var/run/frr && \
    mkdir -p /var/log/frr && \
    mkdir -p /tmp/frr

ENTRYPOINT ["/usr/lib/frr/docker-start"]
