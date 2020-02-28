FROM sakadonohito/php5.6.23-apache

LABEL Author="P3rh4ps"

COPY ./files/ /tmp/files/

RUN mv -f /tmp/files/sources.list /etc/apt/sources.list && \
    rm -rf /var/www/html/* && \
    mv -f /tmp/files/html/ /var/www/ && \
    apt update && \
    echo "debconf mysql-server/root_password password root\ndebconf mysql-server/root_password_again password root" >> /tmp/mysql-passwd && \
    debconf-set-selections /tmp/mysql-passwd && apt install mysql-server -y && rm -rf /tmp/mysql-passwd && \
    mysql_install_db --user=mysql --datadir=/var/lib/mysql && \
    sh -c 'mysqld_safe &' && \
    sleep 5s && \
    chown -R root:root /var/www/html && \
    mysql -e "source /tmp/files/sql.sql;" -uroot -proot && \
    mysql -u root -proot -e "set global read_only=1;" && \
    mysql -u root -proot -e "create user 'noob123'@'localhost' identified by 'noob123';" && \
    mysql -u root -proot -e "grant select on noob123.* to 'noob123'@'localhost';" && \
    chmod 755 -R /var/www/html/
EXPOSE 80
COPY start.sh /start.sh
RUN chmod +x /start.sh
CMD ["/start.sh"]
