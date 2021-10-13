FROM perl:5

COPY cpanfile /
RUN cpanm --installdeps .
RUN mkdir -p /app /var/log/usm_pon

WORKDIR /app
CMD [ "perl", "usm_pon.pl" ]