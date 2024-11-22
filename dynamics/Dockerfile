FROM ikarusproject/dunebase-clang:latest


ARG ccompiler "gcc-12"
ENV C_COMPILER=${ccompiler}

ARG cppcompiler "g++-12"
ENV CPP_COMPILER=${cppcompiler}

LABEL maintainer=jakob@ibb.uni-stuttgart.de

USER root
ENV DEBIAN_FRONTEND "noninteractive"
ENV CI true
EXPOSE 8000

RUN cd dune && \
  git clone https://github.com/rath3t/dune-iga.git  && \
  cd dune-iga  && \
  git checkout feature/workOnCreateLvl && \
  cd / && \
  cd dune && \
  git clone  https://github.com/ikarus-project/dune-localfefunctions.git && \
  cd dune-localfefunctions && \
  git checkout main && \
  cd .. && \
  ./dune-common/bin/dunecontrol --only=dune-iga,dune-localfefunctions --cmake-opts="$DUNE_MODULE_CMAKE_FLAGS" cmake && \
  ./dune-common/bin/dunecontrol --only=dune-iga,dune-localfefunctions make 


RUN git clone https://github.com/ikarus-project/Ikarus.git && \
  cd Ikarus && \
  git checkout feature/dynamics && \
  mkdir build && \
  cd build && \
  cmake .. $DUNE_MODULE_CMAKE_FLAGS && \
  cmake --build . --target ikarus && \
  cmake --install . 

