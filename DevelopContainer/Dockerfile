
ARG compiler "gcc"

FROM henrij22/dunebase-${compiler}:latest
ARG ccompiler "gcc-12"
ENV C_COMPILER=${ccompiler}

ARG cppcompiler "g++-12"
ENV CPP_COMPILER=${cppcompiler}

LABEL maintainer=mueller@ibb.uni-stuttgart.de


RUN cd dune && \
  git clone https://github.com/rath3t/dune-iga.git  && \
  cd dune-iga  && \
  git checkout main && \
  cd .. && \
  ./dune-common/bin/dunecontrol --only=dune-iga --cmake-opts="$DUNE_CMAKE_FLAGS" cmake && \
  ./dune-common/bin/dunecontrol --only=dune-iga make 

