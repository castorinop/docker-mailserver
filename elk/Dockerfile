FROM sebp/elk

RUN mkdir /etc/logstash/patterns.d
#postfix grok and filter
RUN curl -L https://raw.githubusercontent.com/whyscream/postfix-grok-patterns/master/postfix.grok > /etc/logstash/patterns.d/postfix.grok
RUN curl -L https://raw.githubusercontent.com/whyscream/postfix-grok-patterns/master/50-filter-postfix.conf > /etc/logstash/conf.d/15-filter-postfix.conf
# custom amavis grok and filter
ADD amavis.grok  /etc/logstash/patterns.d
RUN curl -L https://raw.githubusercontent.com/ninech/logstash-patterns/master/exmples/50-filter-amavis.conf > /etc/logstash/conf.d/16-filter-amavis.conf
# dovecot grok and filter
RUN curl -L https://raw.githubusercontent.com/ninech/logstash-patterns/master/patterns.d/dovecot.grok > /etc/logstash/patterns.d/dovecot.grok
RUN curl -L https://raw.githubusercontent.com/ninech/logstash-patterns/master/exmples/50-filter-dovecot.conf > /etc/logstash/conf.d/17-filter-dovecot.conf
# FIXME: may be a cron job? 
RUN mkdir  -p /usr/share/GeoIP && \
 curl -L http://geolite.maxmind.com/download/geoip/database/GeoLiteCity.dat.gz | gunzip -c - > /usr/share/GeoIP/GeoLiteCity.dat 

WORKDIR ${LOGSTASH_HOME}
RUN gosu logstash bin/logstash-plugin install --local --no-verify logstash-filter-geoip

# override beats input 
ADD 02-beats-input.conf /etc/logstash/conf.d/
# override syslog
ADD 10-syslog.conf /etc/logstash/conf.d/



