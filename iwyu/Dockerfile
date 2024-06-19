FROM henrij22/dunebase-clang:latest
ENV C_COMPILER="clang-18"

ENV CPP_COMPILER="clang-18"


LABEL maintainer=jakob@ibb.uni-stuttgart.de

USER root
ENV DEBIAN_FRONTEND "noninteractive"
ENV CI true
EXPOSE 8000

RUN git clone https://github.com/include-what-you-use/include-what-you-use.git && \
  cd include-what-you-use  && \
  git checkout clang_18 && \
  cd .. && \
  mkdir build && cd build && \
  cmake -G "Unix Makefiles" -DCMAKE_PREFIX_PATH=/usr/lib/llvm-18 ../include-what-you-use && \
  make


