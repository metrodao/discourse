# NAME:     discourse/discourse_bench
# VERSION:  latest
FROM discourse/discourse_test:latest

MAINTAINER Gerhard Schlager <mail@gerhard-schlager.at>

# Install ApacheBench
RUN apt-get install -y apache2-utils libsqlite3-dev

# configure Git to suppress warnings
RUN git config --global user.email "you@example.com" &&\
    git config --global user.name "Your Name"

RUN git pull &&\
    sudo -u discourse bundle install --standalone

RUN gem install facter &&\
    gem install mailcatcher

# reconfigure PostgreSQL template databases to use UTF-8
# otherwise rake db:create fails
RUN pg_dropcluster --stop 9.5 main &&\
    pg_createcluster --start -e UTF-8 9.5 main

# create role "discourse"
RUN /etc/init.d/postgresql start &&\
    sleep 5 &&\
    sudo -u postgres createuser --superuser discourse

ADD run_bench.sh /tmp/run_bench.sh
RUN chmod +x /tmp/run_bench.sh

ENTRYPOINT ["/tmp/run_bench.sh"]
