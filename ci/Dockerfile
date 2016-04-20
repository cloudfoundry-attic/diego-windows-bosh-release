FROM golang:1.6

RUN apt-get update

RUN apt-get install -y zip unzip git ruby ruby-dev libxml2-dev libsqlite3-dev libxslt1-dev libpq-dev libmysqlclient-dev zlib1g-dev
RUN gem install bosh_cli --no-ri --no-rdoc
