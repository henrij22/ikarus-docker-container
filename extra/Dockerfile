ARG compiler "gcc"

FROM ikarusproject/ikarus-dev-${compiler}:latest


ARG ccompiler "gcc-12"
ENV C_COMPILER=${ccompiler}

ARG cppcompiler "g++-12"
ENV CPP_COMPILER=${cppcompiler}

LABEL maintainer=jakob@ibb.uni-stuttgart.de

USER root
ENV DEBIAN_FRONTEND "noninteractive"
ENV CI true
EXPOSE 8000

# RUN git clone https://github.com/include-what-you-use/include-what-you-use.git && \
#   cd include-what-you-use  && \
#   git checkout clang_16 && \
#   cd .. && \
#   mkdir build && cd build && \
#   cmake -DCMAKE_PREFIX_PATH=/usr/lib/llvm-16 -DCMAKE_C_COMPILER=$C_COMPILER -DCMAKE_CXX_COMPILER=$CPP_COMPILER  ../include-what-you-use && \
#   make


# install muesli
RUN git clone https://bitbucket.org/ignromero/muesli.git && \
  cd muesli/muesli && \
  make 

ENV CMAKE_PREFIX_PATH $CMAKE_PREFIX_PATH:$HOME/muesli/


# # Install gismo

# # C compiler              : {clang-6.0,clang-7,clang-8,clang-9,clang-10,gcc-7,gcc-8,gcc-9,gcc-10,gcc-11}
# ARG CC=clang-16
# # C++ compiler            : {clang++-6.0,clang++-7,clang++-8,clang++-9,clang++-10,g++-7,g++-8,g++-9,g++-10,g++-11}
# ARG CXX=clang++-16

# # Fortran compiler        : {gfortran-7,gfortran-8,gfortran-9,gfortran-10,gfortran-11}
# ARG FC=na

# # CMAKE_CXX_STANDARD      : {11,14,17,20,23}
# ARG CMAKE_CXX_STANDARD=20

# # CMAKE_BUILD_TYPE        : {Release,Debug,RelWithDebInfo,MinSizeRel}
# ARG CMAKE_BUILD_TYPE=Release

# # CMAKE_INSTALL_PREFIX
# ARG CMAKE_INSTALL_PREFIX=/home/gismo/gismo

# # GISMO_BUILD_EXAMPLES    : {ON,OFF}
# ARG GISMO_BUILD_EXAMPLES=OFF

# # GISMO_BUILD_LIB         : {ON,OFF}
# ARG GISMO_BUILD_LIB=ON

# # GISMO_BUILD_PCH         : {ON,OFF}
# ARG GISMO_BUILD_PCH=OFF

# # GISMO_BUILD_UNITTESTS   : {ON,OFF}
# ARG GISMO_BUILD_UNITTESTS=OFF

# # GISMO_COEFF_TYPE        : {float,double,long double,mpfr::mpreal,mpq_class,posit_2_0,posit_3_0,posit_3_1,posit_4_0,posit_8_0,posit_8_1,posit_16_1,posit_32_2,posit_64_3,posit_128_4,posit_256_5}
# ARG GISMO_COEFF_TYPE=double

# # GISMO_INDEX_TYPE        : {int,int8_t,int16_t,int32_t,int64_t,long,long long}
# ARG GISMO_INDEX_TYPE=int

# # GISMO_SHORT_TYPE        : {int,int8_t,int16_t,int32_t,int64_t,long,long long}
# ARG GISMO_SHORT_TYPE=int

# # GISMO_OPTIONAL        : {gsCoDiPack,gsCompFlow,gsElasticity,gsExaStencils,gsGmp,gsIpOpt,gsKLShell,gsMpfr,gsOpenCascade,gsOpennurbs,gsStructuralAnalysis,gsTrilinos,motor,unsupported}
# ARG GISMO_OPTIONAL=''

# # GISMO_VERSION           : {v0.8.1,v0.8.2,v0.8.3,v0.8.4,v21.12.0,HEAD,specific hash}
# ARG GISMO_VERSION=HEAD

# # GISMO_WITH_ADIFF        : {ON,OFF}
# ARG GISMO_WITH_ADIFF=OFF

# # GISMO_WITH_MPI          : {ON,OFF}
# ARG GISMO_WITH_MPI=ON

# # GISMO_WITH_OPENMP       : {ON,OFF}
# ARG GISMO_WITH_OPENMP=OFF

# # GISMO_WITH_PARDISO      : {ON,OFF}
# ARG GISMO_WITH_PARDISO=OFF

# # GISMO_WITH_PASTIX       : {ON,OFF}
# ARG GISMO_WITH_PASTIX=OFF

# # GISMO_WITH_PSOLID       : {ON,OFF}
# ARG GISMO_WITH_PSOLID=OFF

# # GISMO_WITH_SMESH        : {ON,OFF}
# ARG GISMO_WITH_SMESH=OFF

# # GISMO_WITH_SPECTRA      : {ON,OFF}
# ARG GISMO_WITH_SPECTRA=OFF

# # GISMO_WITH_SUPERLU      : {ON,OFF}
# ARG GISMO_WITH_SUPERLU=ON

# # GISMO_WITH_TAUCS        : {ON,OFF}
# ARG GISMO_WITH_TAUCS=OFF

# # GISMO_WITH_UMFPACK      : {ON,OFF}
# ARG GISMO_WITH_UMFPACK=ON

# # Target architecture     : auto by default
# ARG TARGET_ARCHITECTURE=auto


# # Check out the open-source stable version of G+Smo
# RUN git clone https://github.com/gismo/gismo.git gismo

# # Configure and build G+Smo library
# RUN cd gismo    && \
#   mkdir build && \
#   cd build    && \
#   CC=$CC \
#   CXX=$CXX \
#   FC=$FC \
#   cmake .. \
#   -DCMAKE_BUILD_TYPE=$CMAKE_BUILD_TYPE \
#   -DCMAKE_CXX_STANDARD=$CMAKE_CXX_STANDARD \
#   -DCMAKE_INSTALL_PREFIX=$CMAKE_INSTALL_PREFIX \        
#   -DGISMO_BUILD_EXAMPLES=$GISMO_BUILD_EXAMPLES \
#   -DGISMO_BUILD_LIB=$GISMO_BUILD_LIB \
#   -DGISMO_BUILD_PCH=$GISMO_BUILD_PCH \
#   -DGISMO_BUILD_UNITTESTS=$GISMO_BUILD_UNITTESTS \
#   -DGISMO_COEFF_TYPE=$GISMO_COEFF_TYPE \
#   -DGISMO_INDEX_TYPE=$GISMO_INDEX_TYPE \
#   -DGISMO_SHORT_TYPE=$GISMO_SHORT_TYPE \
#   -DGISMO_OPTIONAL=$GISMO_OPTIONAL \
#   -DGISMO_WITH_ADIFF=$GISMO_WITH_ADIFF \
#   -DGISMO_WITH_MPI=$GISMO_WITH_MPI \
#   -DGISMO_WITH_OPENMP=$GISMO_WITH_OPENMP \
#   -DGISMO_WITH_PARDISO=$GISMO_WITH_PARDISO \
#   -DGISMO_WITH_PASTIX=$GISMO_WITH_PASTIX \
#   -DGISMO_WITH_PSOLID=$GISMO_WITH_PSOLID \
#   -DGISMO_WITH_SMESH=$GISMO_WITH_SMESH \
#   -DGISMO_WITH_SPECTRA=$GISMO_WITH_SPECTRA \
#   -DGISMO_WITH_SUPERLU=$GISMO_WITH_SUPERLU \
#   -DGISMO_WITH_TAUCS=$GISMO_WITH_TAUCS \
#   -DGISMO_WITH_UMFPACK=$GISMO_WITH_UMFPACK \
#   -DTARGET_ARCHITECTURE=$TARGET_ARCHITECTURE && \
#   make

# # Add directory of executables to search path
# ENV PATH $PATH:$CMAKE_INSTALL_PREFIX:$HOME/gismo/build/bin


