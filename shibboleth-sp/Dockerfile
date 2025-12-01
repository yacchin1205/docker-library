# Updated: 2024-12 for containerd v2.0+ compatibility (Schema 2 image)
FROM rockylinux:8

COPY security:shibboleth.repo /etc/yum.repos.d/security:shibboleth.repo

RUN dnf -y update \
    && dnf -y install httpd mod_ssl shibboleth \
    && dnf -y clean all

RUN test -d /var/run/lock || mkdir -p /var/run/lock \
    && test -d /var/lock/subsys/ || mkdir -p /var/lock/subsys/ \
    && echo "export LD_LIBRARY_PATH=/opt/shibboleth/lib64:$LD_LIBRARY_PATH" >> /etc/sysconfig/shibd \
    && echo "export SHIBD_USER=shibd" >> /etc/sysconfig/shibd \
    && sed -i \
        -e "s|log4j.appender.shibd_log=.*$|log4j.appender.shibd_log=org.apache.log4j.ConsoleAppender|" \
        -e "s|log4j.appender.warn_log=.*$|log4j.appender.warn_log=org.apache.log4j.ConsoleAppender|" \
        -e "s|log4j.appender.tran_log=.*$|log4j.appender.tran_log=org.apache.log4j.ConsoleAppender|" \
        -e "s|log4j.appender.sig_log=.*$|log4j.appender.sig_log=org.apache.log4j.ConsoleAppender|" \
        /etc/shibboleth/shibd.logger

RUN sed -i -r \
        -e "s|^(\s*ErrorLog)\s+\S+|\1 /dev/stderr|" \
        -e 's|^(\s*CustomLog)\s+\S+\s+(.*$)|\1 /dev/stdout \2 env=\!dontlog|' \
        /etc/httpd/conf/httpd.conf \
    && echo "ServerSignature Off" >> /etc/httpd/conf/httpd.conf \
    && echo "ServerTokens Prod" >> /etc/httpd/conf/httpd.conf \
    && rm -f /etc/httpd/conf.d/{autoindex.conf,welcome.conf}

COPY httpd-shibd-foreground /usr/local/bin/
RUN chmod +x /usr/local/bin/httpd-shibd-foreground

EXPOSE 80 443

CMD ["httpd-shibd-foreground"]
