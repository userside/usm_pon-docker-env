FROM perl:5

COPY cpanfile /
RUN cpanm --installdeps .
RUN mkdir -p /app /var/log/userside/usm_pon/logs

WORKDIR /app
CMD [ "perl", "usm_pon.pl" ]