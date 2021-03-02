FROM ubuntu:20.04

ARG DEBIAN_FRONTEND=noninteractive
RUN apt update && apt install -y git cmake make gcc g++ clang libmysqlclient-dev libssl-dev libbz2-dev libreadline-dev libncurses-dev libace-6.4.5 libace-dev

ADD .git /azerothcore/.git
ADD deps /azerothcore/deps
ADD conf/dist /azerothcore/conf/dist
ADD src /azerothcore/src
ADD modules /azerothcore/modules
ADD CMakeLists.txt /azerothcore/CMakeLists.txt

ARG ENABLE_SCRIPTS=1
ENV ENABLE_SCRIPTS=$ENABLE_SCRIPTS

ENTRYPOINT  cd azerothcore/build && \
            cmake ../ -DCMAKE_INSTALL_PREFIX=/azeroth-server -DCMAKE_C_COMPILER=/usr/bin/clang -DCMAKE_CXX_COMPILER=/usr/bin/clang++ -DTOOLS=0 -DSCRIPTS=$ENABLE_SCRIPTS -DWITH_WARNINGS=1 -DCMAKE_C_FLAGS="-Werror" -DCMAKE_CXX_FLAGS="-Werror" && \
            MTHREADS=`grep -c ^processor /proc/cpuinfo`; MTHREADS=$(($MTHREADS + 2)) && \
            make -j $MTHREADS && \
            make install -j $MTHREADS && \
            # copy the binary files "authserver" and "worldserver" files back to the host
            # - the directories "/binworldserver" and "/binauthserver" are meant to be bound to the host directories
            # - see docker/build/README.md to view the bindings
            cp -f /azeroth-server/bin/worldserver /binworldserver && \
            cp -f /azeroth-server/bin/authserver /binauthserver
