ARG ELK_VERSION

# https://www.docker.elastic.co/
FROM docker.elastic.co/elasticsearch/elasticsearch:${ELK_VERSION}-arm64

# Add your elasticsearch plugins setup here
# Example: RUN elasticsearch-plugin install analysis-icu


# RUN elasticsearch-plugin install https://github.com/KennFalcon/elasticsearch-analysis-hanlp/releases/download/v7.7.1/elasticsearch-analysis-hanlp-7.7.1.zip --batch


# install dependency
RUN yum install -y zip wget vim

# install gradle
# https://gradle.org/releases/
# https://linuxize.com/post/how-to-install-gradle-on-centos-7/

RUN yum install -y java-1.8.0-openjdk-devel \
    && java -version \
    && wget https://services.gradle.org/distributions/gradle-6.6.1-bin.zip -P /tmp \
    && unzip -d /opt/gradle /tmp/gradle-6.6.1-bin.zip \
    && touch /etc/profile.d/gradle.sh \
    && echo "export GRADLE_HOME=/opt/gradle/gradle-6.6.1" >> /etc/profile.d/gradle.sh \
    && echo "export PATH=\${GRADLE_HOME}/bin:\${PATH}" >> /etc/profile.d/gradle.sh \
    && chmod +x /etc/profile.d/gradle.sh \
    && source /etc/profile.d/gradle.sh \
    && gradle -v


# install jieba
# https://github.com/sing1ee/elasticsearch-jieba-plugin/releases
#RUN wget https://github.com/sing1ee/elasticsearch-jieba-plugin/archive/v7.4.2.zip -P /tmp \
#   && mkdir -p /usr/share/elasticsearch/plugins/jieba \
#    && unzip -d /usr/share/elasticsearch/plugins/jieba /tmp/v7.4.2.zip \
#    && cd /usr/share/elasticsearch/plugins/jieba/elasticsearch-jieba-plugin-7.4.2/ \ 
#    && gradle pz \
#    && mv build/distributions/elasticsearch-jieba-plugin-7.4.2.zip /usr/share/elasticsearch/plugins/jieba

RUN wget https://github.com/sing1ee/elasticsearch-jieba-plugin/archive/refs/tags/v7.4.2.zip -P /tmp \    
    && unzip -d /tmp/jieba/ /tmp/v7.4.2.zip \
    && cd /tmp/jieba/elasticsearch-jieba-plugin-7.4.2/ \
    && source /etc/profile.d/gradle.sh \
    && gradle pz


# unzip jieba package and edit config

RUN mkdir -p /usr/share/elasticsearch/plugins/jieba\ 
    && mv /tmp/jieba/elasticsearch-jieba-plugin-7.4.2/build/distributions/elasticsearch-jieba-plugin-7.4.2.zip /usr/share/elasticsearch/plugins/jieba/\
    && cd /usr/share/elasticsearch/plugins/jieba \
    && unzip elasticsearch-jieba-plugin-7.4.2.zip \
    && mkdir -p /usr/share/elasticsearch/config/stopwords \
    && cp /usr/share/elasticsearch/plugins/jieba/dic/stopwords.txt /usr/share/elasticsearch/config/stopwords/stopwords.txt \
    && sed -i s/elasticsearch.version\=7\.4\.2/elasticsearch.version=${ELK_VERSION}/g plugin-descriptor.properties \
    && echo "xpack.security.enabled:false" >> elasticsearch.yml
