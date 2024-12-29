# Use the official Ubuntu base image
FROM ubuntu:latest

# Update and install necessary packages
RUN apt-get update && apt-get install -y nginx && apt-get clean

# Remove the default Nginx configuration
RUN rm /etc/nginx/sites-enabled/default

# Copy your custom Nginx configuration file
COPY nginx.conf /etc/nginx/nginx.conf

# Copy SSL certificates
COPY certs /etc/nginx/certs

# Expose the necessary ports
EXPOSE 80
EXPOSE 443

# Start Nginx
CMD ["nginx", "-g", "daemon off;"]
