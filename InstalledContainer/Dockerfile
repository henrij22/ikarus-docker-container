ARG COMPILER="gcc"

FROM henrij22/ikarus-dev-${COMPILER}:latest
ARG branch "main"
ENV BRANCH=${branch}
USER root
ARG ccompiler "gcc-12"
ENV C_COMPILER=${ccompiler}

ARG cppcompiler "g++-12"
ENV CPP_COMPILER=${cppcompiler}
#docker build --squash -t henrij22/ikarus-gcc:latest --build-arg compiler="gcc" --build-arg ccompiler="gcc-12" --build-arg cppcompiler="g++-12"  . &> build.log
#docker build --squash -t henrij22/ikarus-clang:latest --build-arg compiler="clang" --build-arg ccompiler="clang-16" --build-arg cppcompiler="clang++-16"  . &> build.log


ENV DEBIAN_FRONTEND "noninteractive"
ENV CI true
EXPOSE 8000

# WORKDIR $HOME
# RUN useradd --create-home --no-log-init --shell /bin/bash docker
# USER docker
# WORKDIR /home/docker


RUN git clone https://github.com/ikarus-project/Ikarus.git && \
    cd Ikarus && \
    git checkout ${BRANCH} && \
    mkdir build && \
    cd build && \
    cmake .. -DCMAKE_BUILD_TYPE=Release -DCMAKE_C_COMPILER=${C_COMPILER} -DCMAKE_CXX_COMPILER=${CPP_COMPILER} -DDUNE_PYTHON_ALLOW_GET_PIP=1 -DDUNE_PYTHON_VIRTUALENV_SETUP=TRUE -DDUNE_ENABLE_PYTHONBINDINGS=TRUE -DBUILD_SHARED_LIBS=TRUE && \
    cmake --build . --target ikarus && \
    cmake --install . 



CMD [ "/bin/sh" ]


