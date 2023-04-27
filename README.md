# nginx-sneaky-chatgpt
A way to reverse proxy traffic to the OpenAI API through a custom domain to bypass outbound restrictions to chatgpt api services. 

# Setup 
Setup a domain, or subdomain on a public facing machine utilize certbot --nginx -d domain.com  to have it configure nginx for you. 
Once you have TLS termination add the following to your NGINX config to reverse proxy traffic to OpenAPI.

location / {
        root /var/www/html;
        resolver 8.8.8.8;
        proxy_set_header   X-Real-IP $remote_addr;
        proxy_set_header Connection "";
        proxy_pass https://api.openai.com/;
        proxy_read_timeout 600;
        proxy_connect_timeout 600;
        proxy_send_timeout 600;
        proxy_ssl_session_reuse off;
        proxy_ssl_server_name on;
        proxy_ssl_name $proxy_host;
        send_timeout 600;
}

Next test it out.  Instead of making API calls to:  https://api.openai.com/v1/completions for example, you would start making calls to https://yourdomain.com/v1/completions instead. 

This has worked to bypass outbound web filters and other network restrictions to the API.  This would work for pretty much any REST api or simple website. 
