# Same base as https://hub.docker.com/_/solr/ 6.1
FROM java:openjdk-8-jre

ENV reposIndexingStandaloneVersion=v1.0.0
ENV reposIndexingStandaloneSourceUrl=https://github.com/Reposoft/repos-indexing-standalone/archive/$reposIndexingStandaloneVersion.tar.gz
ENV reposIndexingStandaloneSourceSha1=a1054e0460b660f89a08b8c7fbbdd0a2ad46293a

RUN depsRuntime=' \
	' \
  && depsBuild=' \
		openjdk-8-jdk-headless \
    maven \
	' \
	&& apt-get update \
	&& apt-get install -y --no-install-recommends $depsRuntime \
	&& apt-get install -y --no-install-recommends $depsBuild \
	&& rm -r /var/lib/apt/lists/* \
	&& curl -SLs "$reposIndexingStandaloneSourceUrl" -o repos-indexing-standalone-$reposIndexingStandaloneVersion.tar.gz \
	&& echo "$reposIndexingStandaloneSourceSha1 repos-indexing-standalone-$reposIndexingStandaloneVersion.tar.gz" | sha1sum -c - \
	&& mkdir -p usr/src/repos-indexing-standalone \
	&& tar -xvf repos-indexing-standalone-$reposIndexingStandaloneVersion.tar.gz -C usr/src/repos-indexing-standalone --strip-components=1 \
	&& rm repos-indexing-* \
  && cd usr/src/repos-indexing-standalone/ \
  && mvn package \
  && cd ../../../ \
  && mv usr/src/repos-indexing-standalone/target/repos-indexing-standalone-jar-with-dependencies.jar opt/repos-indexing-standalone-jar-with-dependencies.jar \
  && rm usr/src/repos-indexing-standalone -Rf \
  && rm ~/.m2 -Rf \
  && apt-get purge -y --auto-remove $depsBuild

ENTRYPOINT [ "java", "-jar", "/opt/repos-indexing-standalone-jar-with-dependencies.jar" ]
