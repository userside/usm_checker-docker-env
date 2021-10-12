FROM python:3.8

RUN pip install pyyaml pysnmp requests
RUN mkdir -p /var/log/userside/usm_checker /app

WORKDIR /app

CMD [ "python", "usm_checker.py" ]