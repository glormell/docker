FROM ruby

ONBUILD EXPOSE 3000

RUN apt-get update && apt-get upgrade -y && apt-get install nodejs sqlite3 -y
RUN gem install rails

ONBUILD COPY app /opt/app
ONBUILD WORKDIR /opt/app

ONBUILD RUN bundle install
ONBUILD RUN rake db:migrate
ONBUILD RUN rake db:seed

# docker build -t ft-rails:on-build .
