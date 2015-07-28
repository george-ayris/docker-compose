# Set nginx base image
FROM nginx

# Install Curl
RUN apt-get update -qq && apt-get -y install curl

# Download and Install Consul Template
ENV CT_URL https://github.com/hashicorp/consul-template/releases/download/v0.10.0/consul-template_0.10.0_linux_amd64.tar.gz
RUN curl -L $CT_URL | \
tar -C /usr/local/bin --strip-components 1 -zxf -

# Setup Consul Template Files
RUN mkdir /etc/consul-templates
COPY nginx.conf /etc/consul-templates/nginx.conf
ENV CT_FILE /etc/consul-templates/nginx.conf

# Setup Nginx File
ENV NX_FILE /etc/nginx/app.conf

# Start nginx and consul-template
CMD /usr/sbin/nginx -c /etc/nginx/nginx.conf \
& CONSUL_TEMPLATE_LOG=debug consul-template \
  -consul=consul:8500 \
  -template "$CT_FILE:$NX_FILE:/usr/sbin/nginx -s reload";