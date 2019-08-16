FROM centos
LABEL maintainer="yuji.yasuno@gmail.com"

ENV working_dir=/tmp/docker
ENV magent_dir=/var/www
ENV magento_db_name=magentodb
ENV magento_db_user=magento
ENV magento_db_password=magento
ENV magento_admin_user=admin
ENV magento_admin_password=admin123

WORKDIR ${working_dir}

# copy all required files to working directory
COPY server_data ${workingdir}
# update yum
RUN yum -y update && yum clean all
# install apache
RUN yum -y install httpd
# install epel-release and remi-rep rpm
RUN yum -y install packages/epel-release-latest-7.noarch.rpm && yum -y install packages/remi-release-7.rpm
# install php
RUN yum -y install \
        php72 \
        php72-php \
        php72-php-pdo \
        php72-php-mysqlnd \
        php72-php-opcache \
        php72-php-xml \
        php72-php-mcrypt \
        php72-php-gd \
        php72-php-devel \
        php72-php-intl \
        php72-php-mbstring \
        php72-php-bcmath \
        php72-php-json \
        php72-php-soap \
        php72-php-pecl-zip
# install mariadb client
#RUN cp conf/MariaDB.repo /etc/yum.repos.d
#RUN yum -y install MariaDB-client
# copy magento files to magento directory
RUN cp -r -f magento/html ${magent_dir}
# install magento using command line
# RUN php72 ${magent_dir}/html/bin/magento setup:install \
#     --base-url=http://localhost \
#     --db-host=db \
#     --db-name=${magento_db_name} \
#     --db-user=${magento_db_user} \
#     --db-password=${magento_db_password} \
#     --admin-firstname=Magento \
#     --admin-lastname=Admin \
#     --admin-email=admin@magento.demo \
#     --admin-user=${magento_admin_user} \
#     --admin-password=${magento_admin_password}} \
#     --language=en_US \
#     --currency=USD \
#     --timezone=Asia/Tokyo \
#     --use-rewrites=1 \
#     --backend-frontname=admin
# set permission of magento directory so that apache can read
RUN chown -R apache:apache ${magent_dir}
# copy httpd.conf for magento
RUN cp -r -f conf/httpd.conf /etc/httpd/conf

# expose port 80 for apach
EXPOSE 80

# start apache in forground
ENTRYPOINT ["/usr/sbin/httpd","-DFOREGROUND"]