FROM openeuler/openeuler:22.03-lts AS build

RUN yum update -y && yum install -y \
    git \
    java-17-openjdk \
    python3-pip \
    python3-devel \
    gcc \
    && rm -rf /var/cache/yum \
    && pip3 install virtualenv

WORKDIR /opt
RUN git clone --recurse-submodules https://github.com/opensourceways/sbom-service.git -b BRANCH
WORKDIR /opt/sbom-service
RUN /bin/bash gradlew bootWar

ENTRYPOINT ["/bin/bash", "/opt/sbom-service/docker-entrypoint.sh"]
