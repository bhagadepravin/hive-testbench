MYOS=$(shell uname -s)

all: target/lib/dbgen.jar target/tpch-gen-1.0-SNAPSHOT.jar

target/tpch-gen-1.0-SNAPSHOT.jar: $(shell find -name *.java)
	mvn package

target/tpch_kit.zip:
	mkdir -p target/
	wget -q -O $@ https://github.com/electrum/tpch-dbgen/archive/refs/heads/master.zip 2>/dev/null || \
	curl -sL -o $@ https://github.com/electrum/tpch-dbgen/archive/refs/heads/master.zip

target/lib/dbgen.jar: target/tools/dbgen
	cd target/; mkdir -p lib/; ( jar cvf lib/dbgen.jar tools/ || gjar cvf lib/dbgen.jar tools/ )

target/tools/dbgen: target/tpch_kit.zip
	test -d target/tools/ || (cd target; unzip -q tpch_kit.zip; mv tpch-dbgen-master tools)
	test ! -d patches/${MYOS} || (cd target/tools && cat ../../patches/${MYOS}/*.patch | patch -p0 --batch)
	cd target/tools; make -f makefile.suite clean; make -f makefile.suite CC=gcc DATABASE=ORACLE MACHINE=LINUX WORKLOAD=TPCH

clean:
	mvn clean
