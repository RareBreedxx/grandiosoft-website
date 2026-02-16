FROM nginx:alpine

# Clear default content
RUN rm -rf /usr/share/nginx/html/*

# Copy your site
COPY . /usr/share/nginx/html

# Expose port
EXPOSE 80

# Add a simple healthcheck
HEALTHCHECK --interval=30s --timeout=5s --start-period=5s --retries=3 \
  CMD curl -f http://localhost/ || exit 1
  
# Run nginx
CMD ["nginx", "-g", "daemon off;"]

