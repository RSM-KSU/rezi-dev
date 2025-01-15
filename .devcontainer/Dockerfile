FROM mcr.microsoft.com/devcontainers/python:3.11-bullseye

# adding the rossmann certificates to the container
COPY root.crt /usr/local/share/ca-certificates/root.crt
COPY rms-intern.crt /usr/local/share/ca-certificates/rms-intern.crt
RUN update-ca-certificates

# adding the mozilla ca bundle (these are the certifcates that are also used by the python package certifi) to the container; necessary for python to work
RUN curl -o /usr/local/share/ca-certificates/mozilla-ca-bundle.crt https://curl.se/ca/cacert.pem

# append the rossmann certificates to the mozilla ca bundle
RUN cat /usr/local/share/ca-certificates/root.crt >> /usr/local/share/ca-certificates/mozilla-ca-bundle.crt
RUN cat /usr/local/share/ca-certificates/rms-intern.crt >> /usr/local/share/ca-certificates/mozilla-ca-bundle.crt

# adding the root crt to the environment variables; necessary to automatically install extensions to vscode
ENV NODE_EXTRA_CA_CERTS=/usr/local/share/ca-certificates/root.crt

# adding the certificates to the environment variables
ENV REQUESTS_CA_BUNDLE=/usr/local/share/ca-certificates/mozilla-ca-bundle.crt
ENV SSL_CERT_FILE=/usr/local/share/ca-certificates/mozilla-ca-bundle.crt

# adding the rossmann root certificate to pip
COPY pip.ini ~/.config/pip/pip.conf

# update pip and install requirements
RUN pip install --upgrade pip
