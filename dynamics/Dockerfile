# syntax=docker/dockerfile:1

FROM ikarusproject/dunebase-clang:latest

ARG ccompiler 
ENV C_COMPILER=${ccompiler}

ARG cppcompiler
ENV CPP_COMPILER=${cppcompiler}

LABEL maintainer=jakob@ibb.uni-stuttgart.de

USER root
ENV DEBIAN_FRONTEND "noninteractive"
ENV CI true
EXPOSE 8000

RUN cd dune && \
  git clone https://github.com/henrij22/dune-iga.git  && \
  cd dune-iga  && \
  git checkout main && \
  cd / && \
  cd dune && \
  git clone  https://github.com/ikarus-project/dune-localfefunctions.git && \
  cd dune-localfefunctions && \
  git checkout main && \
  cd .. && \
  ./dune-common/bin/dunecontrol --only=dune-iga,dune-localfefunctions --cmake-opts="$DUNE_MODULE_CMAKE_FLAGS" cmake && \
  ./dune-common/bin/dunecontrol --only=dune-iga,dune-localfefunctions make 

RUN --mount=type=secret,id=github_token,env=TIK_ACCESS_TOKEN,required=true git clone https://ac136110:${TIK_ACCESS_TOKEN}@github.tik.uni-stuttgart.de/ibb-ikarus/Ikarus.git && \
  cd Ikarus && \
  git checkout MA/RMPlatesDynamics && \
  mkdir build && \
  cd build && \
  cmake .. $DUNE_MODULE_CMAKE_FLAGS && \
  cmake --build . --target ikarus && \
  cmake --install . 

  